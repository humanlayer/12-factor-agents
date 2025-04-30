<!--
### DRAFT

Preview - this is a draft of everything I've learned in a year working with agents, and I imagine it may evolve a bit as we go.

--> 


# 12-Factor Agents - Principles for building reliable LLM applications



*In the spirit of [12 Factor Apps](https://12factor.net/)*.  *The source for this project is public at https://github.com/humanlayer/12-factor-agents, and I welcome your feedback and contributions. Let's figure this out together!*

This is the current version of 12-factor agents, version 1.0. There is a draft of version 1.1  on the [v1.1 branch](https://github.com/humanlayer/12-factor-agents/tree/v1.1). There are a few [Issues to track work on v1.1](https://github.com/humanlayer/12-factor-agents/issues?q=is%3Aissue%20state%3Aopen%20label%3Aversion%3A%3A1.1).

 
<div align="center">
<a href="https://creativecommons.org/licenses/by-sa/4.0/">
        <img src="https://img.shields.io/badge/License-CC%20BY--SA%204.0-lightgrey.svg" alt="License: CC BY-SA 4.0"></a>
<a href="https://discord.gg/WffDJYazzy">
    <img src="https://img.shields.io/badge/chat-discord-5865F2" alt="Discord Server"></a>    
<a href="https://www.youtube.com/watch?v=yxJDyQ8v6P0">
    <img src="https://img.shields.io/badge/youtube-deep_dive-crimson" alt="YouTube
Deep Dive"></a>
    
</div>


<img referrerpolicy="no-referrer-when-downgrade" src="https://static.scarf.sh/a.png?x-pxid=2acad99a-c2d9-48df-86f5-9ca8061b7bf9" />

<a href="#visual-nav"><img width="907" alt="Screenshot 2025-04-03 at 2 49 07 PM" src="https://github.com/user-attachments/assets/23286ad8-7bef-4902-b371-88ff6a22e998" /></a>


Hi, I'm Dex. I've been [hacking](https://youtu.be/8bIHcttkOTE) on [AI agents](https://theouterloop.substack.com) for [a while](https://humanlayer.dev). 


**I've tried every agent framework out there**, from the plug-and-play crew/langchains to the "minimalist" smolagents of the world to the "production grade" langraph, griptape, etc. 

**I've talked to a lot of really strong founders**, in and out of YC, who are all building really impressive things with AI. Most of them are rolling the stack themselves. I don't see a lot of frameworks in production customer-facing agents.

**I've been surprised to find** that most of the products out there billing themselves as "AI Agents" are not all that agentic. A lot of them are mostly deterministic code, with LLM steps sprinkled in at just the right points to make the experience truly magical.

Agents, at least the good ones, don't follow the ["here's your prompt, here's a bag of tools, loop until you hit the goal"](https://www.anthropic.com/engineering/building-effective-agents#agents) pattern. Rather, they are comprised of mostly just software. 

So, I set out to answer:

> ### **What are the principles we can use to build LLM-powered software that is actually good enough to put in the hands of production customers?**

Welcome to 12-factor agents. As every Chicago mayor since Daley has consistently plastered all over the city's major airports, we're glad you're here.

*Special thanks to [@iantbutler01](https://github.com/iantbutler01), [@tnm](https://github.com/tnm), [@hellovai](https://www.github.com/hellovai), [@stantonk](https://www.github.com/stantonk), [@balanceiskey](https://www.github.com/balanceiskey), [@AdjectiveAllison](https://www.github.com/AdjectiveAllison), [@pfbyjy](https://www.github.com/pfbyjy), [@a-churchill](https://www.github.com/a-churchill), and the SF MLOps community for early feedback on this guide.*

## The Short Version: The 12 Factors

Even if LLMs [continue to get exponentially more powerful](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md#what-if-llms-get-smarter), there will be core engineering techniques that make LLM-powered software more reliable, more scalable, and easier to maintain.

- [How We Got Here: A Brief History of Software](https://github.com/humanlayer/12-factor-agents/blob/main/content/brief-history-of-software.md)
- [Factor 1: Natural Language to Tool Calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-1-natural-language-to-tool-calls.md)
- [Factor 2: Own your prompts](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-2-own-your-prompts.md)
- [Factor 3: Own your context window](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-3-own-your-context-window.md)
- [Factor 4: Tools are just structured outputs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-4-tools-are-structured-outputs.md)
- [Factor 5: Unify execution state and business state](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-5-unify-execution-state.md)
- [Factor 6: Launch/Pause/Resume with simple APIs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-6-launch-pause-resume.md)
- [Factor 7: Contact humans with tool calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-7-contact-humans-with-tools.md)
- [Factor 8: Own your control flow](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-8-own-your-control-flow.md)
- [Factor 9: Compact Errors into Context Window](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-9-compact-errors.md)
- [Factor 10: Small, Focused Agents](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md)
- [Factor 11: Trigger from anywhere, meet users where they are](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-11-trigger-from-anywhere.md)
- [Factor 12: Make your agent a stateless reducer](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-12-stateless-reducer.md)

### Visual Nav

|    |    |    |
|----|----|-----|
|[![factor 1](https://github.com/humanlayer/12-factor-agents/blob/main/img/110-natural-language-tool-calls.png)](./content/factor-1-natural-language-to-tool-calls.md) | [![factor 2](https://github.com/humanlayer/12-factor-agents/blob/main/img/120-own-your-prompts.png)](./content/factor-2-own-your-prompts.md) | [![factor 3](https://github.com/humanlayer/12-factor-agents/blob/main/img/130-own-your-context-building.png)](./content/factor-3-own-your-context-window.md) |
|[![factor 4](https://github.com/humanlayer/12-factor-agents/blob/main/img/140-tools-are-just-structured-outputs.png)](./content/factor-4-tools-are-structured-outputs.md) | [![factor 5](https://github.com/humanlayer/12-factor-agents/blob/main/img/150-unify-state.png)](./content/factor-5-unify-execution-state.md) | [![factor 6](https://github.com/humanlayer/12-factor-agents/blob/main/img/160-pause-resume-with-simple-apis.png)](./content/factor-6-launch-pause-resume.md) |
| [![factor 7](https://github.com/humanlayer/12-factor-agents/blob/main/img/170-contact-humans-with-tools.png)](./content/factor-7-contact-humans-with-tools.md) | [![factor 8](https://github.com/humanlayer/12-factor-agents/blob/main/img/180-control-flow.png)](./content/factor-8-own-your-control-flow.md) | [![factor 9](https://github.com/humanlayer/12-factor-agents/blob/main/img/190-factor-9-errors-static.png)](./content/factor-9-compact-errors.md) |
| [![factor 10](https://github.com/humanlayer/12-factor-agents/blob/main/img/1a0-small-focused-agents.png)](./content/factor-10-small-focused-agents.md) | [![factor 11](https://github.com/humanlayer/12-factor-agents/blob/main/img/1b0-trigger-from-anywhere.png)](./content/factor-11-trigger-from-anywhere.md) | [![factor 12](https://github.com/humanlayer/12-factor-agents/blob/main/img/1c0-stateless-reducer.png)](./content/factor-12-stateless-reducer.md) |

## How we got here

For a deeper dive on my agent journey and what led us here, check out [A Brief History of Software](./content/brief-history-of-software.md) - a quick summary here:

### The promise of agents

We're gonna talk a lot about Directed Graphs (DGs) and their Acyclic friends, DAGs. I'll start by pointing out that...well...software is a directed graph. There's a reason we used to represent programs as flow charts.

![010-software-dag](https://github.com/humanlayer/12-factor-agents/blob/main/img/010-software-dag.png)

### From code to DAGs

Around 20 years ago, we started to see DAG orchestrators become popular. We're talking classics like [Airflow](https://airflow.apache.org/), [Prefect](https://www.prefect.io/), some predecessors, and some newer ones like ([dagster](https://dagster.io/), [inggest](https://www.inngest.com/), [windmill](https://www.windmill.dev/)). These followed the same graph pattern, with the added benefit of observability, modularity, retries, administration, etc.

![015-dag-orchestrators](https://github.com/humanlayer/12-factor-agents/blob/main/img/015-dag-orchestrators.png)

### The promise of agents

I'm not the first [person to say this](https://youtu.be/Dc99-zTMyMg?si=bcT0hIwWij2mR-40&t=73), but my biggest takeaway when I started learning about agents, was that you get to throw the DAG away. Instead of software engineers coding each step and edge case, you can give the agent a goal and a set of transitions:

![025-agent-dag](https://github.com/humanlayer/12-factor-agents/blob/main/img/025-agent-dag.png)

And let the LLM make decisions in real time to figure out the path

![026-agent-dag-lines](https://github.com/humanlayer/12-factor-agents/blob/main/img/026-agent-dag-lines.png)

The promise here is that you write less software, you just give the LLM the "nodes" of the graph and let it figure out the edges. You can recover from errors, you can write less code, and you may find that LLMs find novel solutions to problems.


### Agents as loops

As we'll see later, it turns out this doesn't quite work.

Let's dive one step deeper - with agents you've got this loop consisting of 3 steps:

1. LLM determines the next step in the workflow, outputting structured json ("tool calling")
2. Deterministic code executes the tool call
3. The result is appended to the context window 
4. Repeat until the next step is determined to be "done"

```python
initial_event = {"message": "..."}
context = [initial_event]
while True:
  next_step = await llm.determine_next_step(context)
  context.append(next_step)

  if (next_step.intent === "done"):
    return next_step.final_answer

  result = await execute_step(next_step)
  context.append(result)
```

Our initial context is just the starting event (maybe a user message, maybe a cron fired, maybe a webhook, etc), and we ask the llm to choose the next step (tool) or to determine that we're done.

Here's a multi-step example:

[![027-agent-loop-animation](https://github.com/humanlayer/12-factor-agents/blob/main/img/027-agent-loop-animation.gif)](https://github.com/user-attachments/assets/3beb0966-fdb1-4c12-a47f-ed4e8240f8fd)

<details>
<summary><a href="https://github.com/humanlayer/12-factor-agents/blob/main/img/027-agent-loop-animation.gif">GIF Version</a></summary>

![027-agent-loop-animation](https://github.com/humanlayer/12-factor-agents/blob/main/img/027-agent-loop-animation.gif)]

</details>

## Why 12-factor agents?

At the end of the day, this approach just doesn't work as well as we want it to.

In building HumanLayer, I've talked to at least 100 SaaS builders (mostly technical founders) looking to make their existing product more agentic. The journey usually goes something like:

1. Decide you want to build an agent
2. Product design, UX mapping, what problems to solve
3. Want to move fast, so grab $FRAMEWORK and *get to building*
4. Get to 70-80% quality bar 
5. Realize that 80% isn't good enough for most customer-facing features
6. Realize that getting past 80% requires reverse-engineering the framework, prompts, flow, etc.
7. Start over from scratch

<details>
<summary>Random Disclaimers</summary>

**DISCLAIMER**: I'm not sure the exact right place to say this, but here seems as good as any: **this in BY NO MEANS meant to be a dig on either the many frameworks out there, or the pretty dang smart people who work on them**. They enable incredible things and have accelerated the AI ecosystem. 

I hope that one outcome of this post is that agent framework builders can learn from the journeys of myself and others, and make frameworks even better. 

Especially for builders who want to move fast but need deep control.

**DISCLAIMER 2**: I'm not going to talk about MCP. I'm sure you can see where it fits in.

**DISCLAIMER 3**: I'm using mostly typescript, for [reasons](https://www.linkedin.com/posts/dexterihorthy_llms-typescript-aiagents-activity-7290858296679313408-Lh9e?utm_source=share&utm_medium=member_desktop&rcm=ACoAAA4oHTkByAiD-wZjnGsMBUL_JT6nyyhOh30) but all this stuff works in python or any other language you prefer. 


Anyways back to the thing...

</details>

### Design Patterns for great LLM applications

After digging through hundreds of AI libriaries and working with dozens of founders, my instinct is this:

1. There are some core things that make agents great
2. Going all in on a framework and building what is essentially a greenfield rewrite may be counter-productive
3. There are some core principles that make agents great, and you will get most/all of them if you pull in a framework
4. BUT, the fastest way I've seen for builders to get high-quality AI software in the hands of customers is to take small, modular concepts from agent building, and incorporate them into their existing product
5. These modular concepts from agents can be defined and applied by most skilled software engineers, even if they don't have an AI background

> #### The fastest way I've seen for builders to get good AI software in the hands of customers is to take small, modular concepts from agent building, and incorporate them into their existing product


## The 12 Factors (again)


- [How We Got Here: A Brief History of Software](./content/brief-history-of-software.md)
- [Factor 1: Natural Language to Tool Calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-1-natural-language-to-tool-calls.md)
- [Factor 2: Own your prompts](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-2-own-your-prompts.md)
- [Factor 3: Own your context window](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-3-own-your-context-window.md)
- [Factor 4: Tools are just structured outputs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-4-tools-are-structured-outputs.md)
- [Factor 5: Unify execution state and business state](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-5-unify-execution-state.md)
- [Factor 6: Launch/Pause/Resume with simple APIs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-6-launch-pause-resume.md)
- [Factor 7: Contact humans with tool calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-7-contact-humans-with-tools.md)
- [Factor 8: Own your control flow](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-8-own-your-control-flow.md)
- [Factor 9: Compact Errors into Context Window](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-9-compact-errors.md)
- [Factor 10: Small, Focused Agents](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-10-small-focused-agents.md)
- [Factor 11: Trigger from anywhere, meet users where they are](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-11-trigger-from-anywhere.md)
- [Factor 12: Make your agent a stateless reducer](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-12-stateless-reducer.md)

## Honorable Mentions / other advice

- [Factor 13: Pre-fetch all the context you might need](./content/appendix-13-pre-fetch.md)

## Related Resources

- Contribute to this guide [here](https://github.com/humanlayer/12-factor-agents)
- [I talked about a lot of this on an episode of the Tool Use podcast](https://youtu.be/8bIHcttkOTE) in March 2025
- I write about some of this stuff at [The Outer Loop](https://theouterloop.substack.com)
- I do [webinars about Maximizing LLM Performance](https://github.com/hellovai/ai-that-works/tree/main) with [@hellovai](https://github.com/hellovai)
- We build OSS agents with this methodology under [got-agents/agents](https://github.com/got-agents/agents)
- We ignored all our own advice and built a [framework for running distributed agents in kubernetes](https://github.com/humanlayer/kubechain)
- Other links from this guide:
  - [12 Factor Apps](https://12factor.net)
  - [Building Effective Agents (Anthropic)](https://www.anthropic.com/engineering/building-effective-agents#agents)
  - [Prompts are Functions](https://thedataexchange.media/baml-revolution-in-ai-engineering/ )
  - [Library patterns: Why frameworks are evil](https://tomasp.net/blog/2015/library-frameworks/)
  - [The Wrong Abstraction](https://sandimetz.com/blog/2016/1/20/the-wrong-abstraction)
  - [Mailcrew Agent](https://github.com/dexhorthy/mailcrew)
  - [Mailcrew Demo Video](https://www.youtube.com/watch?v=f_cKnoPC_Oo)
  - [Chainlit Demo](https://x.com/chainlit_io/status/1858613325921480922)
  - [TypeScript for LLMs](https://www.linkedin.com/posts/dexterihorthy_llms-typescript-aiagents-activity-7290858296679313408-Lh9e)
  - [Schema Aligned Parsing](https://www.boundaryml.com/blog/schema-aligned-parsing)
  - [Function Calling vs Structured Outputs vs JSON Mode](https://www.vellum.ai/blog/when-should-i-use-function-calling-structured-outputs-or-json-mode)
  - [BAML on GitHub](https://github.com/boundaryml/baml)
  - [OpenAI JSON vs Function Calling](https://docs.llamaindex.ai/en/stable/examples/llm/openai_json_vs_function_calling/)
  - [Outer Loop Agents](https://theouterloop.substack.com/p/openais-realtime-api-is-a-step-towards)
  - [Airflow](https://airflow.apache.org/)
  - [Prefect](https://www.prefect.io/)
  - [Dagster](https://dagster.io/)
  - [Inngest](https://www.inngest.dev/)
  - [Windmill](https://www.windmill.dev/)
  - [The AI Agent Index (MIT)](https://aiagentindex.mit.edu/)
  - [NotebookLM on Finding Model Capability Boundaries](https://open.substack.com/pub/swyx/p/notebooklm?selection=08e1187c-cfee-4c63-93c9-71216640a5f8)




