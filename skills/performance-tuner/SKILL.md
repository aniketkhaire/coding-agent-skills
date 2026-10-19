---
name: performance-tuner
description: >-
  Performance diagnosis and optimization workflow. Define the target metric,
  reproduce under realistic load, profile to find the true bottleneck, fix
  one thing at a time, and prove the gain with before/after numbers. Use
  when investigating latency, throughput, memory, or cost problems, or when
  asked to make something faster.
---

# Performance Tuner Guide

## Core Performance Principles
1. **Measure First, Optimize Never on Vibes:** Profile before changing anything. The bottleneck is rarely where it feels like it is.
2. **Optimize the Bottleneck:** Speeding up code that isn't the constraint changes nothing. Work top-down from the profile.
3. **Correctness Before Speed:** A fast wrong answer is a bug. Never trade correctness for performance without explicit approval.
4. **Prove the Gain:** Every optimization ships with before/after numbers on the same workload. Claims without measurements are stories.
5. **Guard Against Regression:** Today's optimization is tomorrow's regression. Turn key benchmarks into automated checks.

---

## Execution Workflow

### 1. Define the Target and Reproduce
* State the goal numerically: p50/p99 latency, requests/sec, memory ceiling, or cost per request — and the workload that defines it.
* Reproduce under realistic load (production-like data volume and concurrency). Optimizing against a toy dataset optimizes the wrong thing.

### 2. Profile
* Use a real profiler (sampling profiler, async-profiler/JFR for JVM, `perf`, browser devtools) — not print statements.
* Capture both CPU and wall-clock profiles, plus allocation/GC behavior for memory issues.
* Identify the top contributors: the 20% of code responsible for 80% of the cost.

### 3. Analyze the Bottleneck
* Classify it: algorithmic complexity, I/O waits, lock contention, GC pressure, serialization, network round-trips, N+1 queries.
* Estimate the theoretical ceiling: what would perfect look like? If the gap is small, stop — the bottleneck may not be worth fixing.

### 4. Fix One Thing at a Time
* Change the single biggest contributor first, then re-profile. Stacked changes make attribution impossible.
* Prefer algorithmic and architectural fixes (better data structure, batching, caching, parallelism) over micro-optimizations.

### 5. Benchmark and Verify
* Re-run the same workload: report before/after for the target metric and check that nothing else regressed (correctness suite green, other endpoints unaffected).
* Record the numbers in the change description so future readers know what "fast enough" meant.

---

## Mandatory Tuner Checklist

*Before claiming victory, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Measured:** Was the bottleneck identified with a profiler on a realistic workload?
2. **Targeted:** Does the change address the profiled bottleneck, not adjacent code?
3. **Correct:** Does the full test suite still pass with identical behavior?
4. **Proven:** Are before/after numbers reported for the target metric on the same workload?
5. **Guarded:** Is there a benchmark or alert that will catch a regression?

---

## Deliverables

* **🎯 Target:** The metric, the workload, and the goal.
* **📊 Profile:** What the profiler showed — top contributors and bottleneck classification.
* **🔧 Change:** The optimization, why this approach, and alternatives considered.
* **📈 Results:** Before/after numbers, plus confirmation that correctness held.
