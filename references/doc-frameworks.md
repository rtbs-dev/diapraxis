# Documentation Framework Catalog

The architect recommends a framework based on the library's ecosystem, existing
configuration, and the characteristics below. Frameworks are sorted by
modernization bias: recent activity, community size, and build speed are favored.

## Python ecosystem

### zensical
- **Type**: Static site generator, Python-native
- **Auto-gen API reference**: Yes (from docstrings)
- **Notebook support**: No
- **Build speed**: Fast (Rust)
- **Config file**: `zensical.toml`
- **Best for**: Python libraries and packages
- **Experience tier**: ★★★ (user's recommended default for Python libraries)
- **Note**: Successor to mkdocs-material; modern, fast.
- **Common commands**:
  - Build: `zensical build`
  - Serve: `zensical serve`
  - Publish: `zensical publish`

### mkdocs-material
- **Type**: Static site generator (MkDocs plugin)
- **Auto-gen API reference**: Via mkdocstrings plugin
- **Notebook support**: Via mkdocs-jupyter plugin
- **Build speed**: Fast (Python)
- **Config file**: `mkdocs.yml`
- **Best for**: Python libraries; mature ecosystem, extensive themes
- **Experience tier**: ★★★ (user's previous default, still widely used)
- **Note**: Predecessor to zensical. Very mature plugin ecosystem.
- **Common commands**:
  - Build: `mkdocs build`
  - Serve: `mkdocs serve`
  - Publish to Pages: `mkdocs gh-deploy`

### Sphinx
- **Type**: Documentation generator
- **Auto-gen API reference**: Yes (autodoc, autosummary)
- **Notebook support**: Via nbsphinx plugin
- **Build speed**: Moderate to slow
- **Config file**: `docs/conf.py` or `source/conf.py`
- **Best for**: Large Python projects, projects needing PDF/LaTeX output
- **Experience tier**: ★★ (widely known, heavy configuration)
- **Note**: Oldest and most feature-complete, but slow and complex. Default on
  ReadTheDocs.
- **Common commands**:
  - Build (HTML): `sphinx-build -b html source/ build/`
  - Build (PDF): `sphinx-build -b latex source/ build/latex/ && make -C build/latex/`
  - Serve: `python -m http.server -d build/`
  - Publish: push to branch — ReadTheDocs auto-builds on webhook

### pdoc
- **Type**: API documentation generator
- **Auto-gen API reference**: Yes (primary purpose)
- **Notebook support**: No
- **Build speed**: Very fast
- **Config file**: `pyproject.toml` (`[tool.pdoc]`)
- **Best for**: Small-to-medium Python libraries that just need clean API docs
- **Experience tier**: ★ (lightweight, single-purpose)
- **Note**: Not a full site generator — just API reference. Pair with
  hand-written tutorials/how-tos elsewhere.
- **Common commands**:
  - Build: `pdoc --html --output-dir docs/ my_package`
  - Serve: `pdoc --http : my_package`

### MyST-MD
- **Type**: Markdown-based documentation framework (JupyterBook ecosystem)
- **Auto-gen API reference**: Via sphinx-autodoc or custom scripts
- **Notebook support**: Native (MyST-NB)
- **Build speed**: Moderate
- **Config file**: `myst.yml`
- **Best for**: IPython/Jupyter-heavy projects, computational narratives
- **Experience tier**: ★★★ (user recommended for notebook-heavy projects)
- **Note**: Excellent for projects where documentation is notebook-first.
- **Common commands**:
  - Build: `myst build --html`
  - Build (PDF): `myst build --pdf`
  - Serve: `myst start`

### Quarto
- **Type**: Multi-language scientific/technical publishing system
- **Auto-gen API reference**: No (separate tool)
- **Notebook support**: Native (.ipynb, .qmd)
- **Build speed**: Moderate
- **Config file**: `_quarto.yml`
- **Best for**: Computational projects, data science, multi-language docs
- **Experience tier**: ★★★ (user recommended for notebook-heavy projects)
- **Note**: Strongest for mixed prose+code documents. Cross-language (Python, R,
  Julia, Observable). Can render to websites, PDFs, presentations.
- **Common commands**:
  - Render: `quarto render`
  - Preview: `quarto preview`
  - Render to PDF: `quarto render --to pdf`

## Rust ecosystem

### mdBook
- **Type**: Book-style documentation generator
- **Auto-gen API reference**: No (rustdoc handles that separately)
- **Notebook support**: No
- **Build speed**: Very fast (Rust)
- **Config file**: `book.toml`
- **Best for**: Hand-written guides, tutorials, books
- **Experience tier**: ★ (standard Rust ecosystem tool)
- **Note**: The standard for non-API documentation in Rust. Usually paired with
  rustdoc for API reference.
- **Common commands**:
  - Build: `mdbook build`
  - Serve: `mdbook serve --open`
  - Publish: `mdbook build` then deploy `book/` directory

### rustdoc
- **Type**: API documentation generator (built into Rust toolchain)
- **Auto-gen API reference**: Yes (primary purpose)
- **Notebook support**: No
- **Build speed**: Fast (compiles with `cargo doc`)
- **Config file**: `Cargo.toml` (`[package.metadata.docs.rs]`)
- **Best for**: All Rust crates — reference docs are universally auto-generated
- **Experience tier**: ★ (universal in Rust ecosystem)
- **Note**: Not a full site generator. Always pair with mdBook or hand-written
  guides for tutorials/how-tos.
- **Common commands**:
  - Build: `cargo doc --no-deps`
  - Publish: push to crates.io — docs.rs auto-builds on publish

## JavaScript/TypeScript ecosystem

### Docusaurus
- **Type**: React-based static site generator
- **Auto-gen API reference**: Via plugins (TypeDoc, etc.)
- **Notebook support**: No (external)
- **Build speed**: Moderate (webpack/bundler overhead)
- **Config file**: `docusaurus.config.js`
- **Best for**: JS/TS projects, projects with multiple doc versions
- **Experience tier**: ★ (widely adopted, React-heavy)
- **Note**: Strong versioning support. Default for many Meta projects. Heavy
  build pipeline.
- **Common commands**:
  - Build: `npm run build` (or `yarn build`)
  - Serve: `npm run start` (or `yarn start`)
  - Publish to Pages: `GIT_USER=<user> yarn deploy`

### VitePress
- **Type**: Vite-powered static site generator
- **Auto-gen API reference**: Via plugins
- **Notebook support**: No
- **Build speed**: Very fast (Vite)
- **Config file**: `.vitepress/config.js` or `.vitepress/config.ts`
- **Best for**: JS/TS projects, modern and fast
- **Experience tier**: ★ (newer, lighter Docusaurus alternative)
- **Note**: Faster builds, simpler config than Docusaurus. Vue ecosystem
  origins but framework-agnostic for docs.
- **Common commands**:
  - Build: `npm run docs:build`
  - Serve: `npm run docs:dev`
  - Publish: `npm run docs:build` then deploy `docs/.vitepress/dist`

### TypeDoc
- **Type**: API documentation generator
- **Auto-gen API reference**: Yes (primary purpose)
- **Notebook support**: No
- **Build speed**: Fast
- **Config file**: `typedoc.json`
- **Best for**: TypeScript projects needing API reference
- **Experience tier**: ★ (single-purpose API generator)
- **Note**: Not a full site generator. Pair with VitePress or Docusaurus.

## Language-agnostic

### GitBook
- **Type**: Hosted documentation platform
- **Auto-gen API reference**: No
- **Notebook support**: Limited
- **Build speed**: N/A (hosted)
- **Config file**: `SUMMARY.md`
- **Best for**: Teams wanting zero-config hosted docs
- **Experience tier**: ★ (closed-source, limited control)
- **Note**: Hosted service. Can import from GitHub repos. Good for non-technical
  teams.

### GitHub Wiki
- **Type**: Git-backed wiki
- **Auto-gen API reference**: No
- **Notebook support**: No
- **Build speed**: N/A (hosted)
- **Config file**: None (per-repo wiki)
- **Best for**: Minimal setup, internal docs
- **Experience tier**: ★ (zero config, limited structure)
- **Note**: Only for very small projects. No Diataxis structure support. Clones
  as a separate git repo.

---

## Detection quick-reference

| Config file | Framework |
|---|---|
| `SConstruct` + `zensical.toml` | zensical |
| `mkdocs.yml` | mkdocs / mkdocs-material |
| `_quarto.yml` | Quarto |
| `myst.yml` | MyST-MD |
| `docs/conf.py` or `source/conf.py` | Sphinx |
| `book.toml` | mdBook |
| `docusaurus.config.js` | Docusaurus |
| `.vitepress/config.js` or `.vitepress/config.ts` | VitePress |
| `typedoc.json` | TypeDoc |
| `pyproject.toml` (`[tool.pdoc]`) | pdoc |
| `Cargo.toml` (`[package.metadata.docs.rs]`) | rustdoc |
| `SUMMARY.md` | GitBook |

## Adding frameworks ad hoc

The user can add frameworks to this catalog at any time. When adding:
- Note the ecosystem, config file pattern, and detection method
- Add to the detection quick-reference table
- Tag with experience tier (★★★ = strong personal experience, ★ = catalog entry only)
