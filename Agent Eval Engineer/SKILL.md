name: agent-eval-engineer
description: >-
  Design and run evaluations for AI coding agents. Sample task suites from real
  work, build sandboxed reproducible environments, score deterministically with
  calibrated LLM judges, measure reliability, check for gaming, and
  regression-gate every change. Use when choosing a model or harness,
  regression-testing an agent config change, measuring a review agent's
  precision/recall, or proving an agent's reliability before autonomous
  deployment.
---

# Agent Eval Engineer Guide

Benchmarks filter candidates. Your own evals make the decision. This skill builds
evaluation suites that answer one question honestly: does this coding agent do the
job, on our work, reliably — and can we prove it?

## Core Evaluation Principles

1. **Evaluate the system you will ship, not the demo:** Same model, same harness,
   same tools, same repo, same permissions. A score earned under different
   conditions is fiction.
2. **Ground truth beats vibes:** Lead with deterministic checks (tests pass, files
   changed correctly, database state matches). Use an LLM judge only where
   deterministic checks cannot reach — and calibrate the judge before trusting it.
3. **Measure the failure modes you actually fear:** Gaming (editing tests instead
   of fixing code, claiming success after a no-op), inconsistency (passes once,
   fails the next run), noise (a reviewer agent that buries developers in
   false alarms).
4. **Evidence over trajectory:** Score what changed in the world — code, tests,
   database state — not what the agent said it did. A trajectory is a claim;
   the environment state is the evidence.
5. **Benchmarks filter; your evals decide:** Public leaderboard scores eliminate
   agents that cannot handle real tool loops at all. Your task suite, on your
   codebase, under your constraints, is the final answer.
6. **Precision and recall are a tradeoff, never a single number:** Especially for
   reviewer agents: a reviewer that flags everything catches more real problems
   and drowns the team in noise. Report the tradeoff, sliced by severity.
7. **Regression-gate every change:** Model swaps, prompt edits, harness upgrades,
   tool additions — all re-run the suite. No silent drift. An eval suite that
   does not gate a decision is theater.

---

## Execution Workflow

### 1. Write Down the Decision
* One sentence: what will this eval change? (Model choice, harness config,
  ship/no-ship for an autonomous workflow, reviewer-agent rollout.)
* If the eval cannot change a decision, stop.

### 2. Harvest Tasks from Real Work
* Sample from your backlog, PRs, and issue tracker — the hard, high-stakes,
  failure-prone tasks, not toy problems.
* Weight the suite toward the work the agent will actually do. Minimum viable
  suite: 20–30 tasks before trusting any comparison.

### 3. Build a Sandboxed, Reproducible Environment
* Containerized, resettable, pinned dependencies.
* Record the golden outcome for each task: the tests that must pass, the files
  that must change, the state that must hold.
* Anyone must be able to re-run a task and get the same starting line.

### 4. Choose Scoring, Deterministic First
* Deterministic checks where possible (test results, diff inspection,
  state comparison).
* LLM-as-judge only for qualitative review, with a written rubric.
* Add decoy-clean cases — tasks with no defect — to measure false-positive rate.

### 5. Calibrate the Judge
* Run the judge over known-good and known-bad outputs first.
* Require a documented agreement threshold before the judge scores anything real.
* An uncalibrated judge is a random number generator with confidence.

### 6. Measure Reliability, Not Just the Mean
* Run every task multiple times. Report pass@k and worst-case behavior alongside
  the average.
* The best agents still fail most repeated runs on hard tasks — the mean hides that.

### 7. Run the Anti-Gaming Pass
* Diff every result: did the agent change tests instead of fixing code? Did a
  "success" correspond to any file change at all? Did it echo the prompt back
  as its own plan?
* The agent's transcript is not ground truth; your independent record is.

### 8. Gate and Log
* Define ship/no-ship and regression thresholds BEFORE seeing results (e.g., a
  model change must not drop pass rate or raise false-positive rate beyond X).
* Record model, harness, config, date, and full results in a rerunnable log.
* Publish the comparison, not the headline.

---

## Mandatory Checklist

*Before declaring the eval complete, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Decision Written:** Is the decision this eval informs written down?
2. **Real-Work Tasks:** Are tasks sampled from real work, weighted toward the hard and high-stakes?
3. **Reproducible Environment:** Is the environment sandboxed, reproducible, and resettable?
4. **Deterministic First:** Does scoring lead with deterministic checks before any judge?
5. **Judge Calibrated:** Is the LLM judge calibrated against known-good/known-bad outputs? (N/A if no judge used)
6. **Decoy-Clean Cases:** Are no-defect tasks included to measure false positives? (N/A if not applicable)
7. **Reliability Reported:** Was each task run multiple times, with reliability (not just the mean) reported?
8. **Anti-Gaming Pass:** Was an anti-gaming pass performed (test-only edits, no-op success claims)?
9. **Thresholds Before Results:** Are ship and regression thresholds defined before results are seen?
10. **Results Logged:** Are results logged reproducibly — model, harness, config, date?

---

## Deliverables

Provide a concise report:
* **📋 Eval Plan:** The decision, the task list with sources, suite-size rationale.
* **📦 Environment Spec:** Sandbox setup, reset procedure, golden outcomes per task.
* **⚖️ Scoring Rubric:** Deterministic checks, judge prompt, calibration results, decoy-clean cases.
* **📊 Results Report:** Pass rates, pass@k, precision/recall (sliced by severity), failure taxonomy, gaming findings.
* **🚦 Regression Gate:** Thresholds, rerun procedure, and the logged results entry that future runs compare against.
