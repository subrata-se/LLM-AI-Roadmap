# Module 01 - LLM Fundamentals

This module builds a deep foundation for understanding, using, and designing systems around large language models. The goal is not only to call an API, but to understand what happens before, during, and after an LLM generates a response.

[Back to roadmap](../README.md)

## Why it exists?

Large language models exist because many software problems involve language, reasoning over text, and generating useful responses from messy human input.

Before LLMs, most NLP systems were narrow: one model for classification, another for translation, another for question answering, another for summarization. LLMs changed this by learning general language patterns from massive text corpora and then applying those patterns to many tasks through prompts.

LLMs are useful because they can:

- Understand natural language instructions.
- Generate text, code, summaries, explanations, and structured data.
- Adapt to new tasks without a custom model for every workflow.
- Act as a reasoning and orchestration layer in AI applications.
- Power chatbots, copilots, document assistants, agents, and automation tools.

## What it Is?

An LLM is a neural network trained to predict the next token in a sequence. A token may be a word, part of a word, punctuation, whitespace, or another text fragment depending on the tokenizer.

At a high level:

- Input text is converted into tokens.
- Tokens are converted into vectors called embeddings.
- A transformer processes those vectors using attention.
- The model produces probabilities for the next token.
- A decoding strategy chooses the next token.
- The process repeats until the answer is complete.

Important terms:

- `Token`: The unit of text the model reads and writes.
- `Context window`: The maximum amount of input and output tokens the model can process at once.
- `Prompt`: The instruction and context sent to the model.
- `Completion`: The generated response.
- `Logits`: Raw model scores before they become probabilities.
- `Temperature`: A setting that controls randomness.
- `Top-p`: A setting that limits generation to the most likely token mass.
- `Hallucination`: A confident answer that is unsupported, incorrect, or fabricated.

## How it works internally?

LLMs usually use the transformer architecture. The core flow is:

1. Text is split into tokens by a tokenizer.
2. Tokens are mapped to dense vectors through embedding tables.
3. Positional information is added so the model can reason about order.
4. Transformer blocks process the vectors.
5. Self-attention lets each token look at other relevant tokens in the context.
6. Feed-forward layers transform the representation further.
7. The final layer produces scores for possible next tokens.
8. A sampler chooses the next token using settings like temperature and top-p.
9. The selected token is appended to the context.
10. The loop repeats until a stop condition is reached.

The key idea is that the model does not retrieve facts like a database by default. It generates the next most likely tokens based on learned statistical patterns and the context you provide.

## When to use it?

Use an LLM when the task benefits from language understanding, flexible reasoning, or generation.

Good use cases:

- Chatbots and assistants
- Summarization
- Text classification with nuanced instructions
- Code generation and code review
- Document question answering with retrieval
- Data extraction into structured formats
- Natural language interfaces for tools and APIs
- Drafting, rewriting, and explanation

Avoid using an LLM as the only solution when:

- You need exact arithmetic or deterministic computation.
- A simple rule, SQL query, or search index is enough.
- The answer must be guaranteed correct without verification.
- The task involves sensitive decisions without human review.
- Latency or cost budgets are too strict for model calls.
- The model has no access to required private or current data.

## Architecture

A production LLM application usually has more than one layer around the model.

Architecture diagram:

