# carlocontext

This repository is a **plugin marketplace**. Add plugins under `plugins/`. Plugins are supported for Grok Build, Claude Code and OpenAI Codex.

Skills live in `plugins/<name>/skills/<name>/SKILL.md`. Catalogs live in `.grok-plugin/`, `.claude-plugin/`, and `.agents/plugins/`.

Do not put always-on style rules in this file — they only apply inside this repo. Put them in a skill so they apply after install.

## Development Guidelines

### Add or change a plugin

1. Create or edit `plugins/<kebab-name>/`.
2. Ship `plugin.json`, `README.md`, and `LICENSE` (MIT).
3. Point the host manifests at that file:
   ```bash
   ln -s ../plugin.json .grok-plugin/plugin.json
   ln -s ../plugin.json .claude-plugin/plugin.json
   ln -s ../plugin.json .codex-plugin/plugin.json
   ```
4. Add only the component directories you use (`skills/`, `commands/`, `agents/`).
5. Add `{name, source, description}` to `.grok-plugin/marketplace.json` and `.claude-plugin/marketplace.json`. Add `{name, source}` to `.agents/plugins/marketplace.json`.
6. Keep `name` stable. Bump `version` in `plugin.json` when behavior changes.

Empty component directories are worse than omitting them.
