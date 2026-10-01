---
name: git-workflow-guardian
description: >-
  Safe version-control workflow for coding agents. Branch per change, stage
  deliberately, write conventional commits, run pre-commit checks, open PRs
  humans can review — and never rewrite shared history. Includes PR
  description standards folded in. Use when committing, branching, pushing,
  opening pull requests, or setting up repository hygiene.
---

# Git Workflow Guardian Guide

## Core Workflow Principles
1. **Never Rewrite Shared History:** Once a commit is pushed to a shared branch, it is immutable. No force-push, no rebase, no amend on `main` or any branch someone else may have pulled. Local-only history is yours to shape; shared history is not.
2. **Branch Per Change:** Every change gets its own branch with a descriptive name (`feat/oauth-refresh`, `fix/null-pointer-checkout`). `main` stays releasable at all times.
3. **Atomic Commits:** One commit = one logical change. A reviewer should be able to understand, revert, or cherry-pick each commit independently. If the message needs "and", split the commit.
4. **Conventional Messages:** `type(scope): description` — `feat`, `fix`, `refactor`, `test`, `docs`, `chore`. The type tells automation and humans what kind of change this is; the scope tells them where.
5. **Verify Before You Push:** Pre-commit hooks, lint, and the affected tests run before code leaves your machine. CI is a backstop, not your first check.
6. **PRs Are for Humans:** The pull request description is written for the reviewer, not for you. Context, test evidence, and what to look at — assume the reviewer has not read your mind.

---

## Execution Workflow

### 1. Start on the Right Branch
* Check `git status` and `git branch --show-current` before doing anything. A clean tree on the wrong branch is still the wrong branch.
* Create a feature branch from an up-to-date `main`: `git checkout -b feat/descriptive-name`.
* For stacked or dependent work, branch from the parent feature branch and say so in the PR.

### 2. Stage Deliberately
* Review the diff before staging: `git status` and `git diff`. Know exactly what is about to be committed.
* Never `git add -A` blindly. Stage files intentionally; use `git add -p` for mixed files.
* Keep secrets, credentials, `.env` files, and build artifacts out of the commit. If it's already tracked, that's a separate fix — don't bundle it.

### 3. Write the Commit
* One logical change per commit. Run the relevant tests for that change before committing it.
* Message format: `type(scope): imperative description` — e.g. `fix(auth): reject expired refresh tokens`.
* Body (when the change needs it): what changed, why, and any non-obvious trade-offs. Reference issue numbers: `Closes #123`.

### 4. Run the Gates
* Pre-commit hooks must pass (lint, format, secret scan). If the repo lacks them, say so — don't silently skip.
* Run the test suite for everything the change touches. New behavior ships with new tests.
* If CI exists, push and watch it. A red CI you ignore is a red CI you own.

### 5. Push and Open the PR
* Push the branch and open a pull request against `main` (or the agreed target).
* PR description must include:
  * **What and why:** the change and the reason, in plain language.
  * **Test evidence:** what you ran and the result — not "tests pass", the actual outcome.
  * **Reviewer guidance:** what to look at first, known trade-offs, anything you want challenged.
  * **Linked issues:** `Closes #123` so the trail is complete.
* Keep the PR focused. A PR that does three things gets three rounds of review; split it instead.

### 6. Handle Feedback Without Rewriting History
* Address review comments with new commits on the branch. Don't force-push over review history unless the team explicitly rebases on merge.
* Rebase only your own unpushed commits. The moment someone else may have the branch, history is frozen.

---

## Mandatory Guardian Checklist

*Before pushing or opening the PR, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Correct Branch:** Am I on a dedicated feature branch, branched from up-to-date `main`?
2. **Deliberate Staging:** Did I review the full diff — no secrets, no artifacts, no unrelated files?
3. **Atomic Commits:** Does each commit represent exactly one logical change?
4. **Conventional Messages:** Do messages follow `type(scope): description` with issue references where applicable?
5. **Gates Green:** Did pre-commit hooks, lint, and the affected tests pass locally?
6. **History Intact:** Have I avoided force-push, rebase, or amend on any shared branch?
7. **PR Complete:** Does the PR describe what/why, show test evidence, guide the reviewer, and link issues?

---

## Deliverables

Provide a concise report:
* **🌿 Branch:** Branch name and base.
* **📦 Commits:** The commit list with messages.
* **🔗 PR:** PR link with the full description as written.
* **✅ Verification:** Hook, lint, and test results; CI status if applicable.
