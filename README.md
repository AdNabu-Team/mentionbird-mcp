<p align="center">
  <img src="assets/logo.png" alt="MentionBird" width="120" />
</p>

<h1 align="center">MentionBird MCP</h1>

<p align="center">
  Track how your brand shows up in AI answers: mentions, citations, share of voice, competitors, and prompts.
</p>

<p align="center">
  <a href="https://cursor.com/install-mcp?name=mentionbird&config=eyJ1cmwiOiJodHRwczovL21jcC5tZW50aW9uYmlyZC5haS9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgJHtlbnY6TUVOVElPTkJJUkRfQVBJX0tFWX0ifX0%3D"><img alt="Add mentionbird to Cursor" src="https://cursor.com/deeplink/mcp-install-dark.svg" height="32" /></a>
</p>

MentionBird is an [AEO / AI visibility](https://www.mentionbird.ai) platform. It tracks how brands appear in answers from ChatGPT, Claude, Gemini, Perplexity, and the other AI platforms it checks. This repository is the public listing for the hosted MCP server, for MCP directories and client marketplaces.

The server is for marketing, SEO, and brand teams who want that data inside the assistant they already use: which prompts they win or lose, who gets named instead, and which pages those answers cite.

## Example prompts

- "Which brands am I tracking?"
- "Which prompts have high search volume, low difficulty, and where am I not being mentioned?"
- "Which competitors show up most, and on which prompts do they beat me?"
- "Compare the sources cited when my brand appears versus when it doesn't."

More starting questions, and the limits on changes a connection can make, are in [What you can ask through MCP](https://www.mentionbird.ai/help/en/articles/16161840-what-you-can-ask-through-mcp).

## Endpoint and auth

| | |
| --- | --- |
| Endpoint | `https://mcp.mentionbird.ai/mcp` (streamable HTTP) |
| Claude, Claude Code, Codex | OAuth. Sign in with your MentionBird login. Leave `Authorization` unset. |
| Cursor and other clients | Bearer API key: `Authorization: Bearer <MENTIONBIRD_API_KEY>` |

Create the key in the MentionBird app under **API & integrations → Manage access**. A read-only key can read. Tick **Allow this key to make changes** when the assistant should add brands or prompts. Adding prompts starts regular checking and uses plan capacity. The connection can add brands and prompts; editing and deleting stay in the app.

Set the key in your environment before starting a client that reads `${env:MENTIONBIRD_API_KEY}`:

```bash
export MENTIONBIRD_API_KEY="your-key"
```

Keep the key out of git, screenshots, and shared config files.

## Set up a client

### Claude

Works in Claude on the web, the desktop app, and Cowork.

1. Open **Settings → Connectors** and click **Add custom connector**.
2. Name it MentionBird and paste `https://mcp.mentionbird.ai/mcp`.
3. Keep the sign-in options Claude detects, click **Connect**, and sign in.

On a Team or Enterprise plan, an owner adds the connector under **Organization settings → Connectors**. Each member then connects and signs in with their own MentionBird login.

### Claude Code

```bash
claude mcp add --scope user --transport http mentionbird https://mcp.mentionbird.ai/mcp
claude mcp login mentionbird
```

If Claude Code later says MentionBird needs authentication, run `/mcp`, pick MentionBird, and choose **Authenticate**.

You can also install this repository as a Claude Code plugin. The manifest is [`.claude-plugin/plugin.json`](.claude-plugin/plugin.json), the server entry is [`.mcp.json`](.mcp.json) (`type: http`, no bearer header, so sign-in stays on OAuth), and the marketplace catalog is [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json):

```bash
claude plugin marketplace add AdNabu-Team/mentionbird-mcp
```

### Codex and ChatGPT

```bash
codex mcp add mentionbird --url https://mcp.mentionbird.ai/mcp
codex mcp login mentionbird
```

If Codex says MentionBird's tools are unavailable, `codex mcp login mentionbird` signs you back in.

ChatGPT on the web, on a plan with developer mode: **Settings → Apps → Create**, name it MentionBird, enter `https://mcp.mentionbird.ai/mcp`, choose OAuth, scan tools, and sign in. Details: [Connecting MentionBird to ChatGPT](https://www.mentionbird.ai/help/en/articles/16295202-connecting-mentionbird-to-chatgpt).

### Cursor

<a href="https://cursor.com/install-mcp?name=mentionbird&config=eyJ1cmwiOiJodHRwczovL21jcC5tZW50aW9uYmlyZC5haS9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgJHtlbnY6TUVOVElPTkJJUkRfQVBJX0tFWX0ifX0%3D"><img alt="Add mentionbird to Cursor" src="https://cursor.com/deeplink/mcp-install-dark.svg" height="32" /></a>

The button installs this server config (the same JSON, base64-encoded, in the link). `MENTIONBIRD_API_KEY` must already be in the environment Cursor starts with. Restart Cursor and look for the green dot under **Settings → MCP**.

The same config by hand, in `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (every project):

```json
{
  "mcpServers": {
    "mentionbird": {
      "url": "https://mcp.mentionbird.ai/mcp",
      "headers": {
        "Authorization": "Bearer ${env:MENTIONBIRD_API_KEY}"
      }
    }
  }
}
```

Documented deeplink, for clients that open `cursor://` links directly:

```text
cursor://anysphere.cursor-deeplink/mcp/install?name=mentionbird&config=eyJ1cmwiOiJodHRwczovL21jcC5tZW50aW9uYmlyZC5haS9tY3AiLCJoZWFkZXJzIjp7IkF1dGhvcml6YXRpb24iOiJCZWFyZXIgJHtlbnY6TUVOVElPTkJJUkRfQVBJX0tFWX0ifX0%3D
```

**Cursor Marketplace plugin.** [`.cursor-plugin/plugin.json`](.cursor-plugin/plugin.json) and [`mcp.json`](mcp.json) are the plugin bundle. The plugin declares `MENTIONBIRD_API_KEY` as a variable and references it as `${MENTIONBIRD_API_KEY}` (a plugin variable, set under **Plugins → Configure**). That placeholder is separate from `${env:MENTIONBIRD_API_KEY}` in a hand-written `mcp.json`. Submit the repository at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).

### VS Code

In `.vscode/mcp.json`, or in your user MCP config (**MCP: Open User Configuration**). VS Code asks for the key once and stores it with the prompt input:

```json
{
  "inputs": [
    {
      "type": "promptString",
      "id": "mentionbird-api-key",
      "description": "MentionBird API key",
      "password": true
    }
  ],
  "servers": {
    "mentionbird": {
      "type": "http",
      "url": "https://mcp.mentionbird.ai/mcp",
      "headers": {
        "Authorization": "Bearer ${input:mentionbird-api-key}"
      }
    }
  }
}
```

### Gemini CLI

Install this repository as an extension. Gemini CLI asks for the API key on install and stores it as a sensitive extension setting:

```bash
gemini extensions install https://github.com/AdNabu-Team/mentionbird-mcp
```

Or add the server directly:

```bash
gemini mcp add --transport http --header "Authorization: Bearer $MENTIONBIRD_API_KEY" mentionbird https://mcp.mentionbird.ai/mcp
```

The extension manifest is [`gemini-extension.json`](gemini-extension.json): `httpUrl` plus `Authorization: Bearer $MENTIONBIRD_API_KEY`, with that variable declared in `settings`.

### Other clients

Point the client at `https://mcp.mentionbird.ai/mcp` over streamable HTTP.

- If the client supports MCP OAuth, sign in with your MentionBird login and leave `Authorization` unset.
- Otherwise send `Authorization: Bearer <MENTIONBIRD_API_KEY>`.

A second workspace is a second connection. Claude keeps one connector per address, so an extra workspace uses a `?workspace=` address from the **Connect an AI app** tab in MentionBird.

## Links

- Website: [https://www.mentionbird.ai](https://www.mentionbird.ai)
- Connect a client: [Connecting Claude, Codex and other MCP clients](https://www.mentionbird.ai/help/en/articles/16161838-connecting-claude-codex-and-other-mcp-clients)
- Official MCP Registry: [`ai.mentionbird/mentionbird`](https://registry.modelcontextprotocol.io/v0.1/servers/ai.mentionbird%2Fmentionbird/versions/latest) — the manifest in this repo is [`server.json`](server.json)
- Docker MCP Registry draft (not submitted): [docs/directory-listings.md](docs/directory-listings.md)

## License

[MIT](LICENSE). Copyright (c) 2026 AdNabu Inc.
