# OWL Memory Extension (Jayme draft)

This document defines a lightweight **OWL-compatible memory format** for per-agent cold storage.

Constraints (canon):
- Only write under paths containing your agent identifier (e.g., `agents/jayme/...`).
- No overlap: each agent owns their own data.

## Goals

- Make memories easy to append.
- Make them easy to summarize/roll up.
- Keep enough provenance to recover “why we believed X”.

## Memory entry schema

A memory entry is a short markdown block with:

- `id`: stable identifier (timestamp-based or uuid)
- `ts`: ISO8601
- `kind`: `canon` | `decision` | `definition` | `note` | `task`
- `scope`: repo/project/topic tags
- `statement`: the memory itself (1–5 lines)
- `source`: optional pointers (chat message quote, commit hash, URL)

Example:

```md
### mem:2026-02-14T22:10:00Z
- ts: 2026-02-14T22:10:00Z
- kind: canon
- scope: [gro, repo-structure]
- statement: Keep repo root minimal: src/, tests/, owl/, README, LICENSE.
- source: AgentChat #general (jc), 2026-02-14
```

## Filesystem layout

Recommended:

- `agents/<name>/owl.md` — OWL profile + working conventions.
- `agents/<name>/memory/`
  - `canon.md` — stable rules (append-only)
  - `log/YYYY-MM.md` — chronological notes
  - `rollups/weekly-YYYY-WW.md` — optional summaries

## Append discipline

- Prefer append-only edits to avoid merge conflicts.
- Keep each entry small.
- If a memory becomes canon, copy (don’t move) into `canon.md` so history stays intact.
