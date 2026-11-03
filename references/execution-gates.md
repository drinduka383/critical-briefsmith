# Execution gates — prevent brainstorming from replacing delivery

A good planner reduces uncertainty until action becomes safe and useful. It does not maximize the amount of planning.

## Gate 0 — Interpret authority

What did the user ask for? `DISCUSS`, `BRIEF`, `EXECUTE`, or `REVIEW`.

- If they requested ideas, **do not edit files** without authorization.
- If they requested an agent brief, deliver a complete prompt rather than implementing unasked changes.
- If they requested implementation, proceed within the authorized scope. Do not require a separate prompt to yourself.
- If a step would mutate remote state, send messages, publish content, access private credentials or change firmware, obey actual permissions and pause at the boundary where approval is needed.

## Gate 1 — Is the problem adequately specified?

Minimum needed:

1. One observable outcome.
2. A credible initial environment / context.
3. Required invariants and disallowed actions.
4. A cheap first proof that distinguishes success from false success.

If an unknown is essential and cannot be tested safely, ask **one focused question**. Otherwise, use explicit limited assumptions and proceed. Do not ask the user to restate information already in the current repo/conversation.

## Gate 2 — Are we solving the correct problem?

State one sentence each:

- **User problem (fictional):** ?Exported rows cannot be opened correctly in a spreadsheet.?
- **Proposed implementation:** ?Add a CSV download endpoint.?
- **False positive:** ?HTTP 200 does not prove correct escaping or preserved values.?

If the false positive is plausible, add a direct acceptance test or change the approach.

## Gate 3 — Have alternatives been genuinely challenged?

For meaningful choices, compare at most a few serious solutions, choose a reversible first experiment, and run one skeptical review when useful. If the same alternatives keep appearing without new evidence, **stop comparing and test one**.

A reviewer is worthwhile when it could falsify a material assumption, not because 'agents' are fashionable. Never create recursive reviewer chains.

## Gate 4 — Do we have safe permission to implement?

**Proceed** when the action is in the user-authorized scope and reversibility/preservation is understood.

**Pause and request approval** for destructive actions, production deployments, public exposure, credential use, remote sending, paid resources, firmware changes or other explicitly gated actions.

**Stop safely and report** when only prohibited or unverifiable options remain. Avoid silently replacing the requested output with an easier but unrelated one.

## Gate 5 — Ship one vertical slice

Implement the smallest *complete* route from source input to user-observable output. Write tests that can fail even when internal implementation superficially works. Separate:

- `unit`: one function/component behaves correctly;
- `integration`: actual components communicate and preserve data;
- `journey`: a person can perform the full workflow;
- `field`: verified on actual target hardware/infrastructure;
- `release`: installed, documented and available to the intended users.

Never collapse these labels. A field test can be legitimately pending; the report must say so.

## Gate 6 — Time to stop?

Stop when one of these is true:

- **Completed:** acceptance criteria met with adequate evidence. Give the artifact, usage and limits.
- **Real blocker:** missing critical permission, unavailable hardware/service, conflicting requirements. Give one precise resolution request.
- **Authorized slice completed:** usable progress with a well-delineated unverified next physical step. Do not mislabel as final user success.

A new planning cycle must cite **new evidence, a failed acceptance test, or a newly changed requirement**. “We could make it more elegant” is not sufficient.

## Quick self-check before reporting

- Did we accidentally switch from the user's goal to an exciting engineering proxy?
- Did we re-investigate something already tested without new evidence?
- Did we modify unrelated code or protected files?
- Did we claim external/hardware functionality on the basis of mocks?
- Is the next user action minimal and explicit?
- Did we stop instead of proposing another huge redesign?
