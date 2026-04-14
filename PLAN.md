# Implementation Plan

1. **Repository scaffolding and config surface**
   - Add Docker Compose and `.env.example` for self-hosted n8n, persistent n8n data, and mounted local notes vault path.
   - Define all required Granola/n8n environment variables with conservative defaults.

2. **MODE A workflow (primary)**
   - Build `workflows/granola_api_to_markdown.json` using built-in nodes only.
   - Implement checkpoint handling via workflow static data with bootstrap fallback.
   - Implement paginated List Notes polling, per-note Get Note enrichment, export-folder filtering, deterministic markdown transformation, overwrite writes, and checkpoint update only on full success.

3. **MODE B workflow (fallback)**
   - Build `workflows/granola_zapier_to_markdown.json` starting from Webhook.
   - Normalize payload to same markdown structure/path conventions and write files deterministically.
   - Return explicit JSON success/failure response.

4. **Docs and examples**
   - Add README and docs (`SETUP`, `OPERATIONS`, `TESTING`, `NODE_BY_NODE_BUILD_GUIDE`) with exact setup, testing scenarios, and operations guidance.
   - Add realistic sample payload files for API list/get and Zapier webhook.
   - Add changelog.

5. **Validation and handoff**
   - Validate JSON/doc syntax and ensure all deliverables exist.
   - Commit changes and prepare PR message.
