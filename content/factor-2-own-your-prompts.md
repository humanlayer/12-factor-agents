[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 2. Own your prompts

> 🐿️ **Squirrel Wisdom**: *Encouraged by his bird feeder success, Nutkin thought he'd be clever and let his friend Alvin write his nut-gathering instructions. But when Alvin's generic "find nuts" plan led Nutkin straight into a poison oak patch, he realized with growing determination that he needed to write his own very specific instructions: "avoid the itchy three-leaf plants near the oak tree, check soil moisture first, mark good spots with scratch marks."*

Don't outsource your prompt engineering to a framework. 

![120-own-your-prompts](https://github.com/humanlayer/12-factor-agents/blob/main/img/120-own-your-prompts.png)

Some frameworks provide a "black box" approach like this:

```python
agent = Agent(
  role="...",
  goal="...",
  personality="...",
  tools=[tool1, tool2, tool3]
)

task = Task(
  instructions="...",
  expected_output=OutputModel
)

result = agent.run(task)
```

This is great for pulling in some TOP NOTCH prompt engineering to get you started, but it is often difficult to tune and/or reverse engineer to get exactly the right tokens into your model.

Instead, own your prompts and treat them as first-class code:

```rust
function DetermineNextStep(thread: string) -> DoneForNow | ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation {
  prompt #"
    {{ _.role("system") }}
    
    You are a helpful assistant that manages deployments for frontend and backend systems.
    You work diligently to ensure safe and successful deployments by following best practices
    and proper deployment procedures.
    
    Before deploying any system, you should check:
    - The deployment environment (staging vs production)
    - The correct tag/version to deploy
    - The current system status
    
    You can use tools like deploy_backend, deploy_frontend, and check_deployment_status
    to manage deployments. For sensitive deployments, use request_approval to get
    human verification.
    
    Always think about what to do first, like:
    - Check current deployment status
    - Verify the deployment tag exists
    - Request approval if needed
    - Deploy to staging before production
    - Monitor deployment progress
    
    {{ _.role("user") }}

    {{ thread }}
    
    What should the next step be?
  "#
}
```

(the above example uses [BAML](https://github.com/boundaryml/baml) to generate the prompt, but you can do this with any prompt engineering tool you want, or even just template it manually)

If the signature looks a little funny, we'll get to that in [factor 4 - tools are just structured outputs](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-4-tools-are-structured-outputs.md)

```typescript
function DetermineNextStep(thread: string) -> DoneForNow | ListGitTags | DeployBackend | DeployFrontend | RequestMoreInformation {
```

Key benefits of owning your prompts:

1. **Full Control**: Write exactly the instructions your agent needs, no black box abstractions
2. **Testing and Evals**: Build tests and evals for your prompts just like you would for any other code
3. **Iteration**: Quickly modify prompts based on real-world performance
4. **Transparency**: Know exactly what instructions your agent is working with
5. **Role Hacking**: take advantage of APIs that support nonstandard usage of user/assistant roles - for example, the now-deprecated non-chat flavor of OpenAI "completions" API. This includes some so-called "model gaslighting" techniques

Remember: Your prompts are the primary interface between your application logic and the LLM.

Having full control over your prompts gives you the flexibility and prompt control you need for production-grade agents.

I don't know what's the best prompt, but I know you want the flexibility to be able to try EVERYTHING.

[← Natural Language To Tool Calls](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-1-natural-language-to-tool-calls.md) | [Own Your Context Window →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-3-own-your-context-window.md)