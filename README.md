# carlocontext

Plugin marketplace for [Grok](https://grok.com), [Claude Code](https://code.claude.com), and [Codex](https://developers.openai.com/codex).

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

After install, the skill fires when you write Python, or invoke it with `/fastcore` (Grok, Claude) or `$fastcore` (Codex).

## Plugins

| Plugin | What it does |
| --- | --- |
| [fastcore](plugins/fastcore) | Prefer [fastcore](https://fastcore.fast.ai) when writing Python |

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT. Developed by [Carlo Lepelaars](https://github.com/CarloLepelaars).
