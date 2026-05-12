# Diapraxis

Documentation structure and audit skill using the [Diataxis framework](https://diataxis.fr).
Diapraxis produces structural scaffolding (outlines, skeletons) and editorial audits
(gap analysis, undocumented APIs, coverage checks).
It does **not** write documentation content for you.

## What it does

Diapraxis has three modes,
each with its own instructions in [`modes/`](modes/):

| Mode | Instructions | What it produces |
|------|-------------|-----------------|
| `architect` | [`modes/architect.md`](modes/architect.md) | Documentation skeleton organized by Diataxis mode |
| `editor` | [`modes/editor.md`](modes/editor.md) | Audit report: coverage gaps, mode violations, undocumented APIs |
| `engineer` | [`modes/engineer.md`](modes/engineer.md) | Build, serve, preview, publish docs; manage CI/CD and environments |

To keep the skill lean at load time,
it uses [progressive disclosure](https://docs.claude-mem.ai/progressive-disclosure):
[`SKILL.md`](SKILL.md) holds the router and shared primer,
while mode-specific instructions in [`modes/`](modes/) are loaded only when needed.

> Diapraxis never writes documentation prose — you write the content.

Instead, it provides the scaffolding and the audit.

## Features

- **AGENTS.md session memory** —
  persists documentation decisions, notes, and tasks between sessions.
  The `[UNDECIDED]` protocol tracks deferred decisions
  and auto-resolves them when you return,
  so nothing falls through the cracks.

- **Framework auto-detection** —
  scans the repo for known doc tooling config files
  (mkdocs, Sphinx, Quarto, etc.) before asking you to choose.

- **Tri-mode architecture** —
  architect for skeletons,
  editor for audits,
  engineer for build/serve/publish.
  Each loads only the instructions it needs.

- **Justfile-first engineer mode** —
  generates reproducible `just` recipes for build,
  preview,
  publish,
  and CI.
  No memorizing framework commands.

- **Progressive disclosure** —
  [`SKILL.md`](SKILL.md) routes to mode-specific instructions loaded on demand,
  keeping the skill lean and fast.

- **Framework-aware scaffolding** —
  skeletons include directory layouts,
  nav stubs,
  and config snippets tailored to your chosen framework.

- **Diataxis mode violation detection** —
  the editor flags tutorials that drift into reference,
  how-to guides that explain instead of instruct,
  and other common anti-patterns.

- **Semantic line breaks** —
  all output uses [one-sentence-per-line](https://sembr.org/),
  so diffs are clean and prose is easy to scan in source.

## Usage

Open a new session and use trigger phrases like:
- "Help me structure documentation for my library"
- "Audit my existing documentation"
- "What's missing from my docs?"
- "Librarian, I need a documentation outline"

Or invoke explicitly:
`/diapraxis architect`,
`/diapraxis editor`,
or `/diapraxis engineer`.

## Installation

Clone the repo into your platform's skills directory:

```bash
git clone https://github.com/rtbs-dev/diapraxis.git <path-to-skills-dir>
```

Or use the installer:

```bash
./install.sh                  # Auto-detect platform
./install.sh --platform cursor
./install.sh --all
```

Or install via [skillfish](https://skill.fish) (cross-platform CLI skill manager):

```bash
npx skillfish add rtbs-dev/diapraxis
```

## Configuration

Diapraxis stores all settings in the repo's `AGENTS.md` under a `## Diapraxis` section.
Running `/diapraxis architect` for the first time pre-fills most of it:
framework,
hosting,
audience,
auto-gen preferences,
prose style rules,
and task tracking.

Review the auto-generated config after the first session —
everything lives in one file,
git-trackable,
and editable by hand.

