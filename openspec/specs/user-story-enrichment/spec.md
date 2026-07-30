# user-story-enrichment Specification

## Purpose
Turns a vague user story into a fully closed set of requirements -- reading real architecture and code context, and refusing to leave any ambiguity open -- before implementation can begin.
## Requirements
### Requirement: Architecture and code context before enriching
The `enrich-us` skill SHALL read `docs/architecture.md` and check the proposed change against the existing codebase for consistency (naming, patterns, duplication) before producing an enriched user story.

#### Scenario: Enriching a ticket
- **WHEN** an agent runs `enrich-us` on a ticket
- **THEN** the agent MUST first read `docs/architecture.md` and cross-check the proposal against existing code, and MUST NOT skip either step even under time pressure

### Requirement: Close all ambiguity before returning
The `enrich-us` skill SHALL ask targeted clarifying questions until no ambiguity about scope, behavior, or edge cases remains, and SHALL NOT fill a gap with a banned vague qualifier (see `closed-decision-language`).

#### Scenario: A ticket has an unresolved ambiguity
- **WHEN** the ticket under enrichment leaves an open question about scope, behavior, or an edge case
- **THEN** the agent MUST ask the user a targeted clarifying question or record it as an explicit flagged Open Question, and MUST NOT produce a final enriched story with that ambiguity silently unresolved

### Requirement: Output format
The `enrich-us` skill SHALL produce output in the format: `## Original` (the raw input, unchanged), `## Scope`, `## Closed Decisions`, `## Expected Behavior`. The prior free-form `## Enhanced` section is removed.

#### Scenario: Final enriched output is produced
- **WHEN** `enrich-us` finishes enriching a ticket
- **THEN** the output MUST contain exactly the sections Original, Scope, Closed Decisions, and Expected Behavior, in that order, with no separate "Enhanced" section

