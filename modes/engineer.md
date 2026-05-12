# Engineer mode

**Purpose**: Execute and manage the operational side of documentation — building,
compiling, rendering, publishing, environments, notebook execution, and CI/CD.
Engineer mode is the hands-on counterpart to architect (design) and editor (audit).

### When to use

The user needs to build docs, preview locally, publish to a host, set up CI/CD,
manage Python environments for doc toolchains, run notebooks, export to PDF, or
version documentation. Anything that involves running a command.

## Core principle: justfile-first

Always check if `just` is available (`just --version`). If it is:

- Create or update a `docs/justfile` (or repo-root `justfile` if the docs are
  the primary product) that encodes every documentation operation as a recipe.
- When the user asks to do something, first ensure there's a recipe for it,
  then run `just <recipe>`.
- The justfile is the source of truth for documentation operations — the user
  commits it and colleagues get reproducible builds.

If `just` is not available:

- Lightly suggest installing it ("Consider `cargo install just` or your package
  manager — it makes documentation operations repeatable across machines").
- Record commands in `AGENTS.md` under `## Diapraxis` > `### Notes` >
  `#### Commands` so they're not lost to shell history.
- Execute commands directly, explaining their function. The user may copy them
  or ask to revisit them later.

## Workflow

### Step 0: Load context

1. Read AGENTS.md `## Diapraxis` section for:
   - **Framework** — what toolchain is in use (Config)
   - **Hosting** — where docs are published (Config)
   - **Existing Commands** — any previously recorded commands in Notes
2. Look up the framework in `references/doc-frameworks.md` to load its common
   commands and characteristics.
3. Check `just --version` to decide the execution path.

### Step 1: Understand the ask

Determine what the user wants. Common categories:

- **Build**: Compile docs into output format (HTML, PDF, ePub)
- **Serve/Preview**: Start a live-reload development server
- **Render/Export**: Produce PDF, single-page HTML, or other formats
- **Publish**: Deploy to hosting (GitHub Pages, ReadTheDocs, self-hosted)
- **CI/CD**: Set up automated doc builds on push or PR
- **Environment**: Create Python venvs, install toolchain dependencies
- **Notebook execution**: Run .ipynb/.qmd files with proper kernels
- **Shell help**: The user wants to understand a command or tool (pandoc,
  xclip, etc.) — explain it, then offer to save it as a just recipe

### Step 2: Execute

**If just is available:**
1. Check the existing justfile for a relevant recipe.
2. If none exists, look up the framework's common commands in
   `references/doc-frameworks.md` and add a recipe with: a descriptive name,
   a `#` comment explaining what it does, and proper dependency ordering between
   recipes.
3. Run `just <recipe>`.
4. Summarize what the recipe did for the user.

**If just is not available:**
1. Look up the framework's common commands in `references/doc-frameworks.md`.
2. Construct the command from the catalog entry.
3. Record it in AGENTS.md Commands for future reference.
4. Execute it and report the result.
5. Remind the user that a just recipe would make this repeatable.

## Environment management

For Python-based frameworks, always create an isolated environment. The engineer
builds this into the justfile as a dependency:

```justfile
# Documentation operations
# Run `just` to see this list

default:
    @just --list

# Set up Python virtual environment and install doc dependencies
venv:
    python3 -m venv .venv
    .venv/bin/pip install -e ".[docs]"

# Build documentation for production
build: venv
    .venv/bin/mkdocs build

# Start live-reload development server
dev: venv
    .venv/bin/mkdocs serve

# Publish to GitHub Pages
publish: build
    .venv/bin/mkdocs gh-deploy
```

For Quarto/MyST-MD with Python kernels:

```justfile
default:
    @just --list

venv:
    python3 -m venv .venv
    .venv/bin/pip install -r docs/requirements.txt

render: venv
    cd docs && ../.venv/bin/quarto render

preview: venv
    cd docs && ../.venv/bin/quarto preview
```

## Jupytext notebook pairing (fallback — load on demand)

When the doc framework lacks native notebook support, jupytext pairing
(.ipynb ↔ .md via `jupytext.toml`) is an alternative. Load the detailed
reference when needed:

> See `references/jupytext-pairing.md` — detection, two-tier workflow,
> image discipline, justfile recipes, and framework evolution note.

## CI/CD setup

When the user asks for CI, generate a workflow file appropriate to their platform
(GitHub Actions is the default unless the repo shows GitLab CI config).

### GitHub Actions

```yaml
name: Docs
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: extractions/setup-just@v2
      - run: just build
      - uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./site
```

Substitute the build command and publish directory for the chosen framework.
If the framework needs a Python environment, add a `setup-python` step before
the build step.

## Justfile conventions

When writing justfiles for documentation projects:

- `default` recipe shows all available commands (`just --list`)
- Each recipe has a `#` comment line above it explaining what it does
- Group related recipes with blank lines and section comments (`# Build`,
  `# Preview`, `# Publish`)
- Environment recipes (`venv`, `install`) are dependencies of build/serve
  recipes, never called directly by the user
- Publish recipes include a confirmation or warn before deploying
- Recipes are named with verbs: `build`, `dev`, `publish`, `render`, `clean`

Example justfile:

```justfile
# docs/justfile — Documentation operations for <project>
# Run `just` to see available recipes

default:
    @just --list

# ---- Environment ----

venv:
    python3 -m venv .venv
    .venv/bin/pip install -e ".[docs]"

# ---- Build ----

build: venv
    .venv/bin/mkdocs build --strict

# Run notebooks and build
render: venv
    cd docs && ../.venv/bin/quarto render

# ---- Preview ----

dev: venv
    .venv/bin/mkdocs serve

preview: venv
    cd docs && ../.venv/bin/quarto preview

# ---- Publish ----

publish: build
    @echo "Deploying to GitHub Pages..."
    .venv/bin/mkdocs gh-deploy

# ---- Maintenance ----

clean:
    rm -rf site/ .venv/
```

## Output format

Engineer mode produces:

- **justfile recipes** (primary): Committed to the repo as the repeatable record
  of how to operate the documentation
- **Terminal output**: What was run and what happened
- **AGENTS.md Commands subsection**: Fallback record if just isn't available;
  otherwise not needed since the justfile is the record
- **CI/CD workflow files**: When the user asks for automation setup