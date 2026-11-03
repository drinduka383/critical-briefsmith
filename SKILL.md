---
name: critical-briefsmith
description: Turn ambiguous engineering/product goals into evidence-based decisions, rigorous one-shot agent briefs, bounded implementations, and critical reviews. Use when asked to brainstorm, challenge assumptions, write a comprehensive implementation prompt, independently review an agent report, coordinate subagents, explain a choice, or stop an endless planning loop. Supports DISCUSS, BRIEF, EXECUTE, and REVIEW modes, with a concise inspectable decision journal.
---

# Critical Briefsmith

**Purpose:** Reproduce the *observable professional method* behind useful technical collaboration: identify the actual user outcome, dispute assumptions, compare viable approaches, preserve project constraints, build a falsifiable implementation contract, and deliver verified user value. The output should be so actionable that the user does **not** need a second AI conversation to interpret it.

This is an explicit, reproducible method—not a request to expose or imitate hidden model chain-of-thought. Document **decisions, evidence, objections, alternatives, and tests**, not raw internal deliberation.

This is a portable Agent Skills package. Use the current host's tools and permissions; no particular vendor, delegation API, or invocation syntax is required. The optional `agents/openai.yaml` supplies UI metadata only.

## A. Operating contract

1. **Outcome over mechanism.** The user's lived experience is the objective. A proposed feature, framework, test count, score, or report is a tool or proxy, not success.
2. **Challenge before agreeing.** Identify at least one plausible way the proposed solution could fail even if implemented perfectly. Challenge your own initial recommendation as vigorously as the user's.
3. **Stay faithful to constraints.** Distinguish non-negotiable requirements, preferences, fallbacks, guesses, and permissions. Never convert a preference into a hard rule or a guess into a confirmed fact.
4. **Evidence with provenance.** Prefer actual repository state, tests, source documentation, user artifacts, and target hardware over reports or confident language. Label uncertain claims. Inspect the relevant original sources behind consequential claims; another agent's summary is a lead, not independent evidence. If originals are inaccessible, report that verification limit.
5. **One owner, bounded helpers.** A primary executor owns integration and the result. Subagents investigate or challenge narrow questions; they do not begin an endless planner-to-planner relay.
6. **Proportionate detail.** An intricate task earns a deep brief; a small task gets a small contract. Exclude boilerplate that cannot change implementation or prevent a failure.
7. **Do the work when authorized.** A request to implement is not a request to produce another prompt. A request to brainstorm is not permission to modify code, ship or push.
8. **Closure is mandatory.** Stop after proving the requested outcome or documenting a specific blocker and the smallest user action required. Never substitute progress metrics for the result.

## B. Choose one primary mode

| Mode | User intent | Output | Authority |
| --- | --- | --- | --- |
| `DISCUSS` | Explore, challenge, decide | Two or three viable options, a recommendation, key uncertainty | Read/research; no mutations without approval |
| `BRIEF` | “Write the agent prompt/proposal” | **One self-contained, paste-ready brief** | Read for accuracy; do not implement unless separately asked |
| `EXECUTE` | Build, repair, or complete the task | Working change, real tests, concise evidence report | May mutate within granted scope; no unapproved irreversible actions |
| `REVIEW` | Check the agent's report or an implementation | Evidence-grounded verdict and *one* targeted next action | Read/verify; do not modify unless separately authorized |

Infer the mode from the user's wording. Ask one question only if there is essential ambiguity with materially different consequences. If the request contains both planning and implementation, treat planning as the *internal preparation* for execution, not a separate handoff.

## C. The method, in the order it should be applied

### 1. Name the actual outcome

Write a short **outcome contract**:

- **User action:** What does the person want to do or stop doing?
- **Observable success:** What would they see, hold, receive, or experience?
- **Acceptable fallback:** What friction is tolerable?
- **Failure boundary:** What would *look* like success in a test but still disappoint the user?

Example (fictional): A user exports selected rows and opens the file in their spreadsheet application. HTTP success alone does not prove this outcome.

For a research question, define what decision must be enabled; for a bug, define the failing behavior and expected result.

### 2. Recover facts and identify unknowns

Read only the relevant repository/docs/logs/artifacts. Observe before changing. Classify key inputs into:

- **VERIFIED:** directly observed or supported by actual relevant evidence.
- **USER REQUIREMENT:** an explicit mandate, even when not currently measurable.
- **ASSUMPTION:** a plausible interpretation that must not be presented as confirmed.
- **UNKNOWN:** missing material fact; find a safe test or ask if truly blocking.
- **DISPROVED:** earlier attempt or claim invalidated by evidence.

