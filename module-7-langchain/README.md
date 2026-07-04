# Module 7 - LangChain

This module teaches how LangChain can be used to compose prompts, models, retrievers, memory, tools, callbacks, and streaming into LLM applications.

[Back to roadmap](../README.md)

## Why it exists?

LangChain exists because production LLM apps often need more than one model call. They need reusable chains, retrievers, output parsers, memory, observability hooks, and tool integrations.

## What it is?

LangChain is an application framework for building LLM workflows. It provides abstractions for models, prompts, retrievers, tools, output parsers, callbacks, agents, and streaming.

## How it works internally?

LangChain composes components into pipelines. A request can flow through a prompt template, model call, output parser, retriever, memory layer, or tool call depending on the application.

```text
Input -> prompt template -> model -> parser -> response
Input -> retriever -> prompt -> model -> response
```

## When to use it?

Use LangChain when you need reusable LLM workflows, integration with retrievers and tools, prompt templates, output parsing, streaming, or rapid prototyping.

Avoid it when a direct SDK call is simpler, when you need total control over every abstraction, or when framework complexity outweighs value.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | Client / UI          |
                         | chat / job request   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Application API      |
                         | auth / tenant / task |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | LangChain Runnable   |
                         | LCEL chain / graph   |
                         +----------+-----------+
                                    |
               +--------------------+--------------------+
               |                    |                    |
               v                    v                    v
      +----------------+   +----------------+   +----------------+
      | Prompt Template|   | Retriever      |   | Memory Store   |
      | vars/examples  |   | vector/search  |   | chat/session   |
      +--------+-------+   +--------+-------+   +--------+-------+
               |                    |                    |
               +--------------------+--------------------+
                                    |
                                    v
                         +----------------------+
                         | Tool Layer           |
                         | APIs / DB / search   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Model Provider       |
                         | stream / tools       |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Output Parser        |
                         | JSON / Pydantic      |
                         +----------+-----------+
                                    |
               +--------------------+--------------------+
               |                                         |
               v                                         v
      +----------------+                         +----------------+
      | Callback Stack |                         | App Response   |
      | traces/costs   |                         | stream/final   |
      +----------------+                         +----------------+
```

User flow diagram:

```text
1. User sends a request
   |
   v
2. Backend invokes a chain
   |
   v
3. Chain gathers memory or retrieval context
   |
   v
4. Prompt template formats the model input
   |
   v
5. Model generates output
   |
   v
6. Parser and callbacks process the response
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Chains | Reusable workflows | Abstractions can hide details |
| Integrations | Faster development | Dependency surface grows |
| LCEL | Composable pipelines | Learning curve |
| Callbacks | Better observability | More setup |
| Agents | Flexible tool use | Harder to test |

## Module Structure

## What topics we learn in this module?

- Chains
- LCEL
- Prompt templates
- Retrievers
- Memory
- Output parsers
- Callbacks
- Tools
- Agents
- Streaming

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
module-7-langchain/
  01-chains/
  02-lcel/
  03-prompt-templates/
  04-retrievers/
  05-memory/
  06-output-parsers/
  07-callbacks/
  08-tools/
  09-agents/
  10-streaming/
```

## Hands-On Project

Build an AI research assistant or customer support chatbot.

Core features:

- Prompt templates.
- Retriever integration.
- Streaming output.
- Output parsing.
- Tool usage.
- Callback logging.

## Interview Questions And Answers

### 1. What is LangChain?

LangChain is a framework for composing LLM application components such as prompts, models, retrievers, tools, and parsers.

### 2. What is LCEL?

LCEL is LangChain Expression Language, a way to compose runnable chains.

### 3. When is LangChain useful?

It is useful when an app needs reusable workflows and integrations beyond a single model call.

### 4. What is an output parser?

An output parser converts model output into a structured form the app can use.

### 5. What is the risk of using agents?

Agents can be flexible but harder to test, constrain, and reason about.

## Production Scenarios

### Scenario 1 - Research assistant

The app retrieves sources, summarizes them, and streams a final answer.

Reasoning: LangChain provides reusable building blocks for retrieval, prompting, parsing, and streaming.

### Scenario 2 - Customer support chatbot

The bot uses memory, retrieval, and tools.

Reasoning: chains make multi-step chatbot workflows easier to compose and observe.

### Scenario 3 - Internal automation

An internal tool runs model calls with output parsing and callbacks.

Reasoning: framework-level callbacks help track latency, cost, and failures.
