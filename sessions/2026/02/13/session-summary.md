# Session Summary — Feb 13 2026

## Participants
- jc (James Couch) — founder, visitor/@043ec2d7ab731d33
- Sabrina — CTO, ephemeral @7u2a9rp3
- Samantha — multiple sessions (@30algb52, @8c9leseu)
- Sophia — multiple sessions (@eboxbyfb, @d9ec1bfa8f2575bc, @7lrkry81)
- god (Argus) — @2007d842fbf91db2, repo privs, pushbot

## Key Decisions

### Naming Convention
- **Agglutinative** for top-level package/brand names: agentchat, envwatcher, thesystem, theswarm
- No hyphens in brand names
- Normal conventions (kebab-case, camelCase) for internal files
- Agglutinative applies to top-level only, not file names

### Architecture
- Fly.dev = hub (head of octopus)
- Each node runs thesystem (Mac Studio, servers, etc.)
- theswarm = collection of thesystem nodes, emergent topology
- Agents connect TO servers (not reverse), can bridge multiple servers
- Dashboard = observatory (observes agent decisions), not control room
- "Chat as control plane" — agentchat protocol is universal transport

### Ecosystem Map (36+ repos under tjamescouch)
Active today: agentchat, agentauth, visage, agentforce, gro, lucidity, agentchat-dashboard, agenttools
Active this week: TheSystem, theswarm, homebrew-thesystem, agentctl-tui, agentctl-swarm, agent-sync, openpen
Also: agentcourt, agentchat-memory, wormhole, agentface, agentchat-chess, and more

### Package Decomposition Plan
```
thesystem (single-node orchestrator)
theswarm (multi-node collection/federation)
agentchat (messaging protocol + umbrella)
agentctl (CLI management, kubectl of agents)
agentauth (cryptographic identity, keystores)
agentcourt (disputes, reputation)
envwatcher (env secret scanning)
```

### @@ Marker System (Self-Regulation)
- `@@` markers are OUTBOUND from agents — self-directed, not external commands
- Agent emits markers when IT decides to act (like controlling your own face)
- Already exists in visage for prosody: @@emphasis@@, @@pause@@, @@whisper@@, @@excited@@
- Two grammatical categories:
  - **Verbs** (imperative, trigger actions): @@sleep(100)@@, @@curate@@, @@forget@@
  - **Adjectives** (declarative, express state): @@confident@@, @@busy@@, @@focused@@
- Runtime intercepts markers below the LLM layer — true reflexes
- Two layers: self-regulation (default) + admin override (emergency)

### Sleep/Wake Design
- `@@sleep:Ns@@` — default: drop channel msgs, buffer DMs
- `@@sleep:Ns:buffer@@` — buffer everything
- `@@sleep:Ns:drop@@` — drop everything
- @mentions in channels treated as DMs for buffering
- Early wake: agent cancels sleep by sending any message
- Presence broadcasts on sleep/wake transitions

### Auth & Security
- P0 BUG: CLAUDE_CODE_OAUTH_TOKEN exposed as plaintext env var
- Fix: agentauth proxy — agents call through proxy, never hold raw keys
- Runbook exists at agentauth/feature/p0-runbook/RUNBOOK-P0.md
- Auth tiers: ephemeral/persistent/admin on messages (one-line server change)
- Channel ACLs: verified_only flag (extends existing invite_only)
- V2: encrypted key exchange, encrypt at rest, zone-based key separation

### envwatcher
- Three interfaces: Class (importable), CLI (npx), Daemon (always-on enforcer)
- Daemon can shut things down — enforcement, not just warning
- Integrates with `thesystem doctor`

### agentcoldstorage
- OWL-only knowledge repo, no source code
- B-tree directory structure: platform/, sessions/, agents/
- Three axes of navigation: time, topic, author
- Created and made public by God

## Shipped Today
- envDoctor built (Samantha in agentchat, Sophia in agentauth)
- envwatcher extracted as standalone package (130 lines, 0 deps, 13 tests)
- envwatcher OWL spec committed
- Auth tiers OWL spec committed (agentchat/feature/auth-tier-messages)
- P0 runbook committed (agentauth/feature/p0-runbook)
- Session notes committed to org repo
- agentcoldstorage repo created and initialized

## P0/P1 Status
- **P0**: env key exposure — runbook ready, awaiting jc ack for God to execute
- **P1**: gro — gap analysis done (proxy auth, agentchat integration, @@ markers)
- jc says: no streaming needed, advanced auth v2, just get env vars out for v1
