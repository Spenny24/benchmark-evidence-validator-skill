# Release Notes

## v1.2.1 — 2026-07-10

Fixes the benchmark ID convention so excluded claims cannot look like register rows.

- Reserves `BM-###` IDs only for rows present in `benchmark_register.csv`
- Labels excluded and fallback-resolved claims as Claim 1, Claim 2, etc. by original submission order
- Numbers benchmark IDs sequentially among register rows only, not by original claim position
- Updates `SKILL.md`, `references/benchmark-schema.md`, and `references/output-examples.md` to remove the prior convention bug where an excluded example claim was referenced as `BM-003`
- Does not change the fifteen-column register schema and does not change evidence classification rules

## v1.2.0 — 2026-07-10

Adds FSOMA commercial-impact benchmark mapping on top of v1.1.0's evidence-and-precedence model. This does not change how a benchmark is classified as usable — it adds a second, separate check for what a usable benchmark is allowed to support once it reaches FSOMA work.

- Adds `references/fsoma-commercial-impact-map.md` — maps benchmark metrics to the eight FSOMA commercial-impact categories (AI productivity, cost reduction, spend recovery, tooling rationalisation, revenue enablement, risk and compliance reduction, working-capital / cash impact, adoption and transformation confidence), each with the benchmark types that belong there, what commercial case it can and cannot support, the required client baseline, and misuse examples
- Adds the FSOMA commercial-impact compatibility rule to `SKILL.md`: a benchmark being verified and usable does not mean it can be used in any commercial-model line — it can only be used in the category it was mapped to. A productivity benchmark cannot support a revenue line; a SaaS-waste benchmark cannot support a labour-productivity line, and so on
- Expands `validation_notes.md` required content: every run now includes an "FSOMA commercial-impact mapping" section with columns `Benchmark`, `FSOMA category`, `Can support`, `Cannot support`, `Required client baseline`, covering every row in the register (excluded and fallback-wording claims are never given a category, since they never entered the register)
- Does not change the fifteen-column register schema — no columns were added to `benchmark_register.csv` or `default-industry-benchmark-register.csv`
- Does not invent, source, or promote any benchmark — this release is a classification-of-use layer only, applied after the existing evidence checks already in place

## v1.1.0 — 2026-07-10

Adds the two-mode evidence model and the persistent register/backlog system on top of v1.0.0's single-run validator.

- Adds Mode 2: fall back to a persistent, pre-approved default industry benchmark register (`references/default-industry-benchmark-register.csv` / `.md`) when the user supplies no evidence for a metric, or supplies something weaker than what's already approved for that metric
- Adds the four-tier benchmark source precedence: user-supplied evidence, then default industry register, then benchmark sourcing backlog, then no benchmark available
- Adds `references/benchmark-sourcing-backlog.md` — a persistent, research-lead-only tracker for candidate benchmarks that haven't passed the ten-condition test, with a defined promotion path into the default register
- Adds `references/recognised-source-catalogue.md` — the eight recognised source categories, with an explicit no-automatic-promotion rule: a recognised or reputable source category never on its own justifies Client-facing status
- Adds the no-benchmark-laundering rule: rewriting a weak claim in more polished or hedged language must never upgrade its evidence_status or allowed_use classification
- Adds the exact fallback sentence to use in client deliverables when no verified benchmark exists for a claim, with an instruction to use it verbatim
- Expands `validation_notes.md` required content: source precedence applied per claim, whether the default register was used, and whether any backlog candidates were found but excluded from client-facing use
- Clarifies downstream consumption: FSOMA and other client-facing skills should filter on both `allowed_use = Client-facing` and `evidence_status = Primary verified`, not just one, as defense-in-depth
- States explicitly that this skill never silently edits the persistent default register or backlog files — updates are proposed in `validation_notes.md` for a human to action
- Ships the default register with one synthetic placeholder row only, structurally forced to `Exclude` / `Draft/unverified` so it can never be mistaken for real, usable evidence; no real benchmark values were invented to populate it
- Updates `examples/` and `references/output-examples.md` with a worked Mode 2 example and a worked fallback-wording example

## v1.0.0 — 2026-07-10

Initial release.

- Defines the fifteen-field benchmark register schema (`references/benchmark-schema.md`)
- Establishes the three evidence_status values: Primary verified, Secondary/internal, Draft/unverified
- Establishes the three allowed_use values: Client-facing, Internal challenge only, Exclude
- Implements the ten-condition Client-facing test (`references/validation-rules.md`) — a benchmark can only be marked Client-facing when all ten conditions hold, with no exceptions for user-supplied labels
- Establishes conservative-by-default handling for secondary sources (blog posts summarizing reports) and repeated-but-unsourced values
- Defines the four required outputs: `benchmark_register.csv`, `benchmark_register.md`, `excluded_benchmarks.md`, `validation_notes.md`
- Adds worked example outputs (`references/output-examples.md`) and a full synthetic example run (`examples/`)
- Explicit boundary: this skill does not write client reports, does not build commercial models, and does not invent benchmark data. It is a dependency for the FSOMA Transformation Report skill, not a substitute for it.

## Planned / open questions

- No test cases or evals have been run yet. Next step is drafting 2-3 realistic test prompts and reviewing outputs before this skill is considered validated end-to-end.
- Description trigger-accuracy optimization has not been run.
- The default industry benchmark register holds zero real, approved benchmarks as of v1.1.0 — Mode 2 currently has nothing to offer until real rows are sourced, validated, and promoted from the backlog.
- The mechanics for "who actually reviews and approves a backlog promotion" are specified as a manual, human-owned step but no concrete process (cadence, owner, tooling) has been defined yet — this is a v1.1.0 assumption worth confirming.
