# CLAUDE.md

This repository is the Claude Code marketplace of Tellagen. People install the plugin from here.

- `plugins/tellagen` is a copy of `Tellagen/tellagen-plugin`. Never edit it here. Change the plugin in its own repository, then run `scripts/sync-marketplace.sh <this checkout>` there and open a PR here with the result.
- `.claude-plugin/marketplace.json` lists the plugin and its version. The sync script sets the version.
- `.github/workflows/plugin-copy.yml` fails when the copy differs from the plugin's master.
- A merge to `master` changes what people install. Do not commit, push or merge unless you are asked to.
- This repository is public. Name no internal ticket, person or private path.
