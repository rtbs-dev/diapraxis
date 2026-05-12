# Architect mode

**Purpose**: Design the skeleton of documentation for a library before writing
begins.

### When to use

The user is starting documentation from scratch, restructuring existing docs, or
wants a map of what to write before writing it.

## Workflow

### Step 0: Detect or select documentation framework

1. **Scan the repo** for known framework config files. See
   `references/doc-frameworks.md` for the detection quick-reference table.

2. **If a config file is found**: confirm with the user ("I see an
   `mkdocs.yml` — stick with mkdocs?") and record the decision in
   AGENTS.md Config.

3. **If no config is found**: load `references/doc-frameworks.md` and present
   a survey table of framework options for the library's ecosystem. Sort by
   modernization bias (last commit freshness, community size, build speed).
   Call out frameworks the user has personal experience with (★★★). Include:
   - Framework name and type
   - Auto-gen capability (yes/no + how)
   - Notebook support (yes/no)
   - Build speed
   - Best-fit summary

   Ask: "These frameworks fit your ecosystem. Which direction?"

4. **If the user says "I don't know"**: record the framework question as
   [UNDECIDED] in AGENTS.md Notes with a summary of what's needed to decide
   (e.g., "Need to determine: static site vs. hosted, auto-gen preference,
   notebook requirements"). Move on — the rest of the architect workflow can
   proceed framework-agnostic.

5. **Record the decision** in the `## Diapraxis` section of AGENTS.md under
   `### Config`, then proceed.

### Step 1: Gather library context

Ask the user for:

- Library name, language, and ecosystem
- Who the primary users are (beginners, domain experts, both?)
- Whether auto-generated API docs from docstrings are desired (tie this to the
  chosen framework's capabilities — e.g., "zen-sical can auto-build API
  reference from docstrings — use that?")
- Core concepts a user must understand before using the library
- Approximate size of the public API surface

For any question the user answers "I don't know", record the item as
[UNDECIDED] in AGENTS.md Notes with the context needed to decide later.

### Step 2: Produce a framework-aware skeleton

Produce a skeleton outline organized by Diataxis mode, in recommended
construction order (tutorials → how-to guides → reference → explanation).

For each section include:
- Section title
- Diataxis mode label
- One-sentence description of scope
- Whether it should be auto-generated or hand-written

Additionally, include:

- **Framework-specific directory layout** — show where files live within the
  chosen framework's conventions (e.g., `docs/` tree for mkdocs, notebook
  directories for Quarto, `src/` + site output for zensical)
- **Nav/configuration stubs** — representative nav entries or TOC headers the
  user can drop directly into the framework's config file
- **Undecided markers** — mark items that depend on unresolved decisions with ⚠️
  and a brief note on what's needed

### Step 3: Flag decisions and set up tracking

- Note framework-specific decisions the user still needs to make:
  - Hosting (GitHub Pages, ReadTheDocs, self-hosted, framework's built-in)
  - CI (automate doc builds? which CI?)
  - Versioning (docs versioned alongside library releases?)
- Ask: "Track documentation tasks here in AGENTS.md or in GitHub Issues?"
  - **AGENTS.md**: Tasks become a checklist in the `### Tasks` section.
    The agent auto-resolves items when they are revisited and decided.
  - **GitHub Issues**: Record a pointer to the issue tracker under
    `### Tasks` (e.g., "See GitHub Issues labeled `docs` in this repo").
- Write all [UNDECIDED] items to AGENTS.md Notes with decision criteria.

## Output format

Produce a markdown outline the user can save directly as a documentation
planning document. Group by Diataxis mode. Include the framework-specific
directory layout and config stubs. Use heading levels to indicate hierarchy.
Do not write any prose documentation content — only structure and labels.

## Example

```
# my-library documentation skeleton
**Framework**: zensical | **Hosting**: GitHub Pages | **CI**: TBD ⚠️

## Project layout
```
docs/
  tutorials/
  how-to/
  reference/
  explanation/
  assets/
```

## Tutorials
### Getting Started with my-library
*Mode: Tutorial | Hand-written*
A beginner-friendly walkthrough: install, configure, first successful use.

## How-to Guides
### How to handle authentication errors
*Mode: How-to | Hand-written*
### How to process batches efficiently
*Mode: How-to | Hand-written*

## Reference
### API Reference
*Mode: Reference | Auto-generated from docstrings (zensical)*
### Configuration options
*Mode: Reference | Hand-written*

## Explanation
### Design philosophy
*Mode: Explanation | Hand-written*
Why the library is structured the way it is; trade-offs made.

## Nav stub for zensical config
```python
nav = [
    ("Tutorials", "tutorials/getting-started"),
    ("How-To", "how-to/auth-errors"),
    ("How-To", "how-to/batch-processing"),
    ("Reference", "reference/api"),
    ("Reference", "reference/config"),
    ("Explanation", "explanation/design"),
]
```
```