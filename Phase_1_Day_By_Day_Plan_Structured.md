# Phase 1 — Day-by-Day Plan
### LLM Fundamentals + Prompt Engineering + First Production RAG

**Duration:** 5 weeks / Weeks 3–7  
**Prerequisite:** Phase 0 + Typed API Starter Kit completed  
**Target effort:** ~28–34 hours/week  
**Primary outcome:** Build and ship **RegRAG**, a production-style regulatory/compliance RAG system with evaluation.

> **Study rule:** Build → Get Stuck → Learn → Improve → Repeat.
>
> Every concept should be attached to code you ship rather than learned only through videos.

---

## How to Use This Plan

Each day has four parts:

- **Learn:** exactly what concepts to study
- **Resources:** direct learning links
- **Build:** the hands-on implementation for that day
- **Done when:** the knowledge/output you should have before moving on

Do not start LangChain/LangGraph in Phase 1. The goal is to understand the fundamentals underneath RAG before using higher-level abstractions.

---

# Week 3 — Transformers, Embeddings & LLM APIs

### Weekly objective
Understand LLMs at a practical level and become comfortable calling model providers directly.

### Week 3 deliverable
`embedding_lab/` + `llm_cli/`

---

## Day 1 (Mon) — LLM Inference Flow

### Learn
- What is a token?
- Why LLMs process token IDs rather than raw text
- Tokenization
- Token IDs
- Embeddings
- Transformer layers
- Attention
- Next-token probabilities
- Autoregressive generation
- Context windows

