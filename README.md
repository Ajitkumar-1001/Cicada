# Cicada

Memory-first, evidence-grounded, multi-agent SRE incident commander, built for the
CockroachDB × AWS Agentic Memory Hackathon.

## What it does

Cicada treats CockroachDB as institutional operational memory instead of a passive
datastore. When an incident fires, it:

1. Pulls live diagnostics from CockroachDB via the Cloud MCP Server and Agent Skills.
2. Retrieves similar past incidents from CockroachDB vector + relational memory, ranked
   by outcome, evidence quality, and environment compatibility — not just text similarity.
3. Proposes a remediation and puts it through an adversarial tribunal (defender,
   prosecutor, evidence verifier, risk gate).
4. Requires a human to approve the exact action payload before anything executes.
5. Executes only allowlisted AWS remediation through a dedicated, least-privilege
   Action Executor — never through `ccloud`, which is scoped to CockroachDB Cloud
   control-plane operations only.
6. Evaluates the outcome and consolidates the incident back into CockroachDB as
   reusable memory.

The demo: a cold incident is diagnosed and resolved from scratch; a similar warm
incident then resolves faster because Cicada remembers what worked.

## Architecture

```
CloudWatch / Demo Simulator
        |
EventBridge -> Lambda Incident Ingestor
        |
FastAPI + LangGraph (ECS Fargate)
  +-- Bedrock (reasoning, embeddings)
  +-- CockroachDB Cloud MCP (live diagnostics)
  +-- CockroachDB Agent Skills (structured procedures)
  +-- CockroachDB SQL + Vector Indexes (all persistent memory)
  +-- AWS Action Executor (approved, allowlisted remediation)
  +-- ccloud adapter (CockroachDB Cloud control-plane only)
  +-- S3 (raw artifacts, referenced from CockroachDB)
```

Agent graph: `INGEST -> TRIAGE -> LIVE_DIAGNOSTICS -> MEMORY_RETRIEVAL ->
REMEDIATION_PLANNER -> DEFENDER -> PROSECUTOR -> EVIDENCE_VERIFIER -> RISK_GATE ->
HUMAN_APPROVAL -> ACTION_EXECUTOR -> OUTCOME_EVALUATOR -> MEMORY_CONSOLIDATOR -> CLOSED`

## Stack

- **Frontend:** Next.js, TypeScript, Tailwind CSS,
- **Backend:** Python, FastAPI, LangGraph
- **Memory / system of record:** CockroachDB Cloud (SQL + vector indexes)
- **AWS:** Bedrock, EventBridge, Lambda, ECS Fargate, S3, Secrets Manager, CloudWatch

## Status

Pre-implementation. The product and engineering contract (PRD-001) is written and
approved; no application code exists in this repository yet, so there are no install
or run instructions to give.

## Documentation

The full PRD, implementation contract, and architecture decisions live in a local
 vault (`docs/`), intentionally excluded from version control. See
`CLAUDE.md` for pointers into that vault.

## License

MIT — see [LICENSE.MD](LICENSE.MD).
