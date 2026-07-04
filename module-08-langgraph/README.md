# Module 08 - LangGraph

This module teaches how to build stateful, controllable, long-running LLM workflows with graph-based execution.

[Back to roadmap](../README.md)

## Why it exists?

LangGraph exists because many LLM workflows are not simple chains. They need branching, loops, persisted state, human approval, retries, and resumable execution.

## What it is?

LangGraph is a framework for modeling LLM workflows as state graphs. Nodes perform work, edges define transitions, and checkpoints preserve progress.

## How it works internally?

A state object moves through graph nodes. Each node reads and updates state. Edges decide what node runs next. Checkpointers can persist the workflow between steps.

```text
State -> node -> updated state -> edge decision -> next node
```

## When to use it?

Use LangGraph for workflows with branching, review steps, retries, multi-step state, human approval, or long-running execution.

Avoid it for simple one-shot prompts or linear flows where a plain function or chain is enough.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | Client / Operator    |
                         | start / resume       |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Workflow API         |
                         | auth / run id        |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Graph State          |
                         | messages / task data |
                         | status / artifacts   |
                         +----------+-----------+
                                    |
                                    v
              +---------------------+---------------------+
              |                                           |
              v                                           v
    +-------------------+                       +-------------------+
    | LangGraph Runtime |<---- checkpoint ----->| Checkpoint Store  |
    | nodes + edges     |                       | Postgres/Redis    |
    +---------+---------+                       +-------------------+
              |
      +-------+--------+----------+------------+----------+
      |                |          |                       |
      v                v          v                       v
+-----------+   +-----------+  +-----------+       +--------------+
| Classify  |   | Retrieve  |  | Generate  |       | Human Review |
| node      |   | node      |  | node      |       | interrupt    |
+-----+-----+   +-----+-----+  +-----+-----+       +------+-------+
      |               |              |                    |
      +---------------+--------------+--------------------+
                                      |
                                      v
                           +----------------------+
                           | Edge Router          |
                           | branch / loop / stop |
                           +----------+-----------+
                                      |
                                      v
                           +----------------------+
                           | Final Result         |
                           | answer / decision    |
                           +----------------------+
```

User flow diagram:

```text
1. User starts a workflow
   |
   v
2. Graph initializes state
   |
   v
3. Nodes run and update state
   |
   v
4. Edges choose next steps
   |
   v
5. Workflow pauses if human approval is needed
   |
   v
6. Workflow resumes and returns result
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Graphs | Clear branching workflows | More design upfront |
| State | Durable multi-step context | State schema must be managed |
| Checkpoints | Resume after interruption | Requires storage |
| Human approval | Safer automation | Slower workflows |
| Loops | Supports agent behavior | Needs stopping conditions |

## Module Structure

## What topics we learn in this module?

- State graphs
- Nodes
- Edges
- Persistence
- Checkpointing
- Human approval
- Interrupt/resume
- Long-running workflows

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
module-08-langgraph/
  01-state-graphs/
  02-nodes/
  03-edges/
  04-persistence/
  05-checkpointing/
  06-human-approval/
  07-interrupt-resume/
  08-long-running-workflows/
```

## Hands-On Project

Build an AI interview platform or resume screening workflow.

Core features:

- Graph state.
- Branching workflow.
- Human approval node.
- Checkpointing.
- Resume after interruption.
- Workflow observability.

## Interview Questions And Answers

### 1. What is LangGraph?

LangGraph is a framework for building stateful LLM workflows as graphs.

### 2. What is a node?

A node is a unit of work that reads and updates workflow state.

### 3. What is checkpointing?

Checkpointing saves workflow state so execution can resume later.

### 4. When should you use human approval?

Use it before high-impact actions, risky decisions, or external side effects.

### 5. Why are stopping conditions important?

Loops and agents need limits to prevent runaway execution.

## Production Scenarios

### Scenario 1 - AI interview platform

The system asks questions, evaluates responses, and decides next steps.

Reasoning: graph state captures candidate progress and branching interview flow.

### Scenario 2 - Resume screening workflow

The app extracts data, scores fit, and pauses for recruiter approval.

Reasoning: human review protects against fully automated hiring decisions.

### Scenario 3 - Customer onboarding

The workflow gathers data, verifies documents, and resumes after missing information arrives.

Reasoning: checkpointing supports long-running user journeys.
