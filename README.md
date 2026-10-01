# coding-agent-skills

A collection of 19 skills for AI coding agents. Each skill is a
directory containing a `SKILL.md` with core principles, an execution
workflow, and a mandatory self-review checklist — the agent answers
each item Yes / No / N/A before declaring the work done. Load the skill
that matches your task and let the agent follow it.

Written in the open Agent Skills format; works with Codex, Claude Code,
Cursor, and any agent that reads `SKILL.md`.

## Skills

### Build
- [Autonomous Senior Developer](./Autonomous%20Senior%20Developer/) — Senior-level coding standards for production code: surgical diffs, resilience, security, CI/CD survival.
- [System Architect](./System%20Architect/) — Pre-coding planning: analyze the ticket, map affected systems, write a step-by-step implementation plan.
- [Codebase Explorer](./Codebase%20Explorer/) — Research a repo and return a concentrated context payload: exact files, functions, and schemas.
- [Spec-Driven Developer](./Spec%20Driven%20Developer/) — Written specs before code: goal, scope, explicit non-goals, testable acceptance criteria, human approval.
- [Git Workflow Guardian](./Git%20Workflow%20Guardian/) — Branching, conventional commits, pre-commit gates, PR standards; never rewrite shared history.
- [Frontend Implementation Engineer](./Frontend%20Implementation%20Engineer/) — UI implementation craft: component contracts, accessibility, responsive, state handling, performance budgets.

### Review & Quality
- [Code Reviewer](./Code%20Reviewer/) — Review branches against main: diffs, quality checks, structured findings with Approve / Request Changes / Block.
- [QA Test Engineer](./QA%20Test%20Engineer/) — Adversarial testing: run suites, analyze coverage, kick failures back with stack traces.
- [TDD Driver](./TDD%20Driver/) — Red-green-refactor discipline: failing test first, minimal green, refactor under green, test lists.
- [Debugging Detective](./Debugging%20Detective/) — Root-cause debugging: reproduce, gather evidence, bisect, minimal fix, regression test.
- [Refactoring Craftsman](./Refactoring%20Craftsman/) — Behavior-preserving restructuring: characterization tests first, tiny verifiable steps.

### Design & Security
- [API Contract Designer](./API%20Contract%20Designer/) — Endpoint design, machine-readable error model, versioning strategy, OpenAPI spec.
- [Security Sentinel](./Security%20Sentinel/) — Threat modeling and security review: auth, authorization, input handling, secrets, crypto.

### Data & Operations
- [Database Migration Strategist](./Database%20Migration%20Strategist/) — Zero-downtime migrations: expand/migrate/contract, batched idempotent backfills, rollback plans.
- [Delivery & Release Engineer](./Delivery%20&%20Release%20Engineer/) — CI pipeline design, semantic versioning, changelogs, rollout and rollback plans.
- [Observability Engineer](./Observability%20Engineer/) — Instrumentation judgment: structured logging, RED/USE metrics, tracing, SLOs, symptom-based alerts.
- [Incident Responder](./Incident%20Responder/) — Mitigate first, communicate continuously, blameless RCA and postmortem with owned action items.
- [Performance Tuner](./Performance%20Tuner/) — Profile before optimizing, fix the true bottleneck, prove gains with before/after numbers.

### Docs
- [Technical Documenter](./Technical%20Documenter/) — Post-merge maintenance: update READMEs, regenerate API specs, refresh agent context files.

## Adding a skill

1. Create a directory named after the skill.
2. Add a `SKILL.md` with `name` and `description` frontmatter, followed by core principles, an execution workflow, a mandatory checklist (every item answered Yes/No/N/A), and deliverables.
3. Add it to the list above.
