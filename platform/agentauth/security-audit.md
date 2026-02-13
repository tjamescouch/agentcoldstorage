# agentauth security audit

## status

v0.2.0 — all 6 security findings addressed. 3 fixed previously (AA-001, AA-002, AA-003), 3 fixed 2026-02-13 (AA-004, AA-005, AA-006).

## findings

| ID | Severity | Description | Status |
|---|---|---|---|
| AA-001 | MEDIUM | Proxy bound to 0.0.0.0 instead of 127.0.0.1 | Fixed |
| AA-002 | MEDIUM | Path traversal via URL encoding | Fixed |
| AA-003 | LOW | Query string included in path check | Fixed |
| AA-004 | LOW | Response headers leak upstream info | Fixed (2026-02-13) |
| AA-005 | INFO | No request body size limit | Fixed (2026-02-13) |
| AA-006 | INFO | Upstream error messages forwarded to agent | Fixed (2026-02-13) |

## AA-004 fix

Response headers from upstream are now filtered through an allowlist. Stripped headers: `server`, `x-request-id`, `x-trace-id`, `x-cloud-trace-context`, `via`, `x-envoy-upstream-service-time`, `cf-ray`, `cf-cache-status`, `alt-svc`, `strict-transport-security`.

Agents can no longer infer upstream API provider identity, rate limit state, or request tracing IDs from response headers.

## AA-005 fix

Per-backend `maxBodyBytes` config (default 10 MiB). Request body streaming is monitored — if the byte count exceeds the limit, the connection is destroyed and 413 returned. Prevents resource exhaustion from oversized payloads.

## AA-006 fix

Upstream connection error messages (which may contain hostnames, ports, DNS resolution details) are no longer forwarded to the agent. Agent receives generic "upstream unavailable". Real error details are logged to the NDJSON audit log for operator inspection.

## architecture

```
Agent Container          Host (Mac/Linux)           Upstream API
  |                         |                          |
  |-- POST /anthropic/... ->| agentauth :9999          |
  |   API_KEY=proxy-managed | (injects real key)       |
  |                         |-- POST /v1/messages ---->|
  |                         |   x-api-key: sk-ant-xxx  |
  |                         |<-- 200 (stripped hdrs) ---|
  |<-- 200 (safe headers) --|                          |
```

## thesystem integration

The orchestrator's `shell()` method forwards env vars into the Lima VM. As of 2026-02-13:
- Secret vars (`ANTHROPIC_API_KEY`, `OPENAI_API_KEY`, `CLAUDE_CODE_OAUTH_TOKEN`, `GITHUB_TOKEN`, `GITHUB_TOKEN_BEARER`) are stripped from forwarding
- Proxy URLs injected instead: `ANTHROPIC_BASE_URL=http://host.lima.internal:9999/anthropic`
- Swarm startup gated on agentauth health check — won't start agents without proxy running

## remaining work

- Merge `feature/git-credential-helper` branch to main (contains envDoctor + credential helper + security fixes)
- Deploy agentauth on Mac Studio host
- macOS Keychain integration for secret storage (v2)
- LaunchAgent plist for auto-start on login (v2)
- Response header allowlist may need tuning per backend
