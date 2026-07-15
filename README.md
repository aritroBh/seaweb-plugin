# SeaWeb plugin

Agent-native search with a real booking rail — SF hotels, attractions, and
restaurants, with honest provenance-labeled results. This plugin connects
Claude to the hosted SeaWeb MCP server.

## Install

1. Mint a free API key at [seaweb.tech](https://seaweb.tech) (Sign in → Generate key).
2. Export it (add to your shell profile to persist):

   ```sh
   export SEAWEB_API_KEY=sw_your-key-here
   ```

3. In Claude Code:

   ```
   /plugin marketplace add aritroBh/seaweb-plugin
   /plugin install seaweb@seaweb
   ```

   Or on claude.ai: Settings → Plugins → Browse → Add marketplace →
   `https://github.com/aritroBh/seaweb-plugin`.

Treat the API key like a password. Revoke it any time from the dashboard.

Other ways to connect (claude.ai custom connector URL, ChatGPT developer
mode, Cursor, Codex, VS Code, Windsurf): https://seaweb.tech/connect
