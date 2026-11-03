# Critical Briefsmith

[Find on Skills.sh](https://skills.sh/drinduka383/critical-briefsmith/critical-briefsmith)

An agent-agnostic skill for evidence-based decisions, actionable implementation briefs, bounded execution, and critical reviews. Uses the open [Agent Skills format](https://agentskills.io/).

## Install

With the [Skills CLI](https://github.com/vercel-labs/skills):

```sh
npx skills add drinduka383/critical-briefsmith
```

Choose your agent and installation scope when prompted. Add `-g` for a user-wide installation.

Or download this repository and put the whole folder, named `critical-briefsmith`, in your agent's supported skills directory. Keep `SKILL.md` and `references/` together. No runtime dependencies or vendor-specific configuration are required.

## Use

Ask your agent:

```text
Use critical-briefsmith in EXECUTE mode: inspect the repository, implement the smallest complete fix, verify the outcome, and stop.
```

- **DISCUSS**: compare options, challenge assumptions, and recommend a direction.
- **BRIEF**: produce one self-contained implementation assignment.
- **EXECUTE**: implement the authorized task and verify the outcome.
- **REVIEW**: check claims against original evidence and identify the smallest next action.

See [SKILL.md](SKILL.md) for the workflow and [behavioral checks](references/evaluation-checks.md) for fictional evaluation scenarios. Skill discovery depends on the agent. Delegation is optional; the skill requires no particular tool API and grants no additional permissions.
