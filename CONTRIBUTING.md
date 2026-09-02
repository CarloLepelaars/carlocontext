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
2. Ship `plugin.json`, `README.md`, and `LICENSE` (MIT).
3. Symlink `plugin.json` into `.grok-plugin/`, `.claude-plugin/`, and `.codex-plugin/`.
4. Add only the component directories you use (`skills/`, `commands/`, `agents/`).
5. List the plugin in the three `marketplace.json` files.
6. Keep `name` stable. Bump `version` in `plugin.json` when behavior changes.

Style rules belong in a **skill**, not root `AGENTS.md`. `AGENTS.md` only applies inside this repo.


### Pull Requests

- Keep PRs focused on a single change
- Update the three `marketplace.json` files in the same PR

## Bug Reports

Include the plugin or skill, what the agent wrote vs what it should have written, and which host (Grok, Claude Code, Codex).
