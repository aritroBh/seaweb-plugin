# SeaWeb plugin

Agent-native search with a real booking rail — SF hotels, attractions, and
restaurants, with honest provenance-labeled results. This repo packages the
hosted SeaWeb MCP server as a plugin for **Claude Code, Cursor, and Codex**.

## Setup (all clients)

1. Mint a free API key at [seaweb.tech](https://seaweb.tech) (Sign in → Generate key).
2. Export it (add to your shell profile to persist):

   ```sh
   export SEAWEB_API_KEY=sw_your-key-here
   ```

Treat the API key like a password. Revoke it any time from the dashboard.

## Claude Code

```
/plugin marketplace add aritroBh/seaweb-plugin
/plugin install seaweb@seaweb
```

Or on claude.ai: Settings → Plugins → Browse → Add marketplace →
`https://github.com/aritroBh/seaweb-plugin`.

The bundled `/setup-seaweb` skill walks Claude through connecting and fixing
auth errors on any surface.

## Claude Cowork, claude.ai, Claude mobile

Cowork sessions run in a remote sandbox — local environment variables never
reach them, so the plugin's bundled server config authenticates only in
Claude Code. On Cowork, claude.ai, and mobile use an account-level custom
connector instead (works everywhere, one-time setup):

Settings → Connectors → Add custom connector →

```text
https://api.seaweb.tech/mcp/k/sw_your-key-here
```

The key rides in the URL path (claude.ai strips query strings). OAuth-based
one-click connect is on the roadmap and will replace the pasted key.

## Cursor

One-click install:

[Add to Cursor](cursor://anysphere.cursor-deeplink/mcp/install?name=seaweb&config=eyJ1cmwiOiJodHRwczovL2FwaS5zZWF3ZWIudGVjaC9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgWU9VUl9TRUFXRUJfS0VZIn19)

Then replace `YOUR_SEAWEB_KEY` with your key in Cursor Settings → MCP.
Or add to `~/.cursor/mcp.json` yourself:

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

## Codex

```sh
codex plugin marketplace add https://github.com/aritroBh/seaweb-plugin
codex plugin add seaweb
```

Or add to `~/.codex/config.toml` yourself:

```toml
[mcp_servers.seaweb]
url = "https://api.seaweb.tech/mcp"
bearer_token_env_var = "SEAWEB_API_KEY"
```

## Other clients

claude.ai custom connector URL, ChatGPT developer mode, VS Code, Windsurf:
https://seaweb.tech/connect
