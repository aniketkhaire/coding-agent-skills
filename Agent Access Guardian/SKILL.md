---
name: agent-access-guardian
description: >-
  Least-privilege identity and permission management for coding agents. Provision
  per-task scoped identities, grant the minimum repository, API, and
  environment access the job needs, require human approval for destructive or
  irreversible actions, and audit everything the agent touches. Use when setting
  up an agent to work in a repo, granting it credentials or tool access,
  configuring sandboxes, or reviewing what permissions an existing agent holds.
---

# Agent Access Guardian Guide

*Scope note:* Security Sentinel threat-models and reviews *code* for vulnerabilities; Git Workflow Guardian owns safe *version-control mechanics* for agents. This skill owns the *agent's own identity, credentials, and permission lifecycle* — who the agent is allowed to be, what it may touch, and what stops it from doing damage.

## Core Access Principles
1. **The Agent Is a New Team Member:** Every agent session gets its own identity — its own tokens, its own scopes, its own audit trail. Never hand an agent your personal credentials or a shared service account with standing access.
2. **Least Privilege, Scoped to the Task:** Grant exactly what this task needs and nothing more: specific repositories, specific environments, read-only where possible. A task that only edits code needs no production access and no secret-read rights. (Where Security Sentinel applies least privilege to *systems*, this skill applies it to the *agent's working identity*.)
3. **Ephemeral by Default:** Credentials issued to an agent should expire with the task. Short-lived tokens beat long-lived ones; re-issue beats renew. For standing agents (IDE assistants, CI bots), enforce a rotation cadence, keep scopes narrow, re-authorize periodically, and review granted scopes on a schedule.
4. **Destructive Actions Need a Gate:** Anything irreversible — deletes, force-pushes, production deploys, secret rotations, database writes — requires an explicit approval step or a dry-run review before it executes. For this destructive class, the default answer to "should the agent run this unattended?" is no.
5. **Audit Everything the Agent Touches:** Log every action with the agent's identity, the permission used, and the result. If you cannot reconstruct what an agent did, the access was too broad.
6. **Granted Access Is Still Exfiltratable:** A scoped token in an agent's hands can be exfiltrated by injected instructions. Constrain what the agent may do with granted access: no writing credentials into tool output or artifacts, no network egress except to allowlisted hosts, and treat any credential-bearing output as a compromise.

---

## Execution Workflow

### 1. Inventory What the Task Requires
* List every resource the task touches: repositories, branches, CI systems, package registries, cloud accounts, databases, secrets.
* For each, mark read vs. write, and flag anything irreversible or production-facing.
* Identify what the task does NOT need — those are the scopes you must not grant.

### 2. Provision a Scoped Identity
* Create a dedicated identity for the agent (bot account, deploy key, or short-lived token). Never reuse a human's credentials or a shared account.
* Apply the narrowest scopes the task allows: repo-scoped tokens, read-only where writes are unnecessary, environment-restricted keys.
* Prefer short-lived credentials: token lifetimes measured in hours, expiring with the session.

### 3. Set Approval Gates
* Define which actions run unattended and which pause for approval: file edits inside the task's repo can run free; force-push, delete, production deploy, and secret access cannot.
* Enforce the gate in the toolchain (approval policy, dry-run mode), not just as an instruction in the prompt. A guardrail in the system prompt is a suggestion; a gate in the toolchain is enforcement.
* Provide a safe escape hatch: a staging or dry-run path so the agent can demonstrate intent before the gate opens.

### 4. Isolate the Execution Environment
* Confine the agent to a working directory: it should not read or write outside the task's tree.
* Restrict network egress to allowlisted hosts; block arbitrary outbound calls.
* Give the agent no access to the host's credential store, shell history, or other users' files.

### 5. Run and Observe
* Launch the agent under its scoped identity and watch the first actions closely — the first few tool calls reveal whether the scope matches reality.
* If the agent repeatedly requests broader access, treat it as a signal: either the scope was wrong (expand deliberately) or the task was mis-scoped (split the task).

### 6. Audit and Revoke
* Review the action log: every write, every permission exercised, every denied request.
* Confirm no credential leaked into code, logs, prompts, or artifacts.
* Revoke or let expire the task's credentials; keep only what a standing identity legitimately needs.

---

## Mandatory Access Checklist

*Before letting an agent act on real systems, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Dedicated Identity:** Does the agent run under its own credentials — not a human's, and not a shared account's?
2. **Every Permission Justified:** Can I name every permission granted and justify each against the task?
3. **Sensitive Excluded:** Are production and secret-bearing resources excluded unless the task truly requires them?
4. **Ephemeral or Rotated:** Do task credentials expire with the task — and for standing agents, is there a rotation cadence with periodic re-authorization?
5. **Gates on Destruction:** Are destructive and irreversible actions blocked by an explicit approval step or dry-run review, enforced by the toolchain?
6. **Environment Isolated:** Is the agent confined to its working tree, with egress allowlisted and no access to the host credential store?
7. **Complete Audit Trail:** Can I reconstruct every action the agent took, each attributable to a specific identity and permission?

---

## Deliverables

Provide a concise report:
* **🪪 Identity:** The agent's dedicated identity, how its credentials are issued, and when they expire.
* **📐 Permission Matrix:** Resources granted (read vs. write), and what was deliberately excluded.
* **🚧 Approval Gates:** Which actions require human approval and how the gate is enforced.
* **📋 Audit Summary:** Actions taken, permissions exercised, denied requests, and confirmation that task credentials were revoked.
