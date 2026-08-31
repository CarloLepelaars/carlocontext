# carlocontext

Plugin marketplace for Grok, [Claude Code](https://code.claude.com), and [Codex](https://developers.openai.com/codex).

Add the marketplace once, then install the plugins you want.

## Install

**Grok**

```bash
grok plugin marketplace add CarloLepelaars/carlocontext
grok plugin install CarloLepelaars/carlocontext#plugins/fastcore --trust
```

**Claude Code**

```text
/plugin marketplace add CarloLepelaars/carlocontext
/plugin install fastcore@carlocontext
```

**Codex**

```bash
codex plugin marketplace add CarloLepelaars/carlocontext
```

Then install `fastcore` from `/plugins`.

Skills auto-invoke when relevant, or call them by name (`/fastcore`, `$fastcore`).

## Plugins

| Plugin | What it does |
| --- | --- |
| [fastcore](plugins/fastcore) | Prefer [fastcore](https://fastcore.fast.ai) when writing Python |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Developed by [Carlo Lepelaars](https://github.com/CarloLepelaars).
