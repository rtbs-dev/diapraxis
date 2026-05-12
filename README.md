# Diapraxis

Documentation structure and audit skill using the [Diataxis framework](https://diataxis.fr).
Diapraxis produces structural scaffolding (outlines, skeletons) and editorial audits
(gap analysis, undocumented APIs, coverage checks) — it does **not** write documentation
content for you.


## What it does

Diapraxis has three modes, each with its own instructions in [`modes/`](modes/):

| Mode | Instructions | What it produces |
|------|-------------|-----------------|
| `architect` | [`modes/architect.md`](modes/architect.md) | Documentation skeleton organized by Diataxis mode |
| `editor` | [`modes/editor.md`](modes/editor.md) | Audit report: coverage gaps, mode violations, undocumented APIs |
| `engineer` | [`modes/engineer.md`](modes/engineer.md) | Build, serve, preview, publish docs; manage CI/CD and environments |

To keep the skill lean at load time, it uses [progressive disclosure](https://docs.claude-mem.ai/progressive-disclosure):
[`SKILL.md`](SKILL.md) holds the router and shared primer, while mode-specific
instructions in [`modes/`](modes/) are loaded only when needed.

> Diapraxis never writes documentation prose — you write the content.

Instead, it provides the scaffolding and the audit.


## Usage

Open a new session and use trigger phrases like:
- "Help me structure documentation for my library"
- "Audit my existing documentation"
- "What's missing from my docs?"
- "Librarian, I need a documentation outline"

Or invoke explicitly: `/diapraxis architect`, `/diapraxis editor`, or `/diapraxis engineer`.



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

