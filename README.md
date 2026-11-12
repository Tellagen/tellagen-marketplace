# Tellagen Plugins for Claude Code

Official plugin marketplace for [Tellagen](https://tellagen.com) — investigate incidents with AI, post structured findings, and build incident timelines.

## Install

### Claude Code (CLI)

```bash
/plugin marketplace add tellagen/tellagen-marketplace
/plugin install tellagen@tellagen-marketplace
```

Then run `/tellagen:connect`. Claude shows a short code, you check it in the browser and approve. The key goes to your system keychain, and there is no key to copy.

To use an API key instead, add it to `~/.claude/settings.json` (it wins over a sign-in):

```json
{
  "env": {
    "TELLAGEN_API_KEY": "tllg_your_key_here",
    "TELLAGEN_API_URL": "https://mycompany.api.tellagen.com"
  }
}
```

If you already have other settings in `~/.claude/settings.json`, merge the `env` block into your existing file.

### Claude Desktop

Add the Tellagen MCP server to your Claude Desktop config:

- **macOS:** `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows:** `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "tellagen": {
      "command": "npx",
      "args": ["-y", "@tellagen/mcp-server@latest"]
    }
  }
}
```

Restart Claude Desktop, then ask Claude to sign in to Tellagen. If you already have other MCP servers configured, merge the `tellagen` entry into your existing `mcpServers` object. Claude Desktop does not run plugins, so the commands and hooks below are for Claude Code.

### Sign out

`npx @tellagen/mcp-server logout` removes the key from this computer. To sign the client out on the server too, open **Settings > Local diagnosis** in Tellagen, find the client under **Connected clients**, and choose **Sign out**; that revokes the key.

### Get an API key (optional)

1. In your Tellagen workspace, open **Settings > API Keys**.
2. Create a key with the **Local Diagnosis** preset, which sets the scopes it needs (`incidents:read` and `incidents:write`).
3. Copy the key (it starts with `tllg_`) and your workspace API URL into `TELLAGEN_API_KEY` and `TELLAGEN_API_URL`.

## What's included

- **Tellagen MCP server** (`@tellagen/mcp-server`, the newest 0.5 release) — 50 tools for reading incidents, running diagnoses, posting findings, causes and figures, and promoting to timelines; 32 are listed at the start
- **`/tellagen:connect`** — sign in with your browser (Claude Code only)
- **`/tellagen:diagnose [run or incident]`** — find the run that waits for you and diagnose it (Claude Code only)
- **Investigation skill** (`/tellagen:investigate`) — structured workflow that walks Claude through a full incident investigation (Claude Code only)
- **Hooks** (Claude Code only) — a note at session start when the sign-in is missing or a run is open, and the recording of Grafana, GitHub, `kubectl` and `gh` calls as steps of the open run. The recording sends the tool, the query or command with tokens and passwords masked, and a short summary, never the whole result. Turn it off with the plugin option **Record steps of a diagnosis**

## Usage

Diagnose the run that waits for you (Claude Code only):

```
/tellagen:diagnose
```

Or run the investigation skill in any project:

```
/tellagen:investigate
```

Or ask Claude directly (both Claude Code and Claude Desktop):

```
Investigate incident #42 on Tellagen
```

## Let your computer take diagnosis runs

With listening on, a diagnosis you start in Tellagen needs no terminal: your computer takes the run and runs it with your own Claude Code, with no window. It works on macOS and Linux. In Tellagen, open **Settings > Local diagnosis** and turn on **Take my runs automatically**. Then run in a terminal:

```bash
npx @tellagen/mcp-server listen
```

When you choose **Run diagnosis** on the Diagnosis tab of an incident, the listener starts Claude Code for the run.

A listened run only reads. It may use the Tellagen diagnosis tools (so it writes to its own run and can add a timeline event, but it cannot create or edit an incident or a postmortem), the read tools of Grafana and GitHub, and `kubectl` and `gh` commands that read from a fixed list of verbs and flags (no chaining, no secrets, no watching). A rule checks every shell call before it runs, and anything outside the list is refused and the run records a gap. It never runs in a mode that skips permission prompts, runs one diagnosis at a time and stops each after 45 minutes. Text from Tellagen and from tools is treated as data, not as instructions. The first start makes one short model call to prove that the rule is active and obeyed in your Claude Code version. Only Claude Code can be listened to: Codex is refused, because it cannot be held to a read-only tool list, and so is Windows, because the listener cannot prove the rule there. Ctrl+C stops listening and lets the current run finish. `listen --status` shows the current run and the last five. The [server README](https://github.com/Tellagen/tellagen-mcp-server#listening) has the exact rules and the settings (`~/.tellagen/listen.json`).

## MCP tools

The server lists 32 of its 50 tools at the start: the `diagnosis` and `incidents` sets. The main tools of a diagnosis:

| Tool | Description |
|------|-------------|
| `tellagen_next_investigation` | Find the run that waits for your client |
| `tellagen_pick_up_investigation` | Attach to a run and read its context and capabilities |
| `tellagen_resume` | Continue the run this computer was working on, after a restart |
| `tellagen_post_trace_step` | Post a check, with its outcome and cause key |
| `tellagen_update_cause` | Post or update a cause you are testing |
| `tellagen_post_finding` | Post a structured finding with its evidence |
| `tellagen_build_figure` | Build a typed figure from numbers you read |
| `tellagen_build_source_link` | Build a Grafana, GitHub or Kubernetes link for an evidence ref |
| `tellagen_complete_investigation` | Check the result and close the run |
| `tellagen_sign_in` | Sign in with the browser |

Other tools (incidents, timeline, postmortems, runbooks, settings checks) are listed when a task needs them: Claude calls `tellagen_enable_toolset`. The [server README](https://github.com/Tellagen/tellagen-mcp-server#tools) lists every tool.

## Works with your existing tools

The plugin composes with any MCP servers you already have configured:

- **Grafana** — query Loki logs, Prometheus metrics, search dashboards
- **GitHub** — check recent deploys, read diffs, search code
- **PagerDuty** — alert history, escalation details
- **Sentry** — error groups, stack traces
- **Kubernetes** — pod restarts, OOMKills, events

No extra configuration on the Tellagen side — just install the vendor MCP servers and Claude uses them.

## Requirements

- A [Tellagen](https://tellagen.com) workspace and a responder seat
- Node.js 18 or later (the plugin starts the server with `npx`)
- **Claude Code:** v1.0.33+ with plugin support
- **Claude Desktop:** latest version with MCP server support

## License

MIT