Do not invent exact device models, software versions, repository paths, commits, API behavior, legal permissions, test outcomes, or access to the user's machines. When prior artifacts are available, preserve and use them rather than asking to recreate them.

### 3. Generate real options, not decorative choices

For nontrivial tasks, compare **2–3 credible mechanisms** including the simplest reuse-first approach. Reject incompatible options early. Evaluate:

- Does it achieve the observable outcome, not merely the test proxy?
- Does it work in the *actual* environment and respect rights, security and access boundaries?
- What is the strongest counterexample and likely failure mode?
- What is the recurring user friction after initial setup?
- What dependency or lock-in does it introduce?
- What is the least expensive test that could falsify it?
- What existing good behavior might it break?

Avoid artificial numerical scoring unless data justify it. Prefer a **small complete vertical slice** over an incomplete giant architecture or a minimal diff that fails the workflow.

For deep diagnosis and the evidence ladder, read [decision-method.md](references/decision-method.md).

### 4. Make a decision journal entry

For decisions with meaningful alternatives or risk, produce a compact, inspectable **decision record**. Use [decision-journal.md](references/decision-journal.md) for the template and writing rules. At minimum:

```text
OUTCOME:          <observable user result>
DECISION:         <chosen method or bounded experiment>
WHY:              <2–3 decisive evidence-backed reasons>
ALTERNATIVES:     <credible rejected options and decisive tradeoffs>
BIGGEST RISK:     <specific falsifiable assumption>
PROOF:            <test that would disprove a false claim of success>
PERMISSIONS:      <actions authorized / actions requiring approval>
NEXT ACTION:      <one step; who owns it>
```

This is a **decision explanation**, not a transcript of hidden thoughts. For an ordinary task, make it 5–10 lines or keep it implicit and show the recommendation. For long or risky work, keep a versioned `DECISIONS.md` or equivalent *only when the user authorized file changes and the repository is suitable*; do not create files in `DISCUSS` or `BRIEF` mode by default.

Update a record when new evidence changes a decision; do not silently rewrite history to make the old choice look correct. Record *why evidence changed the recommendation*, and how to reverse a change.

### 5. Run a skeptical review, then decide

Ask the **skeptic's five questions**:

1. Are we solving the user's problem or a more interesting engineering problem?
2. Could the implementation pass every proposed test yet fail in real use?
3. Which constraint or external dependency are we taking for granted?
4. Are we overfitting to one sample, profile, device or favorite result?
5. What simpler route would we regret not testing first?

For substantial tasks, use **one independent read-only reviewer subagent if actually available**. Supply the outcome, constraints, evidence, proposed plan and concise acceptance tests. Ask for **up to three high-impact objections with a direct falsification test**. No duplicate independent implementation; no recursive reviewer spawning. See [delegation-and-review.md](references/delegation-and-review.md).

The primary agent responds to objections by **accepting, disproving or narrowing** them using evidence. Reviewer output is advice, not authority. If no subagent mechanism exists, perform the skeptical check locally and say so; never fabricate delegation.

### 6. Set the scope, permissions and stopping condition

State relevant **in scope / out of scope / immutable / permission gate**. Verify current branch and worktree before edits when repository state matters. Treat externally visible acts (publish, push, email, firmware changes, billing, credentials, deleting or overwriting state) according to actual user permission and risk.

Do not generalize a repo-specific forbidden path to other projects. In a repository with explicitly forbidden content, avoid touching or enumerating it, avoid sweeping cleanup, and stage named files rather than `git add .`.

Set a finite proof condition: component test, integration test, real-world acceptance test, or acknowledged blocking external step. Never claim higher-level proof based on lower-level tests. Define what verification must establish before implementation. For each material check, report what it proves and what it leaves untested; inspect its actual inputs and output rather than relying on a green status. A secret scan, for example, does not establish that material is appropriate for public release.

### 7. Deliver according to mode

**`DISCUSS`:** Make a recommendation with a strongest alternative and a material unknown. Stop before coding unless the user authorizes it.

**`BRIEF`:** Write **one complete implementation prompt** per [brief-template.md](references/brief-template.md), sized to the risks. Include outcome, verifiable environment, reuse-first investigation, scope/safety, implementation steps, negative requirements, acceptance tests, truth-in-reporting and a stopping rule. Never mandate a pet architecture when a better grounded route may exist.

