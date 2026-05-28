# 🧠 Intelligent Data Dictionary Agent

> An AI-powered platform that automatically interprets complex datasets, generates contextual metadata, and enables natural language interaction — making data governance intuitive, consistent, and real-time.

---

## 📌 Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Tech Stack](#tech-stack)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation](#installation)
  - [Configuration](#configuration)
- [Usage](#usage)
  - [Ingesting a Dataset](#ingesting-a-dataset)
  - [Natural Language Querying](#natural-language-querying)
  - [Viewing Metadata & Lineage](#viewing-metadata--lineage)
- [API Reference](#api-reference)
- [Data Governance & Consistency](#data-governance--consistency)
- [Real-Time Updates](#real-time-updates)
- [Project Structure](#project-structure)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

The **Intelligent Data Dictionary Agent** is an AI-driven metadata management system that eliminates the manual effort of documenting datasets. It connects to your data sources, automatically infers column definitions, detects relationships and lineage, surfaces usage insights, and lets anyone on your team ask questions about the data in plain English.

Whether you're a data engineer managing pipelines, an analyst exploring an unfamiliar schema, or a governance officer enforcing standards — this agent gives everyone a shared, living understanding of your data.

---

## Problem Statement

Modern organizations operate on hundreds of datasets scattered across warehouses, lakes, and operational databases. Documenting these datasets manually is:

- **Time-consuming** — engineers spend hours writing column-level descriptions.
- **Inconsistent** — definitions drift across teams and tools.
- **Stale** — documentation rarely keeps pace with schema changes.
- **Inaccessible** — non-technical stakeholders can't interpret raw metadata.

**The Intelligent Data Dictionary Agent** solves this by using AI to automate metadata generation, enforce governance standards, detect lineage automatically, and provide a conversational interface for discovery.

---

## Key Features

### 🤖 AI-Powered Metadata Generation
- Automatically generates human-readable column descriptions based on column names, data types, and sample values.
- Infers semantic meaning (e.g., PII fields, financial figures, geographic identifiers).
- Suggests business-friendly display names and tags.

### 🔗 Data Lineage Tracking
- Traces how data flows from source systems through transformations to final tables.
- Visualizes upstream and downstream dependencies for any column or table.
- Detects lineage from SQL query logs, ETL configs, and dbt/Spark job definitions.

### 🧩 Relationship Detection
- Automatically identifies foreign key relationships and join candidates.
- Groups related tables by domain (e.g., Customer, Order, Product).
- Generates an entity-relationship map for visual exploration.

### 💬 Natural Language Interface
- Ask questions like:
  - *"What does the `ltv_score` column mean?"*
  - *"Which tables contain customer PII?"*
  - *"Show me the lineage for `orders.total_amount`."*
- Powered by an LLM-backed conversational agent with RAG over the dictionary corpus.

### 📊 Usage Insights
- Surfaces which columns and tables are most frequently queried.
- Identifies unused or deprecated fields.
- Highlights hot tables that may need optimization.

### 🔄 Real-Time Updates
- Monitors schema changes and triggers re-generation of affected metadata.
- Change-detection alerts notify data owners when definitions may be outdated.
- Supports webhook integration for CI/CD pipeline triggers.

### 🛡️ Governance & Consistency
- Policy engine enforces naming conventions, required fields, and tagging standards.
- Role-based access control (RBAC) for viewing and editing definitions.
- Audit log of all metadata changes with author, timestamp, and diff.

---

## System Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Data Sources                             │
│   PostgreSQL │ Snowflake │ BigQuery │ S3 │ dbt │ Spark │ APIs  │
└───────────────────────────┬─────────────────────────────────────┘
                            │  Schema Extraction & Query Logs
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Ingestion & Connector Layer                   │
│         Schema Crawler │ Log Parser │ Lineage Extractor         │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      AI Processing Engine                        │
│   ┌──────────────────┐   ┌──────────────────┐                  │
│   │  Metadata Gen.   │   │ Relationship Det. │                  │
│   │  (LLM + Context) │   │ (Graph Analysis)  │                  │
│   └──────────────────┘   └──────────────────┘                  │
│   ┌──────────────────┐   ┌──────────────────┐                  │
│   │ Lineage Resolver │   │ Usage Analyzer   │                  │
│   └──────────────────┘   └──────────────────┘                  │
└───────────────────────────┬─────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    Dictionary Store (Backend)                    │
│        Vector DB (embeddings) │ Graph DB (lineage/rels)         │
│        Relational DB (metadata catalog) │ Cache Layer           │
└──────────┬──────────────────────────────────────┬───────────────┘
           │                                      │
           ▼                                      ▼
┌──────────────────────┐              ┌──────────────────────────┐
│   REST / GraphQL API │              │  NL Query Agent (RAG)    │
│   Governance Engine  │              │  Conversational Interface│
└──────────┬───────────┘              └───────────┬──────────────┘
           │                                      │
           └──────────────┬───────────────────────┘
                          ▼
              ┌───────────────────────┐
              │      Web UI / CLI     │
              │  Dashboard │ Chat UI  │
              └───────────────────────┘
```

---

## Tech Stack

| Layer | Technology |
|---|---|
| **AI / LLM** | Claude API (Anthropic), LangChain / LlamaIndex |
| **Vector Store** | Pinecone / pgvector / Weaviate |
| **Graph Database** | Neo4j (lineage & relationships) |
| **Metadata Store** | PostgreSQL |
| **Backend API** | FastAPI (Python) |
| **Frontend** | React + TypeScript |
| **Schema Connectors** | SQLAlchemy, Snowflake Connector, BigQuery Client |
| **Streaming / Events** | Apache Kafka / Redis Pub-Sub |
| **Orchestration** | Apache Airflow / Prefect |
| **Auth & RBAC** | Auth0 / Keycloak |
| **Containerization** | Docker + Kubernetes |

---

## Getting Started

### Prerequisites

- Python 3.10+
- Node.js 18+
- Docker & Docker Compose
- An Anthropic API key (or OpenAI API key)
- Access credentials for at least one data source

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-org/intelligent-data-dictionary.git
cd intelligent-data-dictionary

# 2. Set up the Python environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 3. Install frontend dependencies
cd frontend
npm install
cd ..

# 4. Start infrastructure services (Postgres, Neo4j, Redis)
docker-compose up -d

# 5. Run database migrations
alembic upgrade head

# 6. Start the backend API
uvicorn app.main:app --reload --port 8000

# 7. Start the frontend (in a new terminal)
cd frontend && npm run dev
```

The UI will be available at `http://localhost:5173`.

### Configuration

Copy the example environment file and fill in your credentials:

```bash
cp .env.example .env
```

Key variables in `.env`:

```env
# AI
ANTHROPIC_API_KEY=sk-ant-...

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/datadictionary
NEO4J_URI=bolt://localhost:7687
NEO4J_USER=neo4j
NEO4J_PASSWORD=yourpassword

# Vector Store
PINECONE_API_KEY=...
PINECONE_ENVIRONMENT=us-east-1-aws

# Data Source Example (Snowflake)
SNOWFLAKE_ACCOUNT=yourorg.us-east-1
SNOWFLAKE_USER=analyst
SNOWFLAKE_PASSWORD=...
SNOWFLAKE_WAREHOUSE=COMPUTE_WH
SNOWFLAKE_DATABASE=PROD
```

---

## Usage

### Ingesting a Dataset

**Via CLI:**

```bash
python -m agent ingest \
  --source snowflake \
  --database PROD \
  --schema SALES \
  --table orders
```

**Via API:**

```http
POST /api/v1/ingest
Content-Type: application/json

{
  "source_type": "snowflake",
  "database": "PROD",
  "schema": "SALES",
  "tables": ["orders", "order_items", "customers"]
}
```

The agent will:
1. Extract schema and sample data.
2. Generate AI-powered column descriptions.
3. Detect relationships and lineage.
4. Store all metadata in the dictionary.

---

### Natural Language Querying

**Chat UI:** Navigate to `http://localhost:5173/chat` and type your question.

**API:**

```http
POST /api/v1/query
Content-Type: application/json

{
  "question": "Which tables contain personally identifiable information?"
}
```

**Response:**

```json
{
  "answer": "3 tables contain PII fields: customers (email, phone, address), user_profiles (full_name, date_of_birth), and payment_methods (card_last_four, billing_address).",
  "sources": [
    { "table": "customers", "columns": ["email", "phone", "address"] },
    { "table": "user_profiles", "columns": ["full_name", "date_of_birth"] },
    { "table": "payment_methods", "columns": ["card_last_four", "billing_address"] }
  ]
}
```

---

### Viewing Metadata & Lineage

```http
# Get full metadata for a table
GET /api/v1/tables/PROD.SALES.orders

# Get column-level lineage
GET /api/v1/lineage/PROD.SALES.orders/total_amount

# Get related tables
GET /api/v1/relationships/PROD.SALES.orders
```

---

## API Reference

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/v1/ingest` | Ingest one or more tables |
| `GET` | `/api/v1/tables` | List all catalogued tables |
| `GET` | `/api/v1/tables/{fqn}` | Get metadata for a table |
| `PATCH` | `/api/v1/tables/{fqn}` | Update metadata (human override) |
| `GET` | `/api/v1/columns/{fqn}/{col}` | Get column-level metadata |
| `GET` | `/api/v1/lineage/{fqn}` | Get lineage graph for a table |
| `GET` | `/api/v1/lineage/{fqn}/{col}` | Get lineage for a column |
| `GET` | `/api/v1/relationships/{fqn}` | Get detected relationships |
| `POST` | `/api/v1/query` | Natural language query |
| `GET` | `/api/v1/audit` | View audit log |
| `GET` | `/api/v1/usage/{fqn}` | Get usage statistics |
| `POST` | `/api/v1/refresh/{fqn}` | Trigger metadata refresh |

Full OpenAPI docs available at `http://localhost:8000/docs`.

---

## Data Governance & Consistency

The governance engine enforces policies at ingestion and update time:

- **Required Fields Policy**: Ensures every column has a description, owner, and data classification tag.
- **Naming Conventions**: Flags columns that violate snake_case or reserved keyword rules.
- **PII Tagging**: Auto-detects and labels PII fields; alerts the data owner for review.
- **Approval Workflow**: Sensitive metadata changes require approval from a designated steward.
- **Audit Trail**: Every create, update, and delete is logged with actor, timestamp, and before/after diff.

Configure policies in `config/governance_policies.yaml`:

```yaml
policies:
  require_description: true
  require_owner: true
  require_classification: true
  pii_auto_detect: true
  naming_convention: snake_case
  approval_required_for:
    - pii_fields
    - business_critical_tables
```

---

## Real-Time Updates

The agent supports two update modes:

### Scheduled Refresh
Set a cron schedule per data source in `config/sources.yaml`:

```yaml
sources:
  - name: snowflake_prod
    type: snowflake
    refresh_schedule: "0 2 * * *"   # Daily at 2 AM
```

### Event-Driven (Webhook)
Trigger a refresh from your CI/CD pipeline or dbt Cloud on schema change:

```bash
curl -X POST https://your-agent/api/v1/refresh/PROD.SALES.orders \
  -H "Authorization: Bearer <token>"
```

Change notifications are published to subscribed stakeholders via email or Slack when a monitored table's schema changes.

---

## Project Structure

```
intelligent-data-dictionary/
├── app/
│   ├── api/                  # FastAPI route handlers
│   ├── agent/                # AI agent logic (LLM calls, RAG)
│   ├── connectors/           # Data source connectors (Snowflake, PG, BQ...)
│   ├── lineage/              # Lineage extraction & graph management
│   ├── governance/           # Policy engine & audit logging
│   ├── models/               # SQLAlchemy ORM models
│   └── main.py               # App entrypoint
├── frontend/
│   ├── src/
│   │   ├── pages/            # Dashboard, Table View, Chat, Lineage Explorer
│   │   ├── components/       # Reusable UI components
│   │   └── api/              # API client layer
│   └── package.json
├── config/
│   ├── governance_policies.yaml
│   └── sources.yaml
├── migrations/               # Alembic migration files
├── tests/
│   ├── unit/
│   └── integration/
├── docker-compose.yml
├── .env.example
├── requirements.txt
└── README.md
```

---

## Roadmap

- [x] Core schema ingestion & AI metadata generation
- [x] Natural language query interface
- [x] SQL-based lineage extraction
- [x] Relationship detection
- [x] Governance policy engine
- [ ] dbt & Spark native lineage parsing
- [ ] Slack / Teams bot integration
- [ ] Data quality score per column
- [ ] Glossary management (business term linking)
- [ ] Multi-tenant support
- [ ] Fine-tuned domain-specific LLM for metadata generation

---

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and create a feature branch: `git checkout -b feature/your-feature`
2. Commit your changes with clear messages following [Conventional Commits](https://www.conventionalcommits.org/).
3. Add or update tests as appropriate.
4. Open a Pull Request with a description of the change and its motivation.

Please read [CONTRIBUTING.md](./CONTRIBUTING.md) for detailed guidelines.

---

## License

This project is licensed under the MIT License. See [LICENSE](./LICENSE) for details.

---

<p align="center">Built with ❤️ to make data understandable for everyone.</p>
