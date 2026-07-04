# Module 04 - Similarity Search

This module teaches how systems find nearest or most relevant items from vectors, metadata, and hybrid search signals.

[Back to roadmap](../README.md)

## Why it exists?

Similarity search exists because once data is represented as vectors, applications need efficient ways to find the closest matches. Brute-force comparison becomes too slow as data grows.

## What it is?

Similarity search retrieves items that are closest to a query by a distance or similarity function. It can be exact, approximate, vector-only, keyword-only, or hybrid.

## How it works internally?

The query is embedded into a vector. The search system compares it against stored vectors using a metric such as cosine similarity, dot product, or Euclidean distance. Approximate indexes speed this up by searching likely neighborhoods.

```text
Query -> embedding -> vector index -> nearest neighbors -> ranked results
```

## When to use it?

Use similarity search for semantic search, recommendations, document retrieval, code search, product discovery, and RAG pipelines.

Avoid approximate search when you need mathematically exact results over small data or strict filtering before ranking.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | User Query           |
                         | text / filters       |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Query Embedder    |                 | Keyword Analyzer  |
        | text -> vector    |                 | BM25 / terms      |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  v                                     v
        +-------------------+                 +-------------------+
        | Vector ANN Index  |                 | Keyword Index     |
        | HNSW / IVF        |                 | inverted index    |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  +------------------+------------------+
                                     |
                                     v
                         +----------------------+
                         | Candidate Merge      |
                         | vector + keyword     |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Metadata Filter      |
                         | tenant / ACL / type  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Re-ranker            |
                         | cross-encoder/rules  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Final Ranked Results |
                         | score + explanation  |
                         +----------------------+
```

User flow diagram:

```text
1. User searches with natural language
   |
   v
2. Backend creates a query embedding
   |
   v
3. Search index finds nearest vector candidates
   |
   v
4. Metadata filters remove invalid matches
   |
   v
5. Re-ranker improves ordering
   |
   v
6. User receives ranked results
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Exact search | Highest accuracy | Slow at large scale |
| ANN search | Fast at scale | May miss best match |
| HNSW | Strong recall and speed | Memory heavy |
| IVF | Efficient large-scale search | Needs tuning |
| Hybrid search | Balances keywords and meaning | More complex ranking |

## Module Structure

## What topics we learn in this module?

- Exact search vs. Approximate Nearest Neighbor
- HNSW
- IVF
- Hybrid search
- Metadata filtering
- Re-ranking

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
module-04-similarity-search/
  01-exact-vs-ann/
  02-hnsw/
  03-ivf/
  04-hybrid-search/
  05-metadata-filtering/
  06-reranking/
```

## Hands-On Project

Build a product or code search engine.

Core features:

- Store vectors with metadata.
- Run nearest-neighbor search.
- Apply metadata filters.
- Add hybrid keyword and vector ranking.
- Re-rank final candidates.

## Interview Questions And Answers

### 1. What is similarity search?

Similarity search finds items closest to a query according to a distance or similarity metric.

### 2. Why use ANN?

Approximate nearest neighbor search trades perfect accuracy for much faster retrieval at scale.

### 3. What is hybrid search?

Hybrid search combines keyword matching with vector similarity.

### 4. Why use metadata filtering?

Filtering ensures results satisfy constraints such as tenant, category, date, permissions, or language.

### 5. Why re-rank results?

The first retrieval step is optimized for speed. Re-ranking improves final relevance.

## Production Scenarios

### Scenario 1 - Product search

A user searches "comfortable shoes for walking all day."

Reasoning: hybrid search can combine semantic intent with exact product filters like size and price.

### Scenario 2 - Code search

An engineer searches for "where we validate JWT claims."

Reasoning: vector search can find relevant code even if function names do not contain those exact words.

### Scenario 3 - Support article retrieval

A chatbot retrieves candidate help articles before answering.

Reasoning: similarity search is the retrieval backbone for many RAG systems.
