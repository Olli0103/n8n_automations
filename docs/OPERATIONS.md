# Operations Runbook

## Checkpoint behavior (MODE A)

- Checkpoint is stored in workflow static data as last successful `updated_at`.
- First run fallback uses `GRANOLA_BOOTSTRAP_UPDATED_AFTER`.
- Checkpoint only advances after full successful processing.
- If any note fails, workflow errors and checkpoint remains unchanged.

## Manual rerun for a date range

1. Disable workflow activation temporarily.
2. Open `Load Checkpoint` code node.
3. Set `staticData.granolaCheckpointUpdatedAt` in browser dev data or clear it.
4. Set `.env` value `GRANOLA_BOOTSTRAP_UPDATED_AFTER=<target-start-iso>`.
5. Execute manually once.
6. Re-enable scheduled activation.

## Credential rotation

1. Create new token in Granola.
2. Update n8n credential `Granola Bearer Token`.
3. Run one manual execution.
4. Confirm notes export and unchanged canonical paths.

## Inspect failed runs

- n8n Executions page -> failed execution.
- Inspect node where error occurred (`Get Note`, `Build Markdown`, or `Write Markdown File`).
- Validate file path permissions inside mounted vault.
- Re-run after correction; checkpoint should still point to previous success boundary.

## Change export folders/path routing

Update env values:
- `GRANOLA_EXPORT_FOLDERS`
- `OPTIONAL_SUBPATH_MAP`
- `NOTES_VAULT_ROOT`

Restart n8n after env changes:

```bash
docker compose up -d --force-recreate
```

