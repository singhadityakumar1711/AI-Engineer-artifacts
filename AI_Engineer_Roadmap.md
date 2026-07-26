# AI Engineer Transition Roadmap
### From Data Engineer 2 (Tredence) → AI/ML Engineer at a Top Product Company
**Timeline:** 8 months | **Weekly Budget:** ~10-12 hrs weekday + 6-8 hrs weekend = ~28-34 hrs/week

---

## 0. Operating Principles

- **Build → Get Stuck → Learn → Improve → Repeat.** Every concept below is attached to code you ship, not a video you watch.
- **One flagship repo family.** Projects are designed to be *composable* — later projects reuse and extend earlier ones (e.g., your RAG system becomes a tool your agent calls). This is what makes a portfolio look like an "engineer" and not a "tutorial-follower."
- **Every project ships with:** README with architecture diagram, Docker Compose, tests, at least one eval script, and (where feasible) a live demo (Render/Fly.io/HF Spaces — free tiers are fine).
- **Public build log.** Push commits weekly, even messy ones. Recruiters and hiring managers look at commit history and READMEs, not just the final polish.

---

## 1. Phase Overview

| Phase | Focus | Duration | Weeks |
|---|---|---|---|
| 0 | Foundations Refresh (Python for AI, APIs, Docker, Git hygiene) | 2 weeks | 1-2 |
| 1 | LLM Fundamentals + Prompt Engineering + First Production RAG | 5 weeks | 3-7 |
| 2 | Advanced RAG + Vector DB Mastery + Evaluation | 5 weeks | 8-12 |
| 3 | Tool Calling + AI Agents (LangGraph) | 6 weeks | 13-18 |
| 4 | Multi-Agent Systems + MCP | 5 weeks | 19-23 |
| 5 | AI Infra, Deployment, Scaling, Observability (AIOps) | 5 weeks | 24-28 |
| 6 | Fine-tuning + Specialization + Portfolio Polish | 4 weeks | 29-32 |
| 7 | Interview Prep Intensive + Applying (overlaps from Week 24 onward) | ongoing | 24-34 |

Total: ~34 weeks (~8 months), with buffer built in.

---

## PHASE 0 — Foundations Refresh
**Weeks 1-2**

### Objective
Get your engineering environment and Python muscle memory production-grade before touching LLMs. You already know SQL/Databricks/BigQuery — this phase converts that into "software engineer who ships," not "analyst who scripts."

### Topics
- Modern Python: type hints, `pydantic` v2, `async`/`await`, context managers, project structuring with `uv` or `poetry`
- REST API design fundamentals + FastAPI basics
- Docker fundamentals: images, multi-stage builds, docker-compose
- Git workflow: conventional commits, PRs, branching, pre-commit hooks
- Testing basics: `pytest`, fixtures, mocking external APIs

### Why These Matter
Every AI system you build going forward is "a Python backend that happens to call an LLM." If your FastAPI, Docker, and testing hygiene are weak, every later project looks like a notebook, not a product — this is the #1 signal that separates hobbyists from hires.

### Mini Project
**"Typed API Starter Kit"** — A FastAPI template repo with pydantic settings management, structured logging, pytest scaffolding, Dockerfile, docker-compose (with Postgres + Redis), pre-commit hooks, and a GitHub Actions CI pipeline that lints + tests on every push. You will literally reuse this template for every project below.

### Bigger Project
None yet — this phase is infrastructure-only.

