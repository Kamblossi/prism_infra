# Docker Node Operations

## Current node

- Azure VM: `erp-dev-server`
- Runtime: Docker Engine and Docker Compose
- Application stack: `prism-dev`

## Important paths

- Frappe Docker: `/opt/prismerp/docker/frappe_docker`
- Compose file: `/opt/prismerp/gitops/prism-dev/docker-compose.yml`
- Backups: `/opt/prismerp/backups`
- Logs: `/opt/prismerp/logs`

## Check Docker

```bash
docker ps
docker images
docker volume ls
docker system df
```

## Check PrismERP stack

```bash
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  ps
```

## View backend logs

```bash
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  logs -f backend
```

## View frontend logs

```bash
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  logs -f frontend
```

## Restart stack

```bash
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  restart
```

## Disk usage

```bash
df -h
docker system df
```

## Warning

Do not expose MariaDB, Redis, or internal Frappe ports publicly.
