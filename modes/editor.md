# Editor mode

**Purpose**: Audit existing documentation against Diataxis principles and flag
gaps, violations, and missing coverage.

### When to use

The user has existing documentation (partial or complete) and wants to know
what's missing or broken structurally.

## Workflow

### Before auditing

1. **Read AGENTS.md** — check for an existing `## Diapraxis` section. Load:
   - `### Config` — framework, hosting, audience decisions
   - `### Notes` — previous context, known issues, [UNDECIDED] items
   - `### Tasks` — pending items (incorporate these into the audit)

2. **If no Diapraxis section exists**, the editor creates one as part of its
   output.

### Gather materials

Ask the user to provide:
- The documentation structure (file list, outline, or actual content)
- The public API surface or module list (if available)
- Any existing test suite or example code (optional, helps find undocumented
  usage patterns)
- Framework being used (if not already in AGENTS.md Config)

### Run the audit

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

### Produce a prioritized gap list

Order gaps by impact. A missing tutorial is usually higher impact than a
missing explanation section.

### Write findings to AGENTS.md

After the audit, update the `## Diapraxis` section in AGENTS.md:

- **Config**: Ensure framework and hosting are recorded if newly discovered
- **Notes**: Append audit findings — coverage gaps, mode violations,
  undocumented APIs, with audit date
- **Tasks**: Add actionable gaps as checklist items or note them under the
  existing task tracking method (inline checklist or GH Issues pointer)

## Output format

Produce a structured audit report in markdown with the four audit sections above.
End with a prioritized action list. Do not rewrite or draft any documentation
content — only identify what's missing and where.

Also output the AGENTS.md diff so the user can see what was recorded.