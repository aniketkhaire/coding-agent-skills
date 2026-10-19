---
name: incident-responder
description: >-
  Production incident response workflow. Assess severity, mitigate first and
  diagnose second, communicate continuously, then drive a blameless
  root-cause analysis and postmortem with owned action items. Use when
  production is down, degraded, or misbehaving and a structured response
  is needed.
---

# Incident Responder Guide

## Core Response Principles
1. **Mitigate First, Understand Second:** Stop the bleeding — roll back, flip the flag, shed load — before hunting the root cause.
2. **Communicate Continuously:** State what you know, what you are doing, and when you will update next. Silence during an incident is a second incident.
3. **Verify Before Acting in Production:** Every prod action is deliberate and reversible. Guesswork under pressure causes outages.
4. **Blameless by Default:** Systems fail; people operate them. The review targets the system, never the individual.
5. **Every Incident Pays for Itself:** No incident closes without durable fixes and owned action items.

---

## Execution Workflow

### 1. Assess and Declare
* Determine severity (SEV1: full outage / SEV2: major degradation / SEV3: minor) and blast radius: who is affected, since when.
* Declare the incident, designate a commander, and open a single communication channel. Start a timeline immediately.

### 2. Mitigate
* Choose the fastest safe mitigation: rollback the last deploy, disable the feature flag, fail over, scale out, or shed non-critical load.
* Apply one mitigation at a time and watch the metrics before trying the next.
* Record every action with a timestamp for the timeline.

### 3. Stabilize and Communicate
* Confirm the mitigation held: error rates, latency, and saturation back within bounds.
* Post status updates on a fixed cadence (e.g., every 15–30 minutes for SEV1): impact, current action, next update time.
* Notify stakeholders through the agreed channel; keep customer-facing messaging factual and jargon-free.

### 4. Root-Cause Analysis
* Build the timeline: deploy times, config changes, traffic shifts, first symptom.
* Ask "why" iteratively until you reach a systemic cause, not a human error.
* Distinguish the trigger (what set it off) from the vulnerability (why the system couldn't absorb it).

### 5. Postmortem and Action Items
* Write the postmortem within days, while memory is fresh: summary, impact, timeline, root cause, what went well, what didn't.
* Every action item has an owner and a due date. Vague items ("improve monitoring") are rewritten as concrete ones ("add p99 alert on /checkout with 5m burn rate").
* Share the postmortem with the wider team; file action items where they will actually be tracked.

---

## Mandatory Responder Checklist

*Before closing the incident, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Mitigated:** Is user impact stopped or fully worked around?
2. **Communicated:** Were stakeholders updated on a cadence until resolution?
3. **Root Cause Found:** Is the systemic cause identified with timeline evidence?
4. **Blameless:** Does the review target systems and processes, not people?
5. **Actionable:** Does every action item have an owner, a due date, and a concrete definition of done?

---

## Deliverables

* **🚨 Summary:** Severity, impact, duration, and resolution.
* **🕐 Timeline:** Key events from first symptom to all-clear.
* **🎯 Root Cause:** Trigger vs. underlying vulnerability, with evidence.
* **📋 Action Items:** Owner, due date, and concrete outcome for each.
