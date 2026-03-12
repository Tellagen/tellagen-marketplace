# Tellagen Plugins for Claude Code

Official plugin marketplace for [Tellagen](https://tellagen.com) — investigate incidents with AI, post structured findings, and build incident timelines.

## Install

```bash
/plugin marketplace add tellagen/tellagen-marketplace
/plugin install tellagen@tellagen-marketplace
```

## Configure

Add your Tellagen credentials to `~/.claude/settings.json`:

```json
{
  "env": {
    "TELLAGEN_API_KEY": "tllg_your_key_here",
    "TELLAGEN_API_URL": "https://mycompany.api.tellagen.com"
  }
}
```

| Variable | Description |
|----------|-------------|
| `TELLAGEN_API_KEY` | API key starting with `tllg_`. Create one in **Settings > API Keys**. |
| `TELLAGEN_API_URL` | Your workspace API URL (e.g., `https://mycompany.api.tellagen.com`) |

If you already have other settings in `~/.claude/settings.json`, merge the `env` block into your existing file.

## What's included

- **Tellagen MCP server** (`@tellagen/mcp-server`) — 12 tools for reading incidents, running investigations, posting findings, and promoting to timelines
- **Investigation skill** (`/tellagen:investigate`) — structured workflow that walks Claude through a full incident investigation

## Usage

Run the investigation skill in any project:

```
/tellagen:investigate
```

Or ask Claude directly:

```
Investigate incident #42 on Tellagen
```

## MCP tools

### Read

| Tool | Description |
|------|-------------|
| `tellagen_get_incident` | Get incident details |
| `tellagen_list_incidents` | List incidents, filter by status |
| `tellagen_get_incident_timeline` | Get timeline events |
| `tellagen_list_investigation_runs` | List investigation runs |
| `tellagen_list_findings` | List findings for an incident |
| `tellagen_get_finding` | Get a single finding with evidence refs |

### Write

| Tool | Description |
|------|-------------|
| `tellagen_create_incident` | Declare a new incident |
| `tellagen_start_investigation` | Start an investigation run |
| `tellagen_post_finding` | Post a structured finding |
| `tellagen_complete_investigation` | Close an investigation run |
| `tellagen_update_finding` | Dismiss or restore a finding |
| `tellagen_promote_finding` | Promote a finding to the timeline |

## Works with your existing tools

The plugin composes with any MCP servers you already have configured:

- **Grafana** — query Loki logs, Prometheus metrics, search dashboards
- **GitHub** — check recent deploys, read diffs, search code
- **PagerDuty** — alert history, escalation details
- **Sentry** — error groups, stack traces
- **Kubernetes** — pod restarts, OOMKills, events

No extra configuration on the Tellagen side — just install the vendor MCP servers and Claude uses them.

## Requirements

- [Claude Code](https://claude.ai/claude-code) v1.0.33+
- A [Tellagen](https://tellagen.com) workspace
- An API key with `incidents:read` and `incidents:write` scopes

## License

MIT
