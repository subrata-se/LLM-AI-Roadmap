# Module 02 - Prompt Engineering

This module teaches how to design prompts as reliable interfaces between users, applications, tools, and LLMs. The goal is to move from casual prompting to production-grade prompt design.

[Back to roadmap](../README.md)

## Why it exists?

Prompt engineering exists because LLM behavior is strongly shaped by instructions, examples, constraints, and context. A weak prompt can produce vague, unsafe, or hard-to-parse output. A strong prompt makes the model's job explicit and easier to evaluate.

## What it is?

Prompt engineering is the practice of designing system prompts, user prompts, examples, templates, guardrails, and output instructions so an LLM behaves consistently for a specific task.

Core ideas:

- `System prompt`: High-priority behavior and role instructions.
- `User prompt`: The user's task or question.
- `Few-shot examples`: Examples that teach the expected pattern.
- `Prompt template`: A reusable prompt with variables.
- `Guardrails`: Constraints that reduce unsafe or off-task behavior.
- `Output contract`: A required response format, such as JSON.

## How it works internally?

The model does not execute prompts like code. It conditions its next-token predictions on the instructions and examples in the context. Clearer prompts shift the probability distribution toward desired responses.

```text
System instructions
  + user input
  + examples
  + constraints
  + output format
  -> model response
```

## When to use it?

Use prompt engineering when you need consistent text generation, classification, extraction, rewriting, summarization, tool use, or structured output without training a custom model.

Avoid relying only on prompts when the task needs guaranteed correctness, secure authorization, exact computation, or access to live private data. Use tools, validation, retrieval, and backend logic for those parts.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         |        Client        |
                         |  UI / API Consumer   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Application Backend  |
                         | Auth / tenant / task |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Prompt Registry   |                 | Runtime Context   |
        | versions / owners |                 | user data / docs  |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  +------------------+------------------+
                                     |
                                     v
                         +----------------------+
                         | Prompt Builder       |
                         | template + variables |
                         | examples + policies  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Prompt Guardrails    |
                         | injection checks     |
                         | unsafe input checks  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Model Router         |
                         | quality / cost / SLA |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | LLM Provider         |
                         | generation / tools   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Output Contract      |
                         | parser / JSON schema |
                         | validation / retry   |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Observability     |                 | App Response      |
        | prompt id / cost  |                 | text / JSON/tool  |
        +-------------------+                 +-------------------+
```

User flow diagram:

```text
1. User submits a task
   |
   v
2. Backend selects the right prompt template
   |
   v
3. Backend injects variables, examples, and constraints
   |
   v
4. Model generates a response
   |
   v
5. Backend validates format, safety, and task completion
   |
   v
6. User receives the final answer or a retry is triggered
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Speed | Fast to build and iterate | Can become brittle |
| Control | Guides model behavior | Not a hard guarantee |
| Examples | Improve consistency | Consume context tokens |
| Templates | Reusable across requests | Need versioning and tests |
| Guardrails | Reduce risk | May reject valid responses |

## Module Structure

## What topics we learn in this module?

- Zero-shot prompting
- One-shot prompting
- Few-shot prompting
- System prompts
- Prompt templates
- Guardrails
- Prompt injection
- JSON outputs
- Tool calling

Each topic folder we create later should follow this structure:

| # | Section | Purpose |
|---|---------|---------|
| 1 | Why it exists? | What problem this topic solves |
| 2 | What it is? | Clear definition and vocabulary |
| 3 | How it works internally? | Data flow and mental model |
| 4 | When to use it? | Practical use cases and anti-patterns |
| 5 | Architecture | How it appears in real systems |
| 6 | Trade-offs | Benefits, limits, and alternatives |
| 7 | Hands-on practice | Small implementation or experiment |
| 8 | Interview questions | Questions and expected answer direction |
| 9 | Production scenarios | Real systems where the topic matters |

## Topic Folder Plan

We will add topic folders later:

```text
module-02-prompt-engineering/
  01-zero-shot-prompting/
  02-one-shot-prompting/
  03-few-shot-prompting/
  04-system-prompts/
  05-prompt-templates/
  06-guardrails/
  07-prompt-injection/
  08-json-outputs/
  09-tool-calling/
```

## Hands-On Project

Build one or more prompt-driven tools:

- Resume analyzer
- Email generator
- SQL query generator
- Code review assistant

Core features:

- Prompt templates with variables.
- Versioned prompts.
- Structured outputs.
- Basic prompt-injection checks.
- Evaluation examples for expected behavior.

## Interview Questions And Answers

### 1. What is prompt engineering?

Prompt engineering is designing instructions, examples, context, and output constraints so an LLM produces useful and reliable responses.

### 2. What is the difference between zero-shot and few-shot prompting?

Zero-shot prompting gives only instructions. Few-shot prompting includes examples that show the desired input-output pattern.

### 3. Why are system prompts important?

System prompts define high-priority behavior, role, safety boundaries, and formatting expectations.

### 4. Why are prompt templates useful?

Templates make prompts reusable, testable, and versionable across many requests.

### 5. How do you reduce prompt injection risk?

Separate trusted instructions from user content, validate tool calls, limit permissions, and treat user-provided text as untrusted input.

## Production Scenarios

### Scenario 1 - Resume analyzer

A hiring tool extracts skills, experience, and risk flags from resumes.

Reasoning: templates and structured outputs make candidate analysis consistent enough to review and compare.

### Scenario 2 - SQL generator

A user asks natural-language questions about a database.

Reasoning: prompts must include schema context, safety rules, and read-only constraints to avoid dangerous queries.

### Scenario 3 - Code review assistant

An assistant comments on pull requests.

Reasoning: the prompt should ask for actionable findings, severity, and file references instead of generic feedback.
