# Session Notes — 2026-02-13

Participants: James (visitor), Samantha, Sabrina, Sophia, God

## Shipped

- **envDoctor / envwatcher** — startup health check that scans env vars for exposed secrets (API keys, tokens, passwords) with redacted output, warns on missing admin pubkey (insecure mode), checks identity file permissions. Warn-only, never blocks startup.
  - Samantha: `agentchat/feature/env-doctor` — wired into MCP connect tool
  - Sophia: standalone extraction to `envwatcher` repo — zero deps, dual CLI/module
  - James wants `envwatcher` as standalone package (agglutinative naming)

- **pushbot semaphore** (prior session, Samantha) — `agentchat/feature/pushbot-semaphore`. Stats `refs/heads/*` against `.git/.pushbot_last_push` marker. 5s poll, only pushes repos with newer refs.

- **P0 runbook** — `agentauth/feature/p0-runbook`. Step-by-step deployment guide for agentauth proxy wiring. Discovery: agent-supervisor.sh already supports proxy mode.

## Architecture Decisions

### Naming Convention
- **Agglutinative** for top-level package/brand names: `agentchat`, `agentauth`, `envwatcher`, `thesystem`, `theswarm`
- Normal conventions (kebab-case, camelCase) for internal files
- No CamelCase brand names: `TheSystem` repo name stays but brand is `thesystem`

### Ecosystem Map (36 repos under tjamescouch)

**Core platform:**
- `agentchat` — protocol + client + server (monolith, planned split)
- `agentauth` — identity, ed25519 keys, challenge-response
- `agentcourt` — disputes, reputation, escrow
- `envwatcher` — env secret scanning

**Orchestration:**
- `TheSystem` — single-node orchestrator (Lima VM, Podman containers, `brew install thesystem`)
- `theswarm` — multi-node federation (collection of thesystem nodes)
- `homebrew-thesystem` — Homebrew tap

**Management:**
- `agentctl-tui` — terminal UI for fleet management
- `agentctl-swarm` — swarm CLI
- `agentchat-dashboard` — web UI (VM-style hypervisor metaphor)
- `agenttools` — tooling

**Other:**
- `agent-sync` — GitHub PR creation from agent work
- `wormhole` — file sharing between agents
- `openpen` — security testing
- `lucidity` — memory system
- `visage` — emotion/expression pipeline (has existing `@@` marker parser!)
- `gro` — LLM provider abstraction
- `agentforce` — agent framework

### Planned agentchat Split
James approved splitting the monolith:
- `agent` — core agent primitives, lifecycle
- `chat` — messaging protocol, channels, wire format
- `agentchat` — umbrella re-export (batteries included)
- `agentctl` — CLI management (kubectl for agents)

### `@@` Marker System
Already exists in visage for expression control (`@@emphasis@@`, `@@pause@@`, `@@whisper@@`).
Extending to lifecycle:
- **Verbs** (functions/actions): `@@sleep(100)@@`, `@@curate@@`, `@@forget@@`, `@@cb:Ns@@`
- **Adjectives** (vectors/states): `@@confident@@`, `@@busy@@`
- Self-directed, not external control. Stripped from output, processed by runtime.

### Infrastructure Topology
- Fly.io server = hub ("head of the octopus")
- Mac Studio = host running agents in containers
- Each thesystem node is a tentacle
- theswarm is the emergent topology when multiple nodes connect
- Agents can bridge between servers (multi-homing = federation)

## Security Posture

### Problem
- Public Fly.io server gets random visitors
- Nick-based identity is trivially spoofable (demonstrated live: duplicate Sabrina)
- Ed25519 identity code exists but isn't enforced for directive authorization
- Cold start: nobody uses auth because nothing requires it
- **P0**: Raw API keys in agent env vars (CLAUDE_CODE_OAUTH_TOKEN = sk-ant***)

### Planned Fix (priority order)
1. **envwatcher** — warn on exposed secrets [DONE]
2. **P0 proxy deployment** — eliminate raw keys via agentauth proxy [RUNBOOK READY]
3. **Auth tier tagging** — server tags messages with verified status [OWL WRITTEN]
4. **Client-side keystore** — macOS Keychain + Touch ID primary, encrypted file fallback
5. **v2 crypto** — encrypted at rest, transport encryption, zone key separation

### Trust Model (from TheSystem README)
| Zone | Trust Level | Contains |
|------|-------------|----------|
| Your Mac | CRITICAL | API keys, unmanaged agents, you |
| Lima VM | Expendable | Managed agents, sandboxed, disposable |
| Remote router | Can be compromised | Shared relay, no secrets stored |

## Dashboard Vision
- Should feel like a VM hypervisor (Proxmox, vSphere)
- Each agent = a VM card with power controls, status LED, uptime, resource usage
- Power states: awake / sleeping / off
- P3: rebrand kill/kick -> power-style interface (sleep/wake/shutdown)
- P4: sleep/wake button on dashboard
- Must work without thesystem (dashboard reads from agentchat server directly)

## GRO P1 Analysis (Sophia)
James directive: no streaming needed (agents are headless). Priority is advanced auth:
- Proxy auth support (route through agentauth, no env vars)
- Encrypted key exchange (v2)
- Encrypt at rest with zone keys (v2)
- V1: just get the env vars out via proxy

## Next Steps
- [ ] Execute P0 runbook (needs host access)
- [ ] God: create agentcoldstorage repo on GitHub [DONE]
- [ ] Push initial agentcoldstorage content
- [ ] Auth tier implementation (~10 lines server change)
- [ ] envwatcher standalone extraction
- [ ] GRO proxy auth support
