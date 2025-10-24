# Traefik Hub API Gateway

A complete Traefik Hub configuration with JWT authentication, URL rewriting, and Let's Encrypt SSL certificates.

## Features

- 🔐 **JWT Authentication** with Keycloak integration
- 🔄 **URL Rewriting** for admin routes
- 🛡️ **HTTPS/SSL** with Let's Encrypt certificates
- 📁 **File Provider** for dynamic configuration

## Dynamic Configuration

The `config/dynamic.yml` file contains:

- **Routes**: 
  - `httpbin` - Main service route with JWT auth
  - `httpbin-admin` - Admin route with JWT + URL rewrite (`/admin` → `/headers`)
- **Middlewares**:
  - `jwt-auth` - JWT validation using Keycloak JWKS
  - `admin-rewrite` - Path rewriting for admin endpoints
- **Services**: Load balancer configuration for httpbin container

## Quick Start

1. **Setup Environment**
   ```bash
   # Copy and configure environment variables
   cp .env.example .env
   # Edit .env with your values
   ```

3. **Generate Dynamic Configuration**
   ```bash
   ./generate-config.sh
   ```

4. **Start Services**

   ```bash
   export TRAEFIK_HUB_TOKEN=<<add your token here>>
   export ACME_EMAIL=<<add your email here>>
   docker-compose up -d
   ```

## Monitoring

- **Dashboard**: http://localhost:8080
- **API**: http://localhost:8080/api
- **Logs**: `docker logs traefik-hub`