```text
                         +----------------------+
                         |        Client        |
                         |  Web / Mobile / CLI  |
                         +----------+-----------+
                                    |
                                    v
                         +----------------------+
                         | API Gateway / FastAPI|
                         | auth / rate limits   |
                         +----------+-----------+
                                    |
               +--------------------+--------------------+
               |                    |                    |
               v                    v                    v
      +----------------+   +----------------+   +----------------+
      | Request Schema |   | Tenant Policy  |   | Abuse Checks   |
      | validation     |   | limits / plan  |   | prompt safety  |
      +--------+-------+   +--------+-------+   +--------+-------+
               |                    |                    |
               +--------------------+--------------------+
                                    |
                                    v
                         +----------------------+
                         | Token Budgeter       |
                         | input / output cost  |
                         +----------+-----------+
                                    |
              +---------------------+---------------------+
              |                     |                     |
              v                     v                     v
    +-------------------+ +-------------------+ +-------------------+
    | Conversation Store| | Retrieval Layer   | | Prompt Builder    |
    | recent + summary  | | docs / search     | | system + context  |
    +---------+---------+ +---------+---------+ +---------+---------+
              |                     |                     |
              +---------------------+---------------------+
                                    |
                                    v
                         +----------------------+
                         | Prompt Cache         |
                         | semantic / exact hit |
                         +----------+-----------+
                                    |
                 +------------------+------------------+
                 |                                     |
                 v                                     v
        +-------------------+                 +-------------------+
        | Cached Response   |                 | Model Router      |
        | return fast       |                 | cost / quality    |
        +-------------------+                 | context / fallback|
                                              +---------+---------+
                                                        |
                                                        v
                                              +-------------------+
                                              | Provider Adapter  |
                                              | streaming API     |
                                              | retries/timeouts  |
                                              +---------+---------+
                                                        |
                                                        v
                                              +-------------------+
                                              | Output Processor  |
                                              | JSON/tools/checks |
                                              +---------+---------+
                                                        |
                 +--------------------------------------+------------------+
                 |                                      |                  |
                 v                                      v                  v
        +-------------------+                 +-------------------+ +-------------------+
        | Response Stream   |                 | Usage Ledger      | | Observability     |
        | tokens to client  |                 | tokens / cost     | | logs/metrics/eval |
        +-------------------+                 +-------------------+ +-------------------+
```

User flow diagram:

```text
1. User sends message
   |
   v
2. Backend validates request, auth, tenant, and rate limit
   |
   v
3. Backend loads recent conversation memory
   |
   v
4. Backend optionally retrieves relevant documents
   |
   v
5. Prompt builder combines system prompt, memory, retrieval, and user input
   |
   v
6. Model router selects the best model for cost, latency, and quality
   |
   v
7. LLM streams tokens back to the backend
   |
   v
8. Backend parses output, validates structure, and handles tool calls if needed
   |
   v
9. Backend logs latency, tokens, cost, model, and cache status
   |
   v
10. Client receives the final streamed response
```

For the Module 01 project, we will build toward a streaming AI chatbot with:

- Streaming responses
- Conversation memory
- Token accounting
- Cost tracking
- Response caching
- Model selection
- Structured logs

## Trade-Offs

LLMs are powerful, but they introduce engineering trade-offs.

| Area | Benefit | Trade-off |
|---|---|---|
| Flexibility | Handles many language tasks with one interface | Harder to make fully deterministic |
| Speed of development | Prototypes are fast | Production reliability takes careful design |
| Quality | Can produce high-quality natural responses | May hallucinate or overstate uncertainty |
| Context | Can use provided documents and conversation history | Context windows are limited and expensive |
| Cost | Avoids training custom models for every task | Inference cost can grow quickly |
| Integration | Can call tools and produce structured data | Requires validation, retries, and guardrails |
| Model selection | Different models fit different workloads | Routing adds operational complexity |

## Module Structure

## What topics we learn in this module?

- Tokenization
- Context windows
- Attention
- Transformers conceptually
- Temperature
- Top-p
- Hallucinations
- Function calling
- Structured outputs
- Model selection

Each topic folder we create inside this module should follow this same README structure:

| # | Section | Purpose |
|---|---------|---------|
| 1 | Why it exists? | What problem this topic solves |
| 2 | What it is? | Clear definition and vocabulary |
| 3 | How it works internally? | Internals, data flow, and mental model |
| 4 | When to use it? | Practical use cases and anti-patterns |
| 5 | Architecture | How it appears in real LLM systems |
| 6 | Trade-offs | Benefits, limits, and alternatives |
| 7 | Hands-on practice | Small implementation or experiment |
| 8 | Interview questions | Questions and expected answer direction |
| 9 | Production scenarios | Real systems where the topic matters |

