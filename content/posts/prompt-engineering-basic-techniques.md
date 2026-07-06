+++
title = "Prompt Engineering : Basic Techniques"
date = 2026-07-06
summary = "Notes from the Prompt Engineering section of the course Building with Claude API."
tags = ["ai", "dev"]
categories = []
+++

A good prompt gets your task done precisely how you want it done.

A general flow to iteratively test prompts is to:

(1) Set a goal
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;v<br>
(2) Write an initial prompt
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;v<br>
(3) Eval the prompt
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;v<br>
(4) Apply a prompt engineering technique
<br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;v&nbsp;&nbsp;&nbsp;^<br>
(5) Re-eval to verify better performance


---

**(1) Be clear and specifc**

Clear:
* Use simple language
* State what you want explicitly
* Lead your prompt with a simple statement of the model's task

Direct:
* Use instructions, not questions
* Use **direct action verbs** (write, generate, create)

Being clear in the first line of the prompt!

---

**Be specific**

Provide a list of guidelines or steps to direct the model.

For e.g.

```python
prompt = """
Write a short story about a character who discovers a hidden talent.
"""
```

With this prompt, the model can go into an infinite number of directions - random story length, random extra characters, random mood and theme of the story.

For e.g. ... being specific

```python
prompt = """
Write a short story about a character who discovers a hidden talent.

Guidelines:
1. Keep the story under 1000 words.
2. Include a clear action that reveals the character's hidden talent.
3. Include at least one supporting character.
"""
```

2 common types of guidelines used in prompts:
- List qualities that the output should have. (like the prompt above)  -> Output-focused
- Provide steps the model should follow. (like the prompt below)       -> Process-focused

```python
prompt = """
Write a short story about a character who discovers a hidden talent.

Follow these steps:
1. Brainstorm 3 talents that would create dramatic tension.
2. Pick the most interesting talent.
3. Outline a pivotal scene that reveals the talent.
4. Brainstorm 3 supporting character types that could increase the impact of this discovery.
"""
```

In most general problems, the output-focused guidelines are more impactful.

The process-focused listing of steps is more useful for complex problems
where you want the model to consider a wider view or some extra topics beyond what it would naturally consider.
Some cases are: troubleshooting hard problems, decision making, critical thinking, forcing a "wider" view.

---

**Structure with XML tags**

Use XML tags to separate distinct portions of the prompt.

It is most useful when including a lot of context. For e.g. multiple pages of data, long list of guidelines, etc.

It serves as a delimiter.

Example usage: Debugging a piece of code w.r.t. its expected usage as mentioned in the documentation. `<my_code>...</my_code>` & `<docs>...</docs>`

---

**Providing Examples**

Give the model sample input/output pairs.
- Useful for capturing corner cases or complex output formats.
- "One-Shot": provide a single example
- "Multi-Shot": provide multiple examples
- Highly recommend combining with XML tags for structure.

For e.g. Asking a model to do Sentiment Analysis:
```text
Categorize the sentiment of the below tweet:
<input_tweet>
... tweet text...
</input_tweet>

If the tweet has a positive sentiment, respond with "Positive". If it is negative, respond with "Negative".

Here is a sample input with an ideal response:
<sample_input>
Great game tonight!
</sample_input>
<ideal_output>
Positive
</ideal_output>

Be especially careful with tweets that contain sarcasm.
For example:
<sample_input>
Oh yeah, I really needed a flight delay tonight! Excellent!
</sample_input>
<ideal_output>
Negative
</ideal_output>
```
