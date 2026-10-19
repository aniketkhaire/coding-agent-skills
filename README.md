# coding-agent-skills

[![skills.sh](https://skills.sh/b/aniketkhaire/coding-agent-skills)](https://skills.sh/aniketkhaire/coding-agent-skills)
[![GitHub stars](https://img.shields.io/github/stars/aniketkhaire/coding-agent-skills)](https://github.com/aniketkhaire/coding-agent-skills/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Agent Skills](https://img.shields.io/badge/Agent_Skills-open_standard-blue)](https://agentskills.io)

23 senior-level skills that turn a coding agent into a disciplined engineering team — covering the full lifecycle from ticket to production.

Coding agents are fast typists with no judgment. They'll write the code, skip the plan, ignore the edge cases, and declare victory. These skills fix that: each one encodes how a senior engineer actually works — the principles, the workflow, and a mandatory self-review checklist the agent must answer (Yes / No / N/A) before it's allowed to say "done".

## Install

**Claude Code**

```bash
claude plugin marketplace add aniketkhaire/coding-agent-skills
claude plugin install coding-agent-skills
```

**Any other agent** ([skills.sh](https://skills.sh))

```bash
npx skills add aniketkhaire/coding-agent-skills
```

**Cursor / VS Code** — this repo is a spec-conformant [Agent Plugins](https://agent-plugins.org) 1.0 plugin (`plugin.json` at the root), so it loads directly in Cursor, VS Code, and any client implementing the standard.

To update, re-run the install command.

## How it works

Install once. From then on, the agent reads each skill's description, loads the one that matches your task, and follows it:

1. **Principles** — the non-negotiables, e.g. "reproduce before you theorize", "profile before you optimize".
2. **Workflow** — the step-by-step procedure for the task.
3. **Mandatory checklist** — every item answered Yes / No / N/A. The agent answers the whole list before declaring the work done.

The 23 skills cover the whole lifecycle, not a bag of tricks:

- **Plan** — System Architect, Spec-Driven Developer, Codebase Explorer
- **Build** — Autonomous Senior Developer, TDD Driver, Frontend Implementation Engineer, Multi-Agent Orchestrator, Context Optimizer, API Contract Designer, Database Migration Strategist
- **Review** — Code Reviewer, QA Test Engineer, Debugging Detective, Refactoring Craftsman, Security Sentinel, Agent Eval Engineer, Agent Access Guardian
- **Ship** — Git Workflow Guardian, Delivery & Release Engineer
- **Operate** — Incident Responder, Observability Engineer, Performance Tuner, Technical Documenter

## Examples

```text
Review this branch against main and give me a verdict.
```

```text
Checkout is crashing in production but the tests are green. Find the root cause.
```

```text
Plan this ticket before writing any code.
```

```text
Split the users table with zero downtime. Show me the migration plan first.
```

The agent picks the right skill on its own — Code Reviewer, Debugging Detective, System Architect, Database Migration Strategist — and works it end to end.

## The skills

### Build
- [Autonomous Senior Developer](./skills/autonomous-senior-developer/) — Senior-level coding standards for production code: surgical diffs, resilience, security, CI/CD survival.
- [System Architect](./skills/system-architect/) — Pre-coding planning: analyze the ticket, map affected systems, write a step-by-step implementation plan.
- [Codebase Explorer](./skills/codebase-explorer/) — Research a repo and return a concentrated context payload: exact files, functions, and schemas.
- [Spec-Driven Developer](./skills/spec-driven-developer/) — Written specs before code: goal, scope, explicit non-goals, testable acceptance criteria, human approval.
- [Git Workflow Guardian](./skills/git-workflow-guardian/) — Branching, conventional commits, pre-commit gates, PR standards; never rewrite shared history.
- [Frontend Implementation Engineer](./skills/frontend-implementation-engineer/) — UI implementation craft: component contracts, accessibility, responsive, state handling, performance budgets.
- [Context Optimizer](./skills/context-optimizer/) — Agent context management: token budgeting, output compression and sandboxing, rolling session memory, defragmentation.
- [Multi-Agent Orchestrator](./skills/multi-agent-orchestrator/) — Coordinating agent teams: role decomposition, handoff contracts, sequencing, drift monitoring, reconciliation.

### Review & Quality
- [Code Reviewer](./skills/code-reviewer/) — Review branches against main: diffs, quality checks, structured findings with Approve / Request Changes / Block.
- [QA Test Engineer](./skills/qa-test-engineer/) — Adversarial testing: run suites, analyze coverage, kick failures back with stack traces.
- [TDD Driver](./skills/tdd-driver/) — Red-green-refactor discipline: failing test first, minimal green, refactor under green, test lists.
- [Debugging Detective](./skills/debugging-detective/) — Root-cause debugging: reproduce, gather evidence, bisect, minimal fix, regression test.
- [Refactoring Craftsman](./skills/refactoring-craftsman/) — Behavior-preserving restructuring: characterization tests first, tiny verifiable steps.
- [Agent Eval Engineer](./skills/agent-eval-engineer/) — Evals for coding agents: real-work task suites, sandboxed reproducibility, calibrated judges, anti-gaming checks, regression gates.

### Design & Security
- [API Contract Designer](./skills/api-contract-designer/) — Endpoint design, machine-readable error model, versioning strategy, OpenAPI spec.
- [Security Sentinel](./skills/security-sentinel/) — Threat modeling and security review: auth, authorization, input handling, secrets, crypto.
- [Agent Access Guardian](./skills/agent-access-guardian/) — Least-privilege agent identity: scoped credentials, approval gates for destructive actions, full audit trails.

### Data & Operations
- [Database Migration Strategist](./skills/database-migration-strategist/) — Zero-downtime migrations: expand/migrate/contract, batched idempotent backfills, rollback plans.
- [Delivery & Release Engineer](./skills/delivery-release-engineer/) — CI pipeline design, semantic versioning, changelogs, rollout and rollback plans.
- [Observability Engineer](./skills/observability-engineer/) — Instrumentation judgment: structured logging, RED/USE metrics, tracing, SLOs, symptom-based alerts.
- [Incident Responder](./skills/incident-responder/) — Mitigate first, communicate continuously, blameless RCA and postmortem with owned action items.
- [Performance Tuner](./skills/performance-tuner/) — Profile before optimizing, fix the true bottleneck, prove gains with before/after numbers.

### Docs
- [Technical Documenter](./skills/technical-documenter/) — Post-merge maintenance: update READMEs, regenerate API specs, refresh agent context files.

## Design principles

- **Checklists, not vibes.** Every skill ends in a mandatory Yes / No / N/A checklist. "Done" is a verdict, not a feeling.
- **Senior judgment, encoded.** Every skill ships only after an adversarial review pass whose only job is to break it.
- **Composable, not prescriptive.** Skills trigger on task match and combine freely. No framework to buy into, no process that owns you.
- **Agent-agnostic.** Written in the open [Agent Skills](https://agentskills.io) format and packaged as [Agent Plugins](https://agent-plugins.org) 1.0. Works with Codex, Claude Code, Cursor, VS Code, and 40+ other agents.

## Adding a skill

1. Create `skills/<kebab-case-name>/` — directory name must be lowercase
   letters, numbers, and hyphens only (max 64 chars, no leading/trailing
   or doubled hyphens), per the [Agent Skills
   specification](https://agentskills.io/specification).
2. Add a `SKILL.md` with `name` and `description` frontmatter. `name`
   MUST match the directory name; `description` is max 1024 characters
   and must say what the skill does and when (not) to use it. Optional
   spec fields: `license`, `compatibility`, `metadata`.
   Then core principles, an execution workflow, a mandatory checklist
   (every item answered Yes/No/N/A), and deliverables.
3. Add it to the list above.
4. When shipping changes, bump `version` in `plugin.json`,
   `.claude-plugin/plugin.json`, and `.claude-plugin/marketplace.json`.

## License

MIT — see [LICENSE](./LICENSE).
