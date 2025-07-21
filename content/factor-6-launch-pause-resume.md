[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 6. Launch/Pause/Resume with simple APIs

> 🐿️ **Squirrel Wisdom**: *With his unified journaling system working well, Nutkin realized he had a new superpower: he could pause any activity and resume it later! He used to lose track if interrupted mid-task, but now he could bookmark exactly where he was: "Bookmark: Chapter 3, paragraph 2 - digging near the maple tree, 4 inches deep." When his friend called, Nutkin could pick up right where he left off the next day. He was becoming quite the professional squirrel!*

Agents are just programs, and we have things we expect from how to launch, query, resume, and stop them.

[![pause-resume animation](https://github.com/humanlayer/12-factor-agents/blob/main/img/165-pause-resume-animation.gif)](https://github.com/user-attachments/assets/feb1a425-cb96-4009-a133-8bd29480f21f)

<details>
<summary><a href="https://github.com/humanlayer/12-factor-agents/blob/main/img/165-pause-resume-animation.gif">GIF Version</a></summary>

![pause-resume animation](https://github.com/humanlayer/12-factor-agents/blob/main/img/165-pause-resume-animation.gif)]

</details>


It should be easy for users, apps, pipelines, and other agents to launch an agent with a simple API.

Agents and their orchestrating deterministic code should be able to pause an agent when a long-running operation is needed.

External triggers like webhooks should enable agents to resume from where they left off without deep integration with the agent orchestrator.

Closely related to [factor 5 - unify execution state and business state](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-5-unify-execution-state.md) and [factor 8 - own your control flow](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-8-own-your-control-flow.md), but can be implemented independently.



**Note** - often AI orchestrators will allow for pause and resume, but not between the moment of tool selection and tool execution. See also [factor 7 - contact humans with tool calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-7-contact-humans-with-tools.md) and [factor 11 - trigger from anywhere, meet users where they are](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-11-trigger-from-anywhere.md).

[← Unify Execution State](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-5-unify-execution-state.md) | [Contact Humans With Tools →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-7-contact-humans-with-tools.md)