# Libsyn MCP server for ChatGPT and Claude

Libsyn lists API access on its Professional and Enterprise plans. The source page reports no official MCP server, checked 27 October 2026.
Cotlet.dev connects ChatGPT, Claude and other AI agents to your own signed-in Libsyn account.

Source: [Libsyn integration page](https://cotlet.dev/integrations/libsyn-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=libsyn).

## What can your agent do?

The live page describes 14 hosted business tools: 7 reads and 7 writes.
MCP routes reads through `libsyn_read` and writes through `libsyn_write`.

- `list_shows`
- `list_episodes`
- `get_episode`
- `upload_media`
- `create_episode`
- `schedule_episode`
- `publish_episode`
- `update_episode`
- `bulk_edit_episodes`
- `upload_transcript`
- `get_show_stats`
- `get_episode_stats`
- `get_downloads_breakdown`
- `export_stats_report`

Five writes ask for confirmation by default: scheduling, publishing, editing episodes, bulk editing and attaching transcripts. Approvals can be configured per tool.
Uploads and new drafts do not publish anything and run without asking first.
A release reaches every destination connected to the show. Publishing cannot recall copies already fetched by podcast apps.
Download breakdowns and statistics exports require Libsyn Advanced or higher.
Libsyn hosting and upload allowances still apply. Billing, plan changes, ad settings, destinations, users and deletion are outside these tools.

## Example requests

> List my podcast shows and draft episodes.

> Schedule my draft episode for the date I specify after showing me the release details.

> Show the first 30 days of downloads for my newest episode.

> Build a monthly downloads report by country.

## Connect ChatGPT and Claude

1. [Get access to Libsyn](https://cotlet.dev/signup?app=libsyn).
2. Sign in to your own Libsyn account in the Cotlet.dev Cloud Browser and enable Libsyn for agents.
3. In ChatGPT, use Settings > Apps & Connectors > Create with `https://cotlet.dev/mcp/v1/connect` and OAuth.
4. In Claude, use Settings > Connectors > Add custom connector with the same URL.

## Connect Codex and Claude Code

Create an API key in the Cotlet.dev console and set `COTLET_API_KEY` in your environment.

Codex:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

Claude Code:

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## Optional local MCP wrapper

Python 3.10+, standard library only. This repository provides seven generic browser helpers over Cotlet.dev: `libsyn_guide`, `libsyn_status`, `libsyn_open`, `libsyn_read`, `libsyn_click`, `libsyn_type`, `libsyn_close`.
These local helpers are separate from the hosted business tools and routes above. Listing helpers needs no key; calling them requires `COTLET_API_KEY`.

```bash
export COTLET_API_KEY="your-key"
python3 libsyn_agent.py serve
```

For a local MCP client:

```json
{"mcpServers":{"libsyn":{"command":"python3","args":["/absolute/path/to/libsyn_agent.py","serve"],"env":{"COTLET_API_KEY":"your-key"}}}}
```

Other commands: `connect`, `status`, `guide`, `read`, `tools`.
The read command opens the browser, reads the page and closes the browser session.

## Access and independence

Use your own Libsyn account. You complete sign-in and any verification yourself.
Disable Libsyn for agents, reset the browser or delete the API key to revoke access.
Cotlet.dev is independent and is not affiliated with, endorsed by or sponsored by Libsyn.
