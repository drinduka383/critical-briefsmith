# Decision journal — concise, inspectable engineering rationale

## Purpose

Explain *what was decided and why* using externalized evidence and falsifiable criteria. The journal records professional decisions, **not hidden model reasoning, private scratchpads or exhaustive internal deliberation**. A reader should be able to challenge a decision without reading an AI's internal process.

## The size rule

- **Small reversible decision:** one paragraph; do not create a file just to satisfy a template.
- **Medium ambiguous design:** 8–15 structured lines or one table.
- **Long-lived architecture, security or public-facing changes:** one dated decision record, with evidence references and reversibility.

If keeping records inside a repository, create or update a decisions file only when edits are authorized; name it according to existing repo conventions rather than introducing an arbitrary new hierarchy. Don't write sensitive credentials, private source text or personal data into a public decision log.

## Reusable record template

```markdown
### ADR-<id> — <specific decision> (<date>)
Status: PROPOSED | ACCEPTED | SUPERSEDED | BLOCKED
Mode: DISCUSS | BRIEF | EXECUTE | REVIEW

Outcome: <one user-observable result>
Context: <small number of verified relevant facts>
Constraints: <hard requirements vs preferences/fallbacks>
Decision: <precise chosen option or smallest experiment>
Alternatives: <2–3 alternatives; key reason each lost>
Why now: <two or three decisive reasons supported by evidence>
Strongest objection: <a real reason this might be wrong>
Evidence: <file/line, test, vendor doc, or explicitly user-reported observation>
Unknowns: <uncertainty with next direct verification>
Falsification: <observable outcome that would disprove the intended benefit>
Permissions: <actions authorized; actions still gated>
Rollback: <how to reverse safely if relevant>
Next action: <specific owner + next observable milestone>
```

**Evidence labels**: `observed`, `reproduced`, `documented`, `reported`, `hypothesized`. A result must not be labeled observed if it comes only from someone else's summary. Source URLs, local files and private references are different kinds of evidence; include only references the user or reviewer can access.

## When evidence changes the decision

Append a small amendment instead of silently rewriting the original:

```markdown
Amendment (<date>): <new source or test>
Previously believed: <claim>
Observed now: <counterevidence>
Consequence: <keep / revise / reverse the decision>
Regression proof: <test to ensure prior value remains intact>
```

Use amendments for substantive changes only; routine progress belongs in normal implementation notes.

## Anti-patterns

- Post-hoc explanations with no traceable evidence.
- Fake precision (e.g., “option scored 93.6%” without measurement).
- Writing a long journal before a trivial code change.
- Turning the journal into an excuse not to implement.
- Assuming that a positive model review is independent verification.
- Exposing private or third-party restricted content to a public repository.
- Calling a test of the server proof of a physical device experience.
