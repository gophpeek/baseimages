---
title: "Available Images"
description: "Complete matrix of all PHPeek base image tags, variants, and architectures"
weight: 40
---

# Available Images

Complete reference of all available PHPeek base image tags and variants.

## Image Registry

All images are published to GitHub Container Registry:

```
ghcr.io/gophpeek/baseimages/{image-type}:{tag}
```

## Image Tiers

All images come in three tiers to match your needs:

| Tier | Tag Suffix | Size (Alpine) | Best For |
|------|------------|---------------|----------|
| **Slim** | `-slim` | ~120MB | APIs, microservices |
| **Standard** | (none) | ~250MB | Most apps (DEFAULT) |
| **Full** | `-full` | ~700MB | Browsershot, Dusk, PDF |

## Multi-Service Images (PHP-FPM + Nginx)

Single container with both PHP-FPM and Nginx - perfect for simple deployments.

### Standard Tier (Default)

| Image Tag | PHP | OS | Size | Architecture |
|-----------|-----|----|----- |--------------|
| `php-fpm-nginx:8.4-alpine` | 8.4 | Alpine | ~250MB | amd64, arm64 |
| `php-fpm-nginx:8.3-alpine` | 8.3 | Alpine | ~250MB | amd64, arm64 |
| `php-fpm-nginx:8.2-alpine` | 8.2 | Alpine | ~250MB | amd64, arm64 |

### Slim Tier

Optimized for APIs and microservices with minimal footprint:

| Image Tag | PHP | OS | Size | Architecture |
|-----------|-----|----|----- |--------------|
| `php-fpm-nginx:8.4-alpine-slim` | 8.4 | Alpine | ~120MB | amd64, arm64 |
| `php-fpm-nginx:8.3-alpine-slim` | 8.3 | Alpine | ~120MB | amd64, arm64 |
| `php-fpm-nginx:8.2-alpine-slim` | 8.2 | Alpine | ~120MB | amd64, arm64 |

### Full Tier

Includes Chromium for Browsershot, Dusk, and PDF generation:

| Image Tag | PHP | OS | Size | Architecture |
|-----------|-----|----|----- |--------------|
| `php-fpm-nginx:8.4-alpine-full` | 8.4 | Alpine | ~700MB | amd64, arm64 |
| `php-fpm-nginx:8.3-alpine-full` | 8.3 | Alpine | ~700MB | amd64, arm64 |
| `php-fpm-nginx:8.2-alpine-full` | 8.2 | Alpine | ~700MB | amd64, arm64 |

### Rootless Variants

All tiers support rootless execution (runs as `www-data` user):

| Image Tag | Tier | Description |
|-----------|------|-------------|
| `php-fpm-nginx:8.4-alpine-rootless` | Standard | Default + rootless |
| `php-fpm-nginx:8.4-alpine-slim-rootless` | Slim | Slim + rootless |
| `php-fpm-nginx:8.4-alpine-full-rootless` | Full | Full + rootless |

## Tag Format

```
{type}:{php_version}-{os}[-tier][-rootless]

Examples:
php-fpm-nginx:8.4-alpine              # Standard tier (default)
php-fpm-nginx:8.4-alpine-slim         # Slim tier
php-fpm-nginx:8.4-alpine-full         # Full tier
php-fpm-nginx:8.4-alpine-rootless     # Standard + rootless
php-fpm-nginx:8.4-alpine-slim-rootless  # Slim + rootless
php-fpm-nginx:8.4-alpine-full-rootless  # Full + rootless
```

## Rolling Tags (Recommended)

Rolling tags receive weekly security updates:

```yaml
# Automatically gets security patches every Monday
image: ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine
```

## Immutable SHA Tags

For reproducible builds, use SHA-pinned tags:

```yaml
# Locked to specific build
image: ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine@sha256:abc123...
```

## Architecture Support

All images are built for multiple architectures:

| Architecture | Platform | Examples |
|--------------|----------|----------|
| `amd64` | x86_64 | Intel/AMD servers, most cloud VMs |
| `arm64` | aarch64 | Apple Silicon, AWS Graviton, Raspberry Pi 4+ |

Docker automatically pulls the correct architecture:

```bash
# Works on both AMD64 and ARM64
docker pull ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine
```

## OS Variant Comparison

| Feature | Alpine |
|---------|--------|
| **Base Size** | ~5MB |
| **Package Manager** | apk |
| **libc** | musl |
| **Security Updates** | Weekly |
| **Compatibility** | Good |
| **Best For** | Production, size |

## Tier Comparison

| Feature | Slim | Standard | Full |
|---------|------|----------|------|
| **Size (Alpine)** | ~120MB | ~250MB | ~700MB |
| **Core Extensions** | ✅ 25+ | ✅ 25+ | ✅ 25+ |
| **ImageMagick** | ❌ | ✅ | ✅ |
| **vips** | ❌ | ✅ | ✅ |
| **Node.js 22** | ❌ | ✅ | ✅ |
| **Chromium** | ❌ | ❌ | ✅ |
| **Best For** | APIs, microservices | Most apps | Browser automation |

## Version Support

| PHP Version | Status | Security Support Until |
|-------------|--------|------------------------|
| PHP 8.4 | Active | November 2028 |
| PHP 8.3 | Active | November 2027 |
| PHP 8.2 | Active | December 2026 |

**Recommendation**: Use PHP 8.4 for new projects, PHP 8.3 for production stability.

## Usage Examples

### Docker CLI

```bash
# Pull standard tier (most Laravel/PHP apps)
docker pull ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine

# Pull slim tier (APIs, microservices)
docker pull ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine-slim

# Pull full tier (Browsershot, Dusk)
docker pull ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine-full

# Run with volume mount
docker run -p 8000:80 -v $(pwd):/var/www/html \
  ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine
```

### Docker Compose

```yaml
services:
  # Standard tier - most Laravel apps
  app:
    image: ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine
    ports:
      - "8000:80"
    volumes:
      - ./:/var/www/html

  # Slim tier - API service
  api:
    image: ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine-slim
    ports:
      - "8001:80"

  # Full tier - PDF generation service
  pdf:
    image: ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine-full
    environment:
      PHP_MEMORY_LIMIT: "1G"
```

### Dockerfile

```dockerfile
# Standard tier for most apps
FROM ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine

COPY --chown=www-data:www-data . /var/www/html

RUN composer install --no-dev --optimize-autoloader
```

```dockerfile
# Full tier for Browsershot
FROM ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine-full

COPY --chown=www-data:www-data . /var/www/html

RUN composer install --no-dev --optimize-autoloader
```

## Weekly Security Rebuilds

All images are automatically rebuilt every Monday at 03:00 UTC:

- Latest upstream PHP patches
- Latest OS security updates
- CVE scanning with Trivy
- Multi-architecture builds

**Stay secure**: Pull images regularly to get security patches.

```bash
# Pull latest security patches
docker pull ghcr.io/gophpeek/baseimages/php-fpm-nginx:8.4-alpine
docker-compose up -d --pull always
```

---

**Need help choosing?** See [Choosing a Variant](../getting-started/choosing-variant.md) | [Image Tiers Comparison](editions-comparison.md)
