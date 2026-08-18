## Purpose

Requires every addition to this project -- a skill, an agent, an OpenSpec capability, a documentation section, a symlink, or any other new artifact -- to be checked against what already exists before it is finalized, so conflicts are caught before they ship instead of relying on someone remembering to ask for the check.

## ADDED Requirements

### Requirement: Check every addition against existing artifacts
Before any new skill, agent, OpenSpec capability, documentation section, symlink, or other artifact under `ai-specs/`, `docs/`, `.claude/`, `.cursor/`, or `openspec/` is finalized, the agent SHALL check it against what already exists in this repo for conflicts.

#### Scenario: A new skill, agent, or capability is proposed
- **WHEN** an agent is about to finalize a new skill, agent, capability, or other addition to this project
- **THEN** the agent checks it against existing skill names/descriptions in `ai-specs/skills/*`, existing agent definitions in `ai-specs/agents/*`, existing capability names in `openspec/specs/*`, `docs/base-standards.md` and the docs it links to, and `docs/architecture.md`'s Capability Map, before treating the addition as finished

### Requirement: Defined conflict taxonomy
The system SHALL define what counts as a conflict for this check: a naming collision, an overlapping trigger or scope with an existing skill or rule, wording that contradicts or silently supersedes an existing governance rule, or non-compliance with this repo's own established format rules for that artifact type.

#### Scenario: A found conflict falls into one of the defined categories
- **WHEN** the consistency check surfaces an issue
- **THEN** the agent classifies it as a naming collision, an overlapping trigger/scope, contradictory wording, or a convention non-compliance, rather than reporting it as an unstructured concern

### Requirement: Resolve or explicitly flag, never silently merge
Any conflict found by this check SHALL be resolved before the addition is finalized, or, if it cannot be resolved immediately, recorded as an explicit Open Question in the change's `design.md` per the Closed-Decision Language rule (`docs/base-standards.md` §10) -- never merged with the conflict unaddressed and unmentioned.

#### Scenario: A conflict is found but the right resolution isn't obvious
- **WHEN** the consistency check finds a real conflict and the correct fix isn't a closed decision the agent can make alone
- **THEN** the agent asks the user directly, or records it as an explicit Open Question naming what must be resolved and by whom, rather than proceeding silently or guessing
