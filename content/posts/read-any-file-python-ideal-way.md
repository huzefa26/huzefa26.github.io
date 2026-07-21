+++
title = "The ideal, platform-independent way to read all files in Python"
date = 2026-07-19
summary = "A single way to read files throughout platforms and file types."
tags = ["python", "dev"]
categories = []
+++

## Background

Reading non-text files like images or PDFs by just opening the file in "r" mode doesn't work.
Even for text files, the "r" mode is inconsistent, because on Windows it reads new-line characters `\n` as `\r\n` vs reading it directly as `\n` on Linux and MacOS.

## Solution

So, to reliably read files (text and non-text), we need to read the file's bytes and then use `base64` standard library of Python to decode those bytes.

```python
import base64

with open("document.pdf", "rb") as fh:
    base64.standard_b64encode(fh.read()).decode("utf-8")
```
