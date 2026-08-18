## Why

`lidr-specboot` is itself an agent/skill-authoring toolkit -- exactly the kind of repo the CMG AI Governance Co-Pilot skill (`cmg-ai-governance-copilot`) exists to watch, since building new skills and agents here is recurring AI/LLM-assisted building under the CMG AI Policy, and this repo's own Symlink Integrity rule (`docs/base-standards.md` §6) explicitly calls out "new agents or skills" as artifacts that need multi-agent exposure. Today, governance coverage only exists because the skill happens to be symlinked into the user's *personal* global `.claude/skills` folder -- it is not part of the project itself, so any other agent tool working in this repo (Cursor, Codex, Gemini), or a teammate who clones the repo, gets no governance coverage at all. Embedding the skill as a project-level default closes that gap.

## What Changes

- Add a project-local copy of the CMG AI Governance Co-Pilot skill at `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md`, copied from the canonical source in the separate `CMG_AI_Governance` repo, following this repo's normal skill-folder convention.
- Rewrite only the copy's frontmatter `description` field to comply with this repo's own `writing-skills` rules (max 1024 total frontmatter characters, trigger-only wording, no workflow summary) -- the canonical description is 996 characters and narrates the whole workflow, which both exceeds the limit and is the exact anti-pattern `writing-skills` warns against. The skill body (every operational instruction, all seven steps, the hard boundaries) stays byte-for-byte identical to the canonical source; only the description changes.
- Symlink the new skill into `.claude/skills/cmg-ai-governance-copilot` and `.cursor/skills/cmg-ai-governance-copilot` via the existing `sync-agent-symlinks` skill, so it mirrors every other project skill.
- Add a new subsection to `docs/base-standards.md` declaring `cmg-ai-governance-copilot` a standing default skill for this project -- proactively watching any recurring AI/LLM-assisted building that happens while working in this repo (including agent-building-agent work, per the skill's own Step 7), the same way it is embedded by default on the exo cluster platform.
- Document the drift trade-off explicitly: this is a point-in-time copy, not a live link to the canonical source in `CMG_AI_Governance`. Re-syncing it when the canonical skill changes is a manual step, not automatic.

## Capabilities

### New Capabilities
- `ai-governance-copilot`: this project's default embedding of the CMG AI Governance Co-Pilot skill -- the project-local copy, its symlink exposure to Claude/Cursor, and the standing-default rule in `docs/base-standards.md` that makes it apply automatically rather than needing manual attachment.

### Modified Capabilities
(none -- no existing capability's requirements change; this only adds a new one.)

## Impact

- Files: `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md` (new, copied from `CMG_AI_Governance/_local-claude-skills/cmg-ai-governance-copilot/SKILL.md`), `docs/base-standards.md`, `docs/architecture.md`.
- Symlinks: `.claude/skills/cmg-ai-governance-copilot` and `.cursor/skills/cmg-ai-governance-copilot` (new, via `sync-agent-symlinks`).
- Explicitly out of scope: the `CMG_AI_Governance` repo itself (canonical source, not touched); `packages/specboot/template/` (already diverged from this repo independently, per existing project convention); any automation to auto-sync the project-local copy with the canonical source (the proposal above treats re-sync as a deliberate manual step, not a feature to build); `.codex/skills` and `.gemini/skills` mirrors (not managed by this repo's `sync-agent-symlinks` skill today, so out of scope for this change).
