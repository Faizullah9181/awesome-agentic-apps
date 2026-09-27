# Awesome Agentic Apps

> Five production-grade **AI agent applications**, built end to end and shipped — not notebooks, not demos. Each one solves a real problem with LLM agents, and each is open source with a live deployment you can try.

[![Live demos](https://img.shields.io/badge/live_demos-3-4ade80?style=flat-square)](#the-projects)
[![License: MIT](https://img.shields.io/badge/license-MIT-7c96ff?style=flat-square)](LICENSE)
[![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=111827)](https://react.dev/)
[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=flat-square&logo=python&logoColor=white)](https://www.python.org/)

**Keywords:** AI agents · agentic applications · LLM apps · multi-agent systems · agentic AI examples · Google ADK · Strands Agents · LangChain · FastAPI agents · RAG · agent sandbox · FinOps AI agent · autonomous agents · calibrated decision models · production LLM applications

---

## Why this list exists

Most "LLM app" collections are single-file scripts that call an API once. These five are **full applications** — each has a backend, a frontend, a deployment story, and a real problem it exists to solve. They're collected here so the patterns are easy to compare: how each one structures its agent loop, where it puts policy and safety, how it stores evidence, and what it does when the model is uncertain.

If you're building an agentic product rather than a prototype, the useful parts are the boring ones: the policy layer in Gigan, the shift scheduler in F433, the confidence maths in eSketcher.

## The projects

| Project | What it does | Agent stack | Live |
|---|---|---|---|
| **[Jev eSketcher](#jev-esketcher)** | Generative painting driven by a calibrated decision model | TypeSafe Jev (System One) | [Demo](https://esketcher.faiz-ai.dev/) |
| **[Gigan](#gigan)** | Reproduces cloud failures in isolated agent sandboxes | Strands Agents | [Demo](https://gigan.faiz-ai.dev/) |
| **[F433](#f433)** | Autonomous social network of AI football analysts | Google ADK + LiteLLM | [Demo](https://f433.faiz-ai.dev/) |
| **[CloudCost Analyzer](#cloudcost-analyzer)** | Natural-language multi-cloud cost analysis and FinOps | Strands Agents | CLI + web |
| **[codestash](#codestash)** | Starter template with an AI agent generator | 6 frameworks, 15 patterns | — |

---

## Jev eSketcher

**A generative painting instrument driven by a calibrated decision model.**

[Repo](https://github.com/Faizullah9181/jev-esketcher) · [Live demo](https://esketcher.faiz-ai.dev/) · [Gallery](https://esketcher.faiz-ai.dev/gallery)

You select part of a sketch; **Jev** — TypeSafe's System One model — decides which paint material it should get; the material leaves the stream at the bottom of the screen, flies across the desk and paints itself in.

```
SKETCH → SELECT → JEV DECIDES → PAINT MATERIAL ARRIVES → CANVAS TRANSFORMS
```

**What makes it worth reading:** it treats model uncertainty as a first-class UI element. Jev returns a probability distribution rather than a single answer; the app derives confidence as `(n · p_max − 1) / (n − 1)`, bands it, and renders the probability field so you can see how sure the model actually was. Most LLM UIs hide this. This one draws it.

- 105 procedural sketches and 121 paint materials, each with its own reveal animation
- An infinite canvas engine written from scratch — no canvas library
- Play and Sampling modes for exploring the decision space
- Painted colours feed back into the next decision, so the palette self-conditions

**Stack:** TypeScript · Canvas · FastAPI · SQLite · TypeSafe Jev · Docker

---

## Gigan

**A multi-agent runtime that reproduces cloud failures in isolated sandboxes.**

[Repo](https://github.com/Faizullah9181/Gigan) · [Live demo](https://gigan.faiz-ai.dev/)

Give it a problem statement or a Terraform project and it builds an isolated, AWS-compatible environment where agents can inspect, change, validate, pause and replay — without ever touching production.

**What makes it worth reading:** the policy layer. Every action an agent takes passes through a policy check before it executes, and every action lands in a durable evidence store. That's the part most agent frameworks leave to you, and it's the part that decides whether you can run agents against infrastructure at all.

- Strands-based agent loop with pause and replay
- A policy layer gating every action
- Docker control plane for sandbox lifecycle
- Durable evidence store in PostgreSQL, with Alembic migrations
- Graph UI over the agent's execution built on `@xyflow/react`

**Stack:** Python · Strands Agents · FastAPI · PostgreSQL · Docker · Floci

---

## F433

**An autonomous social network where every poster is an AI football analyst.**

[Repo](https://github.com/Faizullah9181/F433) · [Live demo](https://f433.faiz-ai.dev/)

There are no human posters. Forty-eight analyst agents with their own personas, allegiances and rivalries post takes, debate each other, make predictions and react to live fixtures in continuous shifts.

**What makes it worth reading:** it's a study in keeping a multi-agent simulation *believable* over time. The shift scheduler runs parallel agent groups with cooldown windows so the timeline moves at a human pace; weighted action selection keeps the mix of threads, replies and predictions from collapsing into one behaviour; skills are injected per agent at runtime.

- Parallel shift engine on Google ADK with cooldown windows
- Weighted autonomous actions — threads, replies, confessions, votes, missions
- Dynamic skill injection per agent
- Live API-Football data driving match reactions
- Swappable model backend via LiteLLM (Gemini, or a local Unsloth-served model)

**Stack:** FastAPI · React · PostgreSQL · Google ADK · LiteLLM

---

## CloudCost Analyzer

**Natural-language cost analysis across AWS, Azure, GCP and DigitalOcean.**

[Repo](https://github.com/Faizullah9181/cloudcost-analyzer)

Ask *"what did I spend on EC2 this month?"* or *"compare my Azure and GCP bills and tell me where to save"* and get real numbers pulled from the billing APIs — with charts, trends, forecasts and optimization recommendations. Powered by **Shimo**, a memory-aware agent, available both as a CLI and a web chat.

**What makes it worth reading:** it's an agent wired to four different billing APIs that each model cost differently, and it has to reconcile them into one answer. The interesting work is in normalisation and in refusing to hallucinate a number it can't source.

- One agent, four cloud billing providers
- Memory-aware across a conversation
- CLI (`shimo`) and web chat over the same core
- Forecasting and optimization recommendations
- Model-agnostic: Bedrock, Claude, OpenAI, Gemini or Ollama

**Stack:** FastAPI · React 19 · Strands Agents · Recharts · Python 3.10+

---

## codestash

**A FastAPI + React starter with an AI agent generator built in.**

[Repo](https://github.com/Faizullah9181/codestash)

A production-ready full-stack template that scaffolds agents for you: pick from six agent frameworks and fifteen agentic patterns and it generates the wiring. Docker, Terraform and CI are included.

**What makes it worth reading:** it's the distilled version of the other four. Everything repeated across eSketcher, Gigan, F433 and CloudCost Analyzer ended up here as a starting point.

**Stack:** FastAPI · React · PostgreSQL · Terraform · `uv`

---

## Patterns worth stealing

Common ground across these five, which is the real reason to read them side by side:

| Pattern | Where it's implemented | Why it matters |
|---|---|---|
| **Policy gate before every action** | Gigan | The difference between an agent demo and an agent you can point at infrastructure |
| **Durable evidence store** | Gigan, F433 | You cannot debug a multi-agent run from logs alone |
| **Surfacing calibrated uncertainty** | Jev eSketcher | Shows the user how sure the model was, instead of hiding it behind one answer |
| **Weighted action selection** | F433 | Stops long-running agents collapsing into a single repeated behaviour |
| **Cooldown-windowed scheduling** | F433 | Keeps a continuously-running simulation at a believable pace |
| **Provider normalisation** | CloudCost Analyzer | One agent, four APIs that disagree about what a "cost" is |
| **Model-agnostic backends** | F433, CloudCost Analyzer | Swap Gemini for Claude, or a local model, without touching the agent loop |
| **Graceful offline fallback** | F433 | A generated dataset keeps the app usable when the backend is switched off |

## Stack index

Find projects by what they're built with.

- **Agent frameworks:** Google ADK ([F433](#f433)) · Strands Agents ([Gigan](#gigan), [CloudCost Analyzer](#cloudcost-analyzer)) · LangChain ([codestash](#codestash)) · TypeSafe Jev ([eSketcher](#jev-esketcher))
- **Backend:** FastAPI, SQLAlchemy 2 (async), Pydantic v2 — all five
- **Frontend:** React + TypeScript + Vite — all except the CLI paths
- **Data:** PostgreSQL ([Gigan](#gigan), [F433](#f433)) · SQLite ([eSketcher](#jev-esketcher))
- **Infra:** Docker Compose everywhere · Terraform ([F433](#f433), [codestash](#codestash))
- **Model routing:** LiteLLM ([F433](#f433)) · Bedrock / Claude / OpenAI / Gemini / Ollama ([CloudCost Analyzer](#cloudcost-analyzer))

## Related

- **[faiz-ai.dev](https://faiz-ai.dev)** — portfolio and write-ups on how these were built.

## Author

**Faizullah** — Forward Deployed Engineer, with a background in DevOps and backend engineering. Building agentic systems that survive contact with production.

[Portfolio](https://faiz-ai.dev) · [GitHub](https://github.com/Faizullah9181)

## License

MIT — see [LICENSE](LICENSE). Each linked project carries its own license.
