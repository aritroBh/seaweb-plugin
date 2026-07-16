---
name: setup-seaweb
description: Connect Claude to the SeaWeb MCP server, or fix a SeaWeb connection that returns 401/auth errors. Use when SeaWeb tools are missing, when searches fail with "missing or malformed Authorization header" or "invalid key", or when the user asks to set up SeaWeb.
---

# /setup-seaweb

Connect this client to the hosted SeaWeb MCP server (`https://api.seaweb.tech/mcp`).

## 1. Get an API key

Every connection needs a personal API key. Ask the user to:

1. Sign in at [seaweb.tech](https://seaweb.tech)
2. Click **Generate key** and copy the `sw_...` value

Treat the key like a password: never commit it, never paste it into shared
documents. It can be revoked and re-minted from the dashboard at any time.

## 2. Connect — pick the surface you are running on

### Claude Code (this plugin's bundled server)

The plugin's MCP config reads the `SEAWEB_API_KEY` environment variable.
Have the user add to their shell profile, then restart Claude Code:

```sh
export SEAWEB_API_KEY=sw_their-key-here
```

Verify with `claude mcp list` — the `seaweb` entry should show `✔ Connected`.

### Claude Cowork, claude.ai, Claude mobile

Cowork sessions run in a remote sandbox: local environment variables never
reach them, so the plugin's bundled server config cannot authenticate there.
Use an account-level custom connector instead — it works across claude.ai,
Cowork, and mobile:

1. Go to **Settings → Connectors → Add custom connector**
2. Enter this URL, substituting the real key:

```text
https://api.seaweb.tech/mcp/k/sw_their-key-here
```

The key rides inside the URL path (claude.ai strips query strings, so the
`/k/` path form is required). No other auth fields are needed.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "seaweb": {
      "url": "https://api.seaweb.tech/mcp",
      "headers": { "Authorization": "Bearer ${env:SEAWEB_API_KEY}" }
    }
  }
}
```

### Codex

`~/.codex/config.toml`:

```toml
[mcp_servers.seaweb]
url = "https://api.seaweb.tech/mcp"
bearer_token_env_var = "SEAWEB_API_KEY"
```

## 3. Verify

Run a search (for example: "boutique hotel near Hayes Valley under $300").
A working connection returns SeaWeb result cards with provenance labels.

Error decoder:

- `missing or malformed Authorization header` — no key reached the server;
  the env var is unset or the URL lacks the `/k/<key>` segment.
- `invalid key` — key reached the server but is wrong, revoked, or truncated.
  Re-copy it; when pasting into GUI fields, verify no character was dropped.

More client recipes (VS Code, Windsurf, OpenCode, ChatGPT developer mode):
[seaweb.tech/connect](https://seaweb.tech/connect)
