+++
title = "AI Fluency: Framework & Fundamentals"
date = 2026-07-01
summary = "My notes from the Anthropic Academy course of the same name."
tags = ["ai", "learning"]
categories = []
+++

# AI Fluency

Fluent AI use is ...

Maximing the output we get from interactions with AI, without wasting time and energy, in an honest and responsible way that protects the privacy and security of yourself and others.

How are we interacting with AI today?
- Automation: AI does the work for you, based on your explicit instructions.
- Augmentation: You and the AI work collaboratively.
- Agency: AI works independently on your behalf.


## AI Fluency Framework

1. Delegation

When should humans do the work, and when should AI?

2. Description

How do we communicate clearly with AI systems?

3. Discernment

How do we evaluate what AI gives us?

4. Dilligence

How do we ensure our interaction with AI is responsible, transparent and accountable?

## Fundamentals of Generative AI

AI that can create new content, rather than analyzing existing data.

For e.g.

Traditional AI (ML) detects an email is spam or not based on learning patterns from existing dataset of spam and non-spam emails.

Generative AI can draft emails from scratch.

This represents a fundamental shift in AI capabilities.
Large Language Models (LLMs) are a prominent type of Gen AI models.
They are called language models because they are trained to generate and predict human language.

3 pillars made Gen AI possible:
- **Algorithms**: Neural Networks have been developed for decades (1957). The development of Transformers in 2017 was a game changer - it could handle long context.
- **Data**: Explosion of digital data provided the raw material for training. Websites, Code repositories, images, videos, etc.
- **Computation**: Massive increase in computational capacity with the inventions of GPUs and TPUs, and distributed computing concepts like clustering enabled processing that wasn't possible earlier.

These 3 concepts combined enabled the discovery of the **Scaling Laws**: As models grew larger and trained on more data, their performance improved in predictable ways. Additionally, unexpected abilities showed up that were never programmed - reasoning through problems step by step, adapting to new tasks without instructions.

**Under the hood**

During intial training (pre-training), models analyze patterns across billions of text examples - reading every website, piece of text found on the internet - and understand the statistical relationship between words, phrases and concepts.

With pre-training, the model builds a map of its understanding of language and knowledge.

Pre-training is basically showing the model a sequence of text and asking it what comes next. (For e.g. giving it "The capital of France is " and with many iterations of training, it learns that the next word should be "Paris").

After pre-training, models undergo finetuning.

They learn to follow instructions, provide helpful responses and avoid generating harmful content. This often involves human feedback as well as reinforcement learning (using reward and punishment as per objectives).

Then, the model is deployed for use by users like you. You prompt the model, and the model tries to infer the next word and then the next word and then the next word till it thinks it has solved its purpose. 

There's a practical limit to how much a LLM can consider (remember) - **context window**.
It consists of your prompts, the AI's own responses and any other info you've shared.
Companies and research institutes are actively pushing the context window to grow more and more in new models.

**Capabilities and Limitations**

Bounded by training data. They have a **knowledge cutoff data** - based on when they were trained, there is a point beyond which they have no knowledge of the world.

Models need access to tools like Web Search to search for information that it doesn't have.

(For e.g. a model whose knowledge cutoff date was Nov 2024, would need to search the web for someone asking in 2025 "what are today's news?")

The training process doesn't always consider the truthfulness of the training data. The data could have some **incorrect information** and the model could learn and repeat on that incorrect information.

This often leads to a **Hallucination** - an LLM could predict something it thinks is statistically right, but in reality it is not.

Every LLM has a limit to how much information it can consider actively (the **context window**). The AI will not be able to remember things that fall beyond the context window length. This is mostly on a first in, first out basis.

Unlike traditional software that performs predictable behavior, LLMs are by-default unpredictable. Their output is **non-deterministic**. Ask it the same question, and it will answer differently each time.

Due to this nature, LLMs are helpful for creative tasks. Although, sometimes this variability needs to be low and consistent information is seeked. Some LLMs offer the user the ability to tune this variability - the ability to make the responses more creative or more consistent - by the parameter called **temperature**.

Earlier LLM models had reasoning limitations in areas of complex reasoning tasks like mathematical or logical problems requiring multiple steps. Advancements are made in this area with the introduction of **extended thinking** models specializing in step-by-step thinking capabilities.

Lastly, the abilities of these models however smart they get is limited to the extent of their resources. If they don't have access to specific information, they won't be able to help with it. Techniques like RAG help connect LLMs with external tools and databases.

---

Next, to be continued...

## AI Fluency Framework

### 1. Delegation

> When should humans do the work, and when should AI?

Understand your goal and the problem that you are trying to solve.
Know what AI systems can and can't do well.
Decide how to divide the work between you and the AI.

### 2. Description

> How do we communicate clearly with AI systems?

Describe what you are trying to achieve, and the format of the output.

How you want the AI to approach the task, with context and information the AI might need to best work with you on the task.

The tone and style of interaction.

### 3. Discernment

> How do we evaluate what AI gives us?

Does the AI's reasoning make sense?

Are the facts accurate?

Does the output actually help you move forward?

Most of our interactions are looping - giving more detailed instructions and finetuning the response.

### 4. Dilligence

> How do we ensure our interaction with AI is responsible, transparent and accountable?

Verifying the accuracy of the information presented to you. Ensuring fairness and controlling potential biases.

Are you willing to be accountable of your AI-assisted work and do you take ownership of the results?
