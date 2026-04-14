# Granola -> Local Markdown (n8n, self-hosted)

Production-oriented n8n automation that exports **selected Granola meeting notes** into local Markdown files.

## What this automation does

- Uses Granola folder membership as the export gate.
- Writes one canonical `.md` file per Granola note ID.
- Overwrites in place when a note changes (no duplicates for same note ID).
- Never deletes local markdown files.

## Supported modes

1. **MODE A (preferred): Granola Personal API polling -> n8n -> local markdown files**
   - Polls every 10 minutes.
   - Uses workflow static data checkpoint (`updated_at`) + bootstrap fallback.
   - Handles list pagination (`cursor`, `hasMore`).

2. **MODE B (fallback): Granola Zapier folder trigger -> n8n webhook -> local markdown files**
   - Receives POST payload from Zapier.
   - Builds the same markdown canonical format.

## Repo structure

- `docker-compose.yml` - self-hosted n8n stack with persistent data + mounted notes vault.
- `.env.example` - explicit config surface.
- `workflows/granola_api_to_markdown.json` - MODE A workflow.
- `workflows/granola_zapier_to_markdown.json` - MODE B workflow.
- `docs/SETUP.md` - install and publish steps.
- `docs/OPERATIONS.md` - runbook and maintenance.
- `docs/TESTING.md` - dry tests + edge-case tests.
- `docs/NODE_BY_NODE_BUILD_GUIDE.md` - manual build/rebuild guide.
- `examples/` - sample payloads.
- `PLAN.md` - short implementation plan.

## Import workflows into n8n

1. Start n8n with Docker Compose (see `docs/SETUP.md`).
2. In n8n UI, import both JSON files from `workflows/`.
3. Configure credentials (Granola bearer token for MODE A).
4. Verify node parameters and environment values.
5. Activate MODE A first, keep MODE B disabled unless needed.

