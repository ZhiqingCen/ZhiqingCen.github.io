---
title: Prompt Engineering Notes
date: 2024-10-24 15:00:00 +1100
categories: [AI, Prompt Engineering]
tags:
  [
    prompt engineering,
    LLM,
    chain of thought,
    instruction tuning,
    anthropic,
    claude,
  ]
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

## Clear and Specific Instructions

1. give contextual information
   - what the task results will be used for
   - what audience the output is meant for
   - what workflow the task is a part of, and where this task belongs in that workflow
   - the end goal of the task, or what a successful task completion looks like
2. be specific
3. provide instructions as sequential steps
   - use numbering or bullet points
4. ask for structured output
   - request output in formats such as JSON or HTML to ensure clarity and structure in the response
5. check for conditions
   - prompt the model to verify that it has all the necessary context or assumptions to carry out the task successfully
6. use delimiters
   - triple quotes `"""`
   - triple backticks ` "```" `
   - triple dashes `---`
   - angle bracket `<>`
   - XML tags `<tag></tag>`

## Few-shot / Multishot Prompting

- provide a few examples of successful task completions before asking the model to perform the task itself
- this approach improves accuracy and consistency, well-chosen examples boost ability to handle complex tasks
- make sure examples are:
  - **relevant**: mirror your actual use case
  - **diverse**: cover edge cases and potential challenges, and vary enough that model doesn’t inadvertently pick up on unintended patterns
  - **clear**: wrapped in <example> tags (if multiple, nested within <examples> tags) for structure

## System Prompt

- give the model a role
- advantages
  - enhanced accuray
  - tailored tone
  - improve focus
    - model stays more within the bounds of your task’s specific requirements
- system prompt to set role, put everything else, like task-specific instructions, in the user turn instead (pre-defined user messages)

## Prefill Response

- it allows you to direct model’s actions, skip preambles, enforce specific formats like JSON or XML, and even help model to maintain character consistency in role-play scenarios
- to prefill, include the desired initial text in the Assistant message

```python
# example
import anthropic

client = anthropic.Anthropic()
response = client.messages.create(
    model="claude-3-5-sonnet-20240620",
    max_tokens=1024,
    messages=[
        {"role": "user", "content": "What is your favorite color?"},
        {"role": "assistant", "content": "As an AI assistant, I don't have a favorite color, But if I had to pick, it would be green because"}  # Prefill here
    ]
)
```

## Chain of Thought Prompting

1. ask the model to think step-by-step before reaching conclusions, particularly for complex tasks that involve reasoning or multi-step analysis
2. guide the process
   - use structured tags such as `<thinking>` and `<answer>` to differentiate between the model’s reasoning and its final answer

## Prompt Chaining / Iterative Prompt Development

- for task that has multiple distinct steps which each require in-depth thought
- it break down complex tasks into smaller, manageable subtasks
- advantages
  - accuracy
  - clarity
  - traceability
    - easily pinpoint and fix issues in your prompt chain
- steps
  - identify subtasks: break task into distinct, sequential steps
  - structure with XML for clear handoffs: use XML tags to pass outputs between prompts
  - have a single-task goal: each subtask should have a single, clear objective
  - iterate: refine subtasks based on Claude’s performance

## Model Limitaions

- hallucination
  - produces statements that sound plausible but not are true
  - reducing hallucinations
    - instruct the model to quote or extract relevant information directly before answering based on it

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
