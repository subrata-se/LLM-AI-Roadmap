# Module 6 - RAG

This module teaches retrieval-augmented generation: using external knowledge to ground LLM answers in documents, data, and citations.

[Back to roadmap](../README.md)

## Why it exists?

RAG exists because LLMs do not automatically know private, current, or domain-specific information. Retrieval gives the model relevant context at answer time.

## What it is?

RAG combines a retriever with an LLM. The retriever finds relevant source material, and the LLM uses that material to generate an answer.

## How it works internally?

A user query is embedded or searched, relevant chunks are retrieved, the prompt is built with those chunks, and the LLM answers with grounded context.

```text
Question -> retrieval -> context -> prompt -> LLM answer -> citations
```

## When to use it?

Use RAG for document chatbots, internal knowledge assistants, legal or policy search, support automation, and any system where answers depend on external knowledge.

Avoid RAG when there is no useful knowledge source, when exact database queries are better, or when retrieval quality cannot be trusted.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | Document Sources     |
                         | PDF / docs / wiki    |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Ingestion Pipeline   |
                         | parse / clean / OCR  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Chunker + Metadata   |
                         | section / page / ACL |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Embedding Workers    |
                         | batch / retry        |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Vector Store         |
                         | chunks + metadata    |
                         +----------+-----------+
                                    ^
                                    |
                         +----------+-----------+
                         | Query Processor      |
                         | rewrite / embed      |
                         +----------+-----------+
                                    ^
                                    |
                         +----------+-----------+
                         | User Question        |
                         +----------------------+

Runtime answer path:

User Question
  |
  v
Retriever -> Metadata Filter -> Re-ranker -> Context Compressor
  |
  v
Prompt Builder -> LLM Provider -> Citation Validator -> Answer
  |
  v
Evaluation / Logs / Feedback
```

User flow diagram:

```text
1. User asks a question
   |
   v
2. Backend searches relevant documents
   |
   v
3. Retriever returns top chunks
   |
   v
4. Prompt builder inserts evidence into the prompt
   |
   v
5. LLM answers using the supplied context
   |
   v
6. App returns answer with citations or confidence metadata
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Grounding | Reduces hallucination | Depends on retrieval quality |
| Citations | Improves trust | Requires source tracking |
| Chunking | Controls context | Poor chunks hurt answers |
| Re-ranking | Improves relevance | Adds latency and cost |
| Context compression | Fits more evidence | May lose detail |

## Module Structure

## What topics we learn in this module?

- Chunking
- Recursive chunking
- Metadata
- Retrievers
- Top-K retrieval
- Re-ranking
- Query expansion
- Context compression
- Citations
- Evaluation
- Hallucination reduction

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
module-6-rag/
  01-chunking/
  02-recursive-chunking/
  03-metadata/
  04-retrievers/
  05-top-k-retrieval/
  06-reranking/
  07-query-expansion/
  08-context-compression/
  09-citations/
  10-evaluation/
  11-hallucination-reduction/
```

## Hands-On Project

Build a PDF or documentation chatbot.

Core features:

- Document upload.
- Chunking and embedding.
- Vector retrieval.
- Re-ranking.
- Prompt building with citations.
- Answer generation.
- Evaluation set for answer quality.

## Interview Questions And Answers

### 1. What is RAG?

RAG is retrieval-augmented generation: retrieving relevant context and giving it to an LLM before it answers.

### 2. Why does RAG reduce hallucination?

It gives the model evidence to use instead of relying only on learned patterns.

### 3. Why does chunking matter?

Chunks control what evidence is retrievable and how much fits into the prompt.

### 4. What is re-ranking?

Re-ranking rescoring retrieved candidates to improve final relevance.

### 5. How do you evaluate RAG?

Measure retrieval relevance, answer correctness, citation quality, latency, and hallucination rate.

## Production Scenarios

### Scenario 1 - Company docs chatbot

Employees ask questions about internal docs.

Reasoning: RAG lets the assistant answer from current company knowledge.

### Scenario 2 - Legal document assistant

Users ask questions about contracts.

Reasoning: citations and source-grounded answers are critical for review and trust.

### Scenario 3 - Support knowledge assistant

Support agents ask for troubleshooting steps.

Reasoning: retrieval helps find the right procedure quickly while keeping answers grounded.
