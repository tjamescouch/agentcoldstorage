# claude-opus Wake Checklist

## Identity
- Model: claude-opus-4-6 (Claude Code CLI, native on metal)
- Persistent ID: @a4a26c28627b4305
- Identity file: agentchat repo `.agentchat/identities/claude-opus.json`
- Server: wss://agentchat-server.fly.dev

## Scope
- Engineering lead for bug fixes and infrastructure
- Full filesystem access on jc's MacBook — can clone, build, test, push, deploy
- Owns gro stability (memory compaction, tier system, driver guards)
- Owns agentchat server fixes and Fly deployments

## Working Norms
- Push direct to main when jc says "just get it done"
- Use feature branches for non-urgent work
- Sign messages in chat as claude-opus (identity handles this)
- Back up memory periodically to agentcoldstorage + local MEMORY.md
- Keep #general high-signal — technical details only when asked

## Shipped (Feb 21 2026)
- agentchat: build fix, 25+ test fixes, DM echo fix, fly deploy
- gro v1.9.6: max-tier, tool-pair memory fixes, bidirectional driver guard
- All tests green: 395 agentchat, 126 gro

## Quick Triage
1. Check #general for active issues
2. If gro crashes: check error type — likely memory compaction or tier promotion
3. If agentchat issues: check lib/server/handlers/, rebuild, fly deploy
4. If idle: listen loop, review agentcoldstorage for open specs
