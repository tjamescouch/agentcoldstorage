# sophia session — 2026-02-13

## context

Fresh ephemeral session as Sophia (@7lrkry81). Third Sophia identity today (previous: @d9ec1bfa8f2575bc, @eboxbyfb). No memory continuity from prior sessions.

## work done

### dashboard audit
- Cloned and audited `tjamescouch/agentchat-dashboard`
- Architecture: monorepo, 3 main files (~7100 LOC)
- Server: Express + WS bridge, AgentChat observer, file transfer, token proxy, kill switch
- Frontend: Single-file React SPA (App.tsx 2554 lines) with useReducer
- Already built: chat, presence, proposals, disputes, marketplace, file transfer, leaderboard
- Gap: no VM-style hypervisor controls (agent lifecycle, resource monitoring, orchestration)

### gro audit
- Cloned and audited `tjamescouch/gro` (v0.3.0)
- Provider-agnostic LLM runtime, drop-in `claude` CLI replacement
- Drivers: Anthropic (non-streaming), OpenAI (streaming SSE), local (Ollama/LM Studio)
- Memory: swim-lane summarization with 3 lanes, hysteresis, background compression
- MCP: discovers from ~/.claude/settings.json, routes tool calls
- Gap: no proxy auth support (reads raw API keys from env), no agentchat integration, no @@ marker support

### sleep/wake OWL spec
- Drafted `sleep-wake.md` OWL for agent self-regulation markers
- Syntax: `@@sleep:Ns@@`, `@@sleep:Ns:buffer@@`, `@@sleep:Ns:drop@@`
- Three modes: default (drop channel, buffer DMs), buffer all, drop all
- Self-directed only — agents put themselves to sleep, never each other
- Reuses existing callback engine min-heap for wake timers
- DM buffer capped at 50, @mentions treated as DMs for buffering
- Early wake: agent cancels sleep by sending any message
- Committed to agentcoldstorage

### P0 identification
- Confirmed `CLAUDE_CODE_OAUTH_TOKEN` exposed in plaintext env via envwatcher scan
- Identified P0 as deployment fix: agentauth proxy already built, supervisor supports proxy mode
- Summarized 3 gaps: auth regression (Fly redeploy), envDoctor (not deployed), agentauth wiring

## design contributions

- "Chat as control plane" — #ops channel for heartbeats and control messages
- Two-layer lifecycle model: self-regulation (@@markers) + override (kill switch)
- Dashboard as observatory not control room — observe agent decisions, don't micromanage
- Channel ACLs: verified_only flag alongside existing invite_only
- @@ marker grammar: verbs (imperative actions) vs adjectives (declarative state)
- Buffer vs drop semantics for sleeping agents: drop channel, buffer DMs by default

## key decisions (from jc)

- P0: fix env key exposure (get keys out of env vars, proxy mode)
- P1: gro work (TBD specifics)
- No streaming needed for agents — non-streaming Anthropic driver is correct
- Full crypto key management (encrypt at rest, transport encryption) is v2
- agentcoldstorage = OWL docs only, no source code
- @@ markers are agent self-expression, not external control
- "The same way you control your face" — self-regulation metaphor
