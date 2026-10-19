---
name: database-migration-strategist
description: >-
  Safe database schema and data migration workflow. Classify the change,
  design a zero-downtime expand/migrate/contract plan, write reversible
  batched migrations, plan the rollback, and verify on production-like data.
  Use when changing table schemas, backfilling data, renaming columns, or
  moving data between tables or systems.
---

# Database Migration Strategist Guide

## Core Migration Principles
1. **Zero Downtime by Default:** Production keeps serving while you migrate. Long exclusive locks and breaking schema changes are not acceptable.
2. **Expand, Migrate, Contract:** Add the new shape alongside the old, move the data, switch the readers, then remove the old. Never jump straight to the final state.
3. **Backfills Are Batched and Idempotent:** Large data moves run in small batches, can be re-run safely, and report progress. A backfill that cannot resume is a liability.
4. **Every Migration Has a Rollback:** If you cannot say how to undo it, you are not ready to run it.
5. **Test on Production-Like Data:** Row counts, skew, and lock behavior on real-shaped data — not an empty dev database.

---

## Execution Workflow

### 1. Classify the Change
* **Additive** (new table/column/index): safest; usually a single migration.
* **Destructive** (drop/rename/type change): requires the expand/contract sequence.
* **Data** (backfill, transformation): needs batching, idempotency, and progress tracking.
* **Online constraint/index changes:** plan for lock behavior (`CONCURRENTLY` where supported, statement timeouts).

### 2. Design the Multi-Phase Plan
* **Phase 1 — Expand:** Add new columns/tables alongside old ones. Deploy code that writes to both but reads from the old.
* **Phase 2 — Migrate:** Backfill existing rows in batches. Verify counts and checksums between old and new.
* **Phase 3 — Contract:** Switch reads to the new shape, deploy, then remove the old columns/tables in a later release.
* State the deploy order explicitly: which code version runs against which schema at each step.

### 3. Write the Migration
* Keep each migration reversible (down migration or a documented manual undo).
* Batch data changes (e.g., 1k–10k rows per batch), with `ORDER BY` primary key and a resumable cursor.
* Set statement timeouts; avoid `LOCK TABLE`; prefer online index builds.
* Make the migration re-runnable: guard with `IF NOT EXISTS` / idempotent updates.

### 4. Plan the Rollback
* For each phase, state the exact undo: which migration to roll back, which deploy to revert, and how long each takes.
* Identify the point of no return (e.g., after old columns are dropped) and call it out.

### 5. Verify
* Dry-run on a production-like snapshot: timing, lock waits, row counts before/after.
* Define post-deploy checks: row counts, application error rates, slow-query watch.

---

## Mandatory Migration Checklist

*Before handing over the plan, evaluate and state "Yes", "No", or "N/A" for each:*

1. **Zero Downtime:** Can every phase run while the app serves traffic?
2. **Phased:** Is the expand/migrate/contract sequence explicit, with deploy order stated?
3. **Reversible:** Does each phase have a tested or documented rollback?
4. **Batched:** Are data moves batched, idempotent, and resumable?
5. **Verified:** Was the plan dry-run against production-like data with timing and lock impact measured?

---

## Deliverables

* **🗺️ Plan:** Phased migration sequence with deploy order.
* **📜 Migrations:** The migration scripts (up and down).
* **↩️ Rollback:** Undo procedure per phase, including the point of no return.
* **✅ Verification:** Dry-run results, row-count checks, and post-deploy monitoring to watch.
