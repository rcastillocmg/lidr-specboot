---
name: update-architecture-doc
description: Use when an OpenSpec change is being archived, or when a capability, module, or cross-cutting architectural decision has been added, changed, or removed, and the project's architecture document needs to reflect it.
author: LIDR.co
version: 1.0.0
---

# update-architecture-doc Skill

## Overview

Keeps `docs/architecture.md` (this repo's living "shared memory" document, see `docs/base-standards.md` §11) current and consistently formatted, so any agent session can read it and understand the system without re-reading the whole codebase. The value here isn't reminding an agent to do the update -- capable agents already do that under pressure -- it's making sure every update lands in the same place, in the same format, regardless of which session or model performs it.

## When to Use

- After implementing any OpenSpec change that adds, modifies, or removes a capability.
- When adding a genuinely new top-level module/folder (not just a file inside an existing one).
- When making a decision that affects more than one capability or module.
- When raising or resolving an Open Question.

**Not for**: typo fixes or non-architectural tweaks with no capability/module impact.

## Quick Reference

| Section | Touch it when | How |
|---|---|---|
| Capability Map | A capability was added, changed, or removed | Add/update one row: `Capability \| Where it lives \| Introduced by` |
| Change Log | Every invocation of this skill | Append one line; never rewrite or delete prior entries |
| System Overview | A genuinely new top-level module/folder exists | Add one bullet |
| Domain / Glossary | A new recurring term/concept was introduced | Add one bullet with a short definition |
| Cross-Cutting Decisions Log | A decision affects more than one capability/module | Append one line: decision + rationale + change id |
| Open Questions | Something is deliberately left unresolved | Append one line; on resolution, remove it and note the resolution in Change Log |

## Implementation

1. Read the current `docs/architecture.md` in full before editing -- never write blind.
2. Check each row of the Quick Reference table above against what this change actually did. Most updates only touch Capability Map + Change Log.
3. Append; do not delete or rewrite existing Capability Map / Cross-Cutting Decisions Log / Change Log entries unless the thing they describe was actually removed or reverted.
4. Match the exact existing table/bullet format already in the file -- do not reformat unrelated sections.
5. If unsure whether a section applies (e.g. "is this a new top-level module, or just a file inside an existing one?"), make the smaller, more conservative edit and add a one-line Open Questions entry flagging the judgment call -- do not silently guess bigger.
6. Never leave a banned vague qualifier (see `docs/base-standards.md` §10) in what you write.

## Common Mistakes

| Mistake | Why it's wrong |
|---|---|
| Rewriting the whole Change Log or Capability Map instead of appending | Loses history and breaks "who introduced what" traceability |
| Adding speculative content to System Overview/Glossary/Decisions Log/Open Questions "to be thorough" | Scope creep -- only document what this change actually changed |
| Formatting a new row/entry inconsistently with the existing ones (different column order, different id style) | Breaks consistency across changes and sessions -- always match the file's existing format |
| Treating this as lower priority than other mandatory steps because "it's just docs" | It is a mandatory step (`docs/openspec-tasks-mandatory-steps.md` Step N+4) with the same weight as the others |
