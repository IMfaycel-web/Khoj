# Khoj

Khoj is a self-hosted personal AI workspace for searching private knowledge, chatting with local or hosted language models, building specialized agents, and automating recurring research.

It combines conversational AI, semantic document retrieval, web search, scheduled tasks, code execution, memory, and multi-client access in a single application.

## Overview

Khoj connects personal documents, online information, and language models through a browser-based assistant. It can run locally for a private single-user setup or operate as an authenticated multi-user service.

The platform is suitable for:

- Personal knowledge search
- Retrieval-augmented conversations
- Research and information synthesis
- Custom AI agents
- Scheduled reports and notifications
- Local and offline model workflows
- Private document question answering
- Tool and code-assisted tasks

## Features

- Conversations with local and hosted language models
- Support for OpenAI, Anthropic, Google, and OpenAI-compatible providers
- Semantic search across private documents
- PDF, Markdown, Word, Org-mode, Notion, image, and text ingestion
- Retrieval-augmented generation with source-aware responses
- Custom agents with configurable models, personas, knowledge, and tools
- Scheduled automations and recurring research tasks
- Web search through a self-hosted or external search provider
- Webpage extraction and research workflows
- Persistent conversation history and long-term memory
- Image generation and multimodal interactions
- Speech recognition and voice-oriented workflows
- Sandboxed Python code execution
- Optional computer-use environment
- Model Context Protocol integration
- Browser, desktop, Obsidian, Emacs, and mobile-compatible access
- Single-user anonymous mode
- Authenticated multi-user deployment
- API-token access for supported clients
- PostgreSQL vector search
- Built-in database migrations and scheduled jobs
- Docker and Python package deployment

## Tech Stack

| Area | Technologies |
| --- | --- |
| Language | Python 3.10 through 3.12, TypeScript |
| Application backend | FastAPI, Django 5.1 |
| Application server | Uvicorn, Gunicorn |
| Web frontend | Next.js 15, React 18 |
| Frontend tooling | Bun, TypeScript, Tailwind CSS |
| UI components | Radix UI |
| Database | PostgreSQL |
| Vector search | pgvector |
| Embeddings | sentence-transformers, Transformers |
| AI providers | OpenAI, Anthropic, Google GenAI |
| RAG utilities | LangChain text splitters and integrations |
| Scheduling | APScheduler, django-apscheduler |
| Web search | SearXNG and external search providers |
| Code execution | Terrarium or E2B |
| Authentication | Django sessions, OAuth-compatible flows, API tokens |
| Document processing | PyMuPDF, docx2txt, Beautiful Soup, Magika |
| Speech processing | Whisper |
| Testing | Pytest, pytest-django, pytest-asyncio |
| Quality tooling | Ruff, mypy, pre-commit |
| Packaging | uv, Hatchling |
| Deployment | Docker, Docker Compose |

## Installation

Docker Compose is the recommended method for a complete self-hosted environment.

### Docker Compose

#### Requirements

- Git
- Docker Engine or Docker Desktop
- Docker Compose
- Sufficient memory for PostgreSQL, embedding models, search, and sandbox services

Review `docker-compose.yml` before starting the stack. Replace the example administrator password and Django secret key.

Start the services:

```bash
docker compose up -d
```

The default stack includes:

- Khoj application server
- PostgreSQL with pgvector
- SearXNG web search
- Terrarium code sandbox
- Optional computer-use container

The application listens on port `42110`.

