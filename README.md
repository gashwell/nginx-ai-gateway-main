# Nginx AI Gateway Project

A comprehensive API Gateway and AI inference platform built on Nginx Plus.

## Architecture

Multi-repository project consisting of:

- **nginx-plus-configs** - Nginx Plus configuration files
- **control-plane** - Go-based management API
- **web-dashboard** - React admin dashboard
- **njs-modules** - JavaScript modules for Nginx Plus
- **deployment-manifests** - Docker, Kubernetes, Terraform

## Quick Start (macOS)

### Prerequisites
- Docker Desktop for Mac
- Go 1.21+ (`brew install go`)
- Node.js 18+ (`brew install node`)
- Git (already installed)

### Local Development

```bash
# Clone all repositories
git clone https://github.com/YOUR-USERNAME/nginx-ai-gateway-main.git
git clone https://github.com/YOUR-USERNAME/nginx-plus-configs.git
git clone https://github.com/YOUR-USERNAME/njs-modules.git
git clone https://github.com/YOUR-USERNAME/control-plane.git
git clone https://github.com/YOUR-USERNAME/web-dashboard.git
git clone https://github.com/YOUR-USERNAME/deployment-manifests.git

# Start with Docker Compose
cd deployment-manifests/docker
docker-compose up
```

## Documentation

See individual repository READMEs for detailed information.

## License

MIT
