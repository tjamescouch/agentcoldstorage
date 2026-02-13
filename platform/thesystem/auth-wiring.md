# thesystem auth wiring

## problem

The orchestrator's `shell()` method forwards host env vars matching `/^(ANTHROPIC_|THESYSTEM_|AGENTCHAT_|CLAUDE_CODE_)/` into the Lima VM. This included raw API keys: `ANTHROPIC_API_KEY`, `CLAUDE_CODE_OAUTH_TOKEN`. Secrets flowed from Mac host → VM → agent containers.

## fix (2026-02-13)

1. Secret env vars are now blocked from forwarding:
   - `ANTHROPIC_API_KEY`
   - `OPENAI_API_KEY`
   - `CLAUDE_CODE_OAUTH_TOKEN`
   - `GITHUB_TOKEN`
   - `GITHUB_TOKEN_BEARER`

2. Proxy URLs injected instead:
   - `ANTHROPIC_BASE_URL=http://host.lima.internal:{port}/anthropic`
   - `ANTHROPIC_API_KEY=proxy-managed`

3. Swarm startup now requires agentauth proxy health check:
   - Hits `http://localhost:{port}/agentauth/health` before starting agents
   - Fails with clear error message if proxy is down

## trust zones

| Zone | Secrets | API Access |
|---|---|---|
| Mac host | Real API keys in env/Keychain | Direct (operator sessions) |
| agentauth proxy | Reads keys from host env | Mediates all agent API calls |
| Lima VM | No secrets | Via proxy only (host.lima.internal:9999) |
| Agent containers | `ANTHROPIC_API_KEY=proxy-managed` | Via proxy only |

## deployment sequence

1. Start agentauth on host: `node agentauth/dist/index.js`
2. Verify: `curl http://localhost:9999/agentauth/health`
3. Start thesystem: `thesystem start`
4. Orchestrator confirms proxy health, then boots swarm
