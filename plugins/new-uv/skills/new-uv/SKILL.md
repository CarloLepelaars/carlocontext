---
name: new-uv
description: >
  Scaffold a Python repo from CarloLepelaars/uv_template. Clones the
  template, renames the package, replaces every template name, installs deps, removes unnecessary leftover files.
  with uv, and runs ruff + pytest. Use when the user wants a new uv project,
  a new uv repo, scaffolds from uv_template, or runs /new-uv.
when-to-use: new uv project, new uv repo, scaffold uv_template, /new-uv
---

# New UV Project

Create a repo from `https://github.com/CarloLepelaars/uv_template`.

## Inputs

1. **Project name** (required) — directory and PyPI name. Lowercase letters, digits, hyphens, underscores (`my-lib`). Ask once if missing.
2. **Parent directory** (optional) — default: current working directory.

| Role | Rule | Example |
|------|------|---------|
| `DIR` | project name as given | `my-lib` |
| `PKG` | `-` → `_`; valid Python identifier | `my_lib` |
| `TITLE` | hyphens/underscores → spaces, title-case | `My Lib` |

Stop if `PKG` is invalid (empty, starts with a digit, contains spaces/slashes) or `$PARENT/$DIR` already exists.

## Steps

### 1. Clone and detach

```bash
git clone https://github.com/CarloLepelaars/uv_template.git "$PARENT/$DIR"
cd "$PARENT/$DIR"
rm -rf .git
git init
```

No remote unless the user asks.

### 2. Rename the package

```bash
mv src/template "src/$PKG"
```

### 3. Replace every template name

| Find | Replace | Where |
|------|---------|-------|
| `name = "template"` | `name = "$DIR"` | `pyproject.toml` |
| `description = "Template for UV projects"` | user description, else `"$TITLE"` | `pyproject.toml` |
| `from template.base import add` | `from $PKG.base import add` | `tests/test_base.py`, `nbs/ex.ipynb` |
| `uv_template` | `$DIR` | `README.md`, `CONTRIBUTING.md` (titles, badges, clone URLs, `cd`) |
| `pip install template` | `pip install $DIR` | `README.md` |
| `# Template` | `# $TITLE` | `nbs/ex.ipynb` |
| `# project` | `# $DIR` | `llms.txt` |

Then grep the tree for leftovers (`uv_template`, `from template.`, `import template`, `name = "template"`, `pip install template`) and fix any hits.

Rewrite the README intro: drop "This repository is a project template…" and replace "Template repo for building…" with a one-line blurb (`$TITLE` or the user description). The result is a real package, not a template.

Leave LICENSE, author fields (unless the user gave name/email), and the sample `add` function as-is.

### 4. Install and verify

Require `uv` on PATH. If missing, tell the user to install it (`curl -LsSf https://astral.sh/uv/install.sh | sh`) and stop.

```bash
uv sync --all-extras
uv run ruff format
uv run ruff check
uv run pytest -s
```

If a check fails on rename leftovers, fix and re-run.

### 5. Commit and report

Remove unnecessary files. For example, if the user doesn't do Jupyter Notebooks, remove `nbs/`. If actual library code is already written, remove `base.py` and `tests/test_base.py`. 

```bash
git add -A
git commit -m "Initial commit from uv_template"
```

Report the absolute path, `DIR` / `PKG`, quality-gate results, and next steps (edit description, `gh repo create`, open the folder).

## Slash

```
/new-uv my-lib
/new-uv my-lib ~/Source
```
