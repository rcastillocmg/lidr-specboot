## Purpose

Ensures every agent working in this repo -- Claude, Cursor, or a teammate who clones it -- gets the CMG AI Governance Co-Pilot's proactive coverage by default, without depending on any individual's personal global skill configuration.

## ADDED Requirements

### Requirement: Project-local skill copy
The system SHALL provide a project-local copy of the CMG AI Governance Co-Pilot skill at `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md`, whose body (every operational instruction, all seven steps, and the hard boundaries) is byte-for-byte identical to the canonical source in the `CMG_AI_Governance` repo at the time it was copied.

#### Scenario: Agent works in this repo without personal global skills configured
- **WHEN** an agent (Claude, Cursor, or otherwise) opens this repo and has no CMG-specific global skill configuration of its own
- **THEN** `cmg-ai-governance-copilot` is still available to it, sourced from `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md`

### Requirement: Frontmatter complies with this repo's skill-writing rules
The project-local copy's frontmatter `description` SHALL comply with `ai-specs/skills/writing-skills/SKILL.md` (1024-character total frontmatter limit, trigger-only wording, no workflow summary), even where the canonical source does not. The skill body SHALL NOT be altered to achieve this -- only the `description` field changes.

#### Scenario: Copying a canonical skill whose frontmatter exceeds this repo's limit
- **WHEN** a skill is copied into `ai-specs/skills` from an external, non-`lidr-specboot` source and its frontmatter conflicts with `writing-skills`' rules (length, or a description that summarizes workflow instead of stating triggers)
- **THEN** the frontmatter `description` is rewritten to comply, the skill body is left untouched, and the divergence from the canonical description is called out explicitly in `docs/base-standards.md` rather than left unstated

### Requirement: Default, proactive trigger declared for this project
`docs/base-standards.md` SHALL declare `cmg-ai-governance-copilot` a standing default skill for this project, so it is expected to trigger proactively -- the same way it is embedded by default on the exo cluster platform -- rather than requiring manual attachment.

#### Scenario: Recurring AI/LLM-assisted build starts in this repo
- **WHEN** someone begins building a new skill, agent, or automation in this repo (a workflow that will run repeatedly, not a one-off task)
- **THEN** the agent applies `cmg-ai-governance-copilot`'s Step 1 intake proactively, per `docs/base-standards.md`'s standing-default declaration, without being asked to attach the skill first

### Requirement: Agent-facing symlink exposure
The project-local skill SHALL be exposed to `.claude/skills/cmg-ai-governance-copilot` and `.cursor/skills/cmg-ai-governance-copilot` as symlinks to the canonical copy in `ai-specs/skills`, consistent with every other skill in this repo.

#### Scenario: Skill added to ai-specs/skills
- **WHEN** `ai-specs/skills/cmg-ai-governance-copilot` is created
- **THEN** `.claude/skills/cmg-ai-governance-copilot` and `.cursor/skills/cmg-ai-governance-copilot` exist as valid symlinks resolving to it, with no broken or orphaned links left behind

### Requirement: Documented drift trade-off
The project-local copy SHALL be documented as a point-in-time copy of the canonical source, not a live link -- re-syncing it after the canonical skill changes SHALL be an explicit manual step, never an automatic or silent one. This documentation SHALL also disclose that the frontmatter `description` was intentionally reworded at copy time (see Frontmatter complies with this repo's skill-writing rules), distinct from any future drift in the body.

#### Scenario: Canonical skill in CMG_AI_Governance changes after this copy was made
- **WHEN** the canonical `cmg-ai-governance-copilot` skill in the `CMG_AI_Governance` repo is updated
- **THEN** the project-local copy in this repo does not change automatically, and `docs/base-standards.md` states plainly that keeping the two in sync is a manual step someone must perform

#### Scenario: Someone compares the project-local description to the canonical one
- **WHEN** someone notices the frontmatter `description` in `ai-specs/skills/cmg-ai-governance-copilot/SKILL.md` does not match the canonical source word-for-word
- **THEN** `docs/base-standards.md` already explains this was a deliberate, one-time rewording for local skill-format compliance, not an error or unintended drift
