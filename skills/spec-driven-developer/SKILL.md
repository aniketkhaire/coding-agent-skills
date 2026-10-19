---
name: spec-driven-developer
description: >-
  Turn vague requests into written specs before any code exists: goal,
  scope, explicit non-goals, and testable acceptance criteria — reviewed
  with the human, then handed to implementation. This skill produces the
  spec; it does not write the code. Use when a task is ambiguous,
  multi-step, or will be implemented by an autonomous agent.
---


# Spec-Driven Developer Guide

## Core Spec Principles
1. **No Code Before the Spec:** Writing code clarifies thinking, which is exactly why it's dangerous here — it clarifies *your* thinking while the requirement stays fuzzy. The spec comes first, always.
2. **Non-Goals Are as Important as Goals:** Every spec lists what is explicitly out of scope. Most build failures are scope arguments that happened too late; non-goals have them early, on paper.
3. **Acceptance Criteria Must Be Testable:** "Fast", "clean", "user-friendly" are not criteria. Each criterion must be verifiable by a test, a measurement, or a yes/no observation. If you can't test it, rewrite it.
4. **Spec to the Level of the Risk:** A one-line change gets a paragraph. A new subsystem gets pages. Match the spec's weight to the cost of getting it wrong, not to a template.
5. **The Spec Is a Contract With Your Future Self:** The implementer — human or agent — should be able to build from this document without asking you questions. Every question it can't answer is a defect in the spec.

---

## Execution Workflow

### 1. Clarify the Goal
* State the goal in one or two sentences: what outcome does this achieve, and for whom?
* Identify the requester and the real user. If those differ, say so — it changes the acceptance criteria.
* If the goal itself is unclear, ask now. Do not spec around a fuzzy goal.

### 2. Map the Current State
* Describe what exists today that this touches: systems, data flows, APIs, UI surfaces.
* List constraints: tech stack, deadlines, compatibility requirements, regulatory or security boundaries.
* Note unknowns explicitly as open questions with owners — "TBD" without an owner is a deferred failure.

### 3. Define Scope and Non-Goals
* **In scope:** the concrete changes this spec covers, as a list.
* **Out of scope (non-goals):** what this spec deliberately does not cover — adjacent features, migrations, refactors, platforms.
* For each non-goal that might surprise someone, add one line explaining why it's excluded.

### 4. Write Testable Acceptance Criteria
* Each criterion is a verifiable statement: given some input/state, the system does some observable thing.
* Include happy paths, edge cases, and error cases. Include performance or scale criteria only if they were part of the goal — with numbers.
* Number the criteria (AC1, AC2, …). Implementation and testing will reference these numbers.

### 5. Sketch the Approach (Lightly)
* Outline the proposed approach at the architecture level: components touched, data flow changes, API changes.
* This is a sketch, not a design doc — enough to expose big risks and wrong turns, not enough to start coding from.
* Call out risks and alternatives considered, with the reason for the chosen path.

### 6. Review the Spec With the Human
* Present the spec for review before any implementation begins. Walk through goal, non-goals, and acceptance criteria explicitly.
* Resolve every open question. An unresolved question at review time becomes an implementation guess later.
* Get explicit approval. "Looks good" in chat is approval — record it in the spec.

### 7. Hand Off to Implementation
* The approved spec is the input to implementation — this is where it feeds Autonomous Senior Developer or a human engineer.
* During implementation, spec changes go through the same review loop. Silent spec drift is how "done" stops meaning done.

---

## Mandatory Spec Checklist

*Before handing off, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Goal Stated:** Can a stranger read the first paragraph and know what outcome this achieves and for whom?
2. **Non-Goals Listed:** Is there an explicit non-goals list, with a reason recorded for any exclusion that could surprise a reader?
3. **Testable Criteria:** Is every acceptance criterion verifiable by test, measurement, or direct observation — no adjectives-as-requirements?
4. **Open Questions Resolved:** Is every unknown either answered or assigned an owner — no orphan TBDs?
5. **Spec Before Code:** Was the spec reviewed and explicitly approved before any implementation code was written?
6. **Human Approved:** Has the requester explicitly approved the spec, including non-goals?
7. **Implementable Alone:** Could the implementer build this without asking the spec author questions?

---

## Deliverables

Provide the spec document:
* **🎯 Goal:** Outcome and audience in one to two sentences.
* **🗺️ Current State:** Systems touched, constraints, context.
* **✅ In Scope / 🚫 Non-Goals:** The explicit boundary.
* **📏 Acceptance Criteria:** Numbered, testable AC1…ACn.
* **🧭 Approach Sketch:** Architecture-level outline, risks, alternatives.
* **❓ Open Questions:** All resolved, with resolutions recorded.
* **✍️ Approval:** Who approved and when.
