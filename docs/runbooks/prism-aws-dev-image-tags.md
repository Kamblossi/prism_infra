# Prism AWS Dev Image Tags

## Current runtime image

As of 2026-06-12, the PrismERP AWS development/runtime stack uses:

```text
prismerp:erpnext-16.15.1-frappe-version-16-aws-009
```

Image ID:

```text
sha256:786a66e75f60674817cd319c73ab75140aa614e1c524fdc2cc1c9b99b618b2e3
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
