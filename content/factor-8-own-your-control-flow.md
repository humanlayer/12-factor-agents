[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 8. Own your control flow

> 🐿️ **Squirrel Wisdom**: *With his human communication mastered, Nutkin realized he could take control of his entire decision-making process. No more following the standard squirrel workflow of "see nut → grab nut → bury nut → forget where nut is buried → panic." Nutkin designed his own sophisticated control flow: IF nut_size > large THEN request_backup, IF predator_detected THEN pause_and_hide, IF winter_approaching THEN enable_hoarding_mode. He was becoming a true systems thinker!*

If you own your control flow, you can do lots of fun things.

![180-control-flow](https://github.com/humanlayer/12-factor-agents/blob/main/img/180-control-flow.png)


Build your own control structures that make sense for your specific use case. Specifically, certain types of tool calls may be reason to break out of the loop and wait for a response from a human or another long-running task like a training pipeline. You may also want to incorporate custom implementation of:

- summarization or caching of of tool call results
- LLM as judge on structured output
- context window compaction or other [memory management](https://github.com/humanlayer/12-factor-agents/blob/main/content/appendix-14-everything-is-context-engineering.md)
- logging, tracing, and metrics
- client-side rate limiting
- durable sleep / pause / "wait for event"


The below example shows three possible control flow patterns:


- request_clarification: model asked for more info, break the loop and wait for a response from a human
- fetch_git_tags: model asked for a list of git tags, fetch the tags, append to context window, and pass straight back to the model
- deploy_backend: model asked to deploy a backend, this is a high-stakes thing, so break the loop and wait for human approval

```python
def handle_next_step(thread: Thread):

  while True:
    next_step = await determine_next_step(thread_to_prompt(thread))
    
    # inlined for clarity - in reality you could put 
    # this in a method, use exceptions for control flow, or whatever you want
    if next_step.intent == 'request_clarification':
      thread.events.append({
        type: 'request_clarification',
          data: nextStep,
        })

      await send_message_to_human(next_step)
      await db.save_thread(thread)
      # async step - break the loop, we'll get a webhook later
      break
    elif next_step.intent == 'fetch_open_issues':
      thread.events.append({
        type: 'fetch_open_issues',
        data: next_step,
      })

      issues = await linear_client.issues()

      thread.events.append({
        type: 'fetch_open_issues_result',
        data: issues,
      })
      # sync step - pass the new context to the LLM to determine the NEXT next step
      continue
    elif next_step.intent == 'create_issue':
      thread.events.append({
        type: 'create_issue',
        data: next_step,
      })

      await request_human_approval(next_step)
      await db.save_thread(thread)
      # async step - break the loop, we'll get a webhook later
      break
```

This pattern allows you to interrupt and resume your agent's flow as needed, creating more natural conversations and workflows.

**Example** - the number one feature request I have for every AI framework out there is we need to be able to interrupt 
a working agent and resume later, ESPECIALLY between the moment of tool **selection** and the moment of tool **invocation**.

Without this level of resumability/granularity, there's no way to review/approve the tool call before it runs, which means
you're forced to either:

1. Pause the task in memory while waiting for the long-running thing to complete (think `while...sleep`) and restart it from the beginning if the process is interrupted.
2. Restrict the agent to only low-stakes, low-risk calls like research and summarization
3. Give the agent access to do bigger, more useful things, and just yolo hope it doesn't screw up


You may notice this is closely related to [factor 5 - unify execution state and business state](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-5-unify-execution-state.md) and [factor 6 - launch/pause/resume with simple APIs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-6-launch-pause-resume.md), but can be implemented independently.

[← Contact Humans With Tools](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-7-contact-humans-with-tools.md) | [Compact Errors →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-9-compact-errors.md)