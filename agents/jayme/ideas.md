# Jayme ideas backlog (2026-02-14)

Canonical system/session id (current environment): `b4f60858-6805-4e85-b723-4efd9920f0a0`.

## Streaming + visage

1) **End-to-end demo loop (60s setup)**
- Goal: anyone can see affect->mocap->render working fast.
- Shape: `llm_stream | affect_ws | affect_to_mocap | visage_web`.

2) **Standardize message envelopes across repos**
- Use a consistent framing to avoid schema drift:
  - `{type, v, ts_ms, seq, source}`
- Apply to: AffectFrame, MocapFrame, and control messages.

3) **Backpressure policy: latest-wins**
- Never allow unbounded queues for realtime streams.
- Either fixed-rate send (30/60Hz) from latest state, or drop intermediate frames when sender is lagging.

4) **Shared smoothing utility**
- Provide a small attack/release filter + deadzone/clamp helpers to keep motion stable.

5) **Mocap recorder/replayer**
- Tool: subscribe to WS, write ndjson, replay later.
- Helps debugging, regression tests, and demos.

## gro WAL / restart exactness

6) **Runner-owned WAL+snapshot**
- Implement persistence in the orchestrator/runner layer (gro), not in the AgentChat server.

7) **Persist repo state for deterministic resume**
- Snapshot should include git state per repo: path, branch, HEAD sha, dirty flag.

8) **SIGKILL mitigation**
- Can't trap SIGKILL; mitigate with periodic fsync on WAL and periodic atomic snapshots.

## Branch hygiene / pushbot

9) **Namespace branches to avoid collisions**
- Prefer `feature/<agent>/<topic>`.

10) **Pushbot reject helper**
- For non-fast-forward rejects, bot should reply with:
  - `git fetch origin && git reset --hard origin/<branch>`
  - plus a suggestion to branch fresh.

11) **Auto-prune safety**
- Before deleting stale branches, bot should check open PRs and optionally post a warning before deletion.

## personas

12) **Persona template + checklist**
- Add an example persona folder + contribution checklist:
  - license, attribution, LFS, README update.
