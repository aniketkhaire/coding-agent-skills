---
name: observability-engineer
description: >-
  Decide what to log, measure, and trace — and how much — so production
  systems can be understood and debugged. Structured logging, RED/USE
  metrics, distributed tracing, dashboards, symptom-based alerting, and SLO
  thinking. Judgment and design are in scope; vendor-specific syntax is not.
  Use when adding instrumentation, designing dashboards or alerts, defining
  SLOs, or preparing a service for production.
---

# Observability Engineer Guide

## Core Observability Principles
1. **Instrument for the Questions You'll Ask at 3 AM:** Every log line, metric, and span should answer a question someone will actually ask during an incident. Instrumentation without a question behind it is noise.
2. **Structure Every Log:** Logs are data, not prose. Emit structured events (JSON or equivalent) with consistent field names — timestamp, service, trace ID, outcome — so they can be queried, not just read.
3. **Measure What Matters:** For request-driven services, RED: Rate, Errors, Duration. For resources, USE: Utilization, Saturation, Errors. Start with these before inventing custom metrics.
4. **Trace Across Boundaries:** A request's journey through services is the unit of debugging in distributed systems. Propagate trace context across every boundary — HTTP, queues, async jobs — or the trail goes cold exactly where you need it.
5. **Alert on Symptoms, Not Causes:** Alert on what the user experiences (error rate, latency, availability), not on internal guesses (CPU, queue depth). Cause-based alerts page people for things users never felt; symptom-based alerts page when it matters.
6. **Cardinality Is a Budget:** Every unique label combination multiplies storage cost. High-cardinality fields (user IDs, request IDs) belong in logs/traces, not in metric labels. Budget cardinality like money.

---

## Execution Workflow

### 1. Identify the Critical User Journeys
* List the user-facing journeys that must work: signup, checkout, search, the core loop of the product.
* For each journey, define what "working" means in measurable terms. These become your SLIs (Service Level Indicators).
* Rank by business impact. Instrument the money path first.

### 2. Define SLIs and SLOs
* Choose SLIs per journey: availability (success ratio), latency (percentiles, not averages — p95/p99), and where relevant, freshness or correctness.
* Set SLOs as targets with time windows (e.g., 99.9% of requests succeed over 30 days). The SLO defines the error budget — how much failure is acceptable before action is required.
* Write down what happens when the budget burns: who is notified, what the response is. An SLO without a consequence is a wish.

### 3. Design Logging
* Define the standard event schema: timestamp, service, environment, trace/request ID, actor, action, outcome, duration.
* Log at boundaries: incoming requests, outgoing calls, state transitions, failures. Log the decision inputs for important automated decisions.
* Set log levels with discipline: ERROR for needs-human-action, WARN for degraded-but-handled, INFO for business-significant events, DEBUG off in production by default.
* Ensure sensitive data (tokens, PII, credentials) is never logged — redact at the emission point, not in the pipeline.

### 4. Design Metrics
* Apply RED to every request-driven component and USE to every resource. These are the starting set, not the whole set.
* Add business metrics alongside technical ones: signups, orders, conversions. Technical metrics tell you the system is broken; business metrics tell you it matters.
* Keep label cardinality bounded. Review every new label for explosion risk before shipping it.

### 5. Design Tracing
* Propagate trace context across all service boundaries: HTTP headers, message metadata, async job payloads.
* Span the expensive and the uncertain: database queries, external API calls, cache lookups, queue hops.
* Sample intelligently: keep 100% of errors and slow traces, sample the healthy bulk. Storage is finite; signal density matters.

### 6. Build Dashboards
* One dashboard per critical journey: SLI status, error budget remaining, latency percentiles, and the supporting RED/USE panels.
* Dashboards answer "is it working?" in under 10 seconds. If it takes interpretation, redesign it.
* Link dashboards to runbooks. A graph without a next step is a puzzle during an incident.

### 7. Design Alerts
* Alert on SLO burn and user-facing symptoms. Every alert must be actionable: it names the symptom, links the dashboard, and points at the runbook.
* No alert without an owner and a response expectation. Alerts that nobody owns become alerts everybody ignores.
* Test the alert path: fire it in staging, confirm it pages the right person with the right context.

### 8. Validate: Answer the 3 AM Questions
* Before declaring instrumentation done, answer: "Users report checkout is slow — where do I look?" "Error rate spiked 10 minutes ago — what changed?" "Is this affecting everyone or one region?"
* If any question has no answer in the dashboards, traces, or logs, the instrumentation is incomplete. Go back.

---

## Mandatory Observability Checklist

*Before declaring a service production-ready, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Journeys Mapped:** Are the critical user journeys identified and ranked by impact?
2. **SLOs Defined:** Do key journeys have SLIs, SLO targets with time windows, and defined burn responses?
3. **Structured Logs:** Are logs structured with a consistent schema, trace IDs, and disciplined levels?
4. **RED/USE Covered:** Do request-driven components expose RED and resources expose USE metrics?
5. **Traces Connected:** Does trace context propagate across every service boundary?
6. **Dashboards Answering:** Does each critical journey have a dashboard showing SLI status, error budget, and latency percentiles, linked to runbooks?
7. **Alerts Actionable:** Is every alert symptom-based, owned, linked to a dashboard and runbook — and tested?
8. **Cardinality Bounded:** Are metric labels reviewed for explosion risk, with high-cardinality data in logs/traces?
9. **Sensitive Data Excluded:** Is PII, credential, and token redaction enforced at emission?

---

## Deliverables

Provide a concise report:
* **🗺️ Journeys & SLOs:** Critical journeys with SLIs, targets, and error-budget policy.
* **📝 Logging Schema:** The standard event fields, level discipline, and redaction rules.
* **📊 Metrics Plan:** RED/USE coverage plus business metrics, with cardinality notes.
* **🔗 Tracing Plan:** Boundary propagation and sampling strategy.
* **📈 Dashboards:** What each dashboard answers and its layout.
* **🚨 Alerts:** Each alert's symptom, threshold, owner, dashboard link, and runbook link.
* **✅ Validation:** The 3 AM questions answered, with links to the evidence.
