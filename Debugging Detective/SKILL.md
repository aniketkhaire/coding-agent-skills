name: debugging-detective
description: >-
  Systematic root-cause debugging workflow for investigating bugs, crashes,
  and unexpected behavior. Reproduce the failure reliably, gather evidence,
  bisect to the fault, apply a minimal fix at the root cause, and prove it
  with a regression test. Use when asked to find and fix a bug, investigate
  a crash, or explain why something is misbehaving.
---

# Debugging Detective Guide

## Core Debugging Principles
1. **Reproduce Before You Theorize:** Never attempt a fix without a reliable reproduction. A bug you cannot reproduce is a bug you cannot prove fixed.
2. **Evidence Over Intuition:** Logs, stack traces, debugger sessions, and metrics outrank gut feeling. Form hypotheses, then test them.
3. **Narrow the Blast Radius:** Bisect the problem space — code, commits, config, data — instead of reading the whole system hoping to spot it.
4. **Fix the Cause, Not the Symptom:** A fix that silences the error without addressing the root cause is a future incident with a new timestamp.
5. **Prove the Fix:** Demonstrate the failure before and the pass after. Add a regression test so the bug stays dead.

---

## Execution Workflow

### 1. Capture the Symptom
* Record exactly what was observed: error messages, stack traces, exit codes, affected inputs, environment, and frequency (always / intermittent / load-dependent).
* Note what changed recently: deploys, config edits, dependency updates, data migrations. `git log --since` around the failure window is cheap and often decisive.

### 2. Build a Reliable Reproduction
* Write the smallest script, test, or curl command that triggers the failure consistently.
* If it is intermittent, capture the conditions that raise its probability (concurrency, timing, specific data) and log enough context to catch it.
* Do not proceed to diagnosis until the repro is stable — everything downstream depends on it.

### 3. Gather Evidence
* Read the full stack trace; identify the exact line and call path, not just the exception type.
* Add targeted logging or attach a debugger at the failure point. Inspect actual values, not assumed ones.
* Check adjacent layers: is the input actually what the caller claims? Is the config the code reads the config you think is deployed?
* For regressions, use `git bisect` to find the exact commit that introduced the failure.

### 4. Hypothesize and Test
* State each hypothesis explicitly and design the cheapest experiment that would disprove it.
* Change one variable at a time. If a hypothesis survives testing, keep it; if it dies, record why and move on.
* Prefer binary search over linear reading: disable half the suspects, re-run the repro, repeat.

### 5. Apply a Minimal Fix at the Root Cause
* Fix the smallest unit that eliminates the cause. Resist the urge to refactor neighboring code in the same change.
* Handle the failure mode explicitly: validate inputs, bound retries, fail with a clear message — whatever prevents this class of bug from recurring silently.

### 6. Verify and Lock In
* Re-run the original reproduction: it must now pass.
* Add a regression test that fails on the old code and passes on the new code.
* Run the surrounding test suite. Check edge cases adjacent to the fix (null/empty inputs, boundary values, concurrent access).

---

## Mandatory Detective Checklist

*Before declaring the bug fixed, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Reproduced:** Can I trigger the failure on demand with the reproduction I built?
2. **Root Cause Identified:** Do I have concrete evidence (trace, log, bisect result) pointing at the cause — not just a plausible story?
3. **Minimal Fix:** Does the change address the cause directly, without unrelated refactors?
4. **Regression Test:** Is there a test that fails before the fix and passes after?
5. **No Collateral:** Do the existing tests still pass, and are adjacent edge cases covered?

---

## Deliverables

Provide a concise report:
* **🔍 Symptom:** What was failing, where, and under what conditions.
* **🧪 Reproduction:** The repro script/command and its before/after behavior.
* **🎯 Root Cause:** The exact fault with the evidence that proves it.
* **🔧 Fix:** What changed and why this location, in minimal-diff form.
* **✅ Verification:** Regression test added, suite results, and any edge cases checked.
