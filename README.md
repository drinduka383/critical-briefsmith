# Critical Briefsmith

A portable Agent Skills package for turning ambiguous engineering and product goals into evidence-based decisions, actionable implementation briefs, bounded execution, and critical reviews.

## Modes

- **DISCUSS** — compare viable options, challenge assumptions, recommend a direction.
- **BRIEF** — produce one self-contained implementation assignment.
- **EXECUTE** — implement the authorized task, verify the outcome, and stop.
- **REVIEW** — check implementation claims against evidence and identify the smallest next action.

## Install

Clone this repository into your agent's skills directory, using `critical-briefsmith` as the folder name.

Shared skills directory (for agents that support it):

```sh
git clone https://github.com/drinduka383/critical-briefsmith.git ~/.agents/skills/critical-briefsmith
```

Claude Code personal skills:

```sh
git clone https://github.com/drinduka383/critical-briefsmith.git ~/.claude/skills/critical-briefsmith
```

If that destination already exists, update or back it up first. Do not clone over an existing installation.

Other agents can install the whole folder in their supported skills directory or explicitly read `SKILL.md`. Automatic discovery and invocation syntax depend on the host. The optional `agents/openai.yaml` provides OpenAI UI metadata; the core instructions and references are vendor-neutral. No additional dependencies are required.

## Use

For Codex, use `$critical-briefsmith`; for Claude Code, use `/critical-briefsmith`. For example:

```text
$critical-briefsmith EXECUTE: Inspect the repository, implement the smallest complete fix, test the outcome, and stop.
```

Replace the invocation prefix for your host. Start a new turn or session if needed to refresh its skill catalog.

See [SKILL.md](SKILL.md) for the workflow and [behavioral evaluation prompts](references/evaluation-checks.md) for manual checks. Delegation is optional and requires host support; the skill grants no additional permissions.
