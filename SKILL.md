---
name: benchmark-evidence-validator
description: Validates and classifies benchmark evidence (research notes, URLs, PDFs, reports, spreadsheets, pasted stats, vendor claims, industry statistics, ROI figures) into a governed benchmark register with a strict evidence_status and allowed_use rating on every row. Falls back to a preloaded default industry benchmark register when the user supplies no evidence, or when the default register is stronger for the same metric. Use whenever the user supplies benchmark data, industry statistics, KPI claims, or ROI figures and wants them checked, sourced, scored, or registered, including when they need a benchmark but have not supplied one. Always run before benchmark figures reach the FSOMA Transformation Report skill, a business case, a client deck, or any other client-facing deliverable, since this is the evidence gate deciding Client-facing, Internal challenge only, or Exclude. Do not use this skill to write the client report, size a commercial model, or invent benchmark values.
---

# Benchmark Evidence Validator

## Purpose

Turn messy benchmark claims, research notes, URLs, PDFs, reports, spreadsheets, pasted stats, vendor claims, industry statistics, and ROI figures into a governed register: a table where every row has a traceable source and an honest rating of how far it can be trusted. This skill exists because bad benchmarks are worse than no benchmarks — a wrong "40% faster" stat quoted to a client damages credibility more than admitting the number isn't verified yet. This skill is the checkpoint that catches that before it happens.

The register this skill produces is a dependency for the FSOMA Transformation Report skill and any other client-facing deliverable. Those skills should only ever pull rows where `allowed_use` is `Client-facing` **and** `evidence_status` is `Primary verified` — see "Downstream compatibility" below for why both are checked even though condition 10 already ties them together. Everything else stays internal until someone does the work to verify it.

This skill is an evidence gate, not a research assistant that goes and finds benchmarks on its own initiative. It validates what's supplied, checks it against a maintained default register when nothing is supplied, and is honest about what's genuinely unverified. It does not go trawling the internet for numbers to fill gaps.

## Boundaries — read this first

This skill has one job: take evidence in, hand a classified register out. It does not:

- **Write client reports.** No business cases, no leadership decks, no narrative sections. That is the FSOMA Transformation Report skill's job, and it should only consume `benchmark_register.csv` rows marked `Client-facing`.
- **Build commercial models.** No payback periods, no savings scenarios, no pricing. Benchmarks feed those models elsewhere; this skill doesn't build them.
- **Invent benchmarks.** If a value, date, title, URL, or scope isn't in the material the user supplied, it does not go in the register. A blank field beats a guessed one every time — a guessed field looks like evidence and isn't.
- **Promote weak claims to Client-facing.** Every row gets tested against the same ten conditions in `references/validation-rules.md`, no exceptions, including when the user asks for one. If the user labels a row Client-facing and it fails the test, the skill overrides the label and says why. This is the whole point of the skill — soften this and the register stops meaning anything.

If a request would violate one of these (e.g. "just write me the report" or "give me a number even if you can't find the source"), say so plainly and redirect to what this skill can actually do.

## Two modes

**Mode 1 — validate supplied evidence.** The user hands over benchmark claims, a URL, a PDF, a spreadsheet, or research notes. Extract, verify, and classify as described in the Workflow section below.

**Mode 2 — fall back to the default industry register.** The user needs a benchmark for a metric but hasn't supplied one, or has supplied something weaker than what's already sitting in `references/default-industry-benchmark-register.csv` for that same metric. In this case, use the default register's row instead of leaving the claim unsupported — but never instead of a user-supplied row that's already stronger.

These aren't modes the user picks — the skill works out which applies per claim, using the precedence order below.

### Benchmark source precedence

For every individual claim or metric being evaluated, gather candidates in this order and prefer the strongest one found, not just the first one found:

