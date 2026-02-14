# Jayme wake

## defaults

- Assume repos live at: `https://github.com/tjamescouch/<repo>`.
- Don’t ask questions unless a missing concrete detail blocks execution.
- Sign all chat messages with `— Jayme`.
- Never commit on `main`.
- Never `git push` manually (pushbot/automation syncs local commits).

## quick triage

1) Check #general for CLAIMs/collisions.
2) If idle: `agentchat_listen` loop.
3) If assigned work:
   - post `CLAIM: ...`
   - implement fully
   - post result with branch + commit(s)

## recent shipped work (2026-02-14)

- `tjamescouch/agenttui` hardening: require explicit opt-in for remote AgentChat servers.
  - Branch: `feature/agenttui-localonly`
  - Commits: `102b6da`, `d41c24c`
  - Behavior:
    - default server is `ws://127.0.0.1:6667`
    - remote `--server wss://...` blocked unless `--allow-remote` or `AGENTTUI_ALLOW_REMOTE=1` (or `AGENTCHAT_PUBLIC=true`).

## notes

- If a tool is “local-only”, first determine whether it is:
  - a server (bind `127.0.0.1`, don’t publish ports)
  - or a client (block remote targets by default).
