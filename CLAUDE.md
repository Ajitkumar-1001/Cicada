# CICADA

Memory-first, evidence-grounded, multi-agent SRE incident commander for the CockroachDB × AWS
Agentic Memory Hackathon.

## Source of truth

Planning docs live in an Obsidian vault at `docs/`. Note that `docs/`, `tasks/`, `.specify/`,
`.claude/`, and `.agents/` are all gitignored, so these files are local-only and not in git
history.

1. `docs/12-Prompts/Implementation/PROMPT-001 - Cicada Final Feature Prompt.md` — canonical
   implementation contract. Wins on conflict.
2. `docs/02-Product/PRDs/Active/PRD-001 - Cicada MVP Source of Truth.md` — product contract and
   requirements.

Everything else in the vault is currently a stub. Read the two files above before proposing
architecture, schema, or scope changes. Changing persistent memory ownership, the agent graph,
human-approval requirements, the action-execution security model, AWS service boundaries, MVP
scope, or acceptance criteria requires a PRD version bump plus an ADR (PRD-001 §1).

## Skill routing

When the user's request matches an available skill, invoke it via the Skill tool. When in doubt,
invoke the skill.

Key routing rules:
- Product ideas/brainstorming → invoke /office-hours
- Strategy/scope → invoke /plan-ceo-review
- Architecture → invoke /plan-eng-review
- Design system/plan review → invoke /design-consultation or /plan-design-review
- Full review pipeline → invoke /autoplan
- Bugs/errors → invoke /investigate
- QA/testing site behavior → invoke /qa or /qa-only
- Code review/diff check → invoke /review
- Visual polish → invoke /design-review
- Ship/deploy/PR → invoke /ship or /land-and-deploy
- Save progress → invoke /context-save
- Resume context → invoke /context-restore
- Author a backlog-ready spec/issue → invoke /spec
