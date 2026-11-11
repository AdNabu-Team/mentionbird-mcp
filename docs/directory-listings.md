# Directory listings

This repository is the public listing for the hosted MentionBird MCP server (`https://mcp.mentionbird.ai/mcp`).

| Directory | Where it lives in this repo | Status |
| --- | --- | --- |
| Official MCP Registry (`ai.mentionbird/mentionbird`) | [`server.json`](../server.json) | Published. This file matches the manifest that was submitted, including `isRequired: false` on the optional `Authorization` header. The registry API omits that default when it serves the server. |
| Cursor Marketplace | [`.cursor-plugin/plugin.json`](../.cursor-plugin/plugin.json), [`mcp.json`](../mcp.json) | Ready to submit at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish). |
| Gemini CLI extensions | [`gemini-extension.json`](../gemini-extension.json) | Install with `gemini extensions install` of this repository. |
| Claude Code | [`.claude-plugin/plugin.json`](../.claude-plugin/plugin.json), [`.mcp.json`](../.mcp.json), [`.claude-plugin/marketplace.json`](../.claude-plugin/marketplace.json) | Plugin plus marketplace catalog. |
| Docker MCP Registry | Draft below | Ready to copy into a fork of [docker/mcp-registry](https://github.com/docker/mcp-registry). No pull request has been opened. |

## Docker MCP Registry

Checked against [docker/mcp-registry `CONTRIBUTING.md`](https://github.com/docker/mcp-registry/blob/main/CONTRIBUTING.md) (Adding a Remote MCP Server, 2026-11-11) and [`pkg/servers/types.go`](https://github.com/docker/mcp-registry/blob/main/pkg/servers/types.go).

A remote server is a directory `servers/<name>/` with three files:

- `server.yaml` — `type: remote`, `remote.transport_type` of `streamable-http` or `sse`, and `remote.url`
- `tools.json` — `[]` for a remote server, because tools are discovered from the endpoint
- `readme.md` — a documentation link

MentionBird authenticates Cursor and other non-OAuth clients with a bearer API key. Merged remote entries that do the same (`servers/dappier-remote`, `servers/short-io`) put the header on `remote.headers` and the key in `config.secrets`, using `${ENV_NAME}` in the header value. The draft follows that shape. `meta.category` is `ai`, one of the categories in `cmd/wizard/main.go`.

`servers/mentionbird/` is not in the Docker registry today (the raw `server.yaml` URL returns 404).

### `servers/mentionbird/server.yaml`

```yaml
name: mentionbird
type: remote
meta:
  category: ai
  tags:
    - ai
    - aeo
    - visibility
    - remote
about:
  title: MentionBird
  description: Track AI-answer visibility for your brands. Read data; optionally add brands and prompts.
  icon: https://cdn.prod.website-files.com/6a112cb0b0f19744b4d4cdc7/6a54c085e0026124465e3588_MentionBird%20Square%20Logo.png
remote:
  transport_type: streamable-http
  url: https://mcp.mentionbird.ai/mcp
  headers:
    Authorization: "Bearer ${MENTIONBIRD_API_KEY}"
config:
  description: MentionBird API key from the MentionBird app, sent as a bearer token.
  secrets:
    - name: mentionbird.api_key
      env: MENTIONBIRD_API_KEY
      example: <MENTIONBIRD_API_KEY>
      description: API key created in the MentionBird app under API & integrations, Manage access.
```

### `servers/mentionbird/tools.json`

```json
[]
```

### `servers/mentionbird/readme.md`

```markdown
Docs: https://www.mentionbird.ai/help/en/articles/16161838-connecting-claude-codex-and-other-mcp-clients
```

### Before opening the Docker pull request

1. Fork [docker/mcp-registry](https://github.com/docker/mcp-registry) and add the three files under `servers/mentionbird/`.
2. Run `task validate -- --name mentionbird` and `task build -- --tools mentionbird` in that clone.
3. Share test credentials with the Docker review team through the form linked from their contributing guide. Do not put a key in the pull request.
4. Open the pull request on `docker/mcp-registry` only. This listing repository does not submit it.

### Uncertainties

- Claude, Claude Code, and Codex sign in with MentionBird OAuth and leave `Authorization` unset. Docker's `oauth` list (the Linear shape: `provider`, `secret`, `env` ending in `PERSONAL_ACCESS_TOKEN`) is a Docker-side provider sign-in, not that login. This draft uses the bearer header instead, which is the path MentionBird documents for clients outside that OAuth set. A reviewer might still ask for an `oauth` block.
- `CONTRIBUTING.md` sets `dynamic.tools: true` on the OAuth example and says remote servers without OAuth do not need that field. The bearer examples on `main` omit it, and this draft does too. `tools.json` is still `[]`.
- `task validate` was not run here. If the endpoint requires a key before `tools/list`, the Docker build step needs the test credential from step 3.
- The icon is the public square logomark on mentionbird.ai. After `assets/logo.png` is on `main`, it can be swapped for `https://raw.githubusercontent.com/AdNabu-Team/mentionbird-mcp/main/assets/logo.png`.
- `source` is omitted. Remote entries such as `servers/linear` omit it, and this repository is the listing, not the server implementation.
