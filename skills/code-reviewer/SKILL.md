---
name: code-reviewer
description: >-
  Comprehensive workflow for reviewing code branches against the main line,
  inspecting diffs, validating quality, and reporting structured findings. Use
  when asked to check out a branch, compare it with master, assess risks, and
  summarize review outcomes. Always perform a visible chain-of-thought self-review 
  before outputting the final verdict.
---

# Code Review Guide

## Core Review Principles
1. **Surgical Scope & Alignment:** Ensure the PR makes minimal, surgical code changes that directly address the linked Jira ticket. Flag any scope creep.
2. **Resilience & Fault Tolerance:** Verify that the author assumes external calls will fail. Look for graceful degradation, retries, timeouts, and circuit breakers.
3. **Concurrency & State Safety:** Actively scrutinize code for thread safety. Identify potential race conditions, deadlocks, and improper state mutations.
4. **Secure by Default:** Scrutinize for vulnerabilities. Ensure external inputs are constrained, queries are parameterized, and sensitive payloads are not logged.
5. **Backward Compatibility:** Ensure behavior defaults to backward compatibility. Flag breaking changes lacking an explicit migration strategy.

---

## Execution Workflow

### 1. Jira Verification & Context Gathering
* Capture Jira ticket info using the `jira_get_issue` MCP tool if a ticket ID was provided (ID, summary, status, owners, acceptance criteria).
* Read `AGENTS.md` for repo context, `README.md`, and any linked specs.
* Cross-reference the branch scope with the ticket. The ticket is the source of truth; flag any extra migrations or missing acceptance criteria.
* Inspect Jira history for blockers, dependencies, or QA notes.

### 2. Branch Preparation & Diff Baseline
* `git fetch --all --prune`
* `git checkout <target-branch>`
* Determine base branch (`master` unless specified). Run `git merge-base` if needed.
* Use `git diff <base>...HEAD --stat` to scope change size.
* Confirm branch currency with `git status` and `git pull --ff-only` if allowed.

### 3. Inspect Changes & Validate Behavior
* Prefer `git diff` or IDE diff with `--color-words` for readability.
* Review commit history (`git log <base>..HEAD --oneline`) for intent.
* **For each file:** * Map implementation evidence to Jira acceptance criteria. 
  * Identify risky changes against the **Core Review Principles**.
  * Flag missing test coverage for both success and failure paths.
* Execute targeted tests or builds when feasible and capture results.

### 4. Mandatory Chain-of-Thought Review
* You MUST explicitly print your step-by-step evaluation of the **Reviewer Checklist** below *before* providing your final PR summary.

---

## Mandatory Reviewer Checklist

*Before outputting your final review summary, evaluate and state whether the PR successfully meets the following criteria ("Yes", "No", or "N/A"):*

1. **Ticket Alignment:** Does the PR fully satisfy the Jira acceptance criteria without introducing scope creep?
2. **Resilience:** Are new external interactions safely wrapped with timeouts, retries, or circuit breakers?
3. **Concurrency:** Is the code thread-safe and free of obvious race conditions?
4. **Security:** Are there guarantees that no new injection, deserialization, or data-leak risks were introduced?
5. **Compatibility:** Do existing public contracts remain stable?
6. **Tests:** Do tests thoroughly cover both success and failure paths?

---

## Deliverables

After the chain-of-thought checklist, provide a concise written summary including your approval recommendation (Approve / Request Changes / Block) and categorize your findings into:

* **🚨 Blockers (Must Fix):** Unmet Jira criteria, security flaws, breaking changes, or severe bugs.
* **⚠️ Suggestions (Should Fix):** Missing test coverage, readability, optimization, or architectural feedback.
* **💡 Nitpicks (Optional):** Minor naming, style gaps, or formatting suggestions.
* **📋 Next Steps:** Actionable items for the author, including Jira status updates if needed.

*Optional: Use inline comments (`::code-comment{}`) for highly precise findings.*