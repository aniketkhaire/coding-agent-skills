name: context-optimizer
description: >-
  Discipline for managing a coding agent's context window across long or
  complex tasks. Set an explicit token budget, assemble minimal starting
  context, sandbox and compress tool output, maintain rolling session memory,
  and defragment when the window fills up. Use when a task is large enough
  that context exhaustion, forgotten decisions, or re-reading the same files
  is a real risk.
---

# Context Optimizer Guide

## Core Context Principles
1. **Context Is Working Memory, Not Storage:** The window is finite and every byte loaded competes with reasoning. Treat tokens as a budget to be spent, not a disk to be filled.
2. **Compress, Don't Carry:** Keep conclusions, not bulk. Summarize logs, traces, and file dumps; retain the signal and discard the raw mass.
3. **Budget Explicitly:** Decide up front how the window splits across task brief, code, tool output, memory, and reasoning headroom. An implicit budget is always overspent.
4. **Memory Is a Contract:** Session-memory notes are the handoff mechanism between runs and phases. Write what is needed to resume, nothing more, in the smallest form that survives.
5. **Retrieve, Don't Retain:** Re-read a file on demand rather than keeping everything loaded "just in case." Fresh reads beat stale retention.
6. **Protect the Window:** Secrets, credentials, giant binary dumps, and boilerplate noise never earn tokens. Truncate, redact, or exclude them at the source.
7. **Measure and Improve:** After each session, note what burned budget and what paid off. Tighten the assembly rules for next time.

---

## Execution Workflow

### 1. Classify the Task and Set a Budget
* Classify the work: exploration, implementation, debugging, review, or multi-phase.
* Write down the budget explicitly: total window size, and allocations (e.g. task brief 10%, code under edit 40%, tool output 20%, session memory 10%, reasoning headroom 20%).
* Name what is deliberately excluded: files to be re-read on demand, logs to be summarized, artifacts to live outside the window.

### 2. Assemble the Minimal Starting Context
* Load only: the task brief, the directly implicated files, and the smallest interfaces/schemas that define the contracts involved.
* Prefer indexes, outlines, and summaries for orientation (directory trees, symbol lists, README overviews) over full file contents.
* For each item loaded, state in one line why it earned its place; anything that cannot earn its place is deferred.

### 3. Sandbox and Compress Tool Output
* Never dump full logs, test suites, or tracebacks into the working context. Truncate, page, and `grep` for the signal first.
* Extract the failing assertions, error lines, and exit codes; record those and drop the rest.
* Cap repeated identical output: after the second identical failure, the raw log is summarized once and never reloaded.

### 4. Write Rolling Session Memory
* Maintain a compact handoff note as work progresses: what is done, the current state, open questions, and key decisions with their rationale.
* Update it at phase boundaries (after exploring, after implementing, after testing) — not at the end.
* Prune entries the moment they become stale; a memory note that contradicts current state is worse than none.

### 5. Detect Context Pressure and Defragment
* Watch for the symptoms: re-reading files already seen, contradicting earlier decisions, tool output being summarized from memory instead of read, reasoning about things no longer in the window.
* When pressured, defragment: summarize completed phases into the memory note, drop raw outputs and exploratory dead ends, consolidate decisions into one list.
* If the window is truly exhausted, end the session deliberately: write the handoff note so a fresh session can resume without re-reading everything.

### 6. Retrieve on Demand and Verify Before Acting
* Re-read any file whose state may have changed since it was loaded (edits by parallel agents, test runs, new commits).
* Never act on remembered content for expensive or irreversible operations; verify the current state first.
* Prefer targeted reads (a function, a diff hunk) over reloading whole files.

### 7. Review and Tighten
* After the session, record the estimate: what was loaded, what was summarized, what the peak pressure point was.
* Name the biggest budget burners and what would have replaced them (a summary, a deferred read, a tighter grep).
* Update the assembly rules for this codebase so the next session starts leaner.

---

## Mandatory Optimizer Checklist

*Before declaring a session well-managed, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Budget Set:** Is there an explicit context budget for this task — what loads and what stays out?
2. **Minimal Assembly:** Did the starting context contain only files and outputs the task actually needs?
3. **Output Sandboxed:** Are large tool outputs truncated, paged, or summarized rather than fully retained?
4. **Signal Extracted:** For each retained output, can I state in one line why it earned its tokens?
5. **Memory Maintained:** Is there a compact session-memory note that another run could use to resume?
6. **Stale Context Pruned:** Have completed-phase details, dead ends, and raw outputs been summarized or dropped?
7. **No Secrets Retained:** Are credentials, tokens, and secrets excluded from retained context and memory notes?
8. **Verified Before Acting:** Did I re-read any file whose state may have changed before acting on it?

---

## Deliverables

Provide a concise report:
* **📏 Context Budget:** Total window, allocations per category, and estimated actual burn.
* **🧠 Session Memory:** The compact handoff note — done, current state, open questions, decisions.
* **🗜️ Compression Log:** What was truncated or summarized, and the signal that was kept.
* **🔁 Resume Plan:** How a fresh session picks this up without re-reading everything.
* **📉 Efficiency Notes:** What burned the most budget and how to tighten it next time.
