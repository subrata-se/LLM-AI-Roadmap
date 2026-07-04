# Module 9 - Agentic AI

This module teaches how LLM systems can plan, use tools, reflect, manage memory, and execute multi-step tasks with controlled autonomy.

[Back to roadmap](../README.md)

## Why it exists?

Agentic AI exists because some tasks require more than a single response. The system may need to plan, gather information, call tools, evaluate progress, and continue until a goal is reached.

## What it is?

An AI agent is an LLM-driven system that can decide actions, use tools, maintain state, and work toward a goal.

## How it works internally?

The agent receives a goal, breaks it into steps, chooses actions, observes results, updates memory or state, and repeats until done or stopped.

```text
Goal -> plan -> action -> observation -> reflection -> next action
```

## When to use it?

Use agents for research, planning, tool-using assistants, coding helpers, travel planning, and workflows with uncertain steps.

Avoid agents when a deterministic workflow, direct API call, or simple prompt is enough.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | User Goal            |
                         | task + constraints   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Agent Controller     |
                         | run loop / limits    |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Planner           |                 | Memory / State    |
        | steps / strategy  |                 | history / facts   |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  +------------------+------------------+
                                     |
                                     v
                         +----------------------+
                         | Action Selector      |
                         | reason / tool / ask  |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Tool Router       |                 | Safety Guardrails |
        | APIs / search/DB  |                 | budget / approval |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  +------------------+------------------+
                                     |
                                     v
                         +----------------------+
                         | Observation Store    |
                         | tool results/errors  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Reflection / Critic  |
                         | progress / quality   |
                         +----------+-----------+
                                    |
                    +---------------+---------------+
                    |                               |
                    v                               v
          +-------------------+           +-------------------+
          | Next Action       |           | Final Answer      |
          | continue loop     |           | report / artifact |
          +-------------------+           +-------------------+
```

User flow diagram:

```text
1. User gives a goal
   |
   v
2. Agent creates a plan
   |
   v
3. Agent selects a tool or reasoning step
   |
   v
4. Tool returns an observation
   |
   v
5. Agent evaluates progress
   |
   v
6. Agent continues, asks for help, or returns final result
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Autonomy | Handles open-ended tasks | Harder to control |
| Tool use | Accesses real systems | Requires permissions |
| Planning | Breaks down goals | Plans can be wrong |
| Memory | Preserves context | Can store bad assumptions |
| Reflection | Improves quality | Adds latency and cost |

## Module Structure

## What topics we learn in this module?

- ReAct
- Planning
- Reflection
- Tool usage
- Memory
- Goal decomposition
- Autonomous execution

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
module-9-agentic-ai/
  01-react/
  02-planning/
  03-reflection/
  04-tool-usage/
  05-memory/
  06-goal-decomposition/
  07-autonomous-execution/
```

## Hands-On Project

Build an autonomous travel planner or personal research assistant.

Core features:

- Goal intake.
- Plan generation.
- Tool calling.
- Memory/state.
- Reflection step.
- Final report.
- Safety limits and human approval.

## Interview Questions And Answers

### 1. What makes a system agentic?

It can choose actions, use tools, maintain state, and work toward a goal across multiple steps.

### 2. What is ReAct?

ReAct combines reasoning and acting, where the model alternates between thoughts, tool actions, and observations.

### 3. Why do agents need guardrails?

Autonomous systems can take wrong, expensive, or unsafe actions without constraints.

### 4. What is reflection?

Reflection is a review step where the agent critiques progress or output before continuing.

### 5. When should agents ask humans for help?

When confidence is low, risk is high, permissions are needed, or requirements are ambiguous.

## Production Scenarios

### Scenario 1 - Travel planner

The agent searches flights, checks constraints, and proposes an itinerary.

Reasoning: planning and tool use are needed because the task has many dependent steps.

### Scenario 2 - Research assistant

The agent gathers sources, summarizes findings, and identifies gaps.

Reasoning: iterative search and reflection improve coverage.

### Scenario 3 - Coding assistant

The agent inspects files, proposes edits, and runs tests.

Reasoning: tool use and state tracking allow progress across a real codebase.
