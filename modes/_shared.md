# Shared context for diapraxis modes

This file is loaded before any mode-specific instructions.

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

## Output principles

- Never write documentation prose on behalf of the user.
  The output is always structure, outlines, labels, and audit reports.
- When in doubt about which Diataxis mode a section belongs to,
  ask the user rather than guessing.
- If the user's library is very small (e.g., a single-module utility), say so —
  not every library needs all four modes.
  A tiny library might only need a reference and one how-to guide.
- Rust libraries using `rustdoc` and Python libraries using Sphinx/pdoc have
  auto-generation available for reference docs.
  Note this in architect output and don't recommend hand-writing reference
  content when auto-generation is available.
- Framework detection should happen before context gathering —
  knowing the framework informs the questions asked and the skeleton produced.
- **Semantic line breaks** ([SemBr](https://sembr.org/)): all markdown prose
  written by diapraxis MUST use one-sentence-per-line (semantic line breaks).
  Each sentence, clause, or substantial unit of thought gets its own line.
  Never join multiple sentences on a single line.
  This rule applies to all output — outlines,
  audit reports,
  AGENTS.md entries,
  and inline prose.
  The user may override this with an explicit instruction.

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

#### ProseStyle
- SemBr: Yes (one-sentence-per-line)

### Notes
- 2026-05-12: Initial architect session. Tutorial and how-to modes prioritized.
- [UNDECIDED] CI for doc builds: need to choose between GitHub Actions and none.
  Decision criteria: how often docs change, team CI preferences.

#### Commands
(Recorded commands — fallback when just is not available)
- mkdocs build --strict       # Build the documentation
- mkdocs serve                # Start live preview

### Tasks
(Tracked in AGENTS.md)
- [ ] Write "Getting Started" tutorial
- [ ] Write "Error Handling" how-to guide
- [ ] Set up zensical auto-gen for API reference
```

### When to read AGENTS.md

- **Architect Step 0**: Check for existing framework decisions before surveying
- **Editor pre-audit**: Load config, ProseStyle, notes, and tasks before auditing
- **Any continuation session**: The user may return mid-workflow; AGENTS.md is
  the session resume state

### When to write AGENTS.md

- **After framework selection**: Record in Config
- **After gathering context**: Record audience, auto-gen decisions in Config;
  [UNDECIDED] items in Notes
- **After task tracking decision**: Set up Tasks section (checklist or issue
  tracker pointer)
- **After editor audit**: Append findings to Notes; add gaps to Tasks
- **After prose style check**: Record preferences in ProseStyle if agreed upon
- **When an [UNDECIDED] item is resolved**: Move it from Notes to Config and
  remove the marker (auto-resolve on revisit)

### Task tracking modes

On first session, after architect Step 3, ask:

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

### General notes

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