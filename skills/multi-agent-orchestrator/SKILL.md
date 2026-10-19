---
name: multi-agent-orchestrator
description: >-
  Coordinate a team of specialized agents as one unit of work. Decompose a
  large goal into role-bounded sub-tasks, define handoff contracts and shared
  context, launch agents in the right dependency order, watch for drift and
  conflict, and reconcile their outputs into a single coherent result. Use
  when a task is too large or cross-cutting for one agent, when roles like
  frontend, backend, data, and tests must work in parallel, or when asked to
  orchestrate multiple agents.
---

# Multi-Agent Orchestrator Guide

## Core Orchestration Principles
1. **Decompose by Ownership, Not by File:** Give each agent one coherent slice of responsibility — a feature area, a layer, a lifecycle stage — that it fully owns. Scattering one concern across many agents guarantees merge conflicts and duplicated work.
2. **Contracts Before Code:** Write down each agent's inputs, outputs, and handoff format before anyone starts. Ambiguity at an agent boundary is where multi-agent work silently rots.
3. **Shared Context, Private Working Memory:** Every agent reads from one shared source of truth (goal, constraints, architecture decisions, glossary). Scratch work, drafts, and intermediate reasoning stay local to each agent so the shared context never fills with noise.
4. **Orchestrate the Critical Path:** Launch independent lanes in parallel, sequence dependent ones, and keep the bottleneck lane staffed. An idle agent is cheap; a blocked agent is a stalled project.
5. **Trust, Then Reconcile:** Let agents finish their lane without micromanagement, but never merge blindly — verify every handoff against its contract and check for drift before integrating.
6. **One Owner Per Decision:** When two agents disagree on an interface or approach, a named decider resolves it. Consensus-by-committee across agents is a deadlock engine.

---

## Execution Workflow

### 1. Map the Goal and Its Seams
* State the end goal and measurable success criteria in one paragraph.
* Identify the natural decomposition seams: service boundaries, layers (frontend/backend/data), or lifecycle stages (build/test/deploy).
* Call out the seams explicitly — agents will work across them, so they must be named.

### 2. Define Roles and Ownership
* For each agent: a role name, the scope it owns, explicit non-goals (what it must NOT touch), and the deliverable it must produce.
* Keep roles at 3–7. Fewer than 3 means one agent could have done it; more than 7 means the topology is too fine-grained to coordinate.
* Assign each role a launch order and list its dependencies on other roles' outputs.

### 3. Write the Handoff Contracts
* For every dependent pair of agents, document: data format, naming conventions, location of the handoff artifact, and acceptance criteria.
* Example: the API agent delivers an OpenAPI spec at `specs/api.yaml`; the frontend agent may not start integration until the spec passes the agreed validator.
* Contracts are versioned. When one changes, the downstream consumer re-verifies before continuing.

### 4. Establish the Shared Source of Truth
* Write a one-page mission brief: goal, success criteria, constraints (budget, deadlines, tech stack), architecture decisions already made, and a shared glossary.
* Every agent reads the brief first. If it contradicts an agent's assumptions, the brief wins until explicitly amended.

### 5. Launch and Sequence
* Start independent lanes in parallel. Hold dependent launches until the upstream contract they need is ready — launching early just burns tokens on rework.
* Stagger heavyweight launches (test runs, builds) to avoid resource contention if they share CI or rate limits.

### 6. Monitor for Drift and Conflict
* Review interim outputs on a cadence (per milestone, not per message). Watch for: scope creep past non-goals, two agents solving the same problem, contradictory assumptions about a shared interface.
* Intervene early: clarify the contract, re-scope a role, or route the dispute to the named decider. Drift caught at handoff time is cheap; drift caught at integration time is a rewrite.

### 7. Reconcile and Integrate
* Collect each deliverable and verify it against its handoff contract before accepting it.
* Run integration checks end to end: build, test suite, and a smoke pass over the combined result.
* Resolve conflicts through the named decider, record the decision, and propagate the consequences to affected lanes.

### 8. Retro and Codify
* Record what the topology got right and wrong: which seams held, which contracts were too loose, where the critical path actually was.
* Update the mission-brief template and contract checklist so the next orchestration starts cheaper.

---

## Mandatory Orchestrator Checklist

*Before declaring the multi-agent effort complete, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Bounded Roles:** Does each agent own exactly one coherent slice, with non-goals stated in writing?
2. **Written Contracts:** Are inputs, outputs, handoff formats, and acceptance criteria documented for every dependent pair before work began?
3. **Shared Source of Truth:** Is there one mission brief that every agent worked from, with amendments logged?
4. **Dependency Order:** Were dependent agents sequenced so none started work on an unwritten or unready contract?
5. **Drift Monitored:** Were interim outputs checked for scope creep, duplicated effort, or contradictory assumptions?
6. **Handoffs Verified:** Did every deliverable pass its acceptance criteria before integration?
7. **Single Coherent Result:** Does the integrated output behave as one system — built, tested, smoke-verified — not a stitched pile of parts?
8. **Decisions Logged:** Were cross-agent conflicts and their resolutions recorded in the retro notes?

---

## Deliverables

Provide a concise report:
* **🗺️ Team Topology:** Roles, ownership map, non-goals, and launch order with dependencies.
* **📜 Mission Brief:** Goal, success criteria, constraints, architecture decisions, and shared glossary.
* **🤝 Handoff Contracts:** Per-pair input/output formats, artifact locations, and acceptance criteria.
* **📊 Interim Status:** Lane progress at each milestone, plus drift flags raised and how they were resolved.
* **🧩 Integrated Result:** The merged output with integration verification results (build, tests, smoke pass).
* **📝 Retro Notes:** What to change about the topology, contracts, or cadence next time.
