# Module 10 - Multi-Agent AI

This module teaches how multiple specialized agents can collaborate through roles, coordination, review, and shared state.

[Back to roadmap](../README.md)

## Why it exists?

Multi-agent AI exists because complex work often benefits from separation of responsibilities. A planner, researcher, executor, and reviewer can produce better results than one overloaded agent.

## What it is?

A multi-agent system uses multiple agents with different roles, tools, memory, and responsibilities. A coordinator manages task flow and communication.

## How it works internally?

The system decomposes a goal, assigns subtasks to agents, gathers outputs, reviews results, and integrates them into a final deliverable.

```text
Goal -> coordinator -> specialist agents -> review -> final output
```

## When to use it?

Use multi-agent systems for complex research, software development, content pipelines, analysis workflows, and tasks that benefit from critique or parallel work.

Avoid multi-agent systems when a single deterministic workflow is simpler and easier to test.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | User Goal            |
                         | deliverable + rules  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Coordinator Agent    |
                         | task decomposition   |
                         | assignment / budget  |
                         +----------+-----------+
                                    |
             +----------------------+----------------------+
             |                      |                      |
             v                      v                      v
   +------------------+   +------------------+   +------------------+
   | Planner Agent    |   | Research Agent   |   | Executor Agent   |
   | roadmap / tasks  |   | sources / facts  |   | build / write    |
   +--------+---------+   +--------+---------+   +--------+---------+
            |                      |                      |
            +----------------------+----------------------+
                                   |
                                   v
                         +----------------------+
                         | Shared State         |
                         | tasks / artifacts    |
                         | decisions / memory   |
                         +----------+-----------+
                                    |
             +----------------------+----------------------+
             |                                             |
             v                                             v
   +------------------+                         +------------------+
   | Reviewer Agent   |                         | Critic Agent     |
   | correctness      |                         | gaps / risks     |
   +--------+---------+                         +--------+---------+
            |                                             |
            +----------------------+----------------------+
                                   |
                                   v
                         +----------------------+
                         | Coordinator Merge    |
                         | conflict resolution  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Final Deliverable    |
                         | answer / repo / doc  |
                         +----------------------+
```

User flow diagram:

```text
1. User submits a complex goal
   |
   v
2. Coordinator decomposes the work
   |
   v
3. Specialist agents complete subtasks
   |
   v
4. Reviewer agent checks quality and gaps
   |
   v
5. Coordinator merges outputs
   |
   v
6. User receives final result
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Specialization | Better task focus | More coordination needed |
| Parallelism | Faster independent work | Harder state management |
| Review | Higher quality | More cost and latency |
| Communication | Better collaboration | More failure modes |
| Coordination | Clear ownership | Requires orchestration logic |

## Module Structure

## What topics we learn in this module?

- Planner agent
- Research agent
- Reviewer agent
- Executor agent
- Critic agent
- Coordinator
- Communication protocols

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
module-10-multi-agent-ai/
  01-planner-agent/
  02-research-agent/
  03-reviewer-agent/
  04-executor-agent/
  05-critic-agent/
  06-coordinator/
  07-communication-protocols/
```

## Hands-On Project

Build an AI software development team or research organization.

Core features:

- Coordinator agent.
- Specialist roles.
- Shared task state.
- Review loop.
- Final synthesis.
- Cost and step limits.

## Interview Questions And Answers

### 1. Why use multiple agents?

Multiple agents can specialize, work in parallel, and review each other.

### 2. What does a coordinator do?

The coordinator assigns work, manages state, resolves dependencies, and integrates outputs.

### 3. What is the risk of agent communication?

Agents can pass along incorrect assumptions or create loops without clear protocols.

### 4. Why use a reviewer agent?

A reviewer catches errors, checks completeness, and improves final quality.

### 5. When is multi-agent overkill?

When the task is simple, deterministic, or easy to handle with one chain or workflow.

## Production Scenarios

### Scenario 1 - AI software team

Planner, coder, reviewer, and tester agents collaborate on a feature.

Reasoning: specialization mirrors real engineering workflows and adds quality checks.

### Scenario 2 - Research organization

Multiple agents gather evidence from different sources and a reviewer checks claims.

Reasoning: parallel research improves coverage while review reduces unsupported conclusions.

### Scenario 3 - Interview panel

Agents evaluate different candidate dimensions.

Reasoning: role separation can structure feedback, but human oversight remains important.
