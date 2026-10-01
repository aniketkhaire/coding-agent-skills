name: refactoring-craftsman
description: >-
  Behavior-preserving code restructuring workflow. Map the code, lock current
  behavior with characterization tests, then improve structure in tiny
  verifiable steps — extract, rename, decompose, simplify — without changing
  what the code does. Use when asked to clean up, restructure, or modernize
  code, reduce duplication, or improve readability without behavior change.
---

# Refactoring Craftsman Guide

## Core Refactoring Principles
1. **Behavior Is the Contract:** A refactor changes structure, never behavior. Any behavior change is a separate, explicitly requested task.
2. **Tests Are the Safety Net:** Never restructure untested code. Write characterization tests that pin current behavior before touching anything.
3. **Small Steps, Green Always:** Each step must compile and keep the suite green. If a step breaks something, you know exactly which step did it.
4. **Name Things Honestly:** The cheapest, highest-leverage refactor is an accurate name. Rename first, restructure second.
5. **Delete, Don't Comment Out:** Dead code belongs in version control history, not in the file. Remove it outright.

---

## Execution Workflow

### 1. Map the Territory
* Read the target code and identify its callers, inputs, outputs, and side effects.
* Note existing tests and their coverage of the area. Flag untested paths — they need characterization tests first.
* Define the goal in one sentence (e.g., "extract the retry policy so it can be reused" or "decompose this 200-line method into named steps").

### 2. Lock Behavior with Characterization Tests
* For any untested code you will touch, write tests that capture what it does today — including quirks. These tests are scaffolding: they prove you changed nothing.
* Run the full existing suite to establish a green baseline before your first edit.

### 3. Refactor in Tiny Steps
* Work in the smallest possible increments: rename a variable, extract a method, replace a conditional with polymorphism, remove duplication.
* After every step: compile and run the affected tests. Green means continue; red means revert that step immediately.
* Prefer mechanical, tool-assisted moves (IDE extract/rename) over hand-rewrites.
* Common moves, in order of increasing risk: rename → extract variable/method → inline duplication → decompose conditionals → replace type codes with classes → extract module.

### 4. Verify Equivalence
* The full suite is green, including your characterization tests — behavior is unchanged by construction.
* Review the final diff: it should read as a series of small, intention-revealing moves, not a rewrite.
* Check performance-sensitive paths: refactors shouldn't silently add allocations, queries, or round-trips.

---

## Mandatory Craftsman Checklist

*Before presenting the refactor, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Behavior Preserved:** Do all pre-existing tests plus new characterization tests pass unchanged?
2. **Test Coverage:** Is every touched path covered by a test that pins its behavior?
3. **Small Steps:** Could each intermediate state have been committed independently?
4. **No Feature Creep:** Is there zero behavior change, bug fix, or new feature smuggled in?
5. **Readable Diff:** Does the diff tell a clear story a reviewer can follow?

---

## Deliverables

* **🎯 Goal:** The one-sentence restructuring objective.
* **🧪 Safety Net:** Characterization tests added and baseline suite status.
* **🔧 Changes:** Summary of the refactoring moves, in diff order.
* **✅ Verification:** Final test results confirming zero behavior change.
