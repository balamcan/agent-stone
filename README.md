# 🚀 Mastra AI Framework - Dockerized Environment

<div align="center">

![Mastra Logo](https://mastra.ai/logo.svg)

**Next-generation TypeScript framework for building AI agents and intelligent applications**

[![TypeScript](https://img.shields.io/badge/TypeScript-5.0+-blue.svg)](https://www.typescriptlang.org/)
[![Node](https://img.shields.io/badge/Node.js-20+-green.svg)](https://nodejs.org/)
[![Docker](https://img.shields.io/badge/Docker-Ready-2496ED.svg)](https://www.docker.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

</div>

---

## 📋 Table of Contents

- [Features](#-features)
- [Prerequisites](#-prerequisites)
- [Quick Start](#-quick-start)
- [Project Structure](#-project-structure)
- [Configuration](#️-configuration)
- [Usage](#-usage)
- [Development](#-development)
- [Deployment](#-deployment)
- [Available Scripts](#-available-scripts)
- [Architecture](#-architecture)
- [Troubleshooting](#-troubleshooting)
- [Resources](#-resources)

---

## ✨ Features

This dockerized environment for Mastra includes:

- 🐳 **Docker Multi-Stage**: Optimized builds and lightweight images
- 🗄️ **PostgreSQL**: Persistent database for storage
- 🔥 **Hot Reload**: Fast development with automatic reloads
- 🔒 **Security**: Non-root users and best practices
- 📊 **Observability**: Built-in logging and health checks
- 🎯 **Multiple Profiles**: Development, production, and full
- 🛠️ **Helper Scripts**: Automation for common tasks
- 📦 **Redis & PgAdmin**: Optional services for development

---

## 🔧 Prerequisites

Before you begin, ensure you have installed:

- **Docker Desktop** (v20.10+) - [Download](https://www.docker.com/products/docker-desktop)
- **Docker Compose** (v2.0+) - Included with Docker Desktop
- **Node.js** (v20+) - Only for local development - [Download](https://nodejs.org/)
- **Git** - [Download](https://git-scm.com/)

### Verify installation:

```bash
docker --version
docker compose version
node --version
```

---

## 🚀 Quick Start

### 1️⃣ Clone the repository

```bash
git clone <your-repo>
cd agent-stone
```

### 2️⃣ Configure environment variables

```bash
cp .env.example .env
```
Edit `.env` and add your API keys:

```env
# You need at least one of these:
OPENAI_API_KEY=sk-your-openai-api-key-here
ANTHROPIC_API_KEY=sk-ant-your-anthropic-api-key-here
GOOGLE_GENERATIVE_AI_API_KEY=your-google-api-key-here
```

### 3️⃣ Start the environment

**Option A: Using the initialization script (Recommended)**

```bash
./scripts/init.sh
```

**Option B: Manually**

```bash
# Build images
docker compose build

# Start services
docker compose up -d

# View logs
docker compose logs -f mastra
```

### 4️⃣ Verify it works

Open your browser at [http://localhost:4111](http://localhost:4111)

```bash
# Check service status
docker compose ps

# View real-time logs
docker compose logs -f mastra
```

---

## 📁 Project Structure

```
agent-stone/
├── 📄 Dockerfile              # Multi-stage optimized image for production
├── 📄 Dockerfile.dev          # Development image with hot-reload
├── 📄 docker-compose.yml      # Service orchestration
├── 📄 .dockerignore           # Build context exclusions
├── 📄 .env.example            # Example environment variables
├── 📄 package.json            # Project dependencies
├── 📄 tsconfig.json           # TypeScript configuration
├── 📄 mastra.config.ts        # Mastra configuration
│
├── 📂 src/                    # Source code
│   ├── 📄 index.ts           # Entry point
│   ├── 📂 agents/            # AI agents
│   ├── 📂 workflows/         # Workflows and orchestration
│   └── 📂 tools/             # Custom tools
│
├── 📂 scripts/                # Automation scripts
│   ├── 📄 init.sh            # Environment initialization
│   ├── 📄 dev-reload.sh      # Quick reload for development
│   ├── 📄 backup-db.sh       # Database backup
│   └── 📄 restore-db.sh      # Database restore
│
├── 📂 init-db/                # SQL initialization scripts
├── 📂 logs/                   # Log files
└── 📂 backups/                # Database backups
```

---

## ⚙️ Configuration

### Main Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `POSTGRES_USER` | PostgreSQL user | `postgres` |
| `POSTGRES_PASSWORD` | PostgreSQL password | `postgres` |
| `POSTGRES_DB` | Database name | `mastra` |
| `MASTRA_PORT` | Application port | `4111` |
| `OPENAI_API_KEY` | OpenAI API key | - |
| `ANTHROPIC_API_KEY` | Anthropic API key | - |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Google API key | - |
| `NODE_ENV` | Runtime environment | `production` |

See `.env.example` for all available options.

---

## 🎯 Usage

### Modes of Operation

#### 🏭 Production Mode (default)

```bash
docker compose up -d
```

Includes:
- Mastra App (port 4111)
- PostgreSQL (port 5432)

#### 🔥 Development Mode

```bash
./scripts/init.sh --dev
# or
docker compose --profile dev up
```

Includes everything above plus:
- Hot-reload enabled
- Debugging enabled (port 9229)
- PgAdmin (port 5050)
- Volumes mounted for live code

#### 🌟 Full Mode

```bash
./scripts/init.sh --full
# or
docker compose --profile full up -d
```

Includes everything plus:
- Redis for caching (port 6379)
- PgAdmin for DB management

### Accessing Services

- **Mastra API**: http://localhost:4111
- **PgAdmin** (dev mode): http://localhost:5050
  - Email: `admin@mastra.local`
  - Password: `admin`
- **PostgreSQL**: `localhost:5432`
  - Database: `mastra`
  - User: `postgres`
  - Password: `postgres`

---

## 👨‍💻 Development

### Initialize Mastra

If you haven't initialized Mastra locally yet:

```bash
npm install
npx mastra init
```

Follow the prompts to:
1. Select components (agents, workflows, scorers)
2. Choose your preferred LLM provider
3. Add examples (recommended to get started)

### Create an Agent

```bash
# Create directory for your agent
mkdir -p src/agents

# Create agent file
cat > src/agents/my-agent.ts << 'EOF'
import { Agent } from '@mastra/core';
import { openai } from '@ai-sdk/openai';

export const myAgent = new Agent({
  name: 'My Agent',
  instructions: 'You are a helpful AI assistant.',
  model: openai('gpt-4o-mini'),
});
EOF
```

### Hot Reload in Development

In development mode, changes in `src/` are reflected automatically:

```bash
# Start in dev mode
./scripts/init.sh --dev

# Edit code in src/
# Changes are reloaded automatically
```

### Quick Reload

If you need to restart only the Mastra container:

```bash
./scripts/dev-reload.sh
```

---

## 🚢 Deployment

### Build for Production

```bash
# Build optimized image
docker compose build mastra

# Check image size
docker images | grep agent-stone
```

### Cloud Deployment

This environment is ready to be deployed to:

- **AWS ECS/Fargate**
- **Google Cloud Run**
- **Azure Container Instances**
- **DigitalOcean App Platform**
- **Railway, Fly.io, Render**

#### Example: Push to Registry

```bash
# Tag image
docker tag agent-stone:latest your-registry.com/agent-stone:v1.0.0

# Push to registry
docker push your-registry.com/agent-stone:v1.0.0
```

---

## 📜 Available Scripts

### NPM Scripts

```bash
npm run dev              # Local development (no Docker)
npm run build            # Production build
npm start                # Run compiled app
npm run docker:build     # Build Docker image
npm run docker:up        # Start services
npm run docker:down      # Stop services
npm run docker:logs      # View Mastra logs
npm run docker:dev       # Development mode
npm run docker:full      # All services
npm run docker:clean     # Clean everything
```

### Shell Scripts

```bash
./scripts/init.sh              # Full initialization
./scripts/init.sh --dev        # Development mode
./scripts/init.sh --full       # All services
./scripts/init.sh --clean      # Clean and restart
./scripts/dev-reload.sh        # Quick reload
./scripts/backup-db.sh         # Create DB backup
./scripts/restore-db.sh <file> # Restore from backup
```

---

## 🏗️ Architecture

### Service Diagram

```
┌─────────────┐
│   Client    │
│    HTTP     │
└──────┬──────┘
       │ Port 4111
       ▼
┌─────────────┐      ┌──────────────┐
│   Mastra    │─────▶│  PostgreSQL  │
│     App     │      │   Database   │
└──────┬──────┘      └──────────────┘
       │
       │ (Optional)
       ▼
┌─────────────┐
│    Redis    │
│    Cache    │
└─────────────┘
```

### Multi-Stage Dockerfile

The Dockerfile uses 3 stages:

1. **Dependencies**: Installs production dependencies
2. **Builder**: Compiles TypeScript
3. **Runtime**: Final optimized and secure image

Benefits:
- ✅ Smaller final image (~150MB vs ~500MB)
- ✅ No dev dependencies in the final image
- ✅ Optimized cache
- ✅ Faster builds

---

## 🔍 Troubleshooting

### Containers won't start

```bash
# Check logs
docker compose logs

# Check Docker status
docker info

# Restart Docker Desktop
```

### Port already in use

```bash
# Find process using port 4111
lsof -i :4111

# Or change port in .env
MASTRA_PORT=4112
```

### Database won't connect

```bash
# Check PostgreSQL is running
docker compose ps postgres

# View PostgreSQL logs
docker compose logs postgres

# Recreate database
docker compose down -v
docker compose up -d
```

### File permissions

```bash
# On macOS/Linux, make scripts executable
chmod +x scripts/*.sh

# Check volume ownership in container
docker compose exec mastra ls -la /app
```

### Wipe everything and start over

```bash
./scripts/init.sh --clean
# or
docker compose down -v
docker system prune -af
rm -rf node_modules .mastra
```

---

## 📚 Resources

### Official Documentation

- [Mastra Docs](https://mastra.ai/docs)
- [Mastra GitHub](https://github.com/mastra-ai/mastra)
- [Examples](https://mastra.ai/examples)
- [Templates](https://mastra.ai/templates)

### Tutorials

- [Building Your First Agent](https://mastra.ai/course)
- [Workflow Guide](https://mastra.ai/docs/workflows/overview)
- [RAG Implementation](https://mastra.ai/docs/rag/overview)

### Community

- [Discord](https://discord.gg/BTYqqHKUrf)
- [Twitter/X](https://x.com/mastra_ai)
- [YouTube](https://www.youtube.com/@mastra-ai)

### LLM Providers

- [OpenAI](https://platform.openai.com/)
- [Anthropic](https://www.anthropic.com/)
- [Google AI](https://ai.google.dev/)
- [More providers](https://mastra.ai/models)

---

## 📝 Additional Notes

### Security

- Docker images run as non-root user (`mastra:1001`)
- Secrets are not included in images
- Sensitive variables only in `.env` (git-ignored)
- Health checks for all services

### Optimizations

- Docker build cache optimized
- Multi-stage to reduce image size
- `.dockerignore` to exclude unnecessary files
- Dependencies installed once

### Backups

Backups are created with compression:

```bash
# Create backup
./scripts/backup-db.sh

# Restore
./scripts/restore-db.sh backups/mastra_backup_20231020_120000.sql.gz
```

---

## 🤝 Contributing

Found a bug or have a suggestion?

1. Fork the repository
2. Create a branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License. See `LICENSE` for details.

---

## 👨‍💻 Author

**Balam Palma**  
GitHub: [@balamcan](https://github.com/balamcan)

---

## ⭐ Was this helpful?

If this docker environment helped you, consider:
- ⭐ Starring the repository
- 🐦 Sharing on social media
- 🤝 Contributing improvements

---

<div align="center">

**Let's build the future with AI! 🚀**

Made with ❤️ using [Mastra](https://mastra.ai)

**Agent Stone - Alan Stone** 🎯  
_A repository for studying and building AI agents_

</div>
