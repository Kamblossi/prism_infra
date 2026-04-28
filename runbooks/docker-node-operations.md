# Docker Node Operations

## Current node
- **Azure VM**: erp-dev-server
- **Runtime**: Docker Engine and Docker Compose
- **Application stack**: prism-dev

## Important paths
- **Frappe Config**: /opt/prismerp/docker/frappe_docker
- **Compose File**: /opt/prismerp/gitops/prism-dev/docker-compose.yml
- **Backups**: /opt/prismerp/backups
- **Logs**: /opt/prismerp/logs

## Check Docker
Run these to verify the health of the Docker engine:
```bash
docker ps
docker images
docker volume ls
docker system df


#Check PrismERP stack
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  ps

#View logs
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  logs -f backend

#Restart stack
docker compose \
  --project-name prism-dev \
  -f /opt/prismerp/gitops/prism-dev/docker-compose.yml \
  restart

#Disk usage
#Monitor these to prevent database corruption from a full disk
df -h
docker system df


