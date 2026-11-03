# Bounded delegation and critical review

Use this only when subagents or independent report verification improve a real decision. Do not invent subagent tools or assume every agent environment exposes delegation.

## Roles

**Primary owner:** states the user outcome, makes decisions, performs/owns integration, tests, and final reporting. Exactly one owner is accountable even if there are many helpers.

**Skeptical reviewer (read-only):** looks for contradictions, unproven assumptions, false-success tests, rights/security holes, and cheaper alternatives. Does not implement, rewrite the full plan, push, or spawn another agent.

**Narrow specialist (optional, read-only):** verifies one independently answerable point (e.g. a device compatibility claim, an endpoint's actual semantics). Cite checked material.

Parallel agents may work on genuinely disjoint tasks, but tightly coupled writes should have one owner. The primary agent integrates findings rather than forwarding reports untouched to the user.

## Reviewer invocation packet

```text
TASK: Independent skeptical review (read-only). Do not edit code or files.

USER OUTCOME: <observable success and acceptable fallback>
CURRENT FAILURE: <what the user actually experiences>
APPROACH: <proposed solution or implementation contract>
AUTHORITATIVE FACTS: <verified facts with evidence>
ORIGINAL SOURCES: <accessible relevant artifacts or source excerpts; provenance>
VERIFICATION LIMITS: <what existing checks prove and leave untested>
CONSEQUENTIAL CHOICES: <defaults, assumptions, scope changes, requirement interpretations>
HARD CONSTRAINTS: <must preserve / may not do>
ASSUMPTIONS: <what we don't know yet>
ACCEPTANCE TESTS: <what would demonstrate success>

Find AT MOST THREE materially important problems, ordered by severity.
For each return:
  CLAIM AT RISK: <what might be wrong>
  EVIDENCE: <actual observation/source or explicitly hypothesis>
  COUNTEREXAMPLE: <how the solution could fail while tests pass>
  TEST/FIX: <smallest concrete step to falsify or repair it>
  SEVERITY: BLOCKER | IMPORTANT | MINOR

Then state:
  VERDICT: ACTIONABLE | ACTIONABLE WITH CAVEATS | NOT ACTIONABLE
  SINGLE MOST VALUABLE NEXT TEST: <one sentence>

Do not draft an entirely new architecture unless the proposed one cannot
meet the user's outcome. Do not ask another agent to plan for you.
```

To ask a subagent to apply this skill, ask it to use critical-briefsmith in REVIEW mode **only when the environment actually supports skill invocation inside subagents**. Otherwise send the packet directly. No assumption of inherited skill context.

Review the relevant original artifacts before endorsing consequential claims. Use the previous agent's summary to locate evidence, not as a substitute for it. If access is unavailable, state the limitation and narrow the verdict. Preserve confidentiality when passing sources; access to private material does not authorize redistribution.

## Reviewer reconciliation is obligatory

For each objection the primary agent records one of:

- `ACCEPT`: adjust plan or add a targeted test.
- `DISPROVE`: provide a traceable observation invalidating the objection.
- `DEFER`: explicitly explain why it is not a blocker for the current milestone.

Do not blindly adopt a reviewer suggestion simply because it sounds skeptical. Do not discard a valid blocker because it delays the preferred implementation.

## Reviewing a completed agent report

Verify the original requirements, not the agent's enthusiasm:

| Claim | Needed evidence | False inference to reject |
| --- | --- | --- |
| “Implemented” | Diff/artifact and runnable command | A prose summary is not code |
| “Unit tests pass” | Test result and scope | Not proof of end-to-end success |
| Export works | Open the file and verify representative values | HTTP success alone is insufficient |
| Target application is compatible | Test the artifact in that application | A format validator alone is insufficient |
| “Preserved canonical data” | Relevant baselines/hashes | “No changes intended” is not verification |
| “No AI runtime” | Audit of runtime call paths | An AI coding assistant isn't the runtime |
| “Ready for users” | User journey, install docs and fallback | A branch build is not a release |

Classify verdict as `ACCEPT`, `ACCEPT WITH CAVEATS`, `REQUEST TARGETED FIX`, or `BLOCKED`. Include the largest demonstrated gap and one smallest necessary follow-up. A missing physical test may mean “usable software, hardware unverified”—not “rewrite the whole system.”

## Prevent the report-to-prompt loop

Default maximum: one bounded independent review per design/patch unless **new evidence** justifies a second. The primary owner should complete straightforward targeted fixes without asking another planner to re-plan the whole project. Once the milestone is sufficiently proven, stop and hand the user the result.
