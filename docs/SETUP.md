# Setup Guide

## 1) Prepare environment file

```bash
cp .env.example .env
```

Edit `.env` and set at minimum:
- `GRANOLA_API_TOKEN`
- `NOTES_HOST_PATH`
- `NOTES_VAULT_ROOT`
- `GRANOLA_EXPORT_FOLDERS`
- `OPTIONAL_SUBPATH_MAP`

## 2) Start self-hosted n8n

```bash
docker compose up -d
```

n8n UI default URL:
- `http://localhost:${N8N_PORT}` (default `http://localhost:5678`)

## 3) Mount your markdown vault path

`docker-compose.yml` mounts host path to container path:
- host: `${NOTES_HOST_PATH}`
- container: `${NOTES_VAULT_ROOT}`

Ensure host folder exists and is writable.

## 4) Create Granola API credential in n8n

For MODE A workflow:
1. In n8n, create credential type **HTTP Bearer Auth**.
2. Name: `Granola Bearer Token`.
3. Token value: `${GRANOLA_API_TOKEN}`.
4. In `List Notes` and `Get Note`, select this credential.

## 5) Import and publish workflows

1. Import `workflows/granola_api_to_markdown.json`.
2. Import `workflows/granola_zapier_to_markdown.json`.
3. Open each workflow and verify:
   - Node credentials are resolved.
   - Expressions reference env vars as expected.
4. Save each workflow.
5. Activate MODE A.
6. Keep MODE B disabled unless Zapier fallback is required.

## 6) Zapier fallback webhook (MODE B)

After activating MODE B, copy webhook production URL:
- `POST https://<your-n8n>/webhook/granola-zapier-export`

In Zapier, map Granola trigger payload fields to JSON body.
Use `examples/sample_zapier_payload.json` as schema reference.

