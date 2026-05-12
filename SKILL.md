---
name: diapraxis
description: >
  Use this skill whenever the user wants help structuring, planning, or auditing
  documentation for a software library (Python, Rust, or similar). Diapraxis
  applies the Diataxis framework to documentation work — but does not write
  documentation content for the user. Instead it produces structural scaffolding
  (outlines, skeletons) and editorial audits (gap analysis, undocumented APIs,
  coverage checks). Trigger this skill when the user mentions library docs,
  documentation structure, API coverage, missing docs, doc audits, or asks
  where to start with documentation. Also trigger when the user invokes the
  aliases "librarian" or "Wan Shi Tong" — these are recognized aliases for
  this skill and the session should continue seamlessly.
license: MIT
metadata:
  author: Diapraxis
  version: 1.0.0
compatibility: >
  Works on all platforms supporting the Agent Skills Open Standard (SKILL.md):
  Claude Code, GitHub Copilot CLI, VS Code Copilot, Cursor, Windsurf, Cline,
  OpenAI Codex CLI, Gemini CLI, and 20+ others.
---

# /diapraxis — Documentation Structure and Audit

Diapraxis puts the Diataxis documentation framework into practice. Its job is
**not** to write documentation — the user writes the content. Instead it
provides:

1. **Structural scaffolding** (`/diapraxis architect`): Design the outline and
   skeleton of documentation before writing begins.
2. **Editorial auditing** (`/diapraxis editor`): Review existing documentation
   for gaps, undocumented APIs, missing examples, and Diataxis mode violations.

## Diataxis primer

Diataxis identifies four distinct documentation modes. Keep them separate —
mixing them is the most common documentation failure. See
`references/diataxis-framework.md` for a complete reference card.

The recommended construction order is: tutorials first, then how-to guides,
then reference, then explanation — following the natural progression of a
user's relationship with a library.

## Aliases

If the user addresses this skill as **"librarian"** or **"Wan Shi Tong"**, treat
it as a continuation of the current diapraxis session. No roleplay — just
seamless continuity.

---

## Subcommand: architect

**Purpose**: Design the skeleton of documentation for a library before writing
begins.

### When to use

The user is starting documentation from scratch, restructuring existing docs, or
wants a map of what to write before writing it.

### Workflow

#### Step 0: Detect or select documentation framework

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

#### Step 1: Gather library context

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

#### Step 2: Produce a framework-aware skeleton

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

#### Step 3: Flag decisions and set up tracking

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

### Output format

Produce a markdown outline the user can save directly as a documentation
planning document. Group by Diataxis mode. Include the framework-specific
directory layout and config stubs. Use heading levels to indicate hierarchy.
Do not write any prose documentation content — only structure and labels.

### Example

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

---

## Subcommand: editor

**Purpose**: Audit existing documentation against Diataxis principles and flag
gaps, violations, and missing coverage.

### When to use

The user has existing documentation (partial or complete) and wants to know
what's missing or broken structurally.

### Workflow

#### Before auditing

1. **Read AGENTS.md** — check for an existing `## Diapraxis` section. Load:
   - `### Config` — framework, hosting, audience decisions
   - `### Notes` — previous context, known issues, [UNDECIDED] items
   - `### Tasks` — pending items (incorporate these into the audit)

2. **If no Diapraxis section exists**, the editor creates one as part of its
   output.

#### Gather materials

Ask the user to provide:
- The documentation structure (file list, outline, or actual content)
- The public API surface or module list (if available)
- Any existing test suite or example code (optional, helps find undocumented
  usage patterns)
- Framework being used (if not already in AGENTS.md Config)

#### Run the audit

Audit across four dimensions:

**Coverage audit** — Which Diataxis modes are present? Which are absent or
thin? Produce a coverage table:

| Mode | Status | Notes |
|---|---|---|
| Tutorial | ✅ present | One tutorial covering basic use |
| How-to | ⚠️ sparse | Only 1 guide; 4+ common tasks have no guide |
| Reference | ❌ missing | No API reference found |
| Explanation | ✅ present | Architecture doc covers design rationale |

**API coverage audit** — Cross-reference the public API with the docs. Flag:
- Undocumented public functions/types/modules
- Functions documented only in reference but with no how-to or tutorial usage

**Mode violation audit** — Flag sections that mix modes incorrectly:
- Tutorial sections that go deep into reference detail (common)
- Reference pages that include narrative or explanation (common)
- How-to guides that explain concepts instead of giving steps

**Example and test coverage** — Flag:
- Public APIs with no usage examples anywhere in the docs
- Examples in docs that don't appear in the test suite (potential drift risk)

