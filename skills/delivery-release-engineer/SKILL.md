---
name: delivery-release-engineer
description: >-
  Design CI pipelines and ship releases safely: CI check design with fast
  feedback, caching, secrets handling, semantic versioning, changelogs, and
  rollout/rollback plans. Covers the full delivery story from merge to
  production. Use when setting up CI, automating releases, cutting a
  version, or planning a rollout.
---


# Delivery & Release Engineer Guide

## Core Delivery Principles
1. **Every Merge Runs the Gauntlet:** No change reaches the main branch without passing the full CI pipeline. Main is always releasable because the pipeline proves it, every time.
2. **Fast Feedback, Cheap First:** Order checks by cost: lint and format first (seconds), unit tests next (minutes), integration and end-to-end last. A developer should know about a lint failure before their coffee cools.
3. **Hermetic and Cached:** Pipelines must be reproducible — pinned tool versions, locked dependencies — and fast through caching. A pipeline that takes 40 minutes gets skipped, worked around, or resented.
4. **Secrets Stay Secret:** Secrets live in the CI system's secret store, are masked in logs, and are scoped to the minimum jobs that need them. A secret printed in a log is a secret that must be rotated.
5. **Every Release Is Reversible:** If you can't roll it back, you haven't finished releasing it. The rollback plan is written before the rollout starts, not improvised during the incident.
6. **The Changelog Is a User Interface:** Release notes are written for the humans affected by the change — what changed, what to do about it, what might break. "Various fixes" is a changelog failure.

---

## Execution Workflow

### 1. Design the CI Pipeline
* Map the checks to stages: **lint/format → unit tests → build → integration tests → end-to-end** (as applicable).
* Triggers: run the full pipeline on every pull request; run a lighter fast path on pushes to feature branches if needed for speed.
* Keep workflow definitions minimal and readable — pipeline YAML is code, and it rots like code. Extract shared steps into reusable actions where the platform supports it.
* CI must be required for merge (branch protection), not advisory. An optional check is a decoration.

### 2. Make It Fast With Caching
* Cache dependency installs (package managers, build artifacts) keyed on lockfiles. A cache miss should be rare and a cache hit should be the norm.
* Parallelize independent jobs. Split slow test suites by timing, not by directory guesswork.
* Set a pipeline time budget (e.g., under 10 minutes for PR feedback) and treat budget breaches as defects in the pipeline.

### 3. Handle Secrets Properly
* Secrets come from the CI secret store — never hardcoded, never in repo files, never passed as plain job outputs.
* Scope secrets to the jobs that need them. Jobs that don't need production credentials don't get them.
* Enable log masking and audit who can read and modify secrets. Rotate any secret that appears in a log.

### 4. Choose the Versioning Strategy
* Semantic versioning for libraries and APIs: MAJOR for breaking, MINOR for features, PATCH for fixes. No exceptions for "internal" packages — internal consumers break too.
* Tag releases in version control (`v1.2.3`). The tag is the source of truth for what shipped.
* Automate version bumps where the workflow supports it (conventional commits → automated releases), but keep a human in the loop for MAJOR versions.

### 5. Write the Changelog
* Every release gets changelog entries grouped by type: breaking changes, features, fixes.
* Breaking changes get migration notes — what to change in consuming code, with before/after examples where non-obvious.
* Generate from commit history where possible, but edit for humans. Raw commit lists are not changelogs.

### 6. Plan the Rollout and the Rollback
* Choose the rollout shape for the risk: direct deploy for low-risk, staged/canary for anything that touches data, money, or auth.
* Define rollback triggers in advance: which metrics, which thresholds, who decides. Write the exact rollback command or procedure.
* Verify the rollback path works before you need it — a rollback you've never tested is a hope, not a plan.
* For database or state changes, confirm backward compatibility: the new code must tolerate the old schema/state during the rollout window.

### 7. Cut the Release
* Run the full pipeline on the release commit. Tag it. Publish artifacts.
* Announce with the changelog: what changed, what to do about it, known issues.
* Monitor the rollout against the defined triggers. Keep the rollback plan open until the release is declared stable.

---

## Mandatory Delivery Checklist

*Before merging or releasing, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Pipeline Complete:** Does every PR run lint, tests, and build — with branch protection enforcing it?
2. **Fast Feedback:** Do cheap checks run first, with PR feedback inside the time budget?
3. **Reproducible:** Are tool versions pinned, dependencies locked, and caches keyed on lockfiles?
4. **Secrets Safe:** Are all secrets in the secret store, scoped minimally, and masked in logs?
5. **Versioned Correctly:** Does the version bump follow semver, with the release tagged in version control?
6. **Changelog Written:** Does the changelog describe changes for humans, with migration notes for breaking changes?
7. **Rollback Ready:** Is the rollback procedure written, the triggers defined, and the path verified?

---

## Deliverables

Provide a concise report:
* **🔧 Pipeline:** The workflow definition (or diff) with stages, triggers, and caching strategy.
* **🔑 Secrets:** How secrets are stored, scoped, and masked.
* **🏷️ Versioning:** The versioning strategy and the release tag.
* **📝 Changelog:** The release notes as written for humans.
* **🚀 Rollout/Rollback:** The rollout shape, monitoring triggers, and the exact rollback procedure.
* **✅ Verification:** Pipeline run results and rollback-path validation.
