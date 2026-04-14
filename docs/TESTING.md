# Testing Guide

Use **manual execution** for safe dry runs before enabling schedule.

## Pre-test setup

- Import workflows.
- Configure Granola bearer credential.
- Point `NOTES_HOST_PATH` to a disposable test vault directory.

## MODE A tests

1. **First run bootstrap**
   - Clear static checkpoint.
   - Set `GRANOLA_BOOTSTRAP_UPDATED_AFTER` to known older timestamp.
   - Run once and verify exports.

2. **Update existing note overwrite**
   - Change note content in Granola, keep same note ID.
   - Re-run workflow.
   - Verify same filename updated (mtime changes), no new duplicate file.

3. **Skip note outside export folder**
   - Put note in non-export folder.
   - Confirm note counted as skipped and no file created.

4. **Empty transcript**
   - Use note with no transcript.
   - Confirm file has Summary and omits Transcript section.

5. **Missing title fallback**
   - Use note with empty title.
   - Confirm fallback to calendar event title, else note ID.

6. **Pagination across pages**
   - Ensure API returns `hasMore: true` then next cursor page.
   - Verify all pages processed before finishing.

7. **Failed write prevents checkpoint advance**
   - Temporarily make target directory unwritable.
   - Run workflow; expect failure.
   - Confirm checkpoint did not advance.

## MODE B tests

1. Activate webhook workflow.
2. POST sample payload:

```bash
curl -X POST "http://localhost:5678/webhook/granola-zapier-export" \
  -H "content-type: application/json" \
  --data @examples/sample_zapier_payload.json
```

3. Validate response JSON (`ok: true`, `file_path`, summary counts).
4. Validate output markdown file content and overwrite behavior by re-sending with updated content.

## Output verification checklist

- One file per note ID.
- Filename includes `__NOTEID.md`.
- YAML frontmatter present and clean (omit empty fields).
- Summary preserves markdown.
- Transcript section present only when enabled and non-empty.

