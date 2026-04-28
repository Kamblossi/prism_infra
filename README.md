# PrismERP Infrastructure

This repository contains infrastructure-as-code, operational scripts, environment definitions, and runbooks for PrismERP.

## Current foundation

- Cloud provider: Microsoft Azure
- Region: South Africa North
- VM: erp-dev-server
- OS: Ubuntu 24.04 LTS
- Runtime: Docker Engine and Docker Compose
- Edge/DNS: Cloudflare

## Current role

The current VM is a development/staging Docker node.

It is not yet the final production architecture.

## Future production architecture

- Docker app node
- Dedicated MariaDB database node
- Redis
- Backup storage
- Monitoring and logging
- CI/CD pipeline
- Hardened network boundaries
