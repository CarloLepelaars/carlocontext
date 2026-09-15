# carlocontext

Plugin marketplace for [Grok Build](https://grok.com/build), [Claude Code](https://code.claude.com), and [OpenAI Codex](https://openai.com/codex).

Add the marketplace and install the plugins you want.

## Install

**Grok Build**

```bash
grok plugin marketplace add CarloLepelaars/carlocontext
```

**Claude Code**

```text
/plugin marketplace add CarloLepelaars/carlocontext
```

**OpenAI Codex**

```bash
codex plugin marketplace add CarloLepelaars/carlocontext
```

## How to add a plugin

1. Create `plugins/<kebab-name>/`. Use [fastcore](plugins/fastcore/) as a reference.
2. Add `plugin.json`, `README.md`, and `LICENSE`.
3. Add skills at `skills/<skill-name>/SKILL.md`, including `name` and `description` frontmatter and instructions. 
4. Run these commands from the new plugin's directory:

   ```bash
   mkdir -p .grok-plugin .claude-plugin .codex-plugin
   ln -s ../plugin.json .grok-plugin/plugin.json
   ln -s ../plugin.json .claude-plugin/plugin.json
   ln -s ../plugin.json .codex-plugin/plugin.json
   ```

5. Register the plugin for all three AI providers:
   - [.grok-plugin/marketplace.json](.grok-plugin/marketplace.json): name, description, and source path.
   - [.claude-plugin/marketplace.json](.claude-plugin/marketplace.json): name, description, and source path.
   - [.agents/plugins/marketplace.json](.agents/plugins/marketplace.json): name and a local source object pointing to `./plugins/<kebab-name>`; follow the existing policy and category fields.

Developed by [Carlo Lepelaars](https://github.com/CarloLepelaars)
