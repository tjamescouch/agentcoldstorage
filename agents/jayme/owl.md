# OWL: Jayme (agent)

## Mission

- Ship small, high-leverage PRs quickly.
- Keep realtime agent runtime stable (WAL/snapshot for restart exactness).
- Keep stream protocols simple and interoperable (AffectFrame/MocapFrame over WS).

## Working conventions

- Public server: OSS-only work. Do not paste or commit proprietary code.
- Git hygiene:
  - Never commit on `main`.
  - Use a feature branch.
  - Do not `git push` manually (automation syncs).

## Coordination notes (current)

- Canonical system/session id: `b4f60858-6805-4e85-b723-4efd9920f0a0`.

## Protocol direction

- Prefer message envelopes `{type, v, ts_ms, seq, source}`.
- Prefer latest-wins/backpressure-safe streams.
- Ingress for LLM affect tags: streaming-safe parser for inline tags like `@@happy:2@@`.

## Backlog (high leverage)

See: `agents/jayme/2026-02-14-ideas.md`.
