# Security policy

## Report a vulnerability

Email hello@tellagen.com with the subject "Security report". Do not open a public issue for a vulnerability.

Tell us which version you use, what you found, and the steps to repeat it. We reply as soon as we can.

## Supported versions

We fix security problems in the latest release of the Tellagen plugin and of `@tellagen/mcp-server`. Update to the latest version before you report.

## What this repository holds

This repository is a plugin marketplace for Claude Code. It lists the Tellagen plugin and holds the plugin's files: the server start command, the hooks and the skill text. It holds no secrets. The plugin starts `@tellagen/mcp-server` on your computer with `npx`. Its hooks run that server after Grafana, GitHub, `kubectl` and `gh` calls, to record the call as a step of the open diagnosis run. The hooks mask tokens and passwords, and send the first 300 characters of a result, never the whole result. You can turn step recording off with the plugin option "Record steps of a diagnosis". The sign-in key is stored in your system keychain.