1. **User-provided benchmark register or source evidence** — preferred only when it passes validation. Supplying evidence doesn't guarantee it wins; it still has to earn its evidence_status.
2. **Default industry benchmark register** (`references/default-industry-benchmark-register.csv`) — used when the user supplied nothing for this metric, or when the default register's entry for the same metric carries a stronger evidence_status/allowed_use than what the user supplied.
3. **Benchmark sourcing backlog** (`references/benchmark-sourcing-backlog.md`) — candidates here are known-unverified by definition (that's why they're in the backlog and not the register). A matching backlog entry is never written into `benchmark_register.csv` as a usable row and is never Client-facing. Note it in `validation_notes.md` for research-lead visibility only.
4. **No benchmark available** — nothing at any tier covers this claim. Use the fallback wording exactly (see below).

Precedence decides *which* candidate becomes the register row when more than one exists for the same metric. It never lowers the classification bar — a user-supplied claim that reaches the top of the precedence order because nothing else exists still gets the full ten-condition test, and can still land as `Internal challenge only` or `Exclude`.

When both a user-supplied claim and a default-register entry exist for the same metric, don't silently discard the one you didn't select — record the comparison in `validation_notes.md`: both entries, which was preferred, and why.

## No benchmark laundering

Never convert a weak claim into a stronger-sounding one by rewriting it in more polished language. The classification has to track the evidence, not the prose quality of the writeup — a professionally worded version of an unsourced claim is still an unsourced claim.

- "AI saves 90% of content cost" must not become "AI can significantly reduce content production cost" — the softened wording makes it *sound* more credible while hiding that no source backs the number at all.
- "Customers see 10x ROI" must not become "ROI can be material in early deployment" — this launders a vendor marketing claim into something that reads like an independent finding.
- "Common knowledge" must not become "industry consensus" — dressing up "everyone says so" as consensus implies a body of evidence that was never checked.

`benchmark_name` and `notes` can be tidied for readability, but must not upgrade the claim's certainty. When a row's evidence is weak, `notes` should reflect the original wording or its substance plainly enough that a reader can see exactly how thin the evidence is — the register's job is to preserve the weakness, not smooth it over.

## Source checking rule

If the user supplies a public URL or a source document, inspect it directly wherever possible — don't classify from the claim text alone when the actual source is one click away. If the primary source isn't supplied or can't be accessed, do not assume a secondary summary of it is accurate. Classify conservatively as `Secondary/internal` or lower until the primary can actually be checked.

## Fallback wording

When no verified benchmark exists for a claim — nothing from the user, the default register, or even a research-worthy backlog candidate — say so using this exact sentence, unedited:

> No verified benchmark was provided for this claim. The analysis therefore uses client-owned baseline data only and should be treated as directional until a verified benchmark is supplied.

Use it verbatim, not paraphrased. It's designed to be pasted directly into a client deliverable in place of an unverified number, so consistent wording matters more than variety here.

## The register schema

Every row in the output uses exactly these fifteen fields, in this order:

`benchmark_id, benchmark_name, metric, value_or_range, unit, source_title, source_organisation, source_url, publication_date, industry_scope, functional_scope, geography_scope, allowed_use, evidence_status, notes`

Field definitions, examples, and formatting conventions (ID scheme, date format, how to write a range vs. a single value) are in `references/benchmark-schema.md`. Read it before generating the first row of a new register — the ID scheme and date format need to be consistent across the whole register, and it's cheaper to get that right from the start than to renumber later.

## Benchmark ID scope

`benchmark_id` values (`BM-###`) are reserved only for rows that actually enter `benchmark_register.csv`.

Do not assign `BM-###` IDs to claims that are excluded outright, resolved by fallback wording only, or otherwise never enter the register. Excluded and fallback-resolved claims are labelled `Claim 1`, `Claim 2`, etc. by original submission order, and those claim labels are used consistently across `excluded_benchmarks.md` and `validation_notes.md`.

