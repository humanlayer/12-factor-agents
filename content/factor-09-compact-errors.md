[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 9. Compact Errors into Context Window

This one is a little short but is worth mentioning. One of these benefits of agents is "self-healing" - for short tasks, an LLM might call a tool that fails. Good LLMs have a fairly good chance of reading an error message or stack trace and figuring out what to change in a subsequent tool call.


Most frameworks implement this, but you can do JUST THIS without doing any of the other 11 factors. Here's an example: 


```python
thread = {"events": [initial_message]}

while True:
  next_step = await determine_next_step(thread_to_prompt(thread))
  thread["events"].append({
    "type": next_step.intent,
    "data": next_step,
  })
  try:
    result = await handle_next_step(thread, next_step) # our switch statement
  except Exception as e:
    # if we get an error, we can add it to the context window and try again
    thread["events"].append({
      "type": 'error',
      "data": format_error(e),
    })
    # loop, or do whatever else here to try to recover
```

You may want to implement an errorCounter for a specific tool call, to limit to ~3 attempts of a single tool, or whatever other logic makes sense for your use case. 

```python
consecutive_errors = 0

while True:

  # ... existing code ...

  try:
    result = await handle_next_step(thread, next_step)
    thread["events"].append({
      "type": next_step.intent + '_result',
      data: result,
    })
    # success! reset the error counter
    consecutive_errors = 0
  except Exception as e:
    consecutive_errors += 1
    if consecutive_errors < 3:
      # do the loop and try again
      thread["events"].append({
        "type": 'error',
        "data": format_error(e),
      })
    else:
      # break the loop, reset parts of the context window, escalate to a human, or whatever else you want to do
      break
  }
}
```
Hitting some consecutive-error-threshold might be a great place to [escalate to a human](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-07-contact-humans-with-tools.md), whether by model decision or via deterministic takeover of the control flow.

[![195-factor-09-errors](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-09-errors.gif)](https://github.com/user-attachments/assets/cd7ed814-8309-4baf-81a5-9502f91d4043)


<details>
<summary>[GIF Version](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-09-errors.gif)</summary>

![195-factor-09-errors](https://github.com/humanlayer/12-factor-agents/blob/main/img/195-factor-09-errors.gif)

</details>

Benefits:

1. **Self-Healing**: The LLM can read the error message and figure out what to change in a subsequent tool call
2. **Durable**: The agent can continue to run even if one tool call fails

I'm sure you will find that if you do this TOO much, your agent will start to spin out and might repeat the same error over and over again. 

That's where [factor 8 - own your control flow](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md) and [factor 3 - own your context building](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-03-own-your-context-window.md) come in - you don't need to just put the raw error back on, you can completely restructure how it's represented, remove previous events from the context window, or whatever deterministic thing you find works to get an agent back on track. Often, error handling calls can be completely removed from the primary control flow and context window. This can help keep context usage down and reduce the risk of the agent getting distracted from the main goal.

But the number one way to prevent error spin-outs is to embrace [factor 10 - small, focused agents](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md).

[← Own Your Control Flow](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-08-own-your-control-flow.md) | [Small Focused Agents →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md)
