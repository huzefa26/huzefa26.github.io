+++
title = "Prompt Evaluation - Steps"
date = 2026-07-02
summary = "Notes from the Anthropic Academy course Building with Claude API."
tags = ["dev", "ai"]
categories = []
+++


### 1) Initial Prompt Draft

### 2) Eval Dataset

Curate a dataset of questions to test the prompt's reachability

### 3) Feed through Claude

Question -> Answer

### 4) Feed through Grader

(Question, Answer) -> Grade
Avg(Grades) signifies Prompt quality

### 5) Change Prompt and Repeat

**Graders**

3 common ways to grading:

> **Code** - Evaluate programmatically. Define criteria into codable checks.
- Useful for:
    - Checking output lengths
    - Verifying output does(n't) have certain words
    - Syntax validation
    - Readability scores

> **Model** (LLM) - Ask a model to assign a score to the output, or compare two versions.
- Useful for:
    - Response quality
    - Quality of instruction following
    - Completeness
    - Helpfulness
    - Safety

> **Human** - Ask a human to assign a score to the output, or compare two versions.
- Useful for:
    - General response quality
    - Comprehensiveness
    - Depth
    - Conciseness
    - Relevance
