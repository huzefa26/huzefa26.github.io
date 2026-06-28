+++
title = "A brief on the OpenAI (Chat Completions API) format"
date = 2026-06-28
summary = "The format used by most model providers for messaging with an LLM model."
tags = ["ai", "dev"]
categories = []
+++

The [OpenAI Chat Completions API](https://developers.openai.com/api/reference/chat-completions/overview) (`/v1/chat/completions`) is the industry-standard payload structure for LLMs. It uses a stateless **JSON payload** containing a target model and an array of message objects. [[1](https://developers.openai.com/api/reference/chat-completions/overview), [2](https://portkey.ai/blog/open-ai-responses-api-vs-chat-completions-vs-anthropic-anthropic-messages-api/), [3](https://esterox.com/blog/openai-apis-step-by-step-guide), [4](https://www.youtube.com/watch?v=mnJJPltybBM&t=332), [5](https://docs.openclaw.ai/gateway/openai-http-api)]

```json
{
  "model": "glm-5.2",
  "messages": [
    { "role": "system", "content": "You are a helpful coding assistant." },
    { "role": "user", "content": "Write a python function to fetch weather." }
  ],
  "temperature": 0.2
}
```

- **Roles:** Divided strictly into `system` (behavioral instructions), `user` (human prompt), and `assistant` (model's response).
- **Tool/Function Calling:** Tools are passed as an array of JSON schemas under a top-level `"tools"` parameter. The model responds with a role of `"assistant"` and a `"tool_calls"` array rather than a text string. [[1](https://www.digitalapplied.com/blog/ai-function-calling-guide-openai-anthropic-google), [2](https://medium.com/@jayeshpadhiar20/getting-started-with-openai-apis-a-complete-developers-guide-69d91d88855c), [3](https://www.youtube.com/watch?v=mnJJPltybBM&t=332), [4](https://www.youtube.com/watch?v=0pGxoubWI6s&vl=en&t=1043), [5](https://www.digitalapplied.com/blog/ai-function-calling-guide-openai-anthropic-google)]



Deep Dive Resources

- **Official Documentation:** Look at the comprehensive [OpenAI API Reference Guide](https://developers.openai.com/api/reference/overview) for the exact payload schemas and parameter tables.
- **Parameter Blueprint:** For a raw, concrete look at all valid attributes, review the [OpenAI Chat Completions API Reference Community Guide](https://community.openai.com/t/tip-chat-completions-api-reference-as-a-single-request-for-ai-understanding/1355654) which aggregates the complete schema parameters. [[1](https://developers.openai.com/api/reference/overview), [2](https://community.openai.com/t/tip-chat-completions-api-reference-as-a-single-request-for-ai-understanding/1355654), [3](https://futureagi.substack.com/p/how-to-integrate-openai-api-step)]



Alternative Industry Formats

As the AI ecosystem has matured, alternative formats have emerged to handle complex agent behaviors: [[1](https://portkey.ai/blog/open-ai-responses-api-vs-chat-completions-vs-anthropic-anthropic-messages-api/)]

1. Anthropic Messages API (`/v1/messages`)

Built natively for Claude models. [[1](https://portkey.ai/blog/open-ai-responses-api-vs-chat-completions-vs-anthropic-anthropic-messages-api/)]

- **Key Differences:** System prompts are isolated as a standalone top-level parameter (`"system": "..."`) rather than an item inside the `"messages"` array.
- **Blocks:** Instead of raw text strings, content is often bundled as arrays of structured text or image "blocks". It natively includes special parameters like `thinking` blocks for extended reasoning budgets and native metadata for prompt caching. [[1](https://docs.litellm.ai/docs/anthropic_unified/messages_to_responses_mapping), [2](https://portkey.ai/blog/open-ai-responses-api-vs-chat-completions-vs-anthropic-anthropic-messages-api/)]

2. OpenAI Responses API (`/responses`)

OpenAI's stateful, agent-oriented framework meant to succeed basic Chat Completions. [[1](https://portkey.ai/blog/open-ai-responses-api-vs-chat-completions-vs-anthropic-anthropic-messages-api/), [2](https://www.youtube.com/watch?v=0pGxoubWI6s&vl=en&t=1043)]

- **Key Differences:** Instead of passing the whole conversation history back and forth statelessly, it manages interaction state natively. It handles tools like web search or file analysis internally, tracks reasoning metrics via an explicit `reasoning_effort` parameter, and renames token limits to `max_output_tokens`. [[1](https://docs.litellm.ai/docs/anthropic_unified/messages_to_responses_mapping), [2](https://medium.com/@jayeshpadhiar20/getting-started-with-openai-apis-a-complete-developers-guide-69d91d88855c), [3](https://portkey.ai/blog/open-ai-responses-api-vs-chat-completions-vs-anthropic-anthropic-messages-api/), [4](https://www.youtube.com/watch?v=0pGxoubWI6s&vl=en&t=1043)]

3. OpenAI Harmony Response Format [[1](https://developers.openai.com/cookbook/articles/openai-harmony)]

An internal tokenizer-level formatting layout (`<|start|>`, `<|message|>`, `<|end|>`). It explicitly splits chain-of-thought routing, tool routing (`<|call|>`), and text generation loops directly within the context window before it reaches your developer-facing JSON. [[1](https://developers.openai.com/cookbook/articles/openai-harmony)]
