# carlocontext

This repository is a **plugin marketplace**. Add plugins under `plugins/`.

Skills live in `plugins/<name>/skills/<name>/SKILL.md`. Catalogs live in `.grok-plugin/`, `.claude-plugin/`, and `.agents/plugins/`.

Do not put always-on style rules in this file — they only apply inside this repo. Put them in a skill so they apply after install.

## After every plugin or catalog edit

1. Update `.grok-plugin/marketplace.json`, `.claude-plugin/marketplace.json`, and `.agents/plugins/marketplace.json`.
2. Update `.grok-plugin/plugin-index.json`.
3. Run `grok plugin validate plugins/<name>` if `grok` is on `PATH`.

Keep `name` stable. Empty component directories are worse than omitting them.