## Topic Folder Plan

Each topic has its own README and a runnable Python script:

| # | Topic | Hands-on script |
|---|-------|-----------------|
| 01 | [Tokenization](01-tokenization/README.md) | [token_counter.py](01-tokenization/token_counter.py) |
| 02 | [Context windows](02-context-windows/README.md) | [context_budget.py](02-context-windows/context_budget.py) |
| 03 | [Attention](03-attention/README.md) | [attention_demo.py](03-attention/attention_demo.py) |
| 04 | [Transformers conceptually](04-transformers-conceptually/README.md) | [transformer_flow.py](04-transformers-conceptually/transformer_flow.py) |
| 05 | [Temperature](05-temperature/README.md) | [temperature_sampling.py](05-temperature/temperature_sampling.py) |
| 06 | [Top-p](06-top-p/README.md) | [top_p_sampling.py](06-top-p/top_p_sampling.py) |
| 07 | [Hallucinations](07-hallucinations/README.md) | [hallucination_guard.py](07-hallucinations/hallucination_guard.py) |
| 08 | [Function calling](08-function-calling/README.md) | [function_calling_router.py](08-function-calling/function_calling_router.py) |
| 09 | [Structured outputs](09-structured-outputs/README.md) | [structured_output_validation.py](09-structured-outputs/structured_output_validation.py) |
| 10 | [Model selection](10-model-selection/README.md) | [model_selection_router.py](10-model-selection/model_selection_router.py) |

## Hands-On Project

Build a streaming AI chatbot backend.

Core features:

- Accept a user message through an API endpoint.
- Build a prompt from system instructions and conversation history.
- Stream tokens back to the client.
- Count input and output tokens.
- Estimate request cost.
- Cache repeated responses.
- Store recent conversation memory.
- Validate structured output when required.
- Log latency, token usage, cache hits, model name, and cost.

## Interview Questions And Answers

### 1. What is an LLM?

An LLM, or large language model, is a neural network trained on large amounts of text to predict the next token in a sequence. Because it learns language patterns, facts, reasoning traces, code patterns, and instruction-following behavior, it can generate useful responses for many tasks through prompting.

In production, treat an LLM as a probabilistic language engine, not a database or a deterministic rules engine.

### 2. Why do LLMs use tokens instead of raw words?

LLMs use tokens because raw words are not flexible enough. A token can represent a full word, part of a word, punctuation, whitespace, or symbols. This lets the model handle rare words, new words, code, names, URLs, and multiple languages without needing one fixed vocabulary entry for every possible word.

Tokens also matter because model cost, context limits, and latency are usually measured in tokens.

### 3. What is a context window?

A context window is the maximum number of tokens the model can consider in one request, including system instructions, conversation history, retrieved documents, user input, and generated output.

If the context window is exceeded, the application must truncate, summarize, retrieve fewer documents, or use a model with a larger context window. Poor context management can cause the model to forget important information or produce weaker answers.

### 4. How does attention help a model understand context?

Attention lets each token decide which other tokens in the context are most relevant. For example, in a long question, attention helps the model connect a pronoun to the noun it refers to, connect an instruction to a later constraint, or connect a code error to the specific function that caused it.

Without attention, the model would have a much harder time preserving relationships across long sequences.

### 5. What is the difference between temperature and top-p?

Temperature controls how sharp or random the probability distribution is before choosing the next token. Lower temperature makes responses more focused and predictable. Higher temperature makes responses more varied and creative.

Top-p, also called nucleus sampling, limits token selection to the smallest group of likely tokens whose combined probability reaches `p`. Lower top-p removes unlikely tokens from consideration. Higher top-p allows more variety.

Use low temperature for extraction, classification, and structured outputs. Use higher temperature when you want brainstorming, writing variation, or creative exploration.

### 6. Why do LLMs hallucinate?

