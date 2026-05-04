---
name: indx-admin
description: Manage watched directories, scans, jobs, and startup-scan settings through the indx CLI or API. Use for daemon and indexing administration.
---

> Preview note: this skill reflects the current local indx/Hermes integration workflow used during the Hermes hackathon. Paths and setup details may need adjustment until the public indx beta is released.

Use this skill for indx administration and indexing control.

## When to use

- List, add, update, reorder, or remove watched directories.
- Start scans or metadata rescans.
- Poll jobs and interpret scan progress or failure.
- Get or set the startup scan mode.

## When not to use

- File search, metadata edits, lists, or image sequences. Use `$indx-library`.
- GUI-only tasks that depend on visual confirmation.
- Any workflow that does not actually need daemon administration.

## Minimum context needed

- Prefer local shell access with the `indx` CLI configured.
- If the CLI is unavailable, use the HTTP API with a base URL and bearer token.
- Do not rely on MCP for admin flows in this skill.
- Know the watched-directory id or begin by listing watched directories first.

## Workflow

1. Inspect before mutating.
   - Start with `indx watched-directories list --format compact`.
2. Mutate one directory or one setting at a time.
   - Add, update, reorder, or remove watched directories with explicit ids.
3. Treat scans as async jobs.
   - Every scan command returns a job handle.
   - Poll with `indx jobs wait` or `indx jobs get`.
4. Verify after each admin action.
   - Re-read watched directories after CRUD.
   - Re-run a narrow search after scans if the task depends on indexed results.

## Output discipline

- Prefer `--format compact` for listing directories and polling jobs.
- Avoid full-library scans unless the task explicitly needs them.
- For polling loops, use `jobs wait` rather than manual repeated reads when possible.

## Verification

- After watched-directory changes, re-read the watched-directory list.
- After scans, confirm the job succeeded and then verify expected files through a narrow search or watched-files read.
- After setting changes, re-read the setting immediately.

## Stop and escalate

- Stop if a job returns `failed`; inspect the job payload before retrying.
- Stop if the task asks for a broad rescan without a clear reason.
- Stop if the task is really about metadata or collection organization and switch to `$indx-library`.

## References

- For command recipes, see [examples.md](references/examples.md).
