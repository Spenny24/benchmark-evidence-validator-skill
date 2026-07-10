# Output Examples

A compact worked example of all four output files, built from three input claims. Use this as the template for structure and tone — the actual content of any real run will obviously differ.

This example covers Mode 1 only (user-supplied evidence). See "Mode 2 and fallback wording" at the end of this file for how the default register, the backlog, and the exact fallback sentence show up in outputs when the user hasn't supplied evidence for a claim.

## Input (what the user supplied)

1. "Forrester's 2023 Total Economic Impact study on content ops platforms found a 32% reduction in content production cycle time." (user supplied a link to the actual Forrester study)
2. "Marketing teams using AI-assisted review cut approval time by half." (from a vendor blog post, no link to a primary study, no date given in the post itself, dated the post itself March 2024)
3. "Everyone says content reuse rates go up 3x with a DAM." (no source at all)

## 1. benchmark_register.csv

```csv
benchmark_id,benchmark_name,metric,value_or_range,unit,source_title,source_organisation,source_url,publication_date,industry_scope,functional_scope,geography_scope,allowed_use,evidence_status,notes
BM-001,Content production cycle time reduction,Cycle time reduction,32,%,The Total Economic Impact of Content Operations Platforms,Forrester,https://example.com/forrester-tei-content-ops-2023,2023,Cross-industry,Content production,Global,Client-facing,Primary verified,Verified directly against the linked Forrester TEI study; all ten conditions met.
BM-002,AI-assisted approval time reduction,Approval time reduction,50,%,[vendor blog post - no primary study linked],VendorCo,https://example.com/vendorco-blog-ai-review,2024-03,Marketing,Creative review,Not stated,Internal challenge only,Secondary/internal,Vendor blog cites no primary study or original data source; treat as a directional claim only until the underlying research (if any) is supplied.
```

Notes on this example:
- Claim 3 (the DAM reuse claim) does **not** appear here — it failed step 1 of the allowed-use decision logic (no traceable source at all) and lives only in `excluded_benchmarks.md`.
- `BM-002`'s `source_title` is filled with a bracketed description rather than left blank, because the blog post itself has a title-like element but no named underlying study — this is a judgment call to document what exists rather than imply nothing does. When in doubt, prefer a bracketed clarifying note over leaving the field empty and unexplained.

## 2. benchmark_register.md

```markdown
# Benchmark Register

## Client-facing

| ID | Benchmark | Metric | Value | Source | Date |
|---|---|---|---|---|---|
| BM-001 | Content production cycle time reduction | Cycle time reduction | 32% | Forrester — The Total Economic Impact of Content Operations Platforms | 2023 |

## Internal challenge only

| ID | Benchmark | Metric | Value | Source | Date | Why not Client-facing |
|---|---|---|---|---|---|---|
| BM-002 | AI-assisted approval time reduction | Approval time reduction | 50% | VendorCo blog (no primary study linked) | 2024-03 | Fails condition 10: evidence_status is Secondary/internal, not Primary verified. |

## Exclude

_No rows excluded to Internal-challenge-eligible status this run — see excluded_benchmarks.md for fully excluded claims._
```

## 3. excluded_benchmarks.md

```markdown
# Excluded Benchmarks

## Claim 3 — "Content reuse rates go up 3x with a DAM"

**Original claim:** "Everyone says content reuse rates go up 3x with a DAM."

**Reason excluded:** No source, primary or secondary, could be identified. The claim was presented as general industry consensus with no originating study, vendor report, or named organisation.

**Missing fields:** source_organisation, source_title, source_url, publication_date, and evidence basis for value_or_range (3x) — effectively the entire evidence trail.

**What would promote this:** Any named source — a study, a vendor case study, an analyst report — that states this figure and can be linked to. Even a secondary source (a blog citing a named report) would move this from Exclude to Draft/unverified or Internal challenge only; a primary source with all required fields would make it eligible for Client-facing.
```

## 4. validation_notes.md

