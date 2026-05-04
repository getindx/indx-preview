---
name: indx-library
description: Search, inspect, annotate, and organize an indx media library through the CLI or API. Use for file search, metadata work, tags, lists, and image sequences.
---

> Preview note: this skill reflects the current local indx/Hermes integration workflow used during the Hermes hackathon. Paths and setup details may need adjustment until the public indx beta is released.

Use this skill for library-facing work against an existing indx index.

## When to use

- Search or filter the indexed library.
- Inspect file metadata, tags, ratings, custom fields, lists, or image sequences.
- Update file metadata on one or more files.
- Create, rename, or delete lists and image sequences.
- Add or remove files from lists or image sequences.

## When not to use

- Watched-directory CRUD, scans, jobs, or startup-scan settings. Use `$indx-admin`.
- GUI-only tasks that depend on visual inspection.
- Filesystem changes outside indx itself.

## Minimum context needed

- Prefer local shell access with the `indx` CLI already configured.
- If the CLI is unavailable, use the HTTP API with a base URL and bearer token.
- Use MCP only when the host already has indx MCP configured and the task is library-oriented.
- Start with the smallest possible task scope: a search term, file path, list name, or sequence name.

## Workflow

1. Choose the narrowest surface.
   - Prefer the `indx` CLI for local work.
   - Prefer the HTTP API for remote or embedded integrations.
   - Treat MCP as optional and secondary.
2. Bootstrap cheaply.
   - Start with `indx stats --format compact`.
   - Use targeted discovery subcommands instead of the full discovery payload when possible.
3. Search small.
   - Use bounded searches, for example `limit: 20`, before expanding.
4. Inspect only what you need.
   - Start with `indx files metadata get --path ... --view basic --format compact`.
   - Switch to `--view full` only when raw embedded analysis is needed.
5. Mutate with the stable metadata patch shape.
   - `rating` sets the rating.
   - `tags_add`, `tags_remove`, and `tags_set` control tags.
   - `title`, `description`, `creator`, and `custom` update those fields directly.
6. Verify after each mutation.
   - Re-read the file, list, or sequence immediately after the write.

## Output discipline

- Prefer `--format compact` for intermediate machine-to-machine steps.
- Keep result sets bounded unless the task genuinely needs breadth.
- Use `files metadata get --view basic` by default to avoid pulling large embedded payloads.
- Do not dump full discovery unless the task truly needs a fresh bootstrap.

## Verification

- For search tasks, verify the returned file set matches the requested filters.
- For metadata writes, re-read the same file paths and confirm the changed fields.
- For list or image-sequence updates, re-read membership after every mutation.

## Stop and escalate

- Stop if the task actually requires watched-directory or scan control and switch to `$indx-admin`.
- Stop if the task requires broad library rewrites without a clear filter or file list.
- Stop if the CLI/API response shape does not match the expected contract and inspect one narrow read before proceeding.

## References

- For command recipes, see [examples.md](references/examples.md).
