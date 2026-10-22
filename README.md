# TradingAgents-CN — Multi-Agent LLM Stock Analysis Platform

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue.svg)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/Backend-FastAPI-009688.svg)](https://fastapi.tiangolo.com/)
[![Vue 3](https://img.shields.io/badge/Frontend-Vue%203-42b883.svg)](https://vuejs.org/)
[![Docker](https://img.shields.io/badge/Docker-amd64%20%2B%20arm64-2496ED.svg)](https://www.docker.com/)

A production-grade, multi-agent **LLM research platform for stock market analysis** — built on top of the open-source [TradingAgents](https://github.com/TauricResearch/TradingAgents) framework. It coordinates a team of specialized AI agents (analysts, researchers, a trader, and a risk manager) to produce structured, evidence-backed investment research, and wraps them in an enterprise web application.

> **Positioning:** Research and learning only. The platform does **not** execute live trades or provide financial advice.

---

## Why this project

This is a complete, working reference for how to build and ship an **agentic LLM product** in production — not just a notebook. It demonstrates:

- A **multi-agent orchestration** layer (LangGraph-style state machine) where specialized agents debate, cross-examine, and converge on a final decision.
- A **full-stack product** around the AI core: REST API, SPA frontend, user/auth, caching, notifications, and containerized deployment.
- **Provider-agnostic LLM integration**: OpenAI, Google Gemini, DeepSeek, Qwen, and any custom endpoint, with smart model selection.
- **Multi-market data** support (A-shares, HK, US) through Tushare, AkShare, and BaoStock.

---

## Core features

**Agentic analysis engine**
- Coordinated analyst team (fundamental, technical, news/sentiment) + researcher + trader + risk manager
- Multi-agent debate and task decomposition for more grounded conclusions
- Smart news ingestion: quality scoring, multi-level filtering, unified news tool
- Structured reports (Markdown / Word / PDF export)

**Enterprise web application**
- **Backend:** FastAPI + Uvicorn RESTful API, WebSocket + SSE real-time push
- **Frontend:** Vue 3 + Vite + Element Plus single-page app
- **Data layer:** MongoDB + Redis multi-level caching (10× performance over the original)
- User authentication, roles, audit logs, and a config center (LLM providers, data sources, system settings)

**Operations & scale**
- Batch analysis of multiple tickers in one run
- Watchlist, favorites, historical analysis records, simulated-trading sandbox
- Docker multi-arch images (amd64 + arm64, incl. Apple Silicon / AWS Graviton) with GitHub Actions CI and Docker Compose one-command startup

---

## Architecture at a glance

| Layer | Technology |
|---|---|
| Backend | FastAPI + Uvicorn, WebSocket, SSE |
| Frontend | Vue 3 + Vite + Element Plus, Pinia |
| Orchestration | Multi-agent graph (LangGraph-style) |
| Database / Cache | MongoDB + Redis |
| Data sources | Tushare, AkShare, BaoStock |
| LLM providers | OpenAI, Google Gemini, DeepSeek, Qwen, custom endpoints |
| Deployment | Docker (amd64 + arm64), Docker Compose, GitHub Actions |

```
Browser (Vue 3 SPA)
        │ REST / WebSocket / SSE
        ▼
FastAPI backend ──► Agent orchestration graph
        │              ├── Analysts (fundamental / technical / news)
        │              ├── Researcher
        │              ├── Trader
        │              └── Risk manager
        ├──► MongoDB + Redis (cache)
        └──► Market data providers (Tushare / AkShare / BaoStock)
```

---

## Quick start

### Option A — Docker (recommended for production)

```bash
docker compose up -d
# open http://localhost:<port> after ~5 minutes
```

Multi-arch images are built automatically via GitHub Actions for `linux/amd64` and `linux/arm64`.

### Option B — Local development

```bash
# Backend
pip install -r requirements.txt
python db/createTable.py        # initialize database
uvicorn app.main:app --reload

# Frontend
cd frontend
npm install
npm run dev
```

### Configuration

1. Add your LLM API keys for one or more providers (OpenAI / Gemini / DeepSeek / Qwen / custom).
2. Configure market data source credentials (Tushare / AkShare / BaoStock).
3. Sync the stock dataset before running analysis (required — analysis quality depends on current data).
4. Start a batch analysis from the dashboard and watch the agents work in real time.

---

## Repository layout

```text
.
├── app/                 # FastAPI backend (API, auth, orchestration, caching)
├── frontend/            # Vue 3 + Vite + Element Plus SPA
├── tradingagents/       # Multi-agent analysis core (analysts, trader, risk, graph)
├── config/              # Configuration templates
├── docs/                # Architecture, agent, data, deployment docs
├── data/                # Analysis results and exports
└── docker/              # Dockerfiles and compose (amd64 + arm64)
```

---

## Development & contributing

The project accepts focused bug fixes, provider integrations, new data sources, documentation, and tests. See `CONTRIBUTORS.md` for the contributor list and `CONTRIBUTING` workflow.

**License:** The open-source core is **Apache 2.0**. The `app/` and `frontend/` product directories are released under a separate license — see `LICENSE` and `COPYRIGHT.md` for exact terms.

---

## Disclaimer

This project is for **research and education only**. It does not provide investment advice and does not execute real trades. Always do your own due diligence before making financial decisions.

