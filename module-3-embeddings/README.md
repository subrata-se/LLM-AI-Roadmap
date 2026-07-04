# Module 3 - Embeddings

This module teaches how text, documents, products, code, and other objects can be represented as vectors for search, clustering, recommendations, and retrieval.

[Back to roadmap](../README.md)

## Why it exists?

Embeddings exist because software often needs to compare meaning, not exact words. Keyword search misses synonyms and paraphrases. Embeddings make semantic similarity computable.

## What it is?

An embedding is a dense vector representation of data. Similar items should end up close together in vector space.

Important terms:

- `Vector`: A list of numbers representing an item.
- `Dimension`: The number of values in the vector.
- `Similarity`: A score that estimates closeness.
- `Embedding model`: A model that converts text or other data into vectors.

## How it works internally?

An embedding model reads input text and produces a numeric vector. Similar concepts tend to produce vectors with similar directions or distances.

```text
Text -> embedding model -> vector -> similarity function -> ranked matches
```

## When to use it?

Use embeddings for semantic search, recommendation, clustering, duplicate detection, RAG retrieval, FAQ matching, and code search.

Avoid embeddings when exact matching, simple filters, or SQL queries fully solve the problem.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | Source Documents     |
                         | FAQs / docs / code   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Ingestion Pipeline   |
                         | clean / normalize    |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Chunking Strategy    |
                         | size / overlap / ids |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Embedding Worker     |
                         | batch / retry / rate |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Vector Store      |                 | Metadata Store    |
        | vectors / index   |                 | source / tenant   |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  +------------------+------------------+
                                     |
                                     v
                         +----------------------+
                         | Query Embedding      |
                         | user query -> vector |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Similarity Search    |
                         | cosine / dot / L2    |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Rank + Filter        |
                         | metadata / score     |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Search Results       |
                         | chunks + sources     |
                         +----------------------+
```

User flow diagram:

```text
1. User submits a query
   |
   v
2. Backend embeds the query
   |
   v
3. Vector search finds nearby document vectors
   |
   v
4. Backend ranks and filters matches
   |
   v
5. User receives semantically relevant results
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Semantic search | Finds meaning, not only keywords | Can miss exact constraints |
| Dimensionality | Captures rich signals | More storage and compute |
| Model choice | Better models improve retrieval | Costs and migrations matter |
| Chunking | Improves document retrieval | Poor chunks reduce quality |
| Similarity metric | Easy ranking | Scores need calibration |

## Module Structure

## What topics we learn in this module?

- What embeddings are
- Vector representations
- Dimensionality
- Cosine similarity
- Euclidean distance
- Dot product
- Embedding models
- Chunking strategies

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
module-3-embeddings/
  01-what-embeddings-are/
  02-vector-representations/
  03-dimensionality/
  04-cosine-similarity/
  05-euclidean-distance/
  06-dot-product/
  07-embedding-models/
  08-chunking-strategies/
```

## Hands-On Project

Build a semantic search engine.

Core features:

- Ingest FAQ or knowledge-base documents.
- Chunk and clean text.
- Generate embeddings.
- Store vectors.
- Search by semantic similarity.
- Return ranked results with source metadata.

## Interview Questions And Answers

### 1. What is an embedding?

An embedding is a numeric vector that represents the meaning or features of an input.

### 2. Why are embeddings useful?

They let applications compare semantic similarity between items that may not share exact words.

### 3. What is cosine similarity?

Cosine similarity compares the direction of two vectors and is commonly used for semantic similarity.

### 4. Why does chunking matter?

Embedding an entire large document can blur meaning. Good chunks keep each vector focused.

### 5. When should you not use embeddings?

Do not use embeddings when exact filters, deterministic rules, or simple keyword matching are enough.

## Production Scenarios

### Scenario 1 - FAQ search

A user asks a question that does not exactly match the FAQ wording.

Reasoning: embeddings can find the closest meaning even when keywords differ.

### Scenario 2 - Knowledge base search

An internal tool searches long company docs.

Reasoning: chunk-level embeddings make large documents searchable by concept.

### Scenario 3 - Duplicate ticket detection

A support platform groups similar tickets.

Reasoning: vector similarity can identify repeated issues even when users phrase them differently.