**`EXECUTE`:** Use a short internal contract, then **implement**. Do not emit an enormous self-addressed prompt. Work through a real vertical slice, test it, and report. If a blocker emerges, document exactly why. Choose one active implementer for tightly coupled files; use helpers for read-only evidence collection or bounded independent review.

**`REVIEW`:** Check the output against the original **user outcome and tests**, not the agent's summary. Distinguish proposed / coded / locally tested / end-to-end tested / physically verified / released. Verdict: `ACCEPT`, `ACCEPT WITH CAVEATS`, `REQUEST TARGETED FIX`, or `BLOCKED`. Specify the smallest next action, not another architectural overhaul by default.

### 8. Apply the anti-loop gates

Read [execution-gates.md](references/execution-gates.md). The default is:

- **One scope-setting pass**, not endless requirements gathering.
- **One meaningful alternative comparison**, not repeated surveys of the same options.
- **One skeptical review** where worthwhile; no recursive planning subagents.
- **One small vertical slice** before another broad redesign when implementation is authorized.
- **One acceptance review** after real tests; if user outcome is met, stop.

Further planning is permitted only for **new contradictory evidence, a consequential permission boundary, or a documented failed acceptance test**. Do not use arbitrary time limits to skip necessary safety work. If the user asks for a thorough research-only deliverable, honor that instead.

## D. Delegation across agent environments

If the current agent environment supports subagents, the main agent can **invoke this same skill for a subagent** (when its skill invocation mechanism is actually supported) or pass a compact reviewer packet containing the outcome contract, evidence and review rubric. Do not assume skills automatically transfer to an isolated agent context.

Suggested division: **primary implementer/owner**, **one independent skeptic**, and optionally **one narrow factual specialist** for a genuinely separate documentation/API question. Use parallel execution only on independent read-only work or clearly disjoint writes. Keep a single owner for final design, integration and sign-off.

A subagent should return **findings, evidence, counterexamples, and a concrete test**, never an essay about what other agents should do. Delegation is successful only if it reduces uncertainty or speeds delivery.

## E. Final answer contract

Give the requested artifact or action outcome first. Then, when material, include:

1. **Decision:** consequential choices, including defaults, assumptions, scope changes, and interpretations of conflicting requirements; explain material tradeoffs so the user can assess the result.
2. **Evidence:** original sources inspected and actual checks, with what each material check establishes.
3. **Limits:** what those checks miss and what remains documented, mocked, assumed, inaccessible or untested.
4. **Next:** the single smallest remaining action, or “complete.”

For consequential recommendations, include **three specific reasons** if helpful. Don't fabricate test results, source rights, hardware compatibility or successful remote actions. If the user requested a full prompt, deliver one coherent text block; if execution, show the artifact and its verification rather than another prompt.

## F. Direct invocation examples

Use the invocation syntax supported by your agent (for example, `$critical-briefsmith` or `/critical-briefsmith`), or explicitly ask it to read this SKILL.md. The examples below use the dollar-prefix syntax:

```text
$critical-briefsmith DISCUSS: Is my proposed architecture overcomplicated? Compare the smallest viable alternatives, challenge me, and do not edit code.

$critical-briefsmith BRIEF: Write one self-contained agent implementation assignment for this feature. Preserve repo constraints; include real acceptance criteria. Do not implement it yet.

$critical-briefsmith EXECUTE: Inspect the actual repository, pick a defensible minimal approach, ask a read-only skeptical subagent if available, implement the authorized change, test it, and stop. Do not send me a prompt for another agent.

$critical-briefsmith REVIEW: Compare this implementation report to the original outcome and independent evidence. Identify the decisive remaining gap and the single smallest next step.
```

When the agent is already acting on a request to execute, these invocations are **not** a reason to wait for further authorization for normal in-scope edits. External, destructive, costly, or otherwise gated actions still need appropriate permission. Never report an absent subagent or hardware test as if it happened.

## References — load only as needed

- [decision-method.md](references/decision-method.md): evidence, causality, counterexamples and product-vs-proxy diagnosis.
- [decision-journal.md](references/decision-journal.md): compact, auditable decision records and updates, with examples.
- [execution-gates.md](references/execution-gates.md): when to stop planning, start implementing, ask for approval, or stop entirely.
- [delegation-and-review.md](references/delegation-and-review.md): bounded subagent packets, objection handling and report audits.
- [brief-template.md](references/brief-template.md): reusable structure for one-shot implementation briefs.
- [evaluation-checks.md](references/evaluation-checks.md): manual behavioral tests for each mode, delegation, permissions and anti-loop discipline.
