# Decision 0001: Current Cloud Runtime is AWS

Date: 2026-06-10

## Status
Accepted.

## Decision
PrismERP’s current active runtime target is AWS.

The active production URL is:
- https://erp.prismtechco.com

The fallback/dev URL is:
- https://aws-dev-erp.prismtechco.com

The internal Frappe site name remains:
- aws-dev-erp.localhost

## Context
Earlier project materials contained Azure references. Those references are now historical unless explicitly reactivated.

The current working deployment is on AWS EC2 using Docker Compose and Caddy.

## Consequences
- New infrastructure work should target AWS first.
- prism_infra documentation should make AWS the default path.
- Azure notes may be retained, but must be clearly marked as historical.
- Future IaC, monitoring, backup automation, and deployment scripts should be AWS-first.
