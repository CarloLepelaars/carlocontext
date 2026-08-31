# carlocontext

My plugin marketplace for Grok, Claude Code, and Codex. A personal collection of agent skills and context I will open source.

`fastcore` is the first plugin. More will land under `plugins/`.

Style rules live in **skills**, not root `AGENTS.md`. `AGENTS.md` only applies inside this repo.

## Install

Add the marketplace, then install the plugins you need.

**Grok Build**

```bash
grok plugin marketplace add CarloLepelaars/carlocontext
grok plugin marketplace list
grok plugin install CarloLepelaars/carlocontext#plugins/fastcore --trust
```

From a local clone:

```bash
grok plugin install /path/to/carlocontext/plugins/fastcore --trust
```

**Claude Code**

```bash
claude plugin marketplace add CarloLepelaars/carlocontext
claude plugin install fastcore
```

**Codex**

```bash
codex plugin marketplace add CarloLepelaars/carlocontext
```

Then enable the plugin you want in `/plugins`.

If a skill does not fire on its own, invoke it by name (`/fastcore`, `$fastcore`).

## Plugins

| Plugin | What you get |
| --- | --- |
| [fastcore](plugins/fastcore) | Write Python with fastcore substitutions |

## Repo layout

```
.grok-plugin/marketplace.json    Grok catalog
.grok-plugin/plugin-index.json   component index
.claude-plugin/marketplace.json  Claude catalog
.agents/plugins/marketplace.json Codex catalog
plugins/<name>/                  first-party plugins
```

A plugin is a directory with `skills/` (and optional `commands/`, `agents/`) plus manifests under `.grok-plugin/`, `.claude-plugin/`, `.codex-plugin/`, and root `plugin.json`.

## Contributing

Check out the [contributing guidelines](CONTRIBUTING.md) for details on how to contribute to this project.

## Credits

Developed by Carlo Lepelaars.
