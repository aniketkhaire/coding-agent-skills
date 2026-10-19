# coding-agent-skills

A collection of 23 skills for AI coding agents. Each skill is a
directory containing a `SKILL.md` with core principles, an execution
workflow, and a mandatory self-review checklist — the agent answers
each item Yes / No / N/A before declaring the work done. Load the skill
that matches your task and let the agent follow it.

Written in the open [Agent Skills](https://agentskills.io) format and
packaged as an [Agent Plugins](https://agent-plugins.org) 1.0 plugin;
works with Codex, Claude Code, Cursor, VS Code, and any agent that reads
`SKILL.md`.

## Install

**Claude Code** (plugin marketplace):

```bash
claude plugin marketplace add aniketkhaire/coding-agent-skills
claude plugin install coding-agent-skills
```

**Any agent** (via [skills.sh](https://skills.sh)):

```bash
npx skills add aniketkhaire/coding-agent-skills
```

**Cursor / VS Code / other Agent Plugins hosts:** this repo is a
spec-conformant Agent Plugin (`plugin.json` at the root, skills under
`skills/`), so it loads directly in Cursor, VS Code, and any client
implementing the Agent Plugins 1.0 standard. Public Cursor Marketplace
listings are submitted at
[cursor.com/marketplace/publish](https://cursor.com/marketplace/publish)
(manual review; plugins must be open source).

**Claude Directory:** plugin bundles can be submitted for review at
[claude.ai/directory/manage](https://claude.ai/directory/manage)
(requires a paid Claude plan; the repository must be public).

## Skills

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
