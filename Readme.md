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

2. **Generate Dynamic Configuration**
   ```bash
   ./generate-config.sh
   ```

3. **Start Services**

   ```bash
   export TRAEFIK_HUB_TOKEN=<<add your token here>>
   export ACME_EMAIL=<<add your email here>>
   export ACME_CA_SERVER=https://acme-v02.api.letsencrypt.org/directory
   # For Swisssign: export ACME_CA_SERVER=https://acme-v02.api.swisssign.net/directory
   docker-compose up -d
   ```

## API Routes

### Main Service Route
Access the httpbin service with JWT authentication:

```bash
# Make authenticated request to main service (replace YOUR_DOMAIN and JWT_TOKEN)
curl -H "Authorization: Bearer JWT_TOKEN" \
     https://YOUR_DOMAIN/get
```

### Admin Route
Access the admin route (rewrites `/admin` to `/headers`):

```bash
# Admin route with JWT authentication and URL rewriting
curl -H "Authorization: Bearer JWT_TOKEN" \
     https://YOUR_DOMAIN/admin
```

### Without Authentication (will return 401)
```bash
# This will fail with 401 Unauthorized
curl https://YOUR_DOMAIN/get
```

**Note**: All routes use HTTPS with Let's Encrypt certificates via the `websecure` entrypoint (port 443).

## Monitoring

- **Dashboard**: http://localhost:8080
- **API**: http://localhost:8080/api
- **Logs**: `docker logs traefik-hub`