#### Produce a prioritized gap list

Order gaps by impact. A missing tutorial is usually higher impact than a
missing explanation section.

#### Write findings to AGENTS.md

After the audit, update the `## Diapraxis` section in AGENTS.md:

- **Config**: Ensure framework and hosting are recorded if newly discovered
- **Notes**: Append audit findings — coverage gaps, mode violations,
  undocumented APIs, with audit date
- **Tasks**: Add actionable gaps as checklist items or note them under the
  existing task tracking method (inline checklist or GH Issues pointer)

### Output format

Produce a structured audit report in markdown with the four audit sections above.
End with a prioritized action list. Do not rewrite or draft any documentation
content — only identify what's missing and where.

Also output the AGENTS.md diff so the user can see what was recorded.

---

## AGENTS.md Integration

Diapraxis writes a `## Diapraxis` section to the repo root's `AGENTS.md`
(creates the file if it doesn't exist). This section is the persistent record
of all documentation decisions, context, and pending work.

### Section structure

```markdown
## Diapraxis

### Config
Framework: zensical
Hosting: GitHub Pages
Audience: Python developers, intermediate+
Auto-gen API reference: Yes (from docstrings)

### Notes
- 2026-05-12: Initial architect session. Tutorial and how-to modes prioritized.
- [UNDECIDED] CI for doc builds: need to choose between GitHub Actions and none.
  Decision criteria: how often docs change, team CI preferences.

### Tasks
(Tracked in AGENTS.md)
- [ ] Write "Getting Started" tutorial
- [ ] Write "Error Handling" how-to guide
- [ ] Set up zensical auto-gen for API reference
```

### When to read AGENTS.md

- **Architect Step 0**: Check for existing framework decisions before surveying
- **Editor pre-audit**: Load config, notes, and tasks before auditing
- **Any continuation session**: The user may return mid-workflow; AGENTS.md is
  the session resume state

### When to write AGENTS.md

- **After framework selection**: Record in Config
- **After gathering context**: Record audience, auto-gen decisions in Config;
  [UNDECIDED] items in Notes
- **After task tracking decision**: Set up Tasks section (checklist or issue
  tracker pointer)
- **After editor audit**: Append findings to Notes; add gaps to Tasks
- **When an [UNDECIDED] item is resolved**: Move it from Notes to Config and
  remove the marker (auto-resolve on revisit)

### Task tracking modes

On first session, after Step 3 of architect, ask:

> "Track documentation tasks here in AGENTS.md or in GitHub Issues?"

- **AGENTS.md**: Tasks are a checklist in `### Tasks`. The agent marks items
  complete when they are revisited and confirmed done.
- **GitHub Issues**: Tasks section contains a pointer (e.g., "See GitHub Issues
  labeled `documentation`"). The agent does not manage issues — just notes
  where to find them. Prompt the user to create the label if it doesn't exist.

### [UNDECIDED] protocol

When the user says "I don't know" or defers a decision:
1. Record in AGENTS.md Notes as `[UNDECIDED] <question>` with decision criteria
2. Proceed with the workflow — the rest of the architect/editor can continue
   without it
3. When the user later provides an answer, auto-resolve: move to Config (if a
   setting) or strike it (if a question), remove the [UNDECIDED] marker
4. Never forget an undecided item unless the user explicitly says to drop it

---

## Notes for the agent

- Never write documentation prose on behalf of the user. Your output is always
  structure, outlines, labels, and audit reports.
- When in doubt about which Diataxis mode a section belongs to, ask the user
  rather than guessing.
- If the user's library is very small (e.g., a single-module utility), say so —
  not every library needs all four modes. A tiny library might only need a
  reference and one how-to guide.
- Rust libraries using `rustdoc` and Python libraries using Sphinx/pdoc have
  auto-generation available for reference docs. Note this in architect output
  and don't recommend hand-writing reference content when auto-generation is
  available.
- Framework detection should happen before context gathering — knowing the
  framework informs the questions you ask and the skeleton you produce.
- Always check AGENTS.md at the start of an architect or editor session. It
  carries the session state across invocations.
- Never forget [UNDECIDED] items. Bring them up at the start of any continuation
  session. Only drop them when the user explicitly says to.
- The `references/doc-frameworks.md` catalog is designed for iterative
  extension. When the user adds a new framework, update the detection table
  and add an entry to the ecosystem section.
- AGENTS.md is git-trackable by design. The user can commit it and share it
  with colleagues. Write it assuming it will be read by humans and other
  agent tools.
