---
title: Prompt Engineering Notes
date: 2024-10-13 15:00:00 +1000
categories: [AI, Prompt Engineering]
tags: [prompt engineering, LLM, chain of thought, instruction tuning, anthropic, claude]
description: "Notes on prompt engineering techniques for LLMs, including instruction tuning, chain of thought prompting, model limitations, and best practices for interacting with large language models."
---

## Basic

two types of large language models

1. Base LLM

   - predicts next word, based on text training data

   ```python
   # Once upon a time, there was a unicorn
   that lived in a magical forest with all her unicorn friends

   # What is the capital of France?
   What is France's largest city?
   What is France's population?
   What is the currency of France?
   ```

2. Instruction Tuned LLM

   - tries to follow instructions
   - fine-tune on instructions and good attempts at following those instruction
   - Reinforcement Learning with Human Feedback (RLHF)
   - trained to be helpful, honest, harmless

   ```python
   # What is the capital of France?
   The capital of France is Paris.
   ```

## Guidelines

**Principle 1: write clear and specific instructions**

1. use delimiters
   - triple quotes `"""`
   - triple backticks ` "```" `
   - triple dashes `---`
   - angle bracket `<>`
   - XML tags `<tag></tag>`
2. ask for structured output
   - request output in formats such as JSON or HTML to ensure clarity and structure in the response
3. check for conditions
   - prompt the model to verify that it has all the necessary context or assumptions to carry out the task successfully
4. few-shot prompting
   - provide a few examples of successful task completions before asking the model to perform the task itself
   - this approach improves accuracy and consistency

**Principle 2: give the model time to think**

1. Chain of Thought (CoT) prompting
   - ask the model to think step-by-step before reaching conclusions, particularly for complex tasks that involve reasoning or multi-step analysis
2. guide the process
   - use structured tags such as `<thinking>` and `<answer>` to differentiate between the model’s reasoning and its final answer

## Model Limitaions

- hallucination
  - produces statements that sound plausible but not are true
  - reducing hallucinations
    - instruct the model to quote or extract relevant information directly before answering based on it

## Iterative Prompt Development

- idea -> implementation (code/data) prompt -> experimental result -> error analysis -> idea
- prompt guidelines
  - be clear and speific
  - analyse why result does not give desired output
  - refine the idea and the prompt
  - repeat

## Tips

- use multishot prompting
  - providing multiple diverse examples enhances the model’s ability to produce accurate outputs, especially for tasks that require structured responses or adherence to a specific format
- XML tag structuring
  - using tags like `<document>`, `<source>`, or `<example>` can significantly reduce errors in responses and help in managing complex inputs such as multi-document analysis
- dealing with facts, use "extract" rather than "summarise"
- temperature
  - `0` - low temperature
    - more predictive
    - suitable for tasks that require reliability
  - `1` - high temperature
    - more creative
    - suitable for tassks that require variety
- role
  - system - sets behaviour of assistant
  - assistant - chat model
  - user

```python
mesages = [
   {
      "role": "system",
      "content": "You are an assistant...",
   },
   {
      "role": "user",
      "content": "tell me a joke",
   },
   {
      "role": "assistant",
      "content": "Why did the fish...",
   }
]
```

## Resources & References

- [chatgpt prompt engineering for developers](https://www.deeplearning.ai/short-courses/chatgpt-prompt-engineering-for-developers/)
- [anthrop\c prompt engineering](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [blog from Lilian Weng](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/)
