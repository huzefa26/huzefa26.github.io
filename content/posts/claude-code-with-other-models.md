+++
title = "Using other model providers in Claude Code"
date = 2026-06-28
summary = "Change base URL and API key. Use a proxy to translate formats."
tags = ["dev", "ai"]
categories = []
+++

### How to use OpenCode Go subscription from Claude Code?

To run Claude Code using alternative models (such as MiniMax or Qwen models available through OpenCode's API service), you need to set up a local proxy like **LiteLLM**. Claude Code natively expects an Anthropic-compatible API. [Medium Article](https://medium.com/@shawrup.k.suter/use-claude-code-with-open-models-for-10-month-opencode-go-litellm-proxy-ce3588bae3ea)

The workflow relies on this architecture:  
`Claude Code` → `LiteLLM Proxy (localhost)` → `OpenCode Go API` → `Target Models`. [Medium Article](https://medium.com/@shawrup.k.suter/use-claude-code-with-open-models-for-10-month-opencode-go-litellm-proxy-ce3588bae3ea)

1. **Get your API Key**: Sign up for an [OpenCode Go](https://opencode.ai/) subscription and generate an API key.
2. **Install a Proxy**: Install [LiteLLM](https://docs.litellm.ai/), which allows you to translate API calls into formats understood by both Claude Code and OpenCode.
3. **Configure the Environment**: Set your `ANTHROPIC_BASE_URL` to your local LiteLLM proxy and pass your OpenCode API key to `ANTHROPIC_API_KEY` in your environment.
4. **Run Claude Code**: Launch your terminal session and direct Claude Code to your local proxy. [Insta Post](https://www.instagram.com/p/DXZcCekRIt6/), [Medium Article](https://medium.com/@shawrup.k.suter/use-claude-code-with-open-models-for-10-month-opencode-go-litellm-proxy-ce3588bae3ea), [3](https://kkovacs.eu/opencode-go-with-claude-code/), [4](https://www.xda-developers.com/i-use-opencode-over-claude-code-and-its-every-bit-as-good/)
