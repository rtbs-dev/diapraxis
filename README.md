# Diapraxis

Documentation structure and audit skill using the [Diataxis framework](https://diataxis.fr).
Diapraxis produces structural scaffolding (outlines, skeletons) and editorial audits
(gap analysis, undocumented APIs, coverage checks) — it does **not** write documentation
content for you.

## Installation

```bash
# Clone into your platform's skills directory:

# Claude Code
git clone https://github.com/your-org/diapraxis.git ~/.claude/skills/diapraxis

# VS Code Copilot
git clone https://github.com/your-org/diapraxis.git .github/skills/diapraxis

# Cursor
git clone https://github.com/your-org/diapraxis.git .cursor/rules/diapraxis

# Windsurf
git clone https://github.com/your-org/diapraxis.git ~/.codeium/windsurf/skills/diapraxis

# OpenCode
git clone https://github.com/your-org/diapraxis.git ~/.config/opencode/skills/diapraxis

# Cline
git clone https://github.com/your-org/diapraxis.git ~/.cline/rules/diapraxis

# Roo Code
git clone https://github.com/your-org/diapraxis.git .roo/rules/diapraxis

# Gemini CLI
git clone https://github.com/your-org/diapraxis.git ~/.gemini/skills/diapraxis

# Codex CLI / Kiro / Trae / Antigravity / Goose
git clone https://github.com/your-org/diapraxis.git ~/.agents/skills/diapraxis
```

Or use the installer:

```bash
./install.sh                  # Auto-detect platform
./install.sh --platform cursor
./install.sh --all
```

## Usage

Open a new session and use trigger phrases like:
- "Help me structure documentation for my library"
- "Audit my existing documentation"
- "What's missing from my docs?"
- "Librarian, I need a documentation outline"

Or invoke explicitly: `/diapraxis architect` or `/diapraxis editor`.

## Aliases

The skill responds to **"librarian"** and **"Wan Shi Tong"** as session continuation aliases.

## What it does

| Subcommand | What it produces |
|---|---|
| `architect` | Documentation skeleton organized by Diataxis mode |
| `editor` | Audit report: coverage gaps, mode violations, undocumented APIs |

Diapraxis never writes documentation prose — you write the content. It provides
the scaffolding and the audit.