### Stretch
Add OpenTelemetry tracing scaffolding to the starter kit (you'll thank yourself in Phase 5).

### Common Mistakes
- Skipping this phase because "I already know Python" — the gap is usually in *typed, tested, containerized* Python, not syntax.
- Building projects without CI from day one, then bolting it on later (painful).

### Interview Relevance
FastAPI + Docker + testing questions are now standard in AI Engineer interviews at product companies (not just backend roles).

### Industry Relevance
This is literally the substrate of every AI product team's stack in 2026.

### Duration
2 weeks (10 weekday hrs + 14 weekend hrs = ~24 hrs each week)

---

## PHASE 1 — LLM Fundamentals + Prompt Engineering + First Production RAG
**Weeks 3-7**

### Objective
Understand how LLMs actually work (enough to reason about failure modes, not train one from scratch), get fluent with both OpenAI and Anthropic SDKs, and ship your first RAG system with real evaluation — not a toy PDF-chat.

### Topics
- Transformer architecture at a *practical* level: tokens, context windows, attention (conceptual, not from-scratch math unless you want the stretch)
- Embeddings: what they are, cosine similarity, chunking strategies
- Prompt engineering: few-shot, chain-of-thought, structured outputs (JSON mode / tool schemas), system prompt design
- RAG architecture: ingestion → chunking → embedding → retrieval → re-ranking → generation
- Vector DB basics: Qdrant or Chroma (pick one to go deep on first)
- LLM APIs: OpenAI SDK, Anthropic SDK, streaming responses

### Why These Matter
Most "AI Engineer" job descriptions are really "backend engineer who deeply understands retrieval and prompting." Interviewers will test whether you know *why* a RAG pipeline hallucinates or retrieves garbage — that requires understanding embeddings and chunking, not just LangChain syntax.

### Mini Project
**"Chunking Lab"** — A CLI/notebook tool that ingests a messy real-world document set (e.g., a company's public 10-K filings or GitHub docs) and lets you compare 4 chunking strategies (fixed-size, recursive, semantic, structure-aware) against retrieval quality metrics (recall@k). This becomes a reusable evaluation harness.

### Bigger Project
**"RegRAG — Regulatory/Compliance Document Assistant"** (NOT a PDF Q&A toy): A production RAG system over a real, messy, high-stakes corpus (e.g., SEC filings, GDPR/HIPAA text, or Indian RBI/SEBI circulars — pick a domain with real complexity: cross-references, tables, versioned documents).
- FastAPI backend, Qdrant for vectors, Postgres for metadata/audit trail
- Hybrid search (BM25 + dense vectors)
- Source citation with page-level grounding
- Answer confidence scoring + "I don't know" refusal logic
- Eval suite: a golden Q&A dataset (50-100 questions) scored with RAGAS or a custom LLM-judge rubric for faithfulness/relevance
- Dockerized, CI-tested, deployed with a live demo

### Stretch Project
Add a re-ranker (Cohere rerank or a cross-encoder from Hugging Face) and A/B the retrieval quality with/without it in your eval report — this "before/after eval numbers" habit is exactly what interviewers want to hear about.

### Common Mistakes
- Building RAG with no eval — "it feels like it works" is not a portfolio claim.
- Ignoring chunking quality and blaming the LLM for hallucinations.
- Using LangChain for everything instead of understanding what it abstracts (interviewers will ask you to explain retrieval without the framework).

### Interview Relevance
RAG system design is now a standard "AI system design" interview question at nearly every company on your target list.

### Industry Relevance
RAG is still the dominant production pattern for grounding LLMs in proprietary data — this is the single highest-ROI skill you can build first.

### Duration
5 weeks

---

## PHASE 2 — Advanced RAG + Vector DB Mastery + Evaluation
**Weeks 8-12**

### Objective
Move from "RAG works" to "RAG is measurably good and I can defend every architectural choice with numbers."

### Topics
- Advanced retrieval: hybrid search, query rewriting/expansion, HyDE, multi-hop retrieval
- Vector DB internals: HNSW indexing, filtering, metadata search, comparing Qdrant/Milvus/FAISS trade-offs (latency, scale, hosted vs self-managed)
- Structured + semi-structured RAG: tables, images (multimodal RAG), code
- Evaluation frameworks: RAGAS, DeepEval, or building your own LLM-as-judge pipeline; offline eval vs online eval (user feedback loops)
- Caching strategies for LLM calls (semantic caching with Redis)

### Why These Matter
This is where you differentiate from the flood of "I built a RAG chatbot" candidates. Companies want engineers who can say "recall@5 went from 61% to 84% after adding query rewriting, here's the eval harness."

### Mini Project
**"Retrieval A/B Test Harness"** — extend your Phase 1 eval suite into a general-purpose tool: given a corpus + golden dataset, it benchmarks N retrieval strategies and outputs a comparison report (latency, cost, recall, faithfulness).

### Bigger Project
**"Multimodal Financial Research Assistant"** — RAG over a corpus that includes tables, charts, and text (e.g., annual reports with financial tables/images). 
- Table-aware chunking + structured extraction (numbers stay queryable, not flattened to text)
- Multimodal embeddings or a vision-model-assisted extraction step
- Semantic caching layer (Redis) to cut LLM cost on repeated queries
- Full eval dashboard (Streamlit or a simple React page) showing retrieval + generation metrics over time
- CI runs your eval suite on every PR and fails the build if faithfulness drops below a threshold — this "eval-gated CI" idea is a standout portfolio signal

### Stretch Project
Self-host an open-source embedding model (via Hugging Face + `sentence-transformers` or Ollama) and compare cost/quality vs OpenAI/Voyage embeddings in your eval report.

### Common Mistakes
- Treating vector DB choice as a religion instead of a trade-off (know when Postgres+pgvector is *enough* vs when you need Qdrant/Milvus).
- Over-engineering retrieval before you have a working baseline and eval numbers.

### Interview Relevance
"How would you evaluate a RAG system in production?" is now asked at nearly every AI-product interview loop — you'll have a real answer with a real repo to point to.

### Industry Relevance
Eval-driven development is the biggest maturity gap between hobby AI projects and real AI product teams in 2026.

### Duration
5 weeks

---

## PHASE 3 — Tool Calling + AI Agents (LangGraph)
**Weeks 13-18**

### Objective
Move from "answering questions" to "taking actions." This is the core of what "AI Engineer" roles actually pay for now.

### Topics
- Tool/function calling fundamentals (OpenAI + Anthropic tool-use APIs)
- Agent design patterns: ReAct, plan-and-execute, reflection loops
- LangGraph: graphs, state, checkpointing, human-in-the-loop interrupts
- Memory: short-term (conversation state) vs long-term (vector-backed memory)
- Guardrails: input/output validation, structured output enforcement, rate limiting, cost controls

### Why These Matter
Nearly every "AI Engineer" job posting at OpenAI/Anthropic/Google/Meta-adjacent product teams in 2026 references agents and tool-use, not chatbots. This is the highest-leverage phase for interview relevance.

### Mini Project
**"Tool-Calling Sandbox"** — a small FastAPI service exposing 4-5 real tools (a calculator, a live data API, your Phase 1/2 RAG system as a retrieval tool, a code execution sandbox) with an agent loop that decides which tool to call, with full request/response tracing logged to Postgres.

### Bigger Project
**"Ops Incident Copilot"** (NOT a chatbot skin): An agent that helps engineers triage production incidents.
- Given an alert (simulated: a log spike, an error trace, a metric anomaly), the agent:
  1. Retrieves relevant runbooks (reuses your RAG system)
  2. Queries a mock metrics/logs API (tool call)
  3. Proposes a root-cause hypothesis with confidence + evidence
  4. Drafts a remediation plan, but requires human approval before "executing" (simulated action) — human-in-the-loop via LangGraph interrupts
- Built with LangGraph for the state machine, FastAPI for the service layer, Postgres for incident history, full tracing
- Includes a "chaos test suite" — inject bad/missing data and verify the agent degrades gracefully instead of hallucinating a fix

### Stretch Project
Add a self-critique/reflection loop where the agent scores its own proposed fix against a rubric before presenting it, and re-plans if the score is low.

### Common Mistakes
- Building agents with unbounded loops and no cost/step limits (a classic production incident of its own).
- No human-in-the-loop checkpoint for consequential actions — interviewers will probe this directly.
- Treating "agent" as synonymous with "more LangChain chains" rather than an explicit state machine you can reason about.

### Interview Relevance
Expect live-coding or system-design questions like "design an agent that can safely take actions in production" — this project *is* your answer.

### Industry Relevance
This "agent with guardrails and human approval" pattern is exactly what's shipping in real internal tools at most AI-forward companies right now.

### Duration
6 weeks

---

## PHASE 4 — Multi-Agent Systems + MCP
**Weeks 19-23**

### Objective
Understand when multi-agent decomposition actually helps (and when it's overkill), and get hands-on with MCP — increasingly a baseline expectation, not a nice-to-have.

### Topics
- Multi-agent patterns: supervisor/worker, debate, hierarchical delegation
- LangGraph multi-agent graphs, shared vs isolated state
- Model Context Protocol (MCP): building an MCP server, exposing tools/resources, connecting clients
- Cost/latency trade-offs of multi-agent vs single-agent-with-more-tools (this is an interview favorite — know when NOT to use multi-agent)

### Why These Matter
MCP has rapidly become a de facto standard for tool/context interoperability across the industry (Anthropic, OpenAI, and others have converged on it). Being able to say "I built and shipped an MCP server" is a strong, current signal.

### Mini Project
**"MCP Server for Your Own Stack"** — expose your Phase 1-3 projects (RAG retrieval, incident copilot tools) as a single MCP server that can be plugged into Claude Desktop or any MCP client. Document it well — this is a great "look what I built" demo for interviews.

### Bigger Project
**"Multi-Agent Data Pipeline Debugger"** (leans on your existing data engineering background — a strong differentiator):
- A supervisor agent receives "this dbt/Airflow pipeline failed"
- Delegates to specialized sub-agents: a Log Analyst agent, a Schema/Data-Quality agent, a Historical-Incident agent (RAG over past postmortems)
- Supervisor synthesizes sub-agent findings into a single root-cause report with recommended fix
- Exposed via an MCP server so it can be invoked from any MCP-compatible client, plus a FastAPI/React dashboard for direct use
- Full evaluation: a benchmark set of synthetic pipeline failures, measuring root-cause accuracy vs a single-agent baseline (so you can honestly report whether multi-agent even helped)

### Stretch Project
Add inter-agent debate (two agents propose competing root causes, a judge agent adjudicates) and measure whether it improves accuracy or just adds latency/cost — report the honest result either way.

### Common Mistakes
- Reaching for multi-agent because it's trendy, not because a single well-tooled agent was insufficient (interviewers actively probe for this judgment).
- Building an MCP server with no auth/permission model — even for a demo, show you've thought about it.

### Interview Relevance
"When would you NOT use multi-agent architecture" is becoming a common senior-track question — your honest benchmark numbers are the perfect answer.

### Industry Relevance
Combining your existing data-engineering background (Airflow/dbt/Databricks) with agents is a genuinely differentiated portfolio angle few candidates have.

### Duration
5 weeks

---

## PHASE 5 — AI Infra, Deployment, Scaling, Observability (AIOps)
**Weeks 24-28**

### Objective
Prove you can run this stuff in production, not just on localhost — this is the phase that convinces a hiring committee you're "engineer," not "prompt tinkerer."

### Topics
- LLM gateway/routing: LiteLLM for multi-provider routing, fallback, retries
- Serving open models: Ollama for local dev, vLLM for higher-throughput self-hosted serving
- Observability: structured tracing (OpenTelemetry), LLM-specific observability (cost per request, token usage, latency percentiles, eval scores over time)
- Scaling: async request handling, queueing (Redis/Celery or a simple task queue), rate limiting, caching
- Basic Kubernetes: pods, deployments, services — enough to deploy one project on a managed K8s (GKE/EKS) or explain it fluently, not become a K8s expert
- Cost engineering: token budgets, model routing by task complexity (cheap model for easy tasks, strong model for hard ones)

### Why These Matter
"AIOps" and cost/latency awareness are what separates a mid-level from senior AI engineer conversation. Companies increasingly interview specifically for "can this candidate keep our LLM bill and latency under control at scale."

### Mini Project
**"LLM Gateway"** — a LiteLLM-based routing service in front of all your previous projects: automatic fallback (Anthropic → OpenAI → local Ollama model) on failure, cost tracking per request logged to Postgres, and a Grafana/simple dashboard of spend and latency.

### Bigger Project
**"Production Hardening of Your Flagship Project"** — take your Phase 3 or Phase 4 project and take it from "demo" to "production":
- Put it behind the LLM Gateway (automatic model fallback + cost tracking)
- Add full OpenTelemetry tracing across the agent's tool calls
- Add a monitoring dashboard: cost/day, p50/p95 latency, eval score trend, error rate
- Load test it (Locust or k6) and document the results (throughput, bottlenecks found, fixes made)
- Deploy on Kubernetes (even a small managed cluster) with health checks and horizontal scaling config
- Write a postmortem-style README: "what broke under load, what I fixed" — this narrative is gold in interviews

### Stretch Project
Self-host a smaller open-weight model with vLLM for one non-critical path (e.g., query classification/routing) and benchmark cost savings vs always calling a frontier model.

### Common Mistakes
- Never load-testing anything, so you have no real story about scale when asked in interviews.
- Ignoring cost entirely — "it worked" without a cost number is a red flag to AI product teams who live and die by unit economics.

### Interview Relevance
"How would you control cost/latency of an LLM system at scale" is now a standard senior AI-engineer system design question.

### Industry Relevance
This is precisely the skill gap between "can build a demo" and "can be trusted with a production AI system" that hiring managers are filtering for.

### Duration
5 weeks

---

## PHASE 6 — Fine-tuning + Specialization + Portfolio Polish
**Weeks 29-32**

### Objective
Round out your profile with fine-tuning (used judiciously, not everywhere), pick one specialization to go deep on, and polish your portfolio into an unmistakably strong application.

### Topics
- When to fine-tune vs prompt/RAG (know the decision tree — this is more valuable than the mechanics)
- LoRA/QLoRA fine-tuning basics with Hugging Face + a small open model
- Dataset curation and synthetic data generation for fine-tuning
- Pick ONE specialization to go deeper on based on your interest/market signal: (a) AI infra/platform engineering, (b) applied RAG/search, or (c) agents/automation — this becomes your "narrative" in interviews

### Why These Matter
Fine-tuning shows you understand the full spectrum of adaptation techniques, but overusing it (or doing it when RAG/prompting would've worked) is a common junior mistake — the decision-making matters more than the mechanics.

### Mini Project
**"Fine-tune vs RAG vs Prompting — Head to Head"** — take one narrow task (e.g., classifying support tickets into your own taxonomy) and solve it three ways, with a clean comparison report of cost, latency, and accuracy. This single project demonstrates mature judgment better than any single fine-tuning demo.

### Bigger Project
Deepen whichever flagship project best matches your chosen specialization, and make it interview-demo-ready: a 5-minute walkthrough video, a live demo link, a clean architecture diagram, and a written case study (problem → architecture → trade-offs → results with numbers).

### Stretch
Write up your three flagship projects as blog posts (dev.to or your own site) — this is free credibility and interview conversation fuel.

### Common Mistakes
- Fine-tuning as a first resort instead of a last resort.
- Polishing code but never writing the "why" — recruiters and engineers skim READMEs; the story matters as much as the code.

### Interview Relevance
"Would you fine-tune or use RAG here?" is a very common question — your head-to-head project is a ready-made, evidence-backed answer.

### Industry Relevance
Most production AI teams use fine-tuning sparingly and strategically — showing you know *why* signals seniority beyond your years of experience.

### Duration
4 weeks

---

## PHASE 7 — Interview Prep Intensive + Applying
**Overlaps Weeks 24-34 (start prep early, intensify at the end)**

### Objective
Convert your portfolio into interviews and offers.

### Topics/Prep Areas
- **Python**: idiomatic Python, decorators, generators, async, common LeetCode-style questions but scoped to what's actually asked (mediums, not competitive programming depth)
- **DS&A**: light — arrays/strings/hashmaps/trees/graphs/DP basics; AI Engineer interviews at most companies (not FAANG-classic) are lighter on DSA than pure SWE loops, but Google/Meta/Amazon will still test it
- **System Design (classic)**: caching, load balancing, databases, queues, consistency trade-offs
- **AI System Design**: RAG architecture, agent architecture, evaluation strategy, cost/latency trade-offs — this is now its own distinct interview category; your projects ARE your prep
- **ML Fundamentals**: bias-variance, overfitting, embeddings, transformer basics, evaluation metrics — enough to be fluent, not to pass a PhD qualifying exam
- **Behavioral**: STAR-format stories from both your Tredence work and your portfolio projects (especially the "what broke and how I fixed it" stories — these are gold)

### Deliverables
- A polished resume (metrics-driven: "reduced eval-measured hallucination rate by X%", "cut inference cost by Y% via model routing")
- A portfolio README/landing page linking all projects with one-paragraph pitches
- 3-5 case studies (one per flagship project) written up properly
- A mock-interview log (track weak areas and drill them)

### Common Mistakes
- Applying only after "feeling 100% ready" — you will never feel 100% ready; apply once you hit the readiness checklist below.
- Over-indexing on LeetCode grinding at the expense of AI-system-design fluency, which is now weighted heavily for these roles.
- Generic resumes with no numbers.

### When to Start Applying
Start applying (not just prepping) once you have:
- 2 flagship projects fully polished, deployed, with eval numbers and a README a stranger could understand in 5 minutes
- Comfortable explaining trade-offs in RAG, agents, and at least basic AI infra out loud, unscripted
- Baseline DSA fluency (can solve most medium problems in 25-35 min)

Realistically this lines up with **end of Phase 4 / start of Phase 5 (~week 24)** — start applying to a first wave then, while you continue building Phases 5-6. Don't wait for "done."

### Interview Readiness Checklist
- [ ] Can whiteboard a RAG system end-to-end, including failure modes and how you'd measure them
- [ ] Can whiteboard an agent system with tool calling, memory, and guardrails
- [ ] Can explain when you'd choose fine-tuning vs RAG vs prompting, with a real example
- [ ] Can explain a cost/latency trade-off you actually made in a project, with numbers
- [ ] Has 3+ STAR stories from portfolio projects (not just work experience)
- [ ] Comfortable with medium-level Python/DSA under time pressure
- [ ] Has deployed at least 2 projects with a live demo link
- [ ] Can explain an eval methodology (RAGAS/LLM-judge/custom) and defend its limitations

---

## 2. Week-by-Week Plan (34 Weeks)

> Format: **Week N — Focus — Key output**

**Phase 0**
- W1: FastAPI + Docker + pydantic v2 refresher → starter template skeleton
- W2: Testing + CI + pre-commit → starter template complete, pushed to GitHub with green CI

**Phase 1**
- W3: Transformers/embeddings conceptual deep-dive + OpenAI/Anthropic SDK basics → small scripts calling both SDKs, streaming + tool schemas
- W4: Chunking strategies + Qdrant setup → Chunking Lab v1
- W5: Chunking Lab eval metrics (recall@k) → Chunking Lab v2 with report
- W6: RegRAG — ingestion, hybrid search, FastAPI service → working end-to-end pipeline
- W7: RegRAG — golden eval dataset + RAGAS scoring + Docker + deploy → **Flagship #1 shipped**

**Phase 2**
- W8: Query rewriting/HyDE + re-ranker integration → Retrieval A/B Harness v1
- W9: Table-aware extraction for financial docs → Multimodal Assistant ingestion pipeline
- W10: Multimodal embeddings/extraction refinement → retrieval working end-to-end
- W11: Semantic caching (Redis) + eval dashboard → dashboard v1
- W12: Eval-gated CI + polish + deploy → **Flagship #2 shipped**

**Phase 3**
- W13: Tool-calling fundamentals (both SDKs) → Tool-Calling Sandbox v1
- W14: LangGraph basics: graphs, state, checkpoints → Sandbox rebuilt on LangGraph
- W15: Incident Copilot — retrieval + tool integration → core agent loop working
- W16: Human-in-the-loop interrupts + guardrails → approval flow working
- W17: Chaos test suite + tracing → robustness validated and documented
- W18: Reflection/self-critique loop + polish + deploy → **Flagship #3 shipped**

**Phase 4**
- W19: MCP fundamentals + build first MCP server → MCP Server v1 (wraps Phase 1-3 tools)
- W20: Multi-agent supervisor/worker pattern in LangGraph → Pipeline Debugger skeleton
- W21: Specialized sub-agents (log/schema/history) → sub-agents working independently
- W22: Supervisor synthesis + benchmark vs single-agent baseline → honest comparison report
- W23: MCP-expose the system + dashboard + polish → **Flagship #4 shipped**; **start applying (first wave)**

**Phase 5**
- W24: LiteLLM gateway + fallback + cost logging → LLM Gateway v1
- W25: Put a flagship project behind the gateway + OpenTelemetry tracing → tracing dashboard
- W26: Load testing (Locust/k6) → load test report + fixes
- W27: Kubernetes deployment (managed cluster) → deployed with health checks/scaling
- W28: Postmortem write-up + monitoring polish → **Flagship #5 (hardened) shipped**

**Phase 6**
- W29: Fine-tune vs RAG vs Prompting head-to-head → comparison project + report
- W30: Deepen chosen specialization project
- W31: Case studies + demo videos for all flagships
- W32: Resume, portfolio site, blog posts → **portfolio fully polished**

**Phase 7 (concentrated push, though prep runs throughout)**
- W33: Mock interviews (AI system design + behavioral) → weak-area log
- W34: Mock interviews (Python/DSA + classic system design) + applications push → **second, larger application wave**

---

## 3. Dependency Graph

```
Phase 0 (Foundations)
   └──> Phase 1 (LLM Fundamentals + RAG v1)
           └──> Phase 2 (Advanced RAG + Eval)
                   └──> Phase 3 (Tool Calling + Agents)
                           ├──> Phase 4 (Multi-Agent + MCP)
                           │        └──> Phase 5 (Infra/Deployment/Scaling)
                           │                 └──> Phase 6 (Fine-tuning + Polish)
                           └──> Phase 7 (Interview Prep) — starts parallel from
                                    end of Phase 3 onward, intensifies through
                                    Phase 5/6, never fully stops.

Key reuse edges (why order matters):
Phase 0 starter-kit  ──used by──> every later project (FastAPI/Docker/CI/tests)
Phase 1 RegRAG       ──used as tool by──> Phase 3 Incident Copilot
Phase 3 Agent loop   ──generalized into──> Phase 4 Multi-Agent Debugger
Phase 1-3 projects   ──wrapped by──> Phase 4 MCP Server
Phase 3/4 flagship   ──hardened in──> Phase 5 (gateway, tracing, K8s)
```

---

## 4. Top 20 GitHub Repositories to Study

*(Prioritized for reading source code and architecture, not just using as a library)*

1. `langchain-ai/langgraph` — agent state-machine design patterns
2. `anthropics/anthropic-sdk-python` — canonical API client patterns
3. `openai/openai-python` — same, for comparison
4. `modelcontextprotocol/servers` — reference MCP server implementations
5. `qdrant/qdrant` — vector DB internals (Rust, but architecture docs are gold)
6. `chroma-core/chroma` — simpler vector DB to read end-to-end
7. `explodinggradients/ragas` — how RAG evaluation is actually implemented
8. `confident-ai/deepeval` — alternative eval framework design
9. `BerriAI/litellm` — multi-provider routing/gateway patterns
10. `vllm-project/vllm` — high-throughput model serving internals
11. `ollama/ollama` — local model serving, simpler to read than vLLM
12. `run-llama/llama_index` — alternative RAG framework design choices vs LangChain
13. `langchain-ai/langchain` — read the retrievers/agents modules specifically
14. `pgvector/pgvector` — when Postgres-native vector search is enough
15. `huggingface/peft` — LoRA/QLoRA implementation for fine-tuning
16. `huggingface/trl` — fine-tuning/RLHF training loops
17. `open-telemetry/opentelemetry-python` — tracing instrumentation patterns
18. `tiangolo/fastapi` — read the internals, not just the docs
19. `outlines-dev/outlines` — structured/constrained LLM output generation
20. `microsoft/autogen` (or `crewAIInc/crewAI`) — alternative multi-agent framework design for contrast with LangGraph

---

## 5. Books Worth Reading

*(Only ones with genuinely high signal-to-time ratio — skip anything else for now)*

1. **Designing Data-Intensive Applications** — Martin Kleppmann. Not AI-specific, but the single highest-leverage systems book for the infra/scaling phases (5-6). You'll draw on it constantly in system design interviews.
2. **Designing Machine Learning Systems** — Chip Huyen. The best available bridge between "ML concepts" and "production ML/AI systems engineering" — directly maps to Phases 2, 5, 6.
3. **AI Engineering** — Chip Huyen. Modern, directly aimed at exactly your transition (LLM apps, RAG, agents, evaluation) — read this alongside Phases 1-4.
4. **Hands-On Large Language Models** — Jay Alammar & Maarten Grootendorst. Best visual/intuitive treatment of transformer/embedding internals for engineers (not researchers) — read during Phase 1.

Skip: deep theoretical ML/DL textbooks (Goodfellow's *Deep Learning*, Bishop, etc.) unless you specifically pivot toward research roles — they have poor ROI for your 8-month, engineering-first goal.

---

## 6. Common Mistakes People Make Transitioning Into AI Engineering

1. **Framework-first learning.** Learning LangChain syntax before understanding retrieval/embeddings/agents conceptually — you can't defend design choices in interviews if you only know the abstraction layer.
2. **No evaluation, ever.** Shipping RAG/agent projects with zero eval numbers — "it seems to work" is not a portfolio claim.
3. **Toy projects.** PDF chatbots and ChatGPT clones signal "followed a tutorial," not "can engineer a system" — you've correctly avoided this in your requirements.
4. **Skipping production concerns.** No Docker, no tests, no CI, no cost/latency awareness — this is what makes a portfolio look like a notebook collection instead of engineering work.
5. **Fine-tuning too early/too often.** Reaching for fine-tuning before exhausting prompting/RAG — shows immature judgment to reviewers.
6. **Multi-agent for its own sake.** Building multi-agent systems because they're trendy, without benchmarking whether they actually beat a single well-tooled agent.
7. **Ignoring classic engineering fundamentals.** Some candidates over-rotate on "AI" and forget that backend engineering, databases, and system design are still ~half of what's assessed.
8. **Applying too late.** Waiting to feel "100% ready" instead of applying once the readiness checklist is met — momentum and real interview feedback accelerate learning faster than solo prep.
9. **Generic resumes.** No metrics, no links to live demos, no differentiation from the flood of similar-looking "I built an AI chatbot" resumes.
10. **Not writing about the work.** Skipping READMEs/case studies/blog posts — the story around a project is often what gets you the callback, not the code alone.

---

## 7. When to Apply & How to Judge Readiness

- **Start a first (smaller) wave of applications around Week 23-24** — once you have 3-4 flagship projects live, including at least one agentic system, and you can comfortably whiteboard RAG + agent architecture. Treat this wave partly as calibration: the interview feedback you get here is some of the highest-value signal in the entire 8 months.
- **Ramp applications up seriously from Week 28 onward**, once your infra/deployment story (Phase 5) is in place — this is usually the differentiator between "junior AI engineer" and "AI engineer" framing in how recruiters read your resume.
- **Use the Interview Readiness Checklist in Phase 7** as your actual gate, not a feeling. If you can honestly check every box, you are ready to interview — further solo polishing has diminishing returns compared to real interview reps.
- **Target company tiers deliberately:** apply to a mix of "dream" companies (OpenAI/Anthropic/DeepMind-tier), strong product companies with real AI teams (a broader set of tech companies actively building AI products), and AI-native startups — the startups will often move faster and give you offers/feedback sooner, which strengthens your negotiating position and interview skills for the tier-1 conversations.

---

*This roadmap is a living document — revisit and adjust phase boundaries every 4 weeks based on what's actually taking longer or shorter than planned. The schedule is a forecast, not a contract.*
