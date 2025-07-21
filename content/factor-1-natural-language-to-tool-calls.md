[← Back to README](https://github.com/humanlayer/12-factor-agents/blob/main/README.md)

### 1. Natural Language to Tool Calls 

> 🐿️ **Squirrel Wisdom**: *Nutkin was just a novice squirrel who used to chatter randomly at the bird feeder, hoping seeds would somehow appear. One day he realized he could make specific, structured requests: "Please dispense sunflower seeds, quantity: 12, size: large." The birds were confused at first, but wow - Nutkin actually got fed efficiently for the first time! He felt a tiny spark of confidence.*

One of the most common patterns in agent building is to convert natural language to structured tool calls. This is a powerful pattern that allows you to build agents that can reason about tasks and execute them.

![110-natural-language-tool-calls](https://github.com/humanlayer/12-factor-agents/blob/main/img/110-natural-language-tool-calls.png)

This pattern, when applied atomically, is the simple translation of a phrase like

> can you create a payment link for $750 to Terri for sponsoring the february AI tinkerers meetup? 

to a structured object that describes a Stripe API call like

```json
{
  "function": {
    "name": "create_payment_link",
    "parameters": {
      "amount": 750,
      "customer": "cust_128934ddasf9",
      "product": "prod_8675309",
      "price": "prc_09874329fds",
      "quantity": 1,
      "memo": "Hey Jeff - see below for the payment link for the february ai tinkerers meetup"
    }
  }
}
```

**Note**: in reality the stripe API is a bit more complex, a [real agent that does this](https://github.com/dexhorthy/mailcrew) ([video](https://www.youtube.com/watch?v=f_cKnoPC_Oo)) would list customers, list products, list prices, etc to build this payload with the proper ids, or include those ids in the prompt/context window (we'll see below how those are kinda the same thing though!)

From there, deterministic code can pick up the payload and do something with it. (More on this in [factor 3](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-3-own-your-context-window.md))

```python
# The LLM takes natural language and returns a structured object
nextStep = await llm.determineNextStep(
  """
  create a payment link for $750 to Jeff 
  for sponsoring the february AI tinkerers meetup
  """
  )

// Handle the structured output based on its function
if nextStep.function == 'create_payment_link':
    stripe.paymentlinks.create(nextStep.parameters)
    return  # or whatever you want, see below
elif nextStep.function == 'something_else':
    # ... more cases
    pass
else:  # the model didn't call a tool we know about
    # do something else
    pass
```

**NOTE**: While a full agent would then receive the API call result and loop with it, eventually returning something like

> I've successfully created a payment link for $750 to Terri for sponsoring the february AI tinkerers meetup. Here's the link: https://buy.stripe.com/test_1234567890

**Instead**, We're actually going to skip that step here, and save it for another factor, which you may or may not want to also incorporate (up to you!)

[← How We Got Here](https://github.com/humanlayer/12-factor-agents/blob/main/content/brief-history-of-software.md) | [Own Your Prompts →](https://github.com/humanlayer/12-factor-agents/blob/main/content/factor-2-own-your-prompts.md)