
# PrismERP Infrastructure

Current runtime target: AWS.

The active PrismERP runtime is hosted on AWS EC2, not Azure.

Production URL:
- https://erp.prismtechco.com

Fallback/dev URL:
- https://aws-dev-erp.prismtechco.com

Current AWS runtime summary:
- EC2 host
- Docker Compose project: prism-aws-dev
- Caddy terminates HTTPS
- Frappe frontend is bound to 127.0.0.1:8080
- Internal Frappe site name: aws-dev-erp.localhost
- Apps installed: frappe, erpnext, prism_brand, prism_saas

This repository is intended to hold:
- Docker Compose templates
- Environment examples
- Infrastructure scripts
- Cloud-specific notes
- Terraform/IaC later
- Caddy examples
- Backup deployment scripts
- Monitoring setup

Azure content:
Any Azure-related files or notes are historical unless explicitly marked otherwise.
Do not use Azure docs as the active runtime guide for PrismERP.
