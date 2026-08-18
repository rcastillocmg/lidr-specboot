# Architecture

This is the living "shared memory" document for this repository — a map of where the pieces live and why, kept current with every OpenSpec change (see [base-standards.md §11](./base-standards.md)). A new agent session should be able to read this file and understand the shape of the system without re-reading the whole codebase.

## Purpose

This repo (`lidr-specboot`) IS the toolkit: a bundle of AI-agent development standards, skills, and agent role definitions, distributed to other projects via `packages/specboot`. There is no separate backend/frontend application here — the "system" being documented is the toolkit's own governance structure.

## System Overview

- **`docs/`** — the rulebook. `base-standards.md` is the single source of truth; it links out to `backend-standards.md`, `frontend-standards.md`, `documentation-standards.md`, `openspec-tasks-mandatory-steps.md`, and this file.
- **`ai-specs/`** — reusable playbooks: `agents/` (role definitions: backend-developer, frontend-developer, product-strategy-analyst) and `skills/` (workflow instructions, e.g. `enrich-us`, `adversarial-review`, `commit`, `writing-skills`).
- **`.claude/`, `.cursor/`** — per-tool mirrors. Skills and agents are canonically sourced from `ai-specs/` and exposed here via symlinks (see `docs/base-standards.md` §6, Symlink Integrity), kept in sync with the `sync-agent-symlinks` skill.
- **`.codex/`, `.gemini/`** — OpenSpec's own built-in skill/command integrations for those two tools, installed directly by the OpenSpec CLI (not part of the `ai-specs/` symlink convention).
- **Root files `CLAUDE.md`, `AGENTS.md`, `codex.md`, `GEMINI.md`** — symlinks, all pointing at `docs/base-standards.md`, so every AI tool reads the same rules.
- **`packages/specboot/`** — the distributable npm package (`@lidr/lidr-specboot`) that copies this whole toolkit into a consumer project via `npx @lidr/lidr-specboot`. Its `template/` directory is a separate copy for other projects and has already diverged from this repo's own `docs/`/`ai-specs/` — kept out of scope for changes to this repo's own behavior unless a change explicitly says otherwise.
- **`openspec/`** — this repo's own OpenSpec root (initialized as a prerequisite to the `tighten-decision-workflow` change). `openspec/config.yaml` declares this repo's self-referential context; `openspec/changes/` holds in-flight and archived changes; `openspec/specs/` holds the accumulated capability specs.

## Domain / Glossary

- **Skill**: a `SKILL.md` file under `ai-specs/skills/<name>/` giving an AI agent step-by-step instructions for a specific workflow (e.g. `commit`, `enrich-us`).
- **Agent**: a role definition under `ai-specs/agents/<name>.md` describing a planning-only persona (e.g. backend-developer) that proposes a plan without implementing.
- **OpenSpec change**: a folder under `openspec/changes/<name>/` containing `proposal.md`, `design.md`, `specs/*/spec.md`, and `tasks.md` for one tracked unit of work.
- **Capability**: a named, spec-tracked behavior with `## ADDED/MODIFIED/REMOVED/RENAMED Requirements` and `#### Scenario:` blocks, stored under `openspec/specs/<capability>/spec.md` once a change archives.

## Capability Map

| Capability | Where it lives | Introduced by |
|---|---|---|
| `closed-decision-language` | `docs/base-standards.md` §10 | `tighten-decision-workflow` |
| `architecture-doc-maintenance` | `docs/architecture.md` (this file) + `ai-specs/skills/update-architecture-doc/` | `tighten-decision-workflow` |
| `user-story-enrichment` | `ai-specs/skills/enrich-us/SKILL.md` | `tighten-decision-workflow` (tightened; skill existed informally before) |
| `adversarial-review` | `ai-specs/skills/adversarial-review/SKILL.md` | `tighten-decision-workflow` (tightened; skill existed informally before) |
| `commit-pr-summary` | `ai-specs/skills/commit/SKILL.md` | `tighten-decision-workflow` (tightened; skill existed informally before) |
| `openspec-task-planning` | `docs/openspec-tasks-mandatory-steps.md` | `tighten-decision-workflow` (tightened; doc existed before) |
| `ai-governance-copilot` | `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md` + `docs/base-standards.md` §12 | `add-ai-governance-copilot-default-skill` |

## Cross-Cutting Decisions Log

- **OpenSpec bootstrap scope**: initializing OpenSpec in this repo is treated as a prerequisite step, bundled inside `tighten-decision-workflow`'s own task list rather than a separate untracked commit (user decision, 2026).
- **`packages/specboot/template/` left untouched**: it already diverged from this repo's own `docs/`/`ai-specs/` independently of this change; syncing it is a separate, larger effort, not folded in here.
- **Claude Code/Cursor partial OpenSpec skill install**: this repo's pre-existing `openspec-sync-specs` skill collides in name with one of OpenSpec's built-in skills. Claude Code and Cursor received only 3 of 6 built-in OpenSpec skills and no command shortcuts; Codex and Gemini installed cleanly. Accepted as a known, deferred gap (user decision) — not fixed as part of `tighten-decision-workflow`.
- **`enrich-us` output format**: the old free-form "Enhanced" section is removed outright rather than kept as a parallel recap, since a second prose restatement of the same Closed Decisions creates a place for them to silently drift apart; one section per decision is the safer choice.
- **`cmg-ai-governance-copilot` sourced as a project-local copy, not a cross-repo symlink**: its canonical source is the separate `CMG_AI_Governance` repo, outside this project's own `ai-specs` symlink convention (§6). A cross-repo symlink would break for anyone without that exact sibling folder (a teammate, a fresh GitHub clone, CI), so a point-in-time copy was chosen instead, with drift documented as an accepted trade-off in `docs/base-standards.md` §12 (user decision, 2026-08-18, `add-ai-governance-copilot-default-skill`).
- **`cmg-ai-governance-copilot` frontmatter description rewritten, body left untouched**: a consistency check against this repo's own `writing-skills` rules found the canonical description exceeded the 1024-character frontmatter cap and summarized the whole workflow instead of stating only triggers. The description was rewritten to comply; every operational instruction in the body was kept byte-for-byte identical to the canonical source (user decision, 2026-08-18, `add-ai-governance-copilot-default-skill`).

## Open Questions

- Should `packages/specboot/template/` eventually be brought back in sync with this repo's `docs/`/`ai-specs/`? Deferred, not answered here.
- Should the Claude Code/Cursor `openspec-sync-specs` naming collision be resolved (by renaming this repo's own skill) in a future change? Deferred, not answered here.

## Change Log

- **`tighten-decision-workflow`**: bootstrapped OpenSpec in this repo; added `closed-decision-language` and `architecture-doc-maintenance` capabilities; created this document; tightened `enrich-us` (architecture-doc read, blocking-vs-non-blocking gap classification, new output format), `adversarial-review` (loop-back-to-implementer on Blocker/Major), and `commit` (five mandatory PR-description sections); fixed the pre-existing vague-language instance in `docs/openspec-tasks-mandatory-steps.md`; synced `README.md`.
- **`add-ai-governance-copilot-default-skill`**: added the `ai-governance-copilot` capability -- a project-local copy of the CMG AI Governance Co-Pilot skill at `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md`, symlinked into `.claude/skills` and `.cursor/skills`, and declared a standing default in `docs/base-standards.md` §12; frontmatter description rewritten for local skill-format compliance, body kept byte-for-byte identical to the canonical `CMG_AI_Governance` source.
