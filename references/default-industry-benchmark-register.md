# Default Industry Benchmark Register

This is the persistent, pre-approved register the skill falls back to in Mode 2 — when the user hasn't supplied benchmark evidence for a metric, or when a user-supplied claim is weaker than what's already approved here for the same metric.

**Current status: empty of real, approved benchmarks.** This register ships with one synthetic placeholder row only, to demonstrate the format. No real benchmark values have been invented to populate it — that would defeat the purpose of a governed register. Real rows get added here only after a candidate from `benchmark-sourcing-backlog.md` passes the full ten-condition test in `validation-rules.md` and a research lead approves the promotion. See "Keeping the default register and backlog current" in `SKILL.md` for how that promotion path works.

Until real rows are added, Mode 2 has nothing to offer for any metric, and claims with no user-supplied evidence will fall through to the benchmark sourcing backlog and, if nothing is there either, to the exact fallback wording defined in `SKILL.md`.

## Client-facing

_No real, approved rows yet._

## Internal challenge only

_No real, approved rows yet._

## Exclude / placeholder

| ID | Benchmark | Metric | Value | Source | Date | Notes |
|---|---|---|---|---|---|---|
| BM-DEFAULT-000 | [SYNTHETIC PLACEHOLDER] Format demonstration row | Example metric | 00% | [SYNTHETIC - not a real organisation] | — | Synthetic placeholder only. Not a real benchmark. Demonstrates the CSV/Markdown format. Forced to Exclude / Draft-unverified so it can never be read as usable evidence. Remove once the first real row is approved. |

## How to add a real row

1. A candidate benchmark must already exist in `benchmark-sourcing-backlog.md` and must have been checked against the ten-condition test in `validation-rules.md`.
2. If it passes all ten conditions, it's promotion-ready. A research lead reviews the evidence directly (not just the register row) and confirms the promotion.
3. Move the row from the backlog into `default-industry-benchmark-register.csv` and this file, remove it from the backlog, and give it a real sequential `BM-DEFAULT-###` ID.
4. Delete the synthetic placeholder row above once at least one real row exists — it was only ever there to show the format.