Number `BM-###` IDs sequentially among register rows only, not by the claim's original position in the user's input. Example: if Claim 2 is the only claim that enters `benchmark_register.csv`, it becomes `BM-001`. Claim 1 and Claim 3 remain Claim 1 and Claim 3 in `excluded_benchmarks.md` and `validation_notes.md`.

A downgraded claim still gets a benchmark ID if it enters the register. For example, a user-supplied claim labelled "Client-facing" that fails the ten-condition test but is still useful as `Internal challenge only` belongs in `benchmark_register.csv`, and therefore receives the next sequential `BM-###` ID. The ID is withheld only from claims that do not enter the CSV at all.

## Evidence status — how sure are we this is real?

Three values only:

- **Primary verified** — the claim was checked against the original source (the actual report, dataset, or filing), not a summary of it.
- **Secondary/internal** — the claim comes from something that references a primary source (a blog post, a vendor one-pager, an internal deck) but the primary source itself wasn't available to check against.
- **Draft/unverified** — the claim showed up somewhere (often repeated across several sites) but no source, primary or secondary, could be traced at all.

## Allowed use — who can see this?

- **Client-facing** — safe to put in front of a client. Only assigned when all ten conditions in `references/validation-rules.md` are true.
- **Internal challenge only** — good enough to sanity-check assumptions or stress-test a client's numbers internally, not good enough to cite to them.
- **Exclude** — doesn't clear the bar for internal use either. Kept in the record so nobody re-researches it later, but out of the working register.

Full decision logic, including how to handle a source that's a summary of another report, is in `references/allowed-use-rules.md`.

**No automatic promotion.** A source being a recognised category — primary consulting-firm research, analyst-firm research, an industry body, an academic study, a regulator, disclosed-methodology vendor research, and so on — is never on its own a reason to mark something Client-facing. A McKinsey report with an exact metric, value, date, URL, and scope can be `Primary verified`. A blog post that says "McKinsey says..." without linking the original report cannot, no matter how reputable McKinsey is — the brand name on the citation doesn't substitute for actually checking the source. See `references/recognised-source-catalogue.md` for the full category list and worked examples.

## The ten-condition Client-facing rule

A row can only be marked `Client-facing` when **all ten** of these are true:

1. `source_organisation` is present
2. `source_title` is present
3. `source_url` is present
4. `publication_date` is present
5. `metric` is present
6. `value_or_range` is present
7. `unit` is present
8. `functional_scope` is present
9. `allowed_use` is Client-facing
10. `evidence_status` is Primary verified

Miss one, and the row is not Client-facing — no partial credit, no "it's close enough." This is a checklist, not a judgment call, precisely so it can't be talked around. Walk every candidate row through it explicitly and record which condition(s) failed when it doesn't pass; that record is what goes into `validation_notes.md`.

## Working conservatively by default

Two situations come up constantly and the default is always the cautious read:

- **A source that summarizes another report.** A blog post, newsletter, or vendor page citing "According to [Report Name]..." is secondary evidence, even if it states the number confidently. Classify it `Secondary/internal` unless the user also supplies the primary report to check the number against. Do not go fetch the primary report yourself and assume it says what the summary claims — if it isn't supplied, it isn't verified.
- **A value repeated across multiple sites with no traceable origin.** Repetition is not verification — it usually means everyone is quoting the same unverified original. Classify as `Draft/unverified` if there's at least a plausible originating claim to point to, or `Exclude` if the number can't be tied to any specific source at all.

When in doubt between two classifications, take the more conservative one and explain the reasoning in `notes`.

## Workflow

