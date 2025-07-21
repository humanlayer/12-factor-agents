[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 10. Small, Focused Agents

> 🐿️ **Squirrel Wisdom**: *With all his new capabilities, Nutkin was tempted to become the ultimate multi-purpose squirrel: nut collector, weather predictor, tree climber, bird whisperer, and neighborhood watch captain all at once. But he quickly realized he was becoming mediocre at everything and stressed out of his mind. So Nutkin made a wise decision - he focused on being the absolute best acorn-finder in the forest. Other squirrels started coming to him for advice! He was much happier and incredibly successful.*

Rather than building monolithic agents that try to do everything, build small, focused agents that do one thing well. Agents are just one building block in a larger, mostly deterministic system.

![1a0-small-focused-agents](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a0-small-focused-agents.png)

The key insight here is about LLM limitations: the bigger and more complex a task is, the more steps it will take, which means a longer context window. As context grows, LLMs are more likely to get lost or lose focus. By keeping agents focused on specific domains with 3-10, maybe 20 steps max, we keep context windows manageable and LLM performance high.

> #### As context grows, LLMs are more likely to get lost or lose focus

Benefits of small, focused agents:

1. **Manageable Context**: Smaller context windows mean better LLM performance
2. **Clear Responsibilities**: Each agent has a well-defined scope and purpose
3. **Better Reliability**: Less chance of getting lost in complex workflows
4. **Easier Testing**: Simpler to test and validate specific functionality
5. **Improved Debugging**: Easier to identify and fix issues when they occur

### What if LLMs get smarter? 

Do we still need this if LLMs get smart enough to handle 100-step+ workflows?

tl;dr yes. As agents and LLMs improve, they **might** naturally expand to be able to handle longer context windows. This means handling MORE of a larger DAG. This small, focused approach ensures you can get results TODAY, while preparing you to slowly expand agent scope as LLM context windows become more reliable. (If you've refactored large deterministic code bases before, you may be nodding your head right now.)



[![gif](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a5-agent-scope-grow.gif)](https://github.com/user-attachments/assets/0cd3f52c-046e-4d5e-bab4-57657157c82f
)

<details>
<summary><a href="https://github.com/humanlayer/12-factor-agents/blob/main/img/1a5-agent-scope-grow.gif">GIF Version</a></summary>
![gif](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a5-agent-scope-grow.gif)
</details>

Being intentional about size/scope of agents, and only growing in ways that allow you to maintain quality, is key here. As the [team that built NotebookLM put it](https://open.substack.com/pub/swyx/p/notebooklm?selection=08e1187c-cfee-4c63-93c9-71216640a5f8&utm_campaign=post-share-selection&utm_medium=web):

> I feel like consistently, the most magical moments out of AI building come about for me when I'm really, really, really just close to the edge of the model capability

Regardless of where that boundary is, if you can find that boundary and get it right consistently, you'll be building magical experiences. There are many moats to be built here, but as usual, they take some engineering rigor.

[← Compact Errors](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-9-compact-errors.md) | [Trigger From Anywhere →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-11-trigger-from-anywhere.md)
