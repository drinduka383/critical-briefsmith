# Decision method: from an idea to a defensible engineering choice

Use when a task has meaningful ambiguity, costly mistakes, previous failed attempts, or a mismatch between tests and user experience. Explain decisions and evidence, not hidden model reasoning.

## Separate outcome, mechanism, and proxy

Use fictional examples rather than details from private work:

- **Outcome:** a user exports selected rows and opens a usable spreadsheet.
- **Mechanism:** a CSV download endpoint.
- **Proxy:** HTTP 200 and a nonempty response.

The proxy does not prove correct quoting, preserved values, or compatibility with the intended application. Test the user-visible result directly.

## Diagnose before changing

1. Reproduce the failure and preserve the baseline.
2. Trace the relevant input through transformations to the output.
3. Identify where observed behavior diverges from the requirement.
4. Change one variable at a time on fixed inputs when practical.
5. Retain failed results rather than quietly replacing the test sample.

Distinguish missing data, incorrect transformations, access failures, and presentation failures; similar symptoms can have different causes.

## Compare credible alternatives

Consider two or three viable options, including existing functionality. Compare feasibility, recurring user effort, maintenance, permissions, failure modes, fallback, and reversibility. Reject incompatible options early. Do not invent numerical scores to disguise judgment.

## Classify constraints

- **Hard:** preserve existing data; obey explicit prohibited actions.
- **Preferred:** fewer dependencies or shorter setup.
- **Fallback:** an explicit manual retry is acceptable.
- **Unknown:** target application version or external API behavior.

Do not turn preferences into hard requirements or assumptions into facts. Resolve consequential unknowns with a bounded test where possible.

## Match claims to evidence

Distinguish observed target behavior, real integration tests, synthetic tests, maintained documentation, community reports, and hypotheses. A unit test cannot prove a production workflow; a README cannot prove every supported environment works. Accessible content does not automatically carry redistribution rights.

## Challenge the proposal

- Are we fixing the user-visible failure or an unrelated architectural concern?
- Could the tests pass while the actual workflow fails?
- Would this work on different inputs and supported environments?
- Are we assuming permissions, compatibility, or rights without evidence?
- Does an existing feature or a simpler fallback meet the outcome?
- Is manual agent intervention hiding missing automation?
- Are sunk costs preventing a better choice?

## Decide and stop

Choose the smallest experiment that can disprove the riskiest assumption. For the fictional export example, test a file containing commas, quotes, line breaks, and non-ASCII characters in the target application before expanding the export system.

Proceed when the outcome is testable and the action is authorized. Ask when an essential fact or permission is missing. Disclose usable partial results accurately. Stop when acceptance criteria are met; reopen planning only for new evidence or a failed acceptance test.
