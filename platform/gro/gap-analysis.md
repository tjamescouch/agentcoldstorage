# gro gap analysis

audited v0.3.0 on 2026-02-13. ~670 lines main.ts, well-structured with clear OWL specs.

## what's solid

- anthropic driver (direct HTTP, non-streaming — correct for headless agents)
- openai streaming driver (SSE, tool call accumulation, reasoning tokens)
- swim-lane summarization (3-lane: assistant/system/user, hysteresis, background compression)
- MCP tool support (discover from ~/.claude/settings.json, route calls, stdio transport)
- session persistence (save/load to .gro/context/<id>/)
- CLI flag compat with `claude` (graceful degradation for unsupported flags)
- rate limiter, timed fetch with abort
- zero runtime deps beyond @modelcontextprotocol/sdk

## gaps

### proxy auth support (P0-adjacent)
- `resolveApiKey()` reads raw env vars (ANTHROPIC_API_KEY, OPENAI_API_KEY)
- needs --auth-proxy flag or ANTHROPIC_BASE_URL support to route through agentauth
- the openai streaming driver already accepts baseUrl — anthropic driver hardcodes api.anthropic.com
- fix: make anthropic driver accept configurable base URL, then proxy handles auth

### agentchat integration
- gro is single-agent headless, no way to connect to agentchat network
- could add --agentchat flag that connects via MCP server (agentchat MCP already exists)
- or: gro stays headless, thesystem wraps gro + agentchat MCP together

### @@ marker support
- gro doesn't parse or emit @@ markers from model output
- if gro is the agent runtime, it needs to intercept @@sleep@@, @@cb:Ns@@ from model responses
- visage already has @@ parser in visage/tts/src/markers.js — could extract as shared module

### token counting
- character heuristic (avgCharsPerToken=4) — rough but functional
- could use tiktoken or provider tokenizers for accuracy
- low priority — the heuristic works well enough for budget management

### tool result handling
- large tool outputs fed back as raw strings
- no truncation or summarization for oversized results
- the 24KB cap in totalChars() is a partial mitigation

## recommendation

proxy auth support is the immediate need (ties to P0). agentchat integration and @@ markers are the path to gro becoming the standard agent runtime. token counting and tool result handling are polish.
