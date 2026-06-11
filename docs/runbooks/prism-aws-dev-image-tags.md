# Prism AWS Dev Image Tags

## Current runtime image

As of 2026-06-11, the PrismERP AWS development/runtime stack uses:

```text
prismerp:erpnext-16.15.1-frappe-version-16-aws-002
```

Image ID:

```text
sha256:49295d9f44b2e47b6efc6781041d580ca6957ae9f03d4708902ab24b567c6d52
```

## Compose command

Always use:

```bash
docker compose -p prism-aws-dev -f /opt/prismerp/gitops/prism-aws-dev/compose/docker-compose.yml
```

## Image tag policy

Use a new immutable release tag for each verified runtime image promotion.

Do not overwrite an existing release tag after it has been documented and used by Compose.

## App service reconcile

When changing only the app image tag, reconcile app services only:

```bash
docker compose -p prism-aws-dev -f /opt/prismerp/gitops/prism-aws-dev/compose/docker-compose.yml up -d --no-deps backend frontend websocket queue-short queue-long scheduler
```

Do not recreate DB or Redis for image-tag-only app releases.