LLMs hallucinate because they generate likely text, not guaranteed truth. If the prompt lacks enough context, asks for information the model does not know, contains false assumptions, or rewards confident-sounding answers, the model may produce unsupported or fabricated content.

Common mitigations include retrieval, citations, stricter prompts, structured outputs, validation, tool calls, refusal behavior, and human review for high-risk workflows.

### 7. How does function calling differ from plain text generation?

Plain text generation asks the model to respond directly in natural language. Function calling asks the model to choose a defined tool or function and provide structured arguments for that function.

Function calling is better when the application needs to perform an action, query a database, call an API, run business logic, or separate reasoning from execution. The model decides what should be called, but the application remains responsible for executing and validating the result.

### 8. Why are structured outputs important in production?

Structured outputs make LLM responses easier to validate, parse, store, test, and pass into downstream systems. Instead of hoping the model returns readable text in the right format, the application can require fields like `answer`, `confidence`, `category`, `citations`, or `next_action`.

This is important because production systems need predictable contracts. A malformed response can break workflows, create bad data, or cause unsafe automation.

### 9. How would you select a model for a chatbot?

Choose a model based on the chatbot's job, not only benchmark scores. Consider response quality, latency, context window, cost per token, streaming support, tool-calling support, structured output reliability, safety behavior, multilingual needs, and provider reliability.

For a production chatbot, you may use model routing:

- Small, cheaper model for simple FAQ answers.
- Stronger model for complex reasoning.
- Larger-context model for document-heavy conversations.
- Fallback model when the primary provider fails.

### 10. How would you reduce cost in a high-traffic LLM application?

Reduce cost by controlling tokens, routing intelligently, and avoiding unnecessary model calls.

Useful strategies:

- Cache repeated prompts and common answers.
- Use smaller models for simple tasks.
- Summarize or trim conversation history.
- Retrieve only the most relevant context.
- Set output token limits.
- Stream responses but stop early when enough answer is produced.
- Batch offline jobs where possible.
- Track token usage per user, tenant, feature, and endpoint.
- Add budgets, quotas, and alerts.

## Production Scenarios

### Scenario 1 - Customer Support Chatbot

A SaaS company needs a chatbot that answers product questions from documentation and account-specific data.

Key decisions:

- Use retrieval so answers are grounded in current docs.
- Keep conversation memory short and summarize older turns.
- Use structured output for escalation decisions.
- Track token cost per customer account.
- Add fallback to human support when confidence is low.

### Scenario 2 - Internal Engineering Assistant

An engineering team wants an assistant that explains code, reviews pull requests, and answers architecture questions.

Key decisions:

- Use a larger context model for long files and design docs.
- Use function calling to fetch repository files or ticket details.
- Require citations to source files when answering code questions.
- Keep audit logs for generated suggestions.
- Use lower temperature for review and higher temperature for brainstorming.

### Scenario 3 - Multi-Tenant AI Platform

A company offers AI features to many customers from one shared backend.

Key decisions:

- Route requests by tenant plan, task type, and budget.
- Enforce token quotas and monthly spend limits.
- Isolate tenant data in prompts, retrieval, logs, and caches.
- Monitor latency, cost, hallucination reports, and failure rates.
- Use structured outputs so downstream workflows are stable.

### Scenario 4 - Document Analysis Workflow

A legal or finance team uploads documents and asks for summaries, risks, and extracted fields.

Key decisions:

- Chunk documents carefully to fit the context window.
- Use retrieval to select only relevant passages.
- Use structured outputs for extracted fields.
- Validate important fields against source text.
- Add human approval before final decisions.

### Scenario 5 - High-Traffic Public Chatbot

A public chatbot receives thousands of requests per minute.

Key decisions:

- Cache common answers.
- Use smaller models for simple requests.
- Apply rate limits and abuse detection.
- Stream responses for better perceived latency.
- Add provider fallback and circuit breakers.
- Track cost per endpoint and optimize expensive prompts.
