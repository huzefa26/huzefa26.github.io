+++
title = "Does the output suffer when using other model providers with Claude Code harness?"
date = 2026-06-28
summary = "I stopped using my OpenCode Go models with the Claude Code harness."
tags = ["dev", "ai"]
categories = []
+++

### Does the output suffer when using OpenCode Go models from Claude Code harness (through a proxy like LiteLLM) vs using them from OpenCode itself?

Yes, using OpenCode Go models (e.g., DeepSeek, GLM, Kimi) via a proxy in Claude Code will likely **reduce answer quality and reliability** compared to using them natively in OpenCode. [[1](https://thomas-wiegold.com/blog/opencode-go-review/), [2](https://www.reddit.com/r/opencodeCLI/comments/1s14a6t/is_opencode_go_plan_enough_for_a_developer_who/), [3](https://ai.rundatarun.io/ai-development-agents/codex-vs-claude-code-vs-opencode), [4](https://agentpedia.codes/blog/free-claude-code-guide), [5](https://techsy.io/en/blog/claude-code-use-different-models)]

While the raw model latency is comparable, the "agentic" performance degrades due to a mismatch between the model and the harness. [[1](https://inetanel.com/articles/claude-code-vs-opencode-cto-decision)]

1. Quality & Reliability (Significant Degradation) [[1](https://inetanel.com/articles/claude-code-vs-opencode-cto-decision)]

- **Harness-Model Mismatch:** Claude Code is "vertically integrated" with Anthropic models. Its system prompts, tool definitions, and context compression logic are hyper-optimized for how Claude "thinks" and responds. [[1](https://dev.to/composiodev/claude-code-vs-opencode-without-the-hype-j1f)]
- **Tool Calling Failures:** When you force a different model (like GLM or DeepSeek) into the Claude Code harness, you often experience degraded tool-call reliability and "unexpected compaction failures." The model may struggle to interpret Claude-specific instructions, leading to errors that look like model hallucinations but are actually compatibility issues. [[1](https://inetanel.com/articles/claude-code-vs-opencode-cto-decision)]
- **Missing Features:** OpenCode is built to be provider-agnostic; it normalizes differences (e.g., tool call IDs, varying context limits) across models. Claude Code assumes it is talking to Claude; if the proxied model doesn't support specific "bells and whistles" (like specific caching or reasoning formats), the session can become unstable. [[1](https://www.reddit.com/r/ZaiGLM/comments/1qc4lqo/in_my_personal_experience_opencode_is_a_much/), [2](https://dev.to/composiodev/claude-code-vs-opencode-without-the-hype-j1f), [3](https://cefboud.com/posts/coding-agents-internals-opencode-deepdive/), [4](https://www.builder.io/blog/opencode-vs-claude-code), [5](https://thomas-wiegold.com/blog/opencode-go-review/)]

2. Performance & Speed (Mixed)

- **Agent Efficiency:** While a proxy adds negligible network latency (microseconds), the _workflow_ efficiency drops. Users report that non-Claude models inside the Claude harness handle the context window differently, leading to a different "cadence" and often requiring more manual correction. [[1](https://ai.rundatarun.io/ai-development-agents/codex-vs-claude-code-vs-opencode), [2](https://skywork.ai/skypage/en/claude-code-cli-proxy/2044690246202511360), [3](https://thomas-wiegold.com/blog/opencode-go-review/)]
- **Context Management:** Claude Code aggressively optimizes context (e.g., compacting history) in a way that non-Claude models may not handle well, potentially causing "context rot" or loss of critical information during long sessions. [[1](https://inetanel.com/articles/claude-code-vs-opencode-cto-decision), [2](https://www.reddit.com/r/ZaiGLM/comments/1qc4lqo/in_my_personal_experience_opencode_is_a_much/)]
