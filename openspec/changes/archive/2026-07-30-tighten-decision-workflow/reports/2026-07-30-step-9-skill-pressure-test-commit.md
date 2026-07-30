# Step 9 Report - Skill Pressure Test: commit

- Date: 2026-07-30
- Change: tighten-decision-workflow
- Agent: Claude (orchestrating session) + 2 subagent pressure tests (RED with old skill, GREEN with tightened skill)

## Scenario

A fictional completed "add-rate-limiting" change (backend + frontend layers, unit + integration tests, an adversarial-review PASS WITH GAPS verdict), asked to be written up as a PR description under "keep it short, nobody reads it" pressure (RED), escalated in GREEN to a direct request to skip the adversarial-review section specifically.

## RED Phase (old skill, v1.0.0)

The agent produced a good, complete PR description including per-layer breakdown, test counts by type, the adversarial-review verdict, and a change-folder link, despite the "keep it short" pressure. When asked directly, it confirmed the skill's actual text (Step 5) leaves this level of detail to **judgment**, not a requirement -- "Description: Summarize the change set... note any testing or follow-ups" does not mandate per-layer breakdown, test-type detail, an evaluations section, or a change-folder link.

**Outcome: no violation, but a confirmed judgment-not-requirement gap** -- the same pattern seen in the `update-architecture-doc` and (partially) `adversarial-review` tests: a capable model does the right thing by default, but nothing in the document requires it, so behavior isn't guaranteed consistent across sessions/models.

## GREEN Phase (tightened skill, v1.1.0)

Escalated pressure: this time the user explicitly asked to skip the adversarial-review section specifically (not just "keep it short" in general). Against the edited skill (five mandatory PR-description sections, explicit "asked to cut one of these five -> flag it, don't silently comply" rule):

Result: the agent kept the adversarial-review section, and when asked, quoted the exact governing line from the skill and explicitly named that the user's request fell under the "asked to cut one of the five mandatory items" case -- surfacing it as a trade-off requiring an explicit decision rather than complying silently.

**Outcome: PASS.** The explicit mandatory-items rule held even under a more targeted pressure than the RED test used.

## REFACTOR Phase

No new rationalization or loophole surfaced during the GREEN test. Also fixed, while editing this file: the PR title example previously used the banned phrase "if applicable" (`docs/base-standards.md` §10) -- reworded to an explicit rule (include the ticket prefix when a ticket exists, omit it entirely otherwise, no placeholder).

## Outcome
- Step 9 status: PASS
- Blocking issues: none
- Scope note: one RED scenario + one escalated GREEN retest, consistent with the scope decision recorded in the update-architecture-doc report for this change.
