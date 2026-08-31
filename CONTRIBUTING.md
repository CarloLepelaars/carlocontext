# Contributing

## Getting Started

1. Fork
2. Clone:

```bash
git clone https://github.com/CarloLepelaars/carlocontext.git
cd carlocontext
```

This is a plugin marketplace, not a Python package. Add plugins under `plugins/`. New plugins should work for Grok Build, Claude Code and OpenAI Codex.

## Ways to Contribute

- **Add plugins**: New first-party plugins live under `plugins/<kebab-name>/`.
- **Fix skills**: Tighten instructions, fix wrong APIs, add missing cases.
- **Improve documentation**: README, `llms.txt`, or a plugin README.

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

Style rules belong in a **skill**, not root `AGENTS.md`. `AGENTS.md` only applies inside this repo.


### Pull Requests

- Keep PRs focused on a single change
- Update catalog entries and `plugin-index.json` in the same PR

## Bug Reports

Include the plugin or skill, what the agent wrote vs what it should have written, and which host (Grok, Claude Code, Codex).
