+++
title = "Speculative Decoding explained"
date = 2026-07-05
summary = "Understanding an inference technique that makes LLM systems faster without making them dumber."
tags = ["ai"]
categories = []
+++

## The gist, for anyone

Large language models write text one word (technically, one "token") at a time. Each word requires a full pass through a massive neural network - billions of parameters, doing their thing, just to decide the *next* word. Then it does the whole thing again for the word after that. This is why chatbots sometimes feel like they're typing at a fixed, almost deliberate pace: the model really is starting from scratch for every single token.

Speculative decoding is a clever trick to speed this up without changing the model's output at all - same quality, same words, just faster. The idea: instead of always asking the big, slow, expensive model to guess the next word, let a small, fast, cheap model guess several words ahead first. Then ask the big model to check those guesses all at once. Checking is much cheaper than generating, because the big model can verify a whole draft in a single pass instead of many sequential ones. Whatever the small model got right, you keep. Whatever it got wrong, you throw away and let the big model correct it. Net result: the same final answer, produced faster.

It's a bit like a busy executive who has an assistant draft an email. The assistant (small model) writes quickly and is right most of the time. The executive (big model) doesn't rewrite it from scratch - they just skim it and fix the parts that are wrong. Much faster than dictating every word themselves, and the final email is exactly as good as if the executive had written it alone.

## The technical detail

### Why generation is slow in the first place

Autoregressive decoding is inherently sequential: token *t* depends on tokens 1 through *t-1*, so you cannot compute token 5 before token 4 exists. Each step requires a full forward pass through the model. For a large transformer, this is memory-bandwidth-bound rather than compute-bound - the bottleneck is streaming the model's weights (and the growing KV cache) from GPU memory, not the arithmetic itself. Generating one token at a time means you pay that memory-bandwidth cost once per token, and GPUs are wildly underutilized on compute during this process, since there's so little work to overlap with the data movement.

### The core mechanism

Speculative decoding (Leviathan et al., 2022; Chen et al., 2023, both independently proposing essentially the same idea) exploits the fact that verifying a sequence of tokens in parallel is cheap relative to generating them one at a time, because verification can be done in a single forward pass over multiple positions.

The algorithm, per step:

1. **Draft**: A smaller, faster "draft" model generates a candidate continuation of *k* tokens, autoregressively, one at a time (this part is still sequential, but the draft model is small enough that it's fast).
2. **Verify**: The large "target" model runs a single forward pass over the draft sequence (plus the original context), producing a probability distribution over the vocabulary at *each* of those *k* positions simultaneously - something a transformer can do in one parallel pass, unlike generation.
3. **Accept/reject**: Starting from the first drafted token, compare the target model's distribution to the draft model's distribution at each position using a rejection-sampling scheme. If the target model would have assigned at least as much probability to the drafted token as the draft model did, accept it outright. If not, accept it only with probability *p_target(x) / p_draft(x)*, and if rejected, sample a replacement token from an adjusted distribution (the residual `max(0, p_target - p_draft)`, renormalized).
4. **Stop at first rejection**: Once a token is rejected, all subsequent drafted tokens are discarded (since the context they were drafted on is now invalid), the corrected token is appended and the process repeats from there.

The elegant part is the acceptance criterion is constructed so that the *marginal distribution of the accepted token is exactly the target model's distribution* - this is provably lossless. You get identical output statistics to running the target model alone, token for token, in expectation. No quality is sacrificed for the speed gain, which distinguishes this from approximate methods like distillation or quantization.

### Where the speedup comes from

If the draft model's guesses are correct with probability *α* on average, the expected number of tokens accepted per verification round is roughly `(1 - α^(k+1)) / (1 - α)`. The speedup depends on:
- **α (draft accuracy)**: higher agreement between draft and target models means more tokens accepted per round.
- **k (draft length)**: longer drafts amortize the target model's fixed forward-pass cost over more tokens, but returns diminish since a single rejection wastes the remaining draft.
- **Relative cost of draft vs. target**: the draft model needs to be cheap enough that its sequential generation cost doesn't eat the savings.

In practice, this yields roughly 2-3x wall-clock speedups on typical text, with variance depending on how predictable the text is (code and repetitive text see bigger gains than creative, high-entropy prose).

### Variants worth knowing

- **Self-speculative decoding**: instead of a separate small model, use an early-exit or layer-skipped version of the *same* model as the draft, avoiding the need to maintain two separate models.
- **Medusa / lookahead decoding**: attach extra prediction heads to the target model itself that guess multiple future tokens directly, removing the separate draft model entirely.
- **Tree-based speculation** (e.g., SpecInfer, Sequoia): instead of drafting one linear sequence, draft a tree of possible continuations and verify multiple branches in parallel, increasing the odds that *some* path through the tree is accepted.

### The key insight, restated

The bottleneck in LLM inference isn't the model's ability to think fast - it's the sequential dependency forcing one memory-bound pass per token. Speculative decoding sidesteps this by decoupling *guessing* (sequential, cheap) from *checking* (parallel, and provably equivalent to the original model's own behavior). It's one of the more elegant systems-level optimizations in modern inference, because it buys real latency reduction with a mathematical guarantee of zero quality loss - a rare combination.