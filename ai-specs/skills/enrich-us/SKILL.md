---
name: enrich-us
description: Analyze and enhance user stories with complete, implementation-ready technical detail and closed decisions, from direct ticket input or Jira.
author: LIDR.co
version: 2.0.0
---
# enrich-us Skill

Use it when this workflow is required in the project.

## Instructions

Please analyze and enrich the ticket: $ARGUMENTS.

Follow these steps:

1. Determine the ticket input source:
   - **Direct input mode (default when ticket text is provided):** Use the ticket content shared by the user in the prompt/chat.
   - **Jira mode (optional):** If the user provides a Jira id/key, or asks to use Jira (including references like "the one in progress"), use Jira MCP to fetch the ticket details.
2. Act as a product expert with technical knowledge.
3. Read `docs/architecture.md` for system/domain context before analyzing the ticket, and cross-check the proposed change against the existing codebase for consistency (naming, patterns, avoiding duplication) -- not just product completeness. Do this even under time pressure; it is not optional busywork.
4. Understand the problem described in the ticket.
5. Decide whether or not the User Story is completely detailed according to product best practices. Validate that it includes:
   - Full functionality description
   - Comprehensive list of fields to update
   - Required endpoints structure and URLs
   - Files/modules to modify according to architecture and best practices
   - Definition of done (implementation and delivery steps)
   - Documentation and unit test updates
   - Non-functional requirements (security, performance, observability, etc.)
6. Classify any gap found in step 5 as **blocking** or **non-blocking**, and handle each differently -- do not treat every gap as a soft flag:
   - **Blocking**: the gap makes one of the step-5 checklist items impossible to fill honestly (e.g. the target system/module isn't identified at all, so "files/modules to modify" cannot be named). For a blocking gap, you MUST ask the user a direct clarifying question before finalizing, and MUST NOT produce output that looks implementation-ready while that gap remains. State plainly what's blocking and what answer you need.
   - **Non-blocking**: a real but resolvable gap (an edge case, a threshold, a status-mapping detail) where a reasonable default exists. Resolve it into an explicit Closed Decision, or record it as an explicit flagged Open Question -- never leave it as a silent, un-flagged assumption, and never use a banned vague qualifier (see `docs/base-standards.md` §10) to paper over it.
   - **Closing the loophole**: "the user is in a hurry" or "someone with authority already approved the one-liner" are not, by themselves, reasons to downgrade a blocking gap to non-blocking. Authority over a decision you were explicitly given (e.g. a specific threshold value) does not extend to gaps nobody has actually reviewed. If you catch yourself reasoning "I don't have to ask because nothing here technically forces me to," that is the rationalization to stop and reject, not follow.
7. Produce the enriched output using project technical context from `docs/architecture.md` and the codebase. Return the result in markdown.
8. Output format must always include, in this order:
   - `## Original` (the raw input, unchanged)
   - `## Scope` (what's in scope, what's explicitly out of scope)
   - `## Closed Decisions` (every resolved ambiguity from step 6, stated as a decision, not a question)
   - `## Expected Behavior` (what the system should do, concretely)
9. Jira write-back is optional and only applies in Jira mode, **and only when step 6 found no unresolved blocking gap**:
   - If a blocking gap is open, do not write back to Jira and do not change ticket status -- post the direct clarifying question to the user instead, and wait for an answer before touching Jira at all.
   - Once there is no unresolved blocking gap: update the Jira ticket by appending the enhanced content after the original content, with clear `h2` sections matching the output format above, and readable formatting (lists/code snippets when useful).
   - If ticket status is `To refine`, move it to `Pending refinement validation`.

## Notes

- Do not require Jira when the user already provided full ticket content directly.
- If input is ambiguous (for example, user gives a short reference without content), ask whether to resolve via Jira or request the full ticket text.
- A ticket with an unresolved blocking gap is not "enriched" -- it's a request for the missing answer. Say so plainly rather than dressing up an incomplete answer as a finished one.
