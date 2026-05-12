# Jupytext notebook pairing (fallback when no native notebook support)

When the doc framework lacks native notebook execution (e.g., zensical has no
`.ipynb` → HTML pipeline), jupytext pairing is a clean alternative — but only
as a fallback. Native notebook support is always preferred.

This file is loaded on demand by the engineer mode when jupytext pairing is
detected or when the user brings up notebook workflows.

## Detection

Check for a `jupytext.toml` at the repo root with `[[formats]]` sections:

```toml
[[formats]]
"notebooks/user-guide/" = "ipynb"
"docs/user-guide/" = "md"
```

This maps `.ipynb` ↔ `.md` bidirectionally: either file is the source and the
other is derived.

## Two-tier workflow

**Tier 1 — Editing (continuous, primary path)**:
- `.ipynb` files are canonical (created and edited in JupyterLab)
- The JupyterLab jupytext extension watches the `.ipynb` and auto-writes the
  paired `.md` on every save
- The doc framework's live-reload server picks up the `.md` changes immediately
  — no build step needed during editing
- **Critical**: `.md` files are auto-generated from `.ipynb` — never hand-edit
  them. If you must, sync direction reverses (`.md` becomes canonical).

**Tier 2 — Batch re-execute (periodic maintenance)**:
- Re-run all notebooks to regenerate figures and catch bit-rot
- Strip inline image outputs (they bloat `.ipynb` JSON and destroy diffs)
- Images must be saved to a persistent directory (`img/`) via `plt.savefig()`
  rather than displayed inline — the markdown references them by relative path

## Image discipline

Images rendered in notebooks MUST go to disk, not inline:

```python
plt.savefig(imgpath/'my-figure.webp')   # ← persistent, git-friendly
```

Inline `plt.show()` or implicit display output embeds base64 data into the
`.ipynb` cell outputs. When `strip-inline-images` runs, these are removed. Only
`savefig`-persisted images survive — so `savefig` is the canonical persistence
mechanism.

## Recommended justfile recipes (batch path only)

The continuous sync is handled by the JupyterLab extension. The justfile
only needs the batch re-execute and cleanup path:

```justfile
# ---- Notebooks (jupytext) ----

# Strips inline image data from executed .ipynb cells (keeps savefig'd images only)
strip-inline-images:
    python3 -c "
import json, glob
for f in glob.glob('notebooks/**/*.ipynb', recursive=True):
    nb = json.load(open(f))
    for cell in nb['cells']:
        if cell['cell_type'] == 'code':
            cell['outputs'] = [o for o in cell['outputs']
                               if o.get('output_type') != 'display_data'
                               and 'image/' not in o.get('data', {})]
    json.dump(nb, open(f, 'w'), indent=1)
"

# Re-execute all jupytext-paired notebooks, strip inline images
execute-notebooks: venv
    poetry run jupyter nbconvert --to notebook \
      --execute notebooks/user-guide/*.ipynb \
      --output-dir notebooks/user-guide/ \
      --ExecutePreprocessor.timeout=600
    poetry run jupyter nbconvert --to notebook \
      --execute notebooks/case-studies/*.ipynb \
      --output-dir notebooks/case-studies/ \
      --ExecutePreprocessor.timeout=600
    just strip-inline-images

# Full batch pipeline: execute notebooks, strip images
render: execute-notebooks
```

## Note on framework evolution

This entire section is a workaround. If the doc framework adds native notebook
support (`.ipynb` as a first-class source with automatic rendering), the
jupytext pairing should be retired in favor of the native path.