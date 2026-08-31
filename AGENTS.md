# carlocontext

This repository is a **plugin marketplace**. Add plugins under `plugins/`. Plugins are supported for Grok Build, Claude Code and OpenAI Codex.

Skills live in `plugins/<name>/skills/<name>/SKILL.md`. Catalogs live in `.grok-plugin/`, `.claude-plugin/`, and `.agents/plugins/`.

Do not put always-on style rules in this file — they only apply inside this repo. Put them in a skill so they apply after install.

## Development Guidelines

### Add or change a plugin

1. Create or edit `plugins/<kebab-name>/`.
2. Ship at least:
   - `.grok-plugin/plugin.json`
   - `.claude-plugin/plugin.json`
   - `.codex-plugin/plugin.json`
   - `plugin.json` (Agent Plugins 1.0)
   - `README.md`
   - `LICENSE` (MIT)
3. Add only the component directories you use (`skills/`, `commands/`, `agents/`).
4. Add or update the entry in:
   - `.grok-plugin/marketplace.json`
   - `.claude-plugin/marketplace.json`
   - `.agents/plugins/marketplace.json`
   - `.grok-plugin/plugin-index.json`
5. Keep `name` stable. Renaming a published slug breaks existing installs.
6. Bump `version` in the manifests and catalog entries when behavior changes.

## After every plugin or catalog edit

1. Update `.grok-plugin/marketplace.json`, `.claude-plugin/marketplace.json`, and `.agents/plugins/marketplace.json`.

Keep `name` stable. Empty component directories are worse than omitting them.
