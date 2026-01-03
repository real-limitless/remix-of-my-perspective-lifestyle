# Podman Compose Setup

This project can be run using Podman Compose with the provided configuration files.

## Prerequisites

- Podman installed on your system
- podman-compose installed (`pip install podman-compose` or use your package manager)

## Available Services

### Production Build (`web`)
The production service builds the application and serves it using Nginx.

### Development Server (`dev`)
The development service runs Vite's dev server with hot module replacement.

## Quick Start

### Production Mode

Build and run the production container:

```bash
podman-compose up -d web
```

The application will be available at: http://localhost:8080

### Development Mode

Run the development server with hot reload:

```bash
podman-compose --profile dev up dev
```

The development server will be available at: http://localhost:5173

## Common Commands

### Build images
```bash
# Build production image
podman-compose build web

# Build development image
podman-compose build dev
```

### Start services
```bash
# Start production
podman-compose up -d web

# Start development
podman-compose --profile dev up dev
```

### Stop services
```bash
# Stop all services
podman-compose down

# Stop and remove volumes
podman-compose down -v
```

### View logs
```bash
# View production logs
podman-compose logs -f web

# View development logs
podman-compose logs -f dev
```

### Rebuild and restart
```bash
# Production
podman-compose up -d --build web

# Development
podman-compose --profile dev up --build dev
```

## Port Mappings

- **Production**: Port 8080 → Container port 80 (Nginx)
- **Development**: Port 5173 → Container port 5173 (Vite dev server)

## Notes

- The production build uses a multi-stage Docker build to optimize image size
- Nginx is configured with gzip compression and static asset caching
- The development container mounts your source code for hot reloading
- Health checks are configured for the production container
- Both services use Bun as the package manager

## Troubleshooting

If you encounter permission issues with Podman, you may need to:

1. Run Podman in rootless mode (recommended)
2. Or adjust SELinux contexts if enabled: `podman-compose up -d --security-opt label=disable`

For any port conflicts, modify the port mappings in `compose.yml`.
