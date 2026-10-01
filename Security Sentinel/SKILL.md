name: security-sentinel
description: >-
  Security review and lightweight threat-modeling workflow. Map trust
  boundaries and data flows, enumerate threats, then review authentication,
  authorization, input handling, secrets, cryptography, and logging for
  concrete vulnerabilities. Use when reviewing code that handles auth,
  user input, secrets, PII, payments, or any new externally reachable
  surface.
---

# Security Sentinel Guide

## Core Security Principles
1. **Never Trust Input:** Validate and constrain every value at the trust boundary — type, length, range, encoding. Client-side checks are UX, not security.
2. **Least Privilege Everywhere:** Every component gets the minimum access it needs: database roles, IAM policies, API scopes, file permissions.
3. **Secrets Never in Code or Logs:** Credentials live in a secret manager, rotate regularly, and never appear in repos, logs, error messages, or URLs.
4. **Defense in Depth:** No single control is the whole story. Layer validation, auth, rate limiting, and monitoring so one failure doesn't become a breach.
5. **Fail Closed:** When in doubt, deny. An error should leave the system more restrictive, never more permissive.

---

## Execution Workflow

### 1. Map Trust Boundaries and Data Flows
* Identify where untrusted data enters: HTTP inputs, webhooks, file uploads, queues, third-party callbacks.
* Trace sensitive data (credentials, PII, tokens) through storage, logs, caches, and outbound calls. Note every place it rests or moves.

### 2. Enumerate Threats
* For each boundary, ask: spoofing (auth), tampering (integrity), repudiation, information disclosure, denial of service, elevation of privilege.
* Rank by likelihood × impact. Focus energy on the top threats, not an exhaustive list.

### 3. Review the Code
* **Authentication & sessions:** strong password/token handling, secure session storage, safe reset flows, MFA where warranted.
* **Authorization:** checks on every privileged operation, not just at the UI layer; verify object-level access (IDOR).
* **Input handling:** parameterized queries, output encoding, file-type and size limits, SSRF guards on outbound requests.
* **Secrets & crypto:** no hardcoded secrets, standard algorithms with safe parameters, no custom crypto.
* **Logging & errors:** no sensitive data in logs; generic error messages to clients, detailed ones server-side.

### 4. Rate and Report
* Severity per finding: Critical (exploitable now), High (likely exploitable), Medium (weakens defenses), Low (hygiene).
* Each finding states the vulnerable location, the attack scenario in one paragraph, and the concrete remediation.

---

## Mandatory Sentinel Checklist

*Before signing off, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Boundaries Mapped:** Are all untrusted inputs and sensitive data flows identified?
2. **Auth Complete:** Are authentication and per-object authorization enforced server-side on every privileged path?
3. **Input Constrained:** Are injection, SSRF, and upload risks addressed at each boundary?
4. **Secrets Safe:** Is there zero evidence of secrets in code, config, logs, or error output?
5. **Fail Closed:** Do error and edge paths deny by default rather than permit?
6. **Findings Triaged:** Does every finding have an assigned owner and a remediation timeline, in addition to its severity rating?
7. **Residual Risk Recorded:** Are accepted (unfixed) risks explicitly recorded with the reason for acceptance?

---

## Deliverables

* **🗺️ Threat Model:** Trust boundaries, data flows, and top threats.
* **🚨 Findings:** By severity — location, attack scenario, and remediation for each.
* **✅ Verdict:** Approve, approve-with-fixes, or block, with the reasoning.
