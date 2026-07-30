## Purpose
Produces pull request descriptions detailed enough to review without re-reading the whole diff -- what changed per layer, what was tested, and what was evaluated.

## ADDED Requirements

### Requirement: Detailed PR description content
The `commit` skill SHALL include, in every PR description it generates: a summary of changes per layer touched (backend/frontend/docs/tests), tests added or updated by type (unit/integration/e2e/smoke) with their report paths, evaluations applied (adversarial-review verdict, verification checklist outcome), a link to the OpenSpec change folder, and any open follow-ups.

#### Scenario: Creating a PR for a completed change
- **WHEN** the `commit` skill creates or updates a pull request for a change
- **THEN** the PR description MUST include all of: per-layer summary, tests-by-type with report paths, evaluations applied, a link to the change folder, and open follow-ups (or an explicit statement that there are none)