### Resources
- [The Illustrated Transformer — Jay Alammar](https://jalammar.github.io/illustrated-transformer/)
- [Hugging Face NLP Course — Tokenizers & Transformers](https://huggingface.co/learn/nlp-course/)

### Build
Draw and explain this pipeline in your notes:

```text
Text
 ↓
Tokenization
 ↓
Token IDs
 ↓
Embeddings
 ↓
Transformer layers
 ↓
Attention
 ↓
Next-token probabilities
 ↓
Generated tokens
```

### Done when
You can explain the complete inference flow without looking at your notes.

---

## Day 2 (Tue) — Attention & Transformers

### Learn
- Query, Key, Value
- Self-attention
- Multi-head attention
- Positional information
- Why attention captures relationships between tokens
- Encoder vs decoder concepts
- Practical transformer flow

### Resources
- [The Illustrated Transformer — Attention](https://jalammar.github.io/illustrated-transformer/)
- [3Blue1Brown — Attention / Transformers](https://www.youtube.com/results?search_query=3Blue1Brown+attention+transformers)

### Build
Take a short sentence and manually describe:
1. What each token represents
2. What Query/Key/Value mean conceptually
3. Which tokens should attend to each other and why

### Done when
You can explain attention conceptually without relying on equations.

> **Do not spend hours memorizing transformer equations in Phase 1.**

---

## Day 3 (Wed) — Embeddings

### Learn
- Vector representations
- Semantic similarity
- Vector spaces
- Cosine similarity
- Embedding dimensions
- Embedding models
- Why semantic similarity can be represented geometrically

### Resources
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/)
- [Sentence Transformers Documentation](https://www.sbert.net/)

### Build
Create a small notebook/script with examples such as:

```text
"The firm's sales grew."
"The company increased its revenue."
"The weather is extremely cold."
"My dog likes playing outside."
```

Calculate similarity between the sentences.

### Done when
You can explain why embeddings are useful for retrieval.

---

## Day 4 (Thu) — Build an Embedding Experiment

### Learn
Review:
- Embedding generation
- Cosine similarity
- Semantic search
- Similarity ranking

### Resources
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/)
- [Sentence Transformers — Semantic Search](https://www.sbert.net/examples/applications/semantic-search/README.html)

### Build

Create:

```text
embedding_lab/
├── src/
│   └── embeddings.py
├── tests/
└── README.md
```

Use approximately 20 sentences.

Implement:

```text
query
 ↓
embedding
 ↓
cosine similarity
 ↓
ranked results
```

### Done when
You have built semantic similarity search yourself and can explain every step.

---

## Day 5 (Fri) — Tokens & Context Windows

### Learn
- Token ≠ word
- Input tokens
- Output tokens
- Context window
- Token usage
- Truncation
- Why long documents cannot simply be inserted into prompts

### Resources
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/)
- [OpenAI API Documentation](https://platform.openai.com/docs/)

### Build
Take a roughly 10-page document and estimate:

```text
Document
 ↓
Tokens
 ↓
Possible chunks
 ↓
Context usage
```

### Done when
You can explain why chunking is necessary for RAG.

---

## Day 6 (Sat) — OpenAI Python SDK

### Learn
- Python client
- Responses API
- Input
- Developer/system instructions
- Model selection
- Async client
- Streaming
- Usage/token information
- Error handling

### Resources
- [OpenAI Python SDK](https://github.com/openai/openai-python)
- [OpenAI API Documentation](https://platform.openai.com/docs/)

### Build
Make a direct API call **without LangChain**.

Implement:
- synchronous generation
- asynchronous generation
- streaming
- usage logging
- basic error handling

### Done when
You can call an LLM directly from Python and understand the request/response flow.

---

## Day 7 (Sun) — OpenAI Mini Project

### Learn
Review:
- SDK usage
- Streaming
- Token usage
- Async calls
- Error handling

### Resources
- [OpenAI Python SDK](https://github.com/openai/openai-python)
- [OpenAI API Documentation](https://platform.openai.com/docs/)

### Build

Create:

```text
llm_cli/
```

Run:

```bash
python -m llm_cli "Explain Delta Lake"
```

Pipeline:

```text
CLI
 ↓
OpenAI
 ↓
Streaming response
 ↓
Console
 ↓
Usage logging
```

### Deliverable
One clean Git commit with README instructions.

### Done when
You have a working CLI that streams an LLM response and reports usage.

---

## Week 3 Review Checklist

- [ ] I can explain tokens.
- [ ] I can explain context windows.
- [ ] I can explain attention.
- [ ] I can explain embeddings.
- [ ] I can calculate cosine similarity.
- [ ] I can call an LLM directly using the OpenAI SDK.
- [ ] I understand streaming.
- [ ] I understand basic token usage.
- [ ] I built a small LLM CLI.
- [ ] I have not used LangChain to hide the fundamentals.

---

# Week 4 — Prompt Engineering, SDK Abstraction, Chunking & Qdrant

### Weekly objective
Become comfortable with structured LLM interactions and build the first version of the Chunking Lab.

### Week 4 deliverable
`llm-playground/` + `chunking-lab/`

---

## Day 8 (Mon) — Prompt Engineering Fundamentals

### Learn
- System/developer instructions
- User instructions
- Zero-shot prompting
- Few-shot prompting
- Delimiters
- Role specification
- Output constraints

### Resources
- [OpenAI API Documentation](https://platform.openai.com/docs/)
- [OpenAI Cookbook](https://cookbook.openai.com/)

### Build
Take one extraction task and create:

```text
Prompt v1
Prompt v2
Prompt v3
```

Compare the outputs.

### Done when
You understand why prompt changes can affect reliability.

---

## Day 9 (Tue) — Structured Outputs

### Learn
- JSON output
- JSON Schema
- Structured outputs
- Schema validation
- Pydantic integration

### Resources
- [OpenAI Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)
- [Pydantic Models](https://docs.pydantic.dev/latest/concepts/models/)

### Build

Create:

```python
class Person(BaseModel):
    name: str
    age: int
    company: str
```

Make an LLM return data conforming to the schema.

### Done when
You can explain the difference between:

```text
"Please return JSON"
```

and

```text
"Return data conforming to this schema"
```

---

## Day 10 (Wed) — Prompt Failure Modes

### Learn
Experiment with:
- Ambiguous instructions
- Missing context
- Conflicting instructions
- Hallucination
- Output format failures
- Prompt injection concepts

### Resources
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [OpenAI Cookbook](https://cookbook.openai.com/)

### Build
Create weak and constrained versions of the same prompt.

Record:
- input
- prompt
- output
- failure type
- improved prompt
- result

### Done when
You can identify whether an LLM failure is caused by:
- poor instructions
- missing context
- model limitations
- retrieval problems

---

## Day 11 (Thu) — Few-Shot Prompting

### Learn
- Zero-shot vs few-shot
- Example selection
- Consistent input/output formatting
- Measuring whether examples help

### Resources
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)

### Build

Create a classification/extraction experiment:

```text
Input:
"Customer received damaged product"

Output:
{
  "category": "product_damage",
  "priority": "high"
}
```

Compare:

```text
Zero-shot
vs
Few-shot
```

### Done when
You have measured whether examples improve the task.

---

## Day 12 (Fri) — Anthropic SDK

### Learn
- Messages API
- Python client
- Async client
- Streaming
- Usage
- Structured output concepts
- Tool schema concepts

### Resources
- [Anthropic Python SDK](https://platform.claude.com/docs/en/cli-sdks-libraries/sdks/python)
- [Anthropic API Documentation](https://platform.claude.com/docs/)

### Build
Reproduce one OpenAI experiment using Anthropic.

### Done when
You understand the basic request/response and streaming flow of both providers.

---

## Day 13 (Sat) — OpenAI vs Anthropic Abstraction

### Learn
- Provider abstraction
- Interfaces/protocols
- Dependency inversion
- Keeping business logic provider-independent

### Resources
- [OpenAI Python SDK](https://github.com/openai/openai-python)
- [Anthropic Python SDK](https://platform.claude.com/docs/en/cli-sdks-libraries/sdks/python)
- [Python typing — Protocol](https://docs.python.org/3/library/typing.html#typing.Protocol)

### Build

Create:

```python
class LLMClient(Protocol):
    async def generate(...):
        ...
```

Implement:

```text
OpenAIClient
AnthropicClient
```

### Done when
Your application logic can work with either provider without knowing provider-specific details.

---

## Day 14 (Sun) — LLM Playground

### Learn
Integration day — no major new theory.

### Resources
- [OpenAI Python SDK](https://github.com/openai/openai-python)
- [Anthropic Python SDK](https://platform.claude.com/docs/en/cli-sdks-libraries/sdks/python)

### Build

Create:

```text
llm-playground/
```

Features:

```text
OpenAI
Anthropic
├── normal generation
├── streaming
├── structured output
├── token usage
└── error handling
```

Write a README answering:

> When would I use OpenAI vs Anthropic?

### Done when
You have one clean playground that demonstrates both providers.

---

## Day 15 (Mon) — Why Chunking Exists

### Learn
- Fixed-size chunking
- Token-based chunking
- Character-based chunking
- Overlap
- Recursive splitting
- Semantic chunking
- Structure-aware chunking

### Resources
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/)
- [Qdrant Documentation](https://qdrant.tech/documentation/)

### Build

Understand:

```text
Large document
 ↓
Chunks
 ↓
Embeddings
 ↓
Vector database
```

### Done when
You can explain why chunking directly affects retrieval quality.

---

## Day 16 (Tue) — Fixed-Size Chunking

### Learn
- Chunk size
- Overlap
- Context preservation
- Retrieval trade-offs

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/)

### Build

Input:

```text
document.txt
```

Output:

```json
[
  {
    "chunk_id": 1,
    "text": "...",
    "start": 0,
    "end": 500
  }
]
```

Experiment with:

```text
chunk_size = 256
chunk_size = 512
chunk_size = 1024
```

and:

```text
overlap = 0
overlap = 50
overlap = 100
```

### Done when
You understand the trade-off between chunk size, overlap, context and retrieval quality.

---

## Day 17 (Wed) — Recursive Chunking

### Learn
- Recursive splitting
- Preserving natural boundaries
- Paragraph → sentence → word fallback strategy

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)

### Build
Implement recursive splitting based on:

```text
Paragraph
 ↓
Sentence
 ↓
Word
```

### Done when
You can explain why semantic boundaries are often better than blindly cutting every N characters.

---

## Day 18 (Thu) — Structure-Aware Chunking

### Learn
Design chunks that preserve:
- document title
- section
- subsection
- page
- table context
- cross-reference context

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)

### Build

Use a regulatory-style example:

```text
Document: RBI Circular 2025
Section: Capital Requirements
Subsection: Risk Weights

[actual text]
```

### Done when
Your chunk representation retains metadata needed to understand where the text came from.

---

## Day 19 (Fri) — Qdrant Fundamentals

### Learn
- Collection
- Point
- Vector
- Payload
- Distance metric
- Search
- Filtering

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Qdrant Quickstart](https://qdrant.tech/documentation/quickstart/)

### Build
Run Qdrant locally with Docker.

Create a collection and:
1. insert vectors
2. attach payloads
3. search vectors
4. filter results

### Done when
You can create a collection and insert/search vectors independently.

---

## Day 20 (Sat) — Semantic Search

### Learn
- Query embedding
- Vector similarity search
- Top-K retrieval
- Similarity scores

### Resources
- [Qdrant Semantic Search](https://qdrant.tech/documentation/tutorials/search/)

### Build

Implement:

```text
Query
 ↓
Embedding
 ↓
Qdrant
 ↓
Top-K chunks
```

Example:

```text
1. chunk_73  similarity=0.87
2. chunk_12  similarity=0.82
3. chunk_91  similarity=0.79
```

### Done when
You can build semantic search independently of an LLM generation layer.

---

## Day 21 (Sun) — Chunking Lab v1

### Learn
Integration day.

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Hugging Face NLP Course](https://huggingface.co/learn/nlp-course/)

### Build

Combine:

```text
Document
 ↓
Chunker
 ├── fixed
 ├── recursive
 ├── semantic
 └── structure-aware
       ↓
    embeddings
       ↓
     Qdrant
       ↓
     search
```

### Deliverable
**Chunking Lab v1**

### Done when
You can run the same document through multiple chunking strategies and retrieve results from Qdrant.

---

## Week 4 Review Checklist

- [ ] I can explain zero-shot vs few-shot prompting.
- [ ] I can design structured outputs.
- [ ] I understand common prompt failure modes.
- [ ] I can call OpenAI and Anthropic directly.
- [ ] I have an LLM provider abstraction.
- [ ] I understand fixed-size chunking.
- [ ] I understand recursive chunking.
- [ ] I understand structure-aware chunking.
- [ ] I can operate Qdrant locally.
- [ ] I can perform vector similarity search.
- [ ] Chunking Lab v1 is working.

---

# Week 5 — Retrieval Evaluation + Basic RAG

### Weekly objective
Move from "retrieval works" to "I can measure retrieval quality."

### Week 5 deliverable
**Chunking Lab v2 with retrieval evaluation report**

---

## Day 22 (Mon) — Retrieval Metrics

### Learn
- Recall@K
- Relevant vs retrieved chunks
- Why retrieval needs objective evaluation

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [RAGAS Documentation](https://docs.ragas.io/)

### Build

If the correct chunk is:

```text
chunk_73
```

and top-5 results are:

```text
12
31
73
91
104
```

then:

```text
Recall@5 = 1
```

Implement Recall@K yourself before using RAGAS.

### Done when
You can calculate Recall@K manually and programmatically.

---

## Day 23 (Tue) — Golden Dataset

### Learn
- What a golden evaluation dataset is
- Ground-truth relevant chunks
- Question design
- Retrieval evaluation data

### Resources
- [RAGAS Documentation](https://docs.ragas.io/)

### Build

Create:

```json
[
  {
    "question": "...",
    "relevant_chunk_ids": ["chunk_73"]
  }
]
```

Start with approximately:

```text
30 questions
```

Plan to grow the final RegRAG dataset to:

```text
50–100 questions
```

### Done when
You have a reproducible evaluation dataset rather than judging retrieval by intuition.

---

## Day 24 (Wed) — Compare Chunking Strategies

### Learn
Review:
- fixed chunking
- recursive chunking
- structure-aware chunking
- Recall@K

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [RAGAS Documentation](https://docs.ragas.io/)

### Build

Run retrieval experiments.

Report:

```text
Strategy              Recall@5
--------------------------------
Fixed 256               X%
Fixed 512               X%
Recursive               X%
Structure-aware         X%
```

Use your actual measurements.

### Done when
You can identify which strategy works best for your corpus and explain why.

---

## Day 25 (Thu) — Build Basic RAG

### Learn
Understand the complete pipeline:

```text
Question
 ↓
Embedding
 ↓
Qdrant
 ↓
Top-K chunks
 ↓
Prompt
 ↓
LLM
 ↓
Answer
```

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [OpenAI API Documentation](https://platform.openai.com/docs/)

### Build
Implement the pipeline manually.

Do **not** introduce a framework yet.

### Done when
You understand every component of the RAG pipeline.

---

## Day 26 (Fri) — Grounding & Citations

### Learn
- Context grounding
- Source attribution
- "I don't know" behavior
- Page/chunk-level citations
- Separating context from instructions

### Resources
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [OpenAI Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)

### Build

Use a prompt structure like:

```text
CONTEXT
-------
Retrieved content

QUESTION
--------
User question

RULES
-----
Answer only using the supplied context.
If the answer is not present, say you don't know.
```

Return:

```json
{
  "answer": "...",
  "sources": [
    {
      "document": "...",
      "page": 12,
      "chunk_id": "abc"
    }
  ]
}
```

### Done when
Every generated answer can be traced back to retrieved source material.

---

## Day 27 (Sat) — Build the RAG API

### Learn
Review:
- FastAPI architecture from Phase 0
- retrieval/generation separation
- service boundaries

### Resources
- [FastAPI Tutorial](https://fastapi.tiangolo.com/tutorial/)
- [FastAPI Bigger Applications](https://fastapi.tiangolo.com/tutorial/bigger-applications/)
- [Qdrant Documentation](https://qdrant.tech/documentation/)

### Build

Reuse your Phase 0 starter kit.

Architecture:

```text
FastAPI
 ├── Qdrant
 ├── Postgres
 ├── LLM Provider
 ├── Tests
 ├── Docker
 └── CI
```

Suggested endpoint:

```text
POST /retrieve
```

Input:

```json
{
  "query": "..."
}
```

Output:

```json
{
  "results": [...]
}
```

### Done when
Retrieval is exposed as a clean backend service and is independent from generation.

---

## Day 28 (Sun) — Hybrid Search

### Learn
- Dense retrieval
- Lexical retrieval
- BM25-style search
- Hybrid retrieval
- Why exact identifiers may benefit from lexical search
- Why semantic queries benefit from dense retrieval

### Resources
- [Qdrant Hybrid Search](https://qdrant.tech/documentation/tutorials/hybrid-search/)
- [Qdrant Documentation](https://qdrant.tech/documentation/)

### Build

Implement:

```text
Dense retrieval
       +
Lexical/BM25 retrieval
       ↓
Hybrid retrieval
```

### Done when
You can explain why dense retrieval alone can fail and have a first hybrid retrieval implementation.

---

## Week 5 Review Checklist

- [ ] I can explain Recall@K.
- [ ] I built a golden dataset.
- [ ] I measured multiple chunking strategies.
- [ ] I built basic RAG without a framework.
- [ ] I can return source citations.
- [ ] I have separate retrieval and generation stages.
- [ ] I understand hybrid search.
- [ ] I can explain why dense retrieval alone can fail.

---

# Week 6 — RegRAG: Ingestion + Retrieval + FastAPI

### Weekly objective
Start the flagship production-style project.

### Project direction
Build a regulatory/compliance document assistant, **not a generic PDF chatbot**.

---

## Day 29 (Mon) — Select the Corpus

### Learn
Choose one regulatory domain initially.

Suggested directions from the roadmap:
- RBI circulars
- SEBI circulars
- Regulatory notices
- Annual reports

### Resources
- [SEBI — Circulars](https://www.sebi.gov.in/sebiweb/home/HomeAction.do?doListing=yes&sid=1&ssid=7)
- [SEBI — Master Circulars](https://www.sebi.gov.in/sebiweb/home/HomeAction.do?doListing=yes&sid=1&ssid=6)
- [Reserve Bank of India](https://www.rbi.org.in/)

### Build
Select:

```text
1 domain
20–50 high-quality documents
```

Write a one-page problem statement:
- who uses RegRAG
- what questions it answers
- what documents it covers
- what it explicitly does not answer

### Done when
You have a clearly defined corpus and problem statement.

---

## Day 30 (Tue) — Build the Ingestion Pipeline

### Learn
Understand:

```text
Raw Documents
     ↓
Parser
     ↓
Cleaner
     ↓
Metadata Extractor
     ↓
Chunker
     ↓
Embedding
     ↓
Qdrant
```

### Resources
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [Qdrant Quickstart](https://qdrant.tech/documentation/quickstart/)

### Build
Preserve metadata:

```json
{
  "document_id": "...",
  "title": "...",
  "page": 14,
  "section": "...",
  "publication_date": "...",
  "chunk_id": "..."
}
```

### Done when
You can run ingestion repeatedly and get reproducible results.

---

## Day 31 (Wed) — Retrieval Service

### Learn
- Dense retrieval
- Lexical retrieval
- Fusion
- Top-K selection
- Retrieval service boundaries

### Resources
- [Qdrant Hybrid Search](https://qdrant.tech/documentation/tutorials/hybrid-search/)
- [FastAPI Tutorial](https://fastapi.tiangolo.com/tutorial/)

### Build

```text
POST /retrieve

Query
 ↓
Embedding
 ↓
Dense retrieval
 ↓
Lexical retrieval
 ↓
Fusion
 ↓
Top-K results
```

### Done when
You can test retrieval independently from generation.

---

## Day 32 (Thu) — Generation Service

### Learn
- Context assembly
- Grounded generation
- Structured responses
- Citations
- Explicit refusal/"I don't know" behavior

### Resources
- [OpenAI API Documentation](https://platform.openai.com/docs/)
- [OpenAI Structured Outputs](https://platform.openai.com/docs/guides/structured-outputs)

### Build

```text
POST /ask

Question
 ↓
Retrieve
 ↓
Construct context
 ↓
LLM
 ↓
Structured response
 ↓
Citations
```

### Done when
RegRAG works end-to-end and refuses to invent answers when evidence is unavailable.

---

## Day 33 (Fri) — RAG Evaluation with RAGAS

### Learn
- Faithfulness
- Answer relevance
- Context relevance
- Retrieval quality
- LLM-as-judge evaluation
- Retrieval quality vs generation quality

### Resources
- [RAGAS Documentation](https://docs.ragas.io/)

### Build
Evaluate RegRAG using:
1. your custom retrieval metrics
2. RAGAS metrics

Keep the results in a versioned evaluation report.

### Done when
You can distinguish:

```text
Retrieval quality
```

from:

```text
Generation quality
```

---

## Day 34 (Sat) — Production Hardening

### Learn
Review Phase 0 engineering practices:
- Docker Compose
- pytest
- Ruff
- mypy
- CI
- structured logging
- configuration
- error handling
- health checks

### Resources
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [pytest Documentation](https://docs.pytest.org/en/stable/)
- [Ruff Documentation](https://docs.astral.sh/ruff/)
- [GitHub Actions — Python](https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-python)

### Build
Make RegRAG behave like:

> a backend service containing an AI subsystem — not a notebook containing an LLM call.

### Done when
The service is containerized, tested, linted, type-checked and CI-ready.

---

## Day 35 (Sun) — Architecture, README & Demo

### Learn
Integration/polish day.

### Resources
- [Mermaid Documentation](https://mermaid.js.org/)
- [FastAPI Documentation](https://fastapi.tiangolo.com/)
- [Qdrant Documentation](https://qdrant.tech/documentation/)
- [RAGAS Documentation](https://docs.ragas.io/)

### Build

Create an architecture diagram:

```text
                    ┌───────────────┐
                    │    Client     │
                    └───────┬───────┘
                            │
                            ▼
                     ┌─────────────┐
                     │   FastAPI   │
                     └──────┬──────┘
                            │
               ┌────────────┴────────────┐
               ▼                         ▼
         ┌───────────┐             ┌──────────┐
         │ Retrieval │             │ Postgres │
         └─────┬─────┘             └──────────┘
               │
               ▼
         ┌───────────┐
         │  Qdrant   │
         └─────┬─────┘
               │
               ▼
         ┌───────────┐
         │  Context  │
         └─────┬─────┘
               │
               ▼
         ┌───────────┐
         │    LLM    │
         └─────┬─────┘
               │
               ▼
        Answer + Citations
```

README sections:

1. Problem
2. Corpus
3. Architecture
4. Ingestion pipeline
5. Chunking strategy
6. Retrieval architecture
7. Why Qdrant?
8. Why hybrid retrieval?
9. Generation strategy
10. Citation/grounding approach
11. Evaluation methodology
12. Results
13. Latency
14. Cost
15. Failure modes
16. Trade-offs
17. Future improvements

### Done when
A stranger can understand the project in approximately five minutes.

---

# Phase 1 Final Review

## LLM Fundamentals

- [ ] What is a token?
- [ ] What is an embedding?
- [ ] What is attention?
- [ ] What does a context window represent?
- [ ] How does autoregressive generation work?
- [ ] Why are embeddings useful for retrieval?

## Prompt Engineering

- [ ] Zero-shot vs few-shot?
- [ ] What makes a system/developer instruction effective?
- [ ] What causes hallucination?
- [ ] How do structured outputs work?
- [ ] Why is schema validation useful?

## Retrieval

- [ ] What is cosine similarity?
- [ ] What is chunking?
- [ ] Why does chunk size matter?
- [ ] What is chunk overlap?
- [ ] When would structure-aware chunking help?
- [ ] What is dense retrieval?
- [ ] What is lexical/BM25 retrieval?
- [ ] Why use hybrid retrieval?
- [ ] What is Recall@K?

## RAG

- [ ] Explain RAG end-to-end.
- [ ] What happens during ingestion?
- [ ] What happens at query time?
- [ ] Why can RAG hallucinate?
- [ ] How can you distinguish retrieval failure from generation failure?
- [ ] How do you ground an answer?
- [ ] How do citations work?

## Production Engineering

- [ ] Why Qdrant?
- [ ] What belongs in Postgres?
- [ ] How do you test retrieval?
- [ ] How do you evaluate generated answers?
- [ ] How do you handle "I don't know"?
- [ ] How do you containerize the service?
- [ ] How do you deploy it?
- [ ] How do you measure quality?

---

# Phase 1 Deliverables

By the end of the phase you should have:

### 1. Embedding Lab

```text
embedding_lab/
```

Demonstrates:
- embeddings
- cosine similarity
- semantic search

### 2. LLM Playground

```text
llm-playground/
```

Demonstrates:
- OpenAI
- Anthropic
- streaming
- structured output
- token usage
- provider abstraction

### 3. Chunking Lab

```text
chunking-lab/
```

Demonstrates:
- fixed-size chunking
- recursive chunking
- semantic/structure-aware chunking
- Qdrant retrieval
- Recall@K
- chunking comparison report

### 4. RegRAG

```text
regrag/
```

Demonstrates:
- production-style RAG
- FastAPI
- Qdrant
- PostgreSQL
- hybrid retrieval
- grounding
- citations
- confidence/refusal logic
- evaluation
- Docker
- tests
- CI
- deployment

---

# What NOT to Learn in Phase 1

Do **not** divert the schedule into:

- LangChain
- LangGraph
- Multi-agent systems
- MCP
- Fine-tuning
- LoRA/QLoRA
- vLLM
- Kubernetes
- Advanced OpenTelemetry
- Deep transformer mathematics
- Research-level deep learning

These belong to later phases.

The purpose of Phase 1 is to establish a strong understanding of:

```text
LLMs
  +
Embeddings
  +
Retrieval
  +
RAG
```

before introducing higher-level agent and infrastructure abstractions.

---

# Phase 1 Definition of Done

Phase 1 is complete when:

- [ ] I understand transformers at a practical level.
- [ ] I understand embeddings and cosine similarity.
- [ ] I can use OpenAI and Anthropic SDKs directly.
- [ ] I understand streaming and structured outputs.
- [ ] I understand and can implement multiple chunking strategies.
- [ ] I can use Qdrant for vector retrieval.
- [ ] I can measure Recall@K.
- [ ] I have a golden evaluation dataset.
- [ ] I understand dense vs lexical retrieval.
- [ ] I have implemented hybrid retrieval.
- [ ] I can explain RAG end-to-end without a framework.
- [ ] RegRAG has FastAPI + Qdrant + PostgreSQL.
- [ ] RegRAG provides grounded answers with citations.
- [ ] RegRAG has refusal/"I don't know" behavior.
- [ ] RegRAG has automated evaluation.
- [ ] RegRAG has tests.
- [ ] RegRAG has Docker Compose.
- [ ] RegRAG has CI.
- [ ] RegRAG is deployed or deployment-ready.
- [ ] RegRAG has a clear architecture diagram.
- [ ] RegRAG has a strong README.
- [ ] I can defend the major architectural decisions in an interview.

---

# Phase 1 → Phase 2 Handoff

You should enter Phase 2 with:

```text
                   Phase 1
                      │
                      ▼
                 ┌───────────┐
                 │   RegRAG  │
                 └─────┬─────┘
                       │
          ┌────────────┼────────────┐
          ▼            ▼            ▼
       Qdrant      Evaluation     FastAPI
          │            │            │
          ▼            ▼            ▼
      Retrieval    Golden DS    Production
          │
          ▼
     Hybrid Search
```

Phase 2 then extends this into:
- Query rewriting
- HyDE
- Advanced retrieval
- Re-ranking
- Vector DB internals
- Multimodal/structured RAG
- Semantic caching
- Advanced evaluation
- Eval-gated CI

The Phase 1 RegRAG system should therefore be designed for extension rather than discarded after Week 7.

---

# The Phase 1 Transformation

The most important transformation is:

```text
"I can call GPT"
        ↓
"I understand how LLM applications work"
        ↓
"I can build retrieval myself"
        ↓
"I can measure retrieval quality"
        ↓
"I can build RAG"
        ↓
"I can explain why my RAG works or fails"
        ↓
"I can ship it as a tested backend service"
```

That is the standard to aim for before moving to:

**Phase 2 — Advanced RAG + Vector DB Mastery + Evaluation**
