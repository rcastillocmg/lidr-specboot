## Context

`cmg-ai-governance-copilot`'s canonical source lives in a separate repo, `CMG_AI_Governance/_local-claude-skills/cmg-ai-governance-copilot`, and today reaches this repo only via a personal symlink in the user's global `.claude/skills`. This repo's own Symlink Integrity rule (`docs/base-standards.md` §6) treats `ai-specs` as the canonical source for artifacts used inside `lidr-specboot`, mirrored out to `.claude/skills` and `.cursor/skills` by the `sync-agent-symlinks` skill -- but that convention assumes the canonical source is inside this repo. It isn't, here. See proposal.md - Why for the motivation.

## Goals / Non-Goals

**Goals:**
- Make governance coverage part of the project, not dependent on one person's personal machine configuration.
- Keep the sourcing mechanism consistent with this repo's existing skill conventions (`ai-specs/skills` as canonical, symlinked into `.claude/skills` and `.cursor/skills`).
- Be explicit that this is a copy, not a live mirror, so nobody mistakes it for auto-updating.

**Non-Goals:**
- Building tooling to auto-detect or auto-pull changes from the canonical `CMG_AI_Governance` source. That repo isn't part of this project and isn't necessarily even under this repo's version control conventions.
- Changing `packages/specboot/template/`, the distributable copy for other projects (out of scope per existing project convention).
- Extending symlink coverage to `.codex/skills` or `.gemini/skills` -- those mirrors aren't managed by this repo's `sync-agent-symlinks` skill today, and adding that is a separate change.

## Decisions

**Copy the skill content into `ai-specs/skills` rather than symlinking to `CMG_AI_Governance`.** Two options were considered:

1. **Project-local copy (chosen).** Copy `SKILL.md` into `ai-specs/skills/cmg-ai-governance-copilot/`, then mirror it to `.claude/skills` and `.cursor/skills` exactly like every other project skill. Self-contained: works for anyone who clones `lidr-specboot`, with or without `CMG_AI_Governance` present on their machine.
2. **Cross-repo symlink.** Point `ai-specs/skills/cmg-ai-governance-copilot` at the sibling `CMG_AI_Governance/_local-claude-skills/cmg-ai-governance-copilot` folder, mirroring the user's existing personal global setup. Always current automatically, but a relative symlink pointing outside the repo breaks for anyone -- a teammate, a fresh GitHub clone, a CI runner -- who doesn't have that exact sibling folder in place.

The project-local copy was chosen (user decision, 2026-08-18) because `lidr-specboot` is a repo meant to be cloned and worked in by more than one person and more than one agent tool, and a broken external symlink defeats the purpose of making this a *default* rather than personal-only.

**Trade-off accepted explicitly:** the copy can drift from the canonical source. This is recorded as a requirement in `specs/ai-governance-copilot/spec.md` (Documented drift trade-off) rather than solved technically -- there is no automatic re-sync in this change.

**Rewrite the frontmatter `description`, leave the body untouched.** A consistency check against this repo's own `ai-specs/skills/writing-skills/SKILL.md` rules found a real conflict: the canonical skill's frontmatter is 1047 characters (over the 1024-character hard cap `writing-skills` sets), and its 996-character description narrates the entire seven-step workflow instead of stating only trigger conditions -- the exact anti-pattern `writing-skills` documents as causing an agent to follow the description as a shortcut and skip reading the actual instructions. Two options were considered:

1. **Keep the frontmatter as-is, document it as an exception.** Preserves the governance wording 100% faithfully, at the cost of this one skill not matching every other skill's format in this repo.
2. **Rewrite only the description field (chosen).** A new, ~290-character, trigger-only description replaces the canonical one; every operational instruction in the body (all seven steps, the hard boundaries, the Policy-reference note) stays byte-for-byte identical to the canonical source. This makes the skill discoverable and consistent with every other skill here, without touching any of the actual governance instructions.

Chosen (user decision, 2026-08-18) because the description is metadata about the skill, not governance policy text itself -- the actual instructions the skill acts on are unchanged. `docs/base-standards.md` should note this specific divergence (not just future drift) so nobody assumes the description alone was pulled verbatim from `CMG_AI_Governance`.

## Risks / Trade-offs

- **[Risk]** The project-local copy silently goes stale as the canonical skill in `CMG_AI_Governance` evolves (new steps, updated Policy wording, corrected boundaries). → **[Mitigation]** `docs/base-standards.md` states plainly that this is a manual-resync copy, not a live link, so nobody assumes it's current without checking. No automated staleness detection is in scope for this change.
- **[Risk]** Declaring the skill "standing default" in `docs/base-standards.md` could be read as a stronger technical guarantee than it is -- nothing forces every agent session to actually load it, the same limitation the skill's own Step 6 (go-live warning) already acknowledges about itself platform-wide. → **[Mitigation]** Phrase the base-standards.md addition the same way the skill's own text does: a proactive-trigger expectation, not a technical enforcement mechanism.

## Migration Plan

No rollback complexity: this only adds new files (a skill folder, two symlinks) and a documentation section. Reverting means deleting `ai-specs/skills/cmg-ai-governance-copilot`, its two symlinks, and the `docs/base-standards.md` addition.