```markdown
# Validation Notes

**Run date:** 2024-06-01
**ID convention:** benchmark_id (BM-###) assigned only to rows in benchmark_register.csv, numbered sequentially among register rows only. Excluded claims are labelled Claim 1, Claim 2, etc. by original submission order and never receive a benchmark_id.
**Claims reviewed:** 3
**Rows in register:** 2 (1 Client-facing, 1 Internal challenge only)
**Rows excluded:** 1

## Evidence status breakdown
- Primary verified: 1
- Secondary/internal: 1
- Draft/unverified: 0

## Allowed use breakdown
- Client-facing: 1
- Internal challenge only: 1
- Exclude: 1

## Overrides
None this run — no claim arrived with a pre-existing "Client-facing" label from the user.

## Source precedence applied
All three claims resolved from user-supplied evidence (Mode 1). Default industry register and backlog were not consulted, since the user supplied evidence for every claim in scope.

## Default industry register used
No.

## Backlog candidates found but excluded from client-facing use
None this run.

## Notes for follow-up
BM-002 could be promoted to Client-facing if the user supplies the primary research (if any) underlying the VendorCo blog claim, and the post's own publication date is confirmed against the actual study date rather than the blog's post date.

## FSOMA commercial-impact mapping
| Benchmark | FSOMA category | Can support | Cannot support | Required client baseline |
|---|---|---|---|---|
| BM-001 - Content production cycle time reduction | AI productivity | Labour productivity / capacity uplift narrative in the commercial model | Revenue growth; cost reduction (unless headcount or budget is confirmed to shrink); cash-flow timing | Client's own current cycle time for content production, same process scope as the benchmark |
| BM-002 - AI-assisted approval time reduction | AI productivity | Internal sanity-check of productivity assumptions only - not usable in a client-facing commercial line, since the row is Internal challenge only / Secondary/internal | Any client-facing commercial case line, in any category, until evidence_status reaches Primary verified | Client's own current approval-cycle time for creative review, same process scope as the benchmark |

Claim 3 (DAM reuse claim) does not appear in this table - it never entered the register, so there is nothing to map to a commercial category. Excluded and fallback-wording claims are never given an FSOMA category; category mapping only applies to rows that already exist in `benchmark_register.csv`.
```

## Mode 2 and fallback wording

Continuing the same run, suppose the user also asked for a benchmark on a fourth metric — approval-cycle rework rate — but supplied no evidence for it.

**Step 1 — check the default register.** `references/default-industry-benchmark-register.csv` is checked for a `Rework rate` row in the same functional scope. Two outcomes are possible:

**Outcome A — the default register has something.** Say it holds `BM-DEFAULT-004`, a Primary verified, Client-facing row on rework rate from a recognised analyst firm. That row is pulled directly into `benchmark_register.csv` for this run (reusing its `benchmark_id` or assigning a new sequential one per the register being built — follow the ID scheme in `benchmark-schema.md`), and `validation_notes.md` records:

```markdown
## Source precedence applied
- Claim 1 — Content production cycle time: user-supplied, Primary verified (BM-001)
- Claim 2 — AI-assisted approval time: user-supplied, Secondary/internal (BM-002)
- Claim 3 — Content reuse claim: user-supplied, no source (excluded)
- Claim 4 — Approval-cycle rework rate: no user evidence supplied — pulled from default industry register (BM-DEFAULT-004)

## Default industry register used
Yes — one row (rework rate) sourced from the default register in the absence of user-supplied evidence.
```

**Outcome B — the default register has nothing either.** The backlog is checked next. If a backlog candidate touches this metric, it is *not* pulled into the register — it's noted for research-lead visibility only:

```markdown
## Backlog candidates found but excluded from client-facing use
- Claim 4 — Approval-cycle rework rate: one related backlog candidate found ("rework rate drops ~20% with structured review checklists," possible source: unnamed operations blog, status: Secondary/internal, not promotable). Not used in this register. Flagged for the research lead — see benchmark-sourcing-backlog.md.
```

And in `excluded_benchmarks.md`, the claim gets an entry using the exact fallback wording:

```markdown
## Claim 4 — Approval-cycle rework rate

**Original claim:** User requested a benchmark for approval-cycle rework rate; none was supplied.

**Reason excluded:** No verified benchmark exists in user-supplied evidence, the default industry register, or as a promotable backlog candidate.

**Missing fields:** All fields — no evidence was available at any precedence tier for this specific metric.

**What would promote this:** A primary, dated, scoped source for approval-cycle rework rate, from the user or added to the default register.

**Fallback wording for client deliverables:**

> No verified benchmark was provided for this claim. The analysis therefore uses client-owned baseline data only and should be treated as directional until a verified benchmark is supplied.
```
