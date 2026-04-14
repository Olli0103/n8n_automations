# Node-by-Node Build Guide

## MODE A: `granola_api_to_markdown.json`

1. **Schedule**
   - Every 10 minutes.
2. **Load Checkpoint** (Code)
   - Read workflow static data.
   - Apply bootstrap fallback.
   - Load env config.
3. **List Notes** (HTTP Request)
   - GET `${GRANOLA_API_BASE_URL}/notes`
   - Query: `updated_after`, `cursor`.
4. **Accumulate Page** (Code)
   - Append current page notes.
   - Capture `hasMore` and next `cursor`.
5. **Has More?** (If)
   - True -> back to `List Notes`.
   - False -> continue.
6. **Emit Candidates** (Code)
   - Emit one item per listed note.
7. **Loop Candidates** (Split in Batches, 1)
   - Deterministic sequential processing.
8. **Get Note** (HTTP Request)
   - GET `${GRANOLA_API_BASE_URL}/notes/{id}`
   - Query `include_transcript`.
9. **Build Markdown** (Code)
   - Filter export folders.
   - Build canonical markdown + path.
10. **Should Write?** (If)
    - Skip non-export-folder notes.
11. **Convert To File**
    - Markdown text -> binary.
12. **Write Markdown File**
    - Write binary to computed path (overwrite).
13. **Mark Success** (Code)
    - Increment run counters.
14. **Update Checkpoint** (Code)
    - Fail if any errors.
    - Persist checkpoint only on success.

## MODE B: `granola_zapier_to_markdown.json`

1. **Webhook**
   - POST `/webhook/granola-zapier-export`.
2. **Build Markdown** (Code)
   - Normalize Zapier payload to canonical metadata/body.
3. **Ready To Write?** (If)
   - Skip or proceed.
4. **Write Markdown File**
   - Overwrite canonical path.
5. **Respond Success**
   - Return machine-readable JSON result.

## Notes on deterministic behavior

- Stable filename always includes note ID.
- Sanitized title slug only affects readability, not identity.
- Re-export updates same file path.
- No delete operations are used.