View service status:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs -f
```

Stop the services:

```bash
docker compose stop
```

Remove containers while preserving named volumes:

```bash
docker compose down
```

Rebuild after source changes:

```bash
docker compose build --no-cache
docker compose up -d
```

### Python Package

Create and activate a virtual environment:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

On Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

Install Khoj with the embedded PostgreSQL option:

```bash
pip install "khoj[local]"
```

Start a local single-user instance:

```bash
USE_EMBEDDED_DB=true khoj --anonymous-mode
```

On Windows PowerShell:

```powershell
$env:USE_EMBEDDED_DB="true"
khoj --anonymous-mode
```

The first interactive run can create an administrator and configure available model providers.

### Development from Source

#### Requirements

- Python 3.10 through 3.12
- uv or pip
- Bun or Yarn
- PostgreSQL with pgvector, or embedded PostgreSQL
- Git

Run the development setup script:

```bash
bash scripts/dev_setup.sh
```

The script installs backend dependencies, builds the web client, and configures pre-commit hooks.

Start the server with an embedded database:

```bash
USE_EMBEDDED_DB=true uv run khoj --anonymous-mode
```

Start an authenticated instance by removing `--anonymous-mode`.

Build the web client manually:

```bash
cd src/interface/web
bun install
bun run export
```

## Usage

1. Start the Khoj server.
2. Open the local web interface.
3. Configure a local or hosted chat model.
4. Add documents or connect a supported knowledge source.
5. Allow Khoj to generate embeddings and index the content.
6. Ask questions using the indexed knowledge.
7. Create custom agents for specialized workflows.
8. Enable web search for current information.
9. Configure scheduled automations for recurring tasks.
10. Enable code execution or computer use only when required.

### Server Commands

Start on a custom port:

```bash
khoj --port 42110
```

Bind to another network interface:

```bash
khoj --host 0.0.0.0
```

Enable detailed logs:

```bash
khoj -vv
```

Use custom TLS files:

```bash
khoj --sslcert certificate.pem --sslkey private-key.pem
```

Run without interactive setup:

```bash
khoj --non-interactive
```

Display the installed version:

```bash
khoj --version
```

### Authentication

Anonymous mode is intended for trusted, local, single-user environments:

```bash
khoj --anonymous-mode
```

Remove this option for authenticated deployments. Create secure administrator credentials and configure the required identity provider before exposing the service to other users.

## Configuration

Khoj reads database, provider, authentication, networking, search, and execution settings from environment variables and persistent application configuration.

| Variable | Purpose |
| --- | --- |
| `KHOJ_DJANGO_SECRET_KEY` | Signs sessions and security-sensitive application data |
| `KHOJ_ADMIN_EMAIL` | Initial administrator email |
| `KHOJ_ADMIN_PASSWORD` | Initial administrator password |
| `KHOJ_DOMAIN` | Externally accessible hostname or address |
| `KHOJ_ALLOWED_DOMAIN` | Internal hostname accepted behind a proxy |
| `KHOJ_NO_HTTPS` | Allows an explicitly configured non-TLS deployment |
| `KHOJ_DEBUG` | Enables development behavior |
| `KHOJ_TELEMETRY_DISABLE` | Disables telemetry |
| `POSTGRES_HOST` | PostgreSQL host |
| `POSTGRES_PORT` | PostgreSQL port |
| `POSTGRES_DB` | PostgreSQL database |
| `POSTGRES_USER` | PostgreSQL user |
| `POSTGRES_PASSWORD` | PostgreSQL password |
| `USE_EMBEDDED_DB` | Starts the embedded PostgreSQL option |
| `PGSERVER_DATA_DIR` | Embedded PostgreSQL data directory |
| `OPENAI_API_KEY` | OpenAI provider credential |
| `ANTHROPIC_API_KEY` | Anthropic provider credential |
| `GEMINI_API_KEY` | Google model-provider credential |
| `OPENAI_BASE_URL` | OpenAI-compatible local or remote endpoint |
| `KHOJ_DEFAULT_CHAT_MODEL` | Default model for new conversations |
| `KHOJ_SEARXNG_URL` | SearXNG service endpoint |
| `KHOJ_TERRARIUM_URL` | Local code-sandbox endpoint |
| `E2B_API_KEY` | Remote code-sandbox credential |
| `KHOJ_OPERATOR_ENABLED` | Enables the optional computer-use environment |
| `SERPER_DEV_API_KEY` | External web-search credential |
| `FIRECRAWL_API_KEY` | Search and webpage-reading credential |
| `EXA_API_KEY` | Search and webpage-reading credential |
| `OLOSTEP_API_KEY` | Webpage-reading credential |

Production deployments should:

- Replace every example credential
- Use a long, random Django secret key
- Run PostgreSQL on persistent storage
- Restrict PostgreSQL and internal services from public networks
- Configure authentication before remote access
- Use HTTPS behind a trusted reverse proxy
- Set public and internal domains correctly
- Protect model-provider and search credentials
- Review code-execution and computer-use permissions
- Back up the database and persistent configuration
- Pin container versions for controlled upgrades
- Monitor model usage, storage, logs, and scheduled tasks

## Contributing

Create a focused branch and keep backend, interface, database, and client changes within their existing boundaries.

Install the development environment:

```bash
bash scripts/dev_setup.sh
```

Install optional desktop and Obsidian dependencies:

```bash
bash scripts/dev_setup.sh --full
```

Run backend tests:

```bash
uv run pytest
```

Run tests in parallel:

```bash
uv run pytest -n auto
```

Run Python linting and formatting:

```bash
uv run ruff check .
uv run ruff format --check .
```

Run type checking:

```bash
uv run mypy
```

Run all configured hooks:

```bash
uv run pre-commit run --all-files
```

Validate the web client:

```bash
cd src/interface/web
bun run lint
bun run build
```

Before submitting a pull request:

- Add tests for new behavior and regression fixes
- Include migrations for database-model changes
- Keep API and frontend behavior synchronized
- Preserve authenticated and anonymous deployment paths
- Avoid committing model keys, user data, databases, or generated files
- Update configuration guidance for new environment variables
- Validate Docker and Python installation paths when affected
- Keep changes focused and clearly documented
