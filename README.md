# Nginx AI Gateway

A comprehensive API Gateway for intelligent AI model routing, built on Nginx Plus.

## Overview

Nginx AI Gateway provides a unified interface for routing requests to multiple AI providers (OpenAI, Anthropic, Ollama) with intelligent model selection based on prompt complexity and user tier.

## Features

- **Intelligent Routing**: Automatically selects the best AI model based on prompt length and user subscription tier
- **Multi-Provider Support**: OpenAI (GPT-4, GPT-3.5), Anthropic (Claude 3), Ollama (Llama2)
- **Rate Limiting**: Per-user rate limiting with configurable quotas
- **API Key Management**: Create and manage API keys with tiered access levels
- **Real-time Analytics**: Monitor usage, costs, and performance
- **Local LLM Support**: Run Llama2 locally via Ollama for free-tier users
- **Web Dashboard**: React-based admin UI for monitoring and management

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        Web Dashboard                             │
│                     http://localhost:3000                        │
└──────────────────────────┬──────────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────────┐
│                      Control Plane                               │
│                   http://localhost:8000                          │
│            (API Key Management, Analytics)                       │
└──────────────────────────┬──────────────────────────────────────┘
                           │
┌──────────────────────────▼──────────────────────────────────────┐
│                    Nginx Plus Gateway                            │
│                   https://localhost:443                          │
│              (NJS Routing, Rate Limiting)                        │
└───────┬─────────────────┬─────────────────┬─────────────────────┘
        │                 │                 │
        ▼                 ▼                 ▼
   ┌─────────┐      ┌──────────┐      ┌─────────┐
   │ OpenAI  │      │ Anthropic│      │  Ollama │
   │  API    │      │   API    │      │ (Local) │
   └─────────┘      └──────────┘      └─────────┘
```

## Repositories

| Repository | Description |
|------------|-------------|
| [nginx-ai-gateway-main](.) | Main documentation (this repo) |
| [nginx-plus-configs](../nginx-plus-configs) | Nginx Plus configuration with NJS routing |
| [njs-modules](../njs-modules) | JavaScript modules for intelligent routing |
| [control-plane](../control-plane) | Go API server for management |
| [web-dashboard](../web-dashboard) | React admin dashboard |
| [deployment-manifests](../deployment-manifests) | Docker Compose, Kubernetes, Terraform |

## Quick Start (macOS)

### Prerequisites

- Docker Desktop for Mac (`brew install --cask docker`)
- Git

### 1. Clone All Repositories

```bash
mkdir -p ~/projects/nginx-ai-gateway && cd ~/projects/nginx-ai-gateway

# Clone all repos
for repo in nginx-ai-gateway-main nginx-plus-configs njs-modules control-plane web-dashboard deployment-manifests; do
  git clone https://github.com/YOUR-USERNAME/$repo.git
done
```

### 2. Initialize and Start

```bash
cd deployment-manifests/docker
make init    # Creates .env and SSL certs
make up      # Starts all services
```

### 3. Configure API Keys

Edit `deployment-manifests/docker/.env`:
```
OPENAI_API_KEY=sk-your-openai-key
ANTHROPIC_API_KEY=sk-ant-your-anthropic-key
```

### 4. Verify Installation

```bash
make status  # Check service status
make check   # Health check all services
```

## Access Points

| Service | URL | Description |
|---------|-----|-------------|
| AI Gateway | https://localhost | Main AI routing endpoint |
| Dashboard | http://localhost:3000 | Web admin interface |
| Control Plane | http://localhost:8000 | Management API |
| Nginx Plus API | http://localhost:8080/api/ | Nginx metrics |

## Test the Gateway

```bash
# Health check
curl -k https://localhost/health

# AI request (replace with your API key)
curl -k -X POST https://localhost/v1/chat/completions \
  -H "Authorization: Bearer sk-premium-test-123456" \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {"role": "user", "content": "Explain quantum computing in one paragraph"}
    ]
  }'
```

## User Tiers

| Tier | Models Available | Use Case |
|------|-----------------|----------|
| Premium | GPT-4, Claude 3 Sonnet, GPT-3.5, Llama2 | Production applications |
| Standard | GPT-3.5, Claude 3 Sonnet, Llama2 | Development, moderate usage |
| Free | Llama2 (local) | Testing, experimentation |

## Routing Logic

The gateway intelligently routes requests based on:

1. **User Tier**: Premium users get access to more powerful models
2. **Prompt Length**: Longer prompts route to models with larger context windows
3. **Explicit Model Request**: Users can request specific models if their tier allows

## Development

### Run Individual Services

```bash
# Control Plane
cd control-plane && make run

# Dashboard
cd web-dashboard && npm start

# Nginx (Docker)
cd nginx-plus-configs && docker build -t nginx-ai-gateway .
```

### View Logs

```bash
cd deployment-manifests/docker
docker-compose logs -f nginx-plus
docker-compose logs -f control-plane
```

## Contributing

See individual repository READMEs for contribution guidelines.

## License

MIT
