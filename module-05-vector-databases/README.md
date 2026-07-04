# Module 05 - Vector Databases

This module teaches how vector databases store, index, filter, update, and scale embedding-powered applications.

[Back to roadmap](../README.md)

## Why it exists?

Vector databases exist because embedding search needs storage, indexing, metadata filtering, updates, replication, and operational controls. A simple list of vectors is not enough for production.

## What it is?

A vector database stores vectors and metadata, builds indexes for similarity search, and exposes query APIs for retrieval.

## How it works internally?

Documents are chunked, embedded, and inserted with metadata. The database builds an index for nearest-neighbor search and supports filters for tenant, source, category, and permissions.

```text
Documents -> chunks -> embeddings -> vector database -> filtered search
```

## When to use it?

Use a vector database when you need scalable semantic search, RAG retrieval, product search, recommendation, or multi-tenant knowledge bases.

Avoid it for tiny datasets where in-memory search is enough or for exact transactional queries that belong in a relational database.

## Architecture

Architecture diagram:

```text
                         +----------------------+
                         | Data Sources         |
                         | files / DB / events  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Ingestion Service    |
                         | parse / clean / diff |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Chunk + Metadata     |
                         | ids / ACL / tenant   |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Embedding Workers    |
                         | queue / batch/retry  |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Vector Database   |                 | Metadata Store    |
        | index / shards    |                 | filters / lineage |
        | replicas / comp.  |                 | permissions       |
        +---------+---------+                 +---------+---------+
                  |                                     |
                  +------------------+------------------+
                                     |
                                     v
                         +----------------------+
                         | Retriever API        |
                         | top-k / filter / SLA |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Search / RAG App     |
                         | UI or LLM context    |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | Observability        |
                         | recall / latency     |
                         | index health / cost  |
                         +----------------------+
```

User flow diagram:

```text
1. Admin uploads documents
   |
   v
2. Pipeline chunks and embeds content
   |
   v
3. Vectors and metadata are stored
   |
   v
4. User asks a question
   |
   v
5. Retriever searches vectors with filters
   |
   v
6. App returns matching sources or passes them to an LLM
```

## Trade-Offs

| Area | Benefit | Trade-off |
|---|---|---|
| Indexing | Fast vector search | Needs tuning and maintenance |
| Sharding | Scales data volume | More operational complexity |
| Replication | Improves availability | Higher infrastructure cost |
| Filtering | Supports production constraints | Can slow retrieval |
| Updates | Keeps knowledge fresh | Re-embedding can be expensive |

## Module Structure

## What topics we learn in this module?

- Why vector databases exist
- Indexing
- Sharding
- Replication
- Compression
- Updates
- Filtering
- Hybrid search

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
module-05-vector-databases/
  01-why-vector-databases-exist/
  02-indexing/
  03-sharding/
  04-replication/
  05-compression/
  06-updates/
  07-filtering/
  08-hybrid-search/
```

## Hands-On Project

Build a document search platform or multi-tenant knowledge base.

Core features:

- Document ingestion.
- Chunking and embedding.
- Vector storage.
- Metadata filtering.
- Tenant isolation.
- Query API.
- Observability for retrieval latency and recall.

## Interview Questions And Answers

### 1. What problem does a vector database solve?

It stores vectors and metadata and provides scalable similarity search over them.

### 2. Why is metadata important?

Metadata lets systems filter by tenant, permissions, source, date, and category.

### 3. What is an index?

An index is a data structure that makes vector search faster than scanning every vector.

### 4. Why is updating embeddings hard?

Model changes, document changes, and chunking changes may require re-embedding and re-indexing data.

### 5. Why does multi-tenancy matter?

Customer data must be isolated in storage, search filters, caches, and logs.

## Production Scenarios

### Scenario 1 - Multi-tenant knowledge base

Each customer has private documents.

Reasoning: metadata filters and tenant isolation prevent cross-customer data leaks.

### Scenario 2 - Enterprise document search

Employees search policies, tickets, and technical docs.

Reasoning: vector databases provide fast retrieval across large document collections.

### Scenario 3 - Fresh content updates

Docs change every day.

Reasoning: ingestion and update pipelines must keep vectors synchronized with source documents.
