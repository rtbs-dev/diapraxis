# Diapraxis

Documentation structure and audit skill using the [Diataxis framework](https://diataxis.fr).
Diapraxis produces structural scaffolding (outlines, skeletons) and editorial audits
(gap analysis, undocumented APIs, coverage checks) — it does **not** write documentation
content for you.


## What it does

| Subcommand | What it produces |
|---|---|
| `architect` | Documentation skeleton organized by Diataxis mode |
| `editor` | Audit report: coverage gaps, mode violations, undocumented APIs |

Diapraxis never writes documentation prose — you write the content. It provides
the scaffolding and the audit.


## Usage

Open a new session and use trigger phrases like:
- "Help me structure documentation for my library"
- "Audit my existing documentation"
- "What's missing from my docs?"
- "Librarian, I need a documentation outline"

Or invoke explicitly: `/diapraxis architect` or `/diapraxis editor`.



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

Or install via [skillfish](https://skill.fish) (cross-platform CLI skill manager):

```bash
npx skillfish add rtbs-dev/diapraxis
```

