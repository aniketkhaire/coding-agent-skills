---
name: tdd-driver
description: >-
  Red-green-refactor discipline for coding agents. Write the failing test
  first, make it pass with the smallest possible change, then refactor under
  green — with a test list, triangulation, and explicit rules for when not
  to TDD. Use when implementing new behavior, fixing a bug test-first, or
  asked to follow test-driven development.
---


# TDD Driver Guide

## Core TDD Principles
1. **Test First, Always (for New Behavior):** No production code exists until a failing test demands it. The test is the specification; the code is the answer.
2. **Red Must Be Real:** A test that fails for the wrong reason (typo, missing import) proves nothing. Run it, read the failure, and confirm it fails because the behavior is missing.
3. **Minimal Green:** Write the simplest production code that makes the test pass — even if it looks naive. Cleverness comes later, under green.
4. **Refactor Only on Green:** Restructuring happens with a passing suite as a safety net, never while tests are red. Red means "implement"; green means "improve."
5. **One Behavior Per Test:** Each test asserts exactly one behavior with a name that reads as a specification. When it fails, the name tells you what broke.
6. **Know When Not to TDD:** Spikes, pure exploration, throwaway scripts, and genuinely unknown domains earn a spike-first pass — then delete the spike and TDD the real thing. TDD is the default, not a religion.

---

## Execution Workflow

### 1. Build the Test List
* Before writing anything, list the behaviors the change requires as a checklist: happy paths, edge cases, error cases.
* Order the list from simplest to hardest. Start with the case that forces the least code into existence.
* Keep the list visible and check items off — it is your progress tracker and your stop condition.

### 2. Write One Failing Test (Red)
* Pick the next unchecked behavior and write a single test for it. Name it as a specification: `rejects_expired_tokens`, not `test_3`.
* Keep the test small: one behavior, one assertion concept, no test logic that itself needs testing.
* Use the real interfaces where cheap; reach for fakes/mocks only at genuine boundaries (network, clock, filesystem).

### 3. Verify the Failure Is Real
* Run the test and confirm it fails. Read the failure message — it must fail because the behavior is absent, not because the test is broken.
* If the test fails for the wrong reason, fix the test first. A red you don't understand is not a red you can trust.

### 4. Write Minimal Production Code (Green)
* Implement the smallest change that makes the test pass. Hardcode if the test only demands one case — the next test will force generalization.
* Do not add behavior the current test doesn't require. YAGNI is enforced by the test list, not by willpower.
* Run the test. Then run the whole suite — green everywhere, not just the new test.

### 5. Triangulate Toward Generality
* When a second or third test demands a more general solution, let the tests pull the abstraction out. Two examples justify a generalization; one does not.
* Resist generalizing ahead of the tests. Premature abstraction is the most common way TDD dies.

### 6. Refactor Under Green
* With the suite passing, clean up: remove duplication (including test duplication), clarify names, simplify structure.
* After every refactoring step, re-run the suite. If it goes red, revert the step — the suite is the boss.
* Never mix refactoring with new behavior in the same step. Alternate: green (behavior), green (structure), repeat.

### 7. Repeat Until the List Is Empty
* Return to the test list, pick the next behavior, and loop. Stop when the list is done — not when you feel done.

---

## Mandatory Driver Checklist

*Before declaring the work complete, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Test List Built:** Did I enumerate the required behaviors before writing code?
2. **Failing First:** Was every behavior introduced by a test that failed before the implementation existed?
3. **Real Red:** Did I verify each failure was caused by missing behavior, not a broken test?
4. **Minimal Green:** Is the implementation the simplest code the tests demand — no speculative generality?
5. **Suite Green:** Does the entire test suite pass, not just the new tests?
6. **Refactored on Green Only:** Did all restructuring happen with a passing suite, re-verified after each step?
7. **One Behavior Per Test:** Can each test's name alone explain what broke if it fails?

---

## Deliverables

Provide a concise report:
* **📋 Test List:** The behavior checklist, all items checked off.
* **🔴🟢🔄 Cycles:** The key red-green-refactor cycles — what each test demanded and what changed.
* **🧪 Tests:** The new tests, named as specifications.
* **🔧 Implementation:** The minimal diff the tests drove.
* **✅ Verification:** Full suite results and any deliberately skipped TDD cases (spikes) with justification.
