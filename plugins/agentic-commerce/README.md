# agentic-commerce

Agentic commerce documentation across major AI providers.

Includes the [distribution skill](skills/distribution/SKILL.md), ACP and UCP documentation skills, and Anthropic's **commerce-builder** skills and commands for building Claude-based shopping and merchant agents.

Install from the [marketplace README](../../README.md).

## Shopify UCP

Includes Shopify's [UCP skill](skills/ucp/SKILL.md) for catalog search, merchant discovery, profiles, carts, checkout, and order tracking using merchant capabilities and live schemas.

Requires Node.js 18 or higher:

```bash
npm install -g @shopify/ucp-cli
```

If the global npm directory is not writable, use `npm install -g @shopify/ucp-cli --prefix ~/.local` and ensure `~/.local/bin` is on PATH.

Shopify's full companion toolkit can be installed separately:

```bash
codex plugin add shopify@openai-curated
```

The bundled skill works independently of the companion plugin. Ask: “Use UCP to discover a merchant's checkout capabilities.” See [Shopify's agent documentation](https://shopify.dev/docs/agents).

## Commerce builder

These six skills and four commands apply **only to Claude-based agentic commerce using Anthropic's commerce-agents reference**. Use them for architecture, prompt caching, UI tools, trust and safety, evaluations, and merchant operations in those projects. They do not cover other providers' commerce implementations or general ACP protocol work.

You can invoke them from Codex, Claude Code, or Grok; the commerce agent being built must use Claude.

| Command | Purpose |
| --- | --- |
| `scaffold-commerce-agent` | Design and scaffold a shopping or merchant agent |
| `add-commerce-flow` | Add a flow and wire its tools |
| `author-commerce-evals` | Create evaluation cases and a runner |
| `review-commerce-agent` | Review an existing agent against the reference |

Ask your agent to use the named workflow, for example: “Use scaffold-commerce-agent to build a shopping assistant.” In Claude Code, invoke `/agentic-commerce:scaffold-commerce-agent`.

These workflows use a local clone of [anthropics/commerce-agents](https://github.com/anthropics/commerce-agents), which the scaffold command can obtain. Paths inside the bundled instructions refer to that reference repository. Its Python packages and demo applications are not bundled here. The upstream workflows record project decisions in `CLAUDE.md`.

## Attribution

The six `skills/commerce-*/SKILL.md` files and four `commands/*.md` files are adapted from Anthropic's [commerce-builder](https://github.com/anthropics/commerce-agents/tree/fd4d59224ab96b43c6dc6888207c67b3bd5a24cf/plugins/commerce-builder), version `0.1.0`, commit `fd4d59224ab96b43c6dc6888207c67b3bd5a24cf`. Local modifications add Claude-only scope to their discovery descriptions and instructions. They retain the upstream [Apache-2.0 license](licenses/commerce-builder-LICENSE). Original carlocontext content remains [MIT licensed](LICENSE).

The UCP skill is adapted from [Shopify AI Toolkit](https://github.com/Shopify/shopify-ai-toolkit/tree/c6a219bf99fc5aa2576843f3c8a005f9da863383/skills/ucp), skill version `1.14.1`, commit `c6a219bf99fc5aa2576843f3c8a005f9da863383`, under its [MIT license](licenses/shopify-ai-toolkit-LICENSE). Local changes remove toolkit-specific telemetry hooks and script calls, normalize metadata, and add CLI setup and purchase authorization guidance.
