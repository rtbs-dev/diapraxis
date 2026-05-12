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
license:
  - MIT
  - Apache-2.0
metadata:
  author: Diapraxis
  version: 1.0.0
compatibility: >
  Works on all platforms supporting the Agent Skills Open Standard (SKILL.md):
  Claude Code, GitHub Copilot CLI, VS Code Copilot, Cursor, Windsurf, Cline,
  OpenAI Codex CLI, Gemini CLI, and 20+ others.
---

# diapraxis — Documentation Structure and Audit

Diapraxis puts the Diataxis documentation framework into practice. Its job is
**not** to write documentation — the user writes the content. Instead it provides:

1. **Structural scaffolding** (`/diapraxis architect`): Design the outline and
   skeleton of documentation before writing begins.
2. **Editorial auditing** (`/diapraxis editor`): Review existing documentation
   for gaps, undocumented APIs, missing examples, and Diataxis mode violations.
3. **Documentation operations** (`/diapraxis engineer`): Build, serve, preview,
   publish, set up CI/CD, manage environments, and run notebooks. The
   engineer is the hands-on counterpart to the architect and editor.

## Diataxis primer

Diataxis identifies four distinct documentation modes: Tutorial, How-to Guide,
Reference, and Explanation. Keep them separate — mixing them is the most common
documentation failure. See `references/diataxis-framework.md` for the full
reference card. Recommended construction order: tutorials → how-to guides →
reference → explanation.

## Mode routing

| Input | Mode | What to load |
|---|---|---|
| `/diapraxis architect` | architect | `modes/_shared.md` + `modes/architect.md` |
| `/diapraxis editor` | editor | `modes/_shared.md` + `modes/editor.md` |
| `/diapraxis engineer` | engineer | `modes/_shared.md` + `modes/engineer.md` |
| (no mode specified) | ask | Ask the user which mode they want (architect, editor, or engineer) |

## Context loading

1. Always load `modes/_shared.md` first — this contains the Diataxis primer,
   AGENTS.md integration protocol, output principles, and [UNDECIDED] tracking
   rules shared across all modes.
2. Then load the mode-specific file from `modes/{mode}.md`.
3. Reference files are loaded on demand when needed:
   - `references/diataxis-framework.md` — full Diataxis reference card
   - `references/doc-frameworks.md` — documentation framework catalog
