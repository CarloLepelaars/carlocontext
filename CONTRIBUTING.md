Info on contributing to this project.

## Getting Started

1. Fork the repository
2. Clone your fork:

```bash
git clone https://github.com/CarloLepelaars/carlocontext.git
cd carlocontext
```

This is a plugin marketplace, not a Python package. No `uv sync`. It is a personal collection that will be open sourced — add plugins under `plugins/`, do not fold new topics into `fastcore`.

If `grok` is installed:

```bash
for d in plugins/*; do grok plugin validate "$d"; done
```

Install a plugin from the checkout:

```bash
grok plugin install ./plugins/fastcore --trust
grok plugin details fastcore
```

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

### Skills

| Kind | Path | `name` must equal |
| --- | --- | --- |
| Skill | `skills/<name>/SKILL.md` | directory name |

Frontmatter is required. `description` says what it does **and** when to fire.

### Before Committing

```bash
for d in plugins/*; do grok plugin validate "$d"; done
```

Validate every plugin you touched. JSON is 2-space indent, UTF-8, one trailing newline.

### Pull Requests

- Keep PRs focused on a single change
- Update catalog entries and `plugin-index.json` in the same PR
- Reference related issues

## Feature Requests

Before implementing new features:

1. Open a GitHub Issue to discuss the proposal
2. Get community feedback
3. Consider maintenance burden

## Bug Reports

Include:

- Which plugin / skill
- What the agent wrote vs what it should have written
- Environment (Grok, Claude Code, Codex)

## Questions?

Open a GitHub Issue for questions about the marketplace or potential contributions.
