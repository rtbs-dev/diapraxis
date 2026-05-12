# Diataxis Framework Reference

The Diataxis framework identifies four distinct documentation modes. The cardinal
rule: **keep them separate** — mixing modes is the most common documentation failure.

## The Four Modes

| Mode | Oriented toward | Reader's state | Answers |
|---|---|---|---|
| Tutorial | Learning | Beginner, studying | "Help me get started" |
| How-to guide | Doing | Practitioner, working | "How do I accomplish X?" |
| Reference | Information | Practitioner, consulting | "What does this do?" |
| Explanation | Understanding | Anyone, reflecting | "Why does it work this way?" |

### Tutorial

A lesson that takes the reader by the hand through a series of steps toward
understanding. The goal is to build confidence and a mental model — not to
produce a usable result. Tutorials are for beginners and should be safe,
reproducible, and forgiving. Every step should succeed.

Common failure: turning tutorials into how-to recipes or reference dumps.

### How-to Guide

A recipe for solving a specific real-world problem. Assumes the reader knows
what they want to accomplish but not how. Each guide addresses a single task
and gives a direct path to the solution — no digressions, no theory.

Common failure: explaining concepts instead of giving steps, or trying to
cover too many tasks in one guide.

### Reference

Dry, factual description of the machinery. API signatures, configuration keys,
module structure, CLI flags. Reference material is consulted, not read. It
should be scannable, accurate, and complete. Auto-generation from docstrings
(Sphinx, rustdoc, pdoc, JSDoc) is the norm for API reference.

Common failure: including tutorial-style narrative or explanation in reference
pages. Reference should be austere — just the facts.

### Explanation

Background, context, and design rationale. Why was the library built this way?
What trade-offs were made? What alternatives were considered? Explanation
provides understanding, not instruction. It's for anyone who wants to go deeper
after they already know how to use the tool.

Common failure: explanation sections that try to teach (tutorial) or prescribe
(how-to). Explanation is discursive, not procedural.

## Recommended Construction Order

When building documentation from scratch, Diataxis recommends:

1. Tutorials first
2. How-to guides
3. Reference
4. Explanation

This follows the natural progression of a user's relationship with a library:
they start as a learner, become a practitioner, then consult reference, and
finally seek deeper understanding.

## Sizing Guidance

Not every library needs all four modes:

| Library size | Recommended modes |
|---|---|
| Tiny (single-module utility) | Reference + 1 How-to guide |
| Small (2–5 modules) | Tutorial + Reference + 3–5 How-to guides |
| Medium (5–20 modules) | All four modes |
| Large (20+ modules) | All four modes, multiple entries per mode |

## Mode Violation Patterns

Most common mistakes found in documentation audits:

1. **Tutorial→Reference bleed**: Tutorials that go deep into API surface details
   instead of keeping the reader on a guided path.
2. **Reference→Explanation bleed**: Reference pages that include narrative,
   opinions, or design discussion.
3. **How-to→Explanation bleed**: How-to guides that explain why instead of
   showing how. "Here's why you'd want to do this..." belongs in explanation.
4. **Missing tutorial**: Only reference and how-to exist. New users have no
   on-ramp.
5. **Missing reference**: Everything is prose. No machine-readable API listing.
