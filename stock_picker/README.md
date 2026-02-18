# StockPicker — AI-Powered Multi-Agent Investment Research System

An autonomous multi-agent AI pipeline that identifies trending companies in any market sector, conducts financial research, and recommends the best stock investment — all without human intervention.

## Architecture

```
┌─────────────────────────────────────────────────────┐
│                   Manager Agent                      │
│               (GPT-4o · Orchestrator)                │
│         Delegates and coordinates all tasks           │
└──────────┬──────────────┬──────────────┬────────────┘
           │              │              │
           ▼              ▼              ▼
  ┌────────────┐  ┌──────────────┐  ┌────────────┐
  │  Trending   │  │  Financial   │  │   Stock    │
  │  Company    │  │  Researcher  │  │   Picker   │
  │  Finder     │  │              │  │            │
  │ (GPT-4o-m) │  │ (GPT-4o-m)  │  │ (GPT-4o-m) │
  └──────┬─────┘  └──────┬───────┘  └─────┬──────┘
         │               │                │
    Web Search      Web Search       Push Notification
    (Serper)        (Serper)          (Pushover)
```

### How It Works

1. **Discovery** — The Trending Company Finder agent searches the web for 2-3 companies gaining attention in a given sector
2. **Research** — The Financial Researcher agent conducts deep analysis on each company's market position, outlook, and investment potential
3. **Decision** — The Stock Picker agent evaluates all research and selects the best investment, then sends a push notification with thse recommendation

All agents are coordinated by a **Manager Agent** using a hierarchical delegation process.

## Key Features

- **Multi-Agent Orchestration** — 4 specialized AI agents coordinated via CrewAI's hierarchical process
- **Autonomous Web Research** — Real-time web search integration via Serper API
- **Structured Data Pipelines** — Pydantic models enforce validated, typed outputs at each stage
- **Push Notifications** — Instant investment alerts delivered via Pushover API
- **Configurable** — Agents and tasks defined in YAML for easy customization

## Tech Stack

| Component | Technology |
|-----------|-----------|
| Agent Framework | CrewAI v1.9.3 |
| LLMs | GPT-4o (manager), GPT-4o-mini (agents) |
| Embeddings | text-embedding-3-small |
| Web Search | Serper API |
| Notifications | Pushover API |
| Data Validation | Pydantic |
| Package Manager | UV (Astral) |
| Language | Python 3.11+ |

## Output

The pipeline generates three artifacts:

| File | Description |
|------|-------------|
| `output/trending_companies.json` | Trending companies with ticker symbols and reasons |
| `output/research_report.json` | Detailed financial analysis per company |
| `output/decision.md` | Final investment recommendation with rationale |

## Installation

Requires Python >=3.11 <3.14 and [UV](https://docs.astral.sh/uv/).

```bash
pip install uv
```

Install dependencies:

```bash
crewai install
```

### Configuration

Create a `.env` file with your API keys:

```
OPENAI_API_KEY=your-openai-key
SERPER_API_KEY=your-serper-key
PUSHOVER_USER=your-pushover-user
PUSHOVER_TOKEN=your-pushover-token
```

Customize the agents and tasks:

- `src/stock_picker/config/agents.yaml` — Agent roles, goals, and backstories
- `src/stock_picker/config/tasks.yaml` — Task descriptions and expected outputs
- `src/stock_picker/crew.py` — Crew logic, tools, and memory configuration

## Running

```bash
crewai run
```

This launches the full pipeline: discovery → research → decision → notification.
