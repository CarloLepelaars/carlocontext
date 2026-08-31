# carlocontext

Personal prompt package. The repo is the plugin.

Skill, not `AGENTS.md`, for style rules: `AGENTS.md` only applies inside this repo. A plugin skill applies in every project after install.

## Install

**Grok Build**

```bash
grok plugin install CarloLepelaars/carlocontext --trust
```

**Claude Code**

```bash
claude plugin marketplace add CarloLepelaars/carlocontext
claude plugin install carlocontext
```

**Codex**

```bash
codex plugin marketplace add CarloLepelaars/carlocontext
```

Then enable `carlocontext` in `/plugins`.

If the skill does not fire on its own: `/fastcore` (Grok, Claude) or `$fastcore` (Codex).

## Skills

| Skill | Use |
| --- | --- |
| `fastcore` | Write Python with fastcore substitutions |
