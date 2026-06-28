+++
title = "A Mental Model for Git Rebase"
date = 2026-06-28
summary = "See rebase as replaying commits."
tags = ["dev", "ideas"]
categories = []
+++

## The mental model

Commits as patches; rebase replays them onto another commit.

## When I use it

- Tidying a feature branch before merging
- Never on shared branches, as it changes the commit hash