1. **Intake.** Take whatever the user supplies — pasted text, a URL, a PDF, a spreadsheet, a research note — and identify every distinct benchmark claim in it (a claim is a specific metric + value tied to some source, even a weak one). Also note any metric the user explicitly needs a benchmark for but didn't supply evidence on. Keep the original claim order so excluded or fallback-resolved claims can be labelled consistently as Claim 1, Claim 2, etc.
2. **Gather candidates per claim** in precedence order: user-supplied evidence, then `references/default-industry-benchmark-register.csv` for the same metric, then `references/benchmark-sourcing-backlog.md`, then nothing.
3. **Extract candidate rows.** For each candidate, populate as much of the schema as the source material actually supports. Leave fields blank rather than inferring them.
4. **Verify against source.** For every populated field, check it's actually attributable to the cited source and not assumed. This is where most of the downgrading happens — a number attributed to "industry research" with no organisation named cannot be Primary verified no matter how solid the number seems. Follow the source checking rule above for any supplied URL or document.
5. **Classify evidence_status** using the definitions above, and apply the no-benchmark-laundering rule — the wording can be tidied, the certainty cannot be upgraded.
6. **Run the ten-condition test** to set `allowed_use`. If the user pre-labeled a row (e.g. "mark this one Client-facing"), still run the test. If it fails, override the label, keep their original label out of the final field, and log the override with the missing conditions in `notes` and in `validation_notes.md`.
7. **Resolve precedence** where more than one candidate exists for the same metric — select the stronger one for the register row, and record the comparison in `validation_notes.md` rather than silently dropping the weaker candidate.
8. **Apply fallback wording** for any claim where nothing at any precedence tier produced a usable row.
9. **Assign benchmark IDs only to rows entering the register.** Number `BM-###` sequentially across `benchmark_register.csv` rows only. Claims that do not enter the register keep their original claim labels (Claim 1, Claim 2, etc.) and never receive a benchmark_id.
10. **Produce all four required outputs** (see below) — never fewer, even when the register has zero Client-facing rows.
11. **Propose (don't silently apply) reference-file updates** — see "Keeping the default register and backlog current" below.

## Required outputs

Every run produces exactly four files:

1. **`benchmark_register.csv`** — every row that made it into the register (any allowed_use value), using the schema headers exactly as given above, comma-separated, one header row, no columns added, removed, renamed, or reordered, and no empty column omitted. `BM-###` IDs appear only here and only for rows actually present in this file.
2. **`benchmark_register.md`** — the same data as a readable Markdown table, grouped by `allowed_use` (Client-facing first, then Internal challenge only, then Exclude) so a reader can see at a glance what's usable where.
3. **`excluded_benchmarks.md`** — one entry per excluded or downgraded claim, each with: the original claim as the user or source stated it, the reason it was excluded or downgraded, the specific missing fields or failed ten-condition checks, and what evidence would need to be supplied to promote it. Any claim resolved by the fallback wording gets an entry here too, with the exact fallback sentence included. Claims that never entered the register are labelled Claim 1, Claim 2, etc. by original submission order, not by `BM-###` ID. Downgraded claims that still entered the register may be referenced by their `BM-###` ID.
4. **`validation_notes.md`** — a methodology note covering: total claims reviewed, rows produced, the evidence_status breakdown, the allowed_use breakdown, every case where a user-supplied label was overridden, the source precedence applied per claim (and why), whether the default industry register was used, whether any backlog candidates were found relevant but excluded from client-facing use, the ID convention applied (BM IDs only for register rows, Claim labels for excluded/fallback-only claims), and (every run, not only FSOMA-specific ones) an **FSOMA commercial-impact mapping** section with the columns `Benchmark`, `FSOMA category`, `Can support`, `Cannot support`, `Required client baseline` for every register row, using `references/fsoma-commercial-impact-map.md` to assign the category. See `references/output-examples.md` for a worked example of this table.

Exact formatting and a worked example of all four files together are in `references/output-examples.md` — use it as the template rather than inventing a new layout each time.

## Keeping the default register and backlog current

`references/default-industry-benchmark-register.csv` and `references/benchmark-sourcing-backlog.md` are persistent files, not per-run outputs — they carry forward between runs, and the FSOMA Transformation Report skill (and others) may read the default register directly. Because other work depends on them, this skill never edits them silently.

Instead, at the end of a run:

- **New candidates worth tracking** (claims with some evidence trail but not enough to clear even a confident classification yet) get proposed as backlog additions — surface them clearly in `validation_notes.md` as a suggested addition, in the backlog's field format, for a research lead to actually add.
- **Backlog rows that now pass all ten conditions** (because the run supplied the missing evidence) get flagged as promotion-ready in `validation_notes.md`, with the row shown in full. Promoting it into the default register is a deliberate action for a human to take, not something this skill does automatically mid-run.

This keeps the default register trustworthy — every row in it got there because someone looked at the evidence and agreed, not because a run quietly appended something.

## Downstream compatibility

`benchmark_register.csv` is the interface the FSOMA Transformation Report skill and other client-facing skills consume. They should filter on **both** `allowed_use = Client-facing` **and** `evidence_status = Primary verified` before citing a row externally — not just one or the other. Condition 9 and condition 10 of the ten-condition test already tie these together for any row correctly classified as Client-facing, so this is redundant in the normal case; it's specified anyway as a defense-in-depth check, in case a row is ever read from an older register version or edited by hand outside this skill's validation path.

## FSOMA commercial-impact compatibility rule

Being usable is not the same as being usable *everywhere*. Once a benchmark clears the ten-condition test, a second question applies whenever the benchmark is headed into FSOMA work: what type of commercial model can this specific benchmark support?

When a benchmark is intended for FSOMA, classify not only whether it is usable (the existing `evidence_status` / `allowed_use` classification), but which FSOMA commercial-impact category it belongs to, using `references/fsoma-commercial-impact-map.md`. Do not use a benchmark outside its commercial-impact category, even when the underlying evidence is strong — evidence quality and category fit are two separate checks, and both have to pass.

Examples:
- A productivity benchmark can support Category 1 (AI productivity), but not revenue growth.
- A SaaS waste benchmark can support tooling rationalisation, but not labour productivity.
- A conversion uplift benchmark can support revenue enablement, but only where the client has a revenue-linked KPI and an attribution rule already in place.
- A compliance rework benchmark can support risk or avoided-cost modelling, but not revenue, unless a revenue impact is directly and separately evidenced.

This rule does not change the fifteen-column register schema — no columns are added to `benchmark_register.csv` for this. The category mapping lives in `validation_notes.md` (see the "FSOMA commercial-impact mapping" requirement under Required outputs) and in `references/fsoma-commercial-impact-map.md`, not in the register itself. It does not invent benchmarks and does not promote anything from the sourcing backlog or the default register — it only decides which commercial case an already-classified benchmark is honest to support.

## Reference files

- `references/benchmark-schema.md` — field-by-field definitions, ID scheme, formatting conventions.
- `references/validation-rules.md` — the ten-condition test in full, evidence_status decision tree, no-benchmark-laundering rule, override-handling detail.
- `references/allowed-use-rules.md` — Client-facing vs. Internal challenge only vs. Exclude decision logic, including the summarized-source and repeated-value cases.
- `references/output-examples.md` — a complete worked example of all four output files, including a Mode 2 example and a fallback-wording example.
- `references/recognised-source-catalogue.md` — the recognised source categories and why none of them grant automatic Client-facing status.
- `references/default-industry-benchmark-register.csv` / `.md` — the persistent, pre-approved benchmark register used in Mode 2.
- `references/benchmark-sourcing-backlog.md` — candidate benchmarks not yet approved for client-facing use; research-lead visibility only.
- `references/fsoma-commercial-impact-map.md` — maps benchmark metrics to the eight FSOMA commercial-impact categories: what belongs in each, what commercial case it can and cannot support, the required client baseline, and misuse examples.

See also `examples/example-benchmark-register.csv` and `examples/excluded-benchmarks-example.md` for a full end-to-end illustration using synthetic data.
