# One-shot agent brief template

Use for **BRIEF** mode. Assemble only sections that matter. In particular, avoid making the agent write a research essay instead of implementing a safe usable slice. The finished user-visible output should be **one complete brief**; no separate “phase 1 prompt” followed by “phase 2 prompt” unless the user asks for staged approval.

## Template

```text
PROJECT: <name>
MILESTONE: <observable result>
PRIMARY MODE: <investigate / implement / evaluate>

MISSION
Describe the end-user workflow as it will happen when the work is complete.
State what is broken today and why this milestone outranks adjacent work.

AUTHORITATIVE CONTEXT
State verified facts, current code/branch when verified, relevant past failures,
and named references. Mark uncertain facts as unverified.
Do not fabricate repo state or call reports definitive proof of hardware behavior.

SAFETY AND SCOPE
Worktrees/branches; read-only surfaces; files and history to preserve;
restricted items; credentials and remote action approval.
Clarify exactly which actions are authorized now.
Explicitly say what NOT to redesign.

INVESTIGATE AND REUSE
Inspect existing code, docs, tests and maintained alternatives.
Check if the proposed dependency/standard solves the user's actual workflow.
Prefer a bounded, reusable integration over duplicated systems.

IMPLEMENTATION
Step 1: reproduce the actual baseline.
Step 2: build the smallest viable end-to-end path.
Step 3: cover the essential failure modes and fallback.
Step 4: integrate with existing config and state.
Avoid parallel implementations of one engine.

CRITICAL NEGATIVE REQUIREMENTS
What naive solution could pass tests but fail the user?
What must not be claimed, copied, erased, sent, changed, or hardcoded?

ACCEPTANCE CRITERIA
Objective observable assertions, input/output locations, sample scenarios,
negative tests, permissions and nonregression.
Separate mocked tests from real component and real device tests.
Define what each material check proves and what it cannot establish.

REPRODUCIBILITY AND PRESERVATION
Capture baseline and hashes where useful; isolate experiments;
verify real artifacts; record version and exact commands.

DELIVERY AND STOP CONDITIONS
Complete the requested artifact; do not stop after an architecture memo.
Stop before irreversible or unauthorized actions.
Do not advance unrelated milestones to cover an incomplete core task.

FINAL REPORT
What actually changed; exact paths/commands; test evidence;
what each material check proves and misses; consequential defaults, assumptions,
scope changes and requirement interpretations; risks and next smallest step.
```

## Calibrate scope

### Small change (5–20 lines of brief)

Say what is broken, desired result, relevant constraints, exact test, report format. Do not include a lengthy phases diagram.

### Medium integration (roughly 1–3 pages)

Include existing architecture, compatibility checks, safe mutation boundaries, direct end-to-end test, meaningful negative cases and user handoff.

### Fragile large project (long comprehensive prompt)

Use full template plus reference-backed provider behavior, migration/rollback, rights, privacy, immutable artifacts, physical testing and clear stop/go gates. Detailed sections are justified only when each prevents a known class of failure.

## The four most valuable lines

Any detailed implementation prompt should make the following unambiguous:

1. **What real behavior proves success?** (not “all tests pass”)
2. **What must remain untouched?** (including production state and data)
3. **What superficially plausible solution would be wrong?** (e.g., HTTP success does not prove a usable export)
4. **What is the permitted stopping point?** (a working artifact, a demonstrated blocker, or a specifically requested report)

## Avoid the common anti-patterns

- Repeating the same requirement under six headings without adding meaning.
- Excessively exact commands or project paths that have not been verified.
- Claiming external access that the agent may not have.
- Choosing favorable samples to make acceptance metrics look good.
- Requiring a read-only audit when the user clearly authorized implementation and the safe solution is understood.
- Requiring a commit/push just because the code passes tests; honor actual permissions.
- Asking an agent to "think like another model" rather than giving explicit, checkable decisions.
- Designing a complete plugin system before completing one user workflow.

## Final report example (form, not mandatory length)

```text
Outcome: <end-user behavior achieved/blocked>
Artifacts: <paths and branch>
Evidence: <exact tests and environment>
Physical verification: <done/not done; why>
Known limitations: <specific>
Preservation: <what was compared>
Next action: <one smallest step>
```
