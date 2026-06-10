# Current AWS Runtime

Status: Active current runtime.

Public URLs:
- https://erp.prismtechco.com
- https://aws-dev-erp.prismtechco.com

Internal site:
- aws-dev-erp.localhost

Runtime:
- AWS EC2
- Docker Compose project: prism-aws-dev
- Caddy on host for HTTPS
- Frappe frontend on 127.0.0.1:8080
- MariaDB and Redis are container-internal

Current production status:
- Production URL is active.
- This is not yet full SaaS tenant provisioning.
- The current system is still single-site.
- Multi-tenant lifecycle, Clerk authentication, and automated tenant provisioning are future phases.

Operational notes:
- Do not rename the internal Frappe site casually.
- Do not edit ERPNext/Frappe core for branding.
- Branding lives in prism_brand.
- SaaS lifecycle logic belongs in prism_saas.
- Infrastructure automation belongs here in prism_infra.
