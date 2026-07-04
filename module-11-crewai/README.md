# Module 11 - CrewAI

This module teaches CrewAI as a framework for organizing agents, tasks, tools, memory, and processes into collaborative AI crews.

[Back to roadmap](../README.md)

## Why it exists?

CrewAI exists to make multi-agent workflows easier to define through agents, tasks, crews, and process orchestration.

## What it is?

CrewAI is an agent orchestration framework. It lets you define agents with roles, assign tasks, provide tools, and run processes sequentially or hierarchically.

## How it works internally?

You define agents, tasks, and a crew. The crew executes tasks according to a process, passing context and outputs between agents.

```text
Agents + tasks + tools -> crew -> process -> final output
```

## When to use it?

Use CrewAI for content pipelines, research workflows, business reports, market analysis, and multi-role task automation.

Avoid it when a single prompt, chain, or deterministic workflow is enough.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | User Goal / Trigger  |
                         | report / pipeline    |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Crew Definition      |
                         | agents + tasks       |
                         | process + tools      |
                         +----------+-----------+
                                    |
             +----------------------+----------------------+
             |                      |                      |
             v                      v                      v
   +------------------+   +------------------+   +------------------+
   | Agent Role       |   | Task Definition  |   | Tool Registry    |
   | goal/backstory   |   | expected output  |   | APIs/search/DB   |
   +--------+---------+   +--------+---------+   +--------+---------+
            |                      |                      |
            +----------------------+----------------------+
                                   |
                                   v
                         +----------------------+
                         | CrewAI Process       |
                         | sequential / hier.   |
                         +----------+-----------+
                                    |
             +----------------------+----------------------+
             |                                             |
             v                                             v
   +------------------+                         +------------------+
   | Shared Context   |                         | Memory           |
   | task outputs     |                         | run history      |
   +--------+---------+                         +--------+---------+
            |                                             |
            +----------------------+----------------------+
                                   |
                                   v
                         +----------------------+
                         | Review / Synthesis   |
                         | quality gate         |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Final Deliverable    |
                         | report / content     |
                         +----------------------+
```

User flow diagram:

```text
1. User defines or triggers a crew task
   |
   v
2. CrewAI assigns tasks to agents
   |
   v
3. Agents use tools and context
   |
   v
4. Process controls task order
   |
   v
5. Outputs are reviewed or combined
   |
   v
6. User receives the final deliverable
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Agent roles | Clear responsibilities | Role design matters |
| Tasks | Structured workflows | Task boundaries can be fuzzy |
| Processes | Easy orchestration | Less control than custom code |
| Tools | Real-world actions | Need permissions and validation |
| Memory | Better continuity | Can preserve bad context |

## Module Structure

## What topics we learn in this module?

- Crews
- Agents
- Tasks
- Process orchestration
- Sequential vs. hierarchical execution
- Memory
- Tool integration

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
module-11-crewai/
  01-crews/
  02-agents/
  03-tasks/
  04-process-orchestration/
  05-sequential-vs-hierarchical/
  06-memory/
  07-tool-integration/
```

## Hands-On Project

Build an automated content or market research pipeline.

Core features:

- Multiple agent roles.
- Task definitions.
- Tool integration.
- Sequential or hierarchical process.
- Review step.
- Final report generation.

## Interview Questions And Answers

### 1. What is a crew?

A crew is a group of agents and tasks configured to work together.

### 2. What is an agent role?

An agent role defines responsibilities, behavior, and expected contribution.

### 3. What is a task?

A task is a unit of work assigned to an agent.

### 4. What is hierarchical execution?

Hierarchical execution uses a manager-like agent or process to coordinate other agents.

### 5. Why validate tool usage?

Agents can request unsafe or incorrect actions, so tools need permission checks and validation.

## Production Scenarios

### Scenario 1 - Market research report

Researcher, analyst, and writer agents produce a report.

Reasoning: CrewAI structures the workflow into clear roles and tasks.

### Scenario 2 - Content pipeline

Agents create, edit, and review content.

Reasoning: sequential tasks provide a repeatable content workflow.

### Scenario 3 - Business report generator

Agents gather metrics, summarize insights, and create an executive report.

Reasoning: tool integration and role separation make the pipeline easier to maintain.
