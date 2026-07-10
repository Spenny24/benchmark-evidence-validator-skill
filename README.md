# Benchmark Evidence Validator

A Claude Skill that turns messy benchmark claims into a governed, source-checked register. It is the evidence gate that sits in front of client-facing work — nothing gets called a "benchmark" in a client deliverable unless it passed through here first.

## Why this exists

Transformation and operating-model work leans on benchmark stats constantly: "top-quartile teams cut cycle time by X%," "AI-assisted review saves Y hours." Those numbers get quoted from blog posts, half-remembered reports, and slide decks passed down between projects, and a lot of them can't survive a client asking "where's that from?" This skill forces that question to get answered before the number ever reaches a client, by classifying every claim against a fixed, non-negotiable checklist.

## What it does

Give it raw material — pasted text, a URL, a PDF, a spreadsheet, research notes, vendor claims, industry statistics, ROI figures — and it will:

1. Pull out every distinct benchmark claim in the material
2. Check what can actually be verified against the cited source, versus what's asserted or assumed
3. Rate each claim's evidence quality (`Primary verified`, `Secondary/internal`, `Draft/unverified`)
4. Decide who's allowed to see it (`Client-facing`, `Internal challenge only`, `Exclude`) using a fixed ten-condition test
5. Fall back to a pre-approved default industry register when the user hasn't supplied evidence for a metric, or when the default register is stronger for the same metric
6. Produce a full register plus a record of everything that was excluded and why

## Two modes and source precedence

**Mode 1 — validate what the user supplies.** Standard path: extract claims, check sources, classify.

**Mode 2 — fall back to the default industry register.** Used when the user supplies no evidence for a metric, or supplies something weaker than what's already approved in `references/default-industry-benchmark-register.csv` for that same metric.

For every claim, evidence is gathered in this order, and the strongest candidate found wins the register row:

1. User-provided evidence (only if it passes validation)
2. Default industry benchmark register
3. Benchmark sourcing backlog (never client-facing — research-lead visibility only)
4. Nothing available — the run uses this exact fallback sentence instead of a number:

   > No verified benchmark was provided for this claim. The analysis therefore uses client-owned baseline data only and should be treated as directional until a verified benchmark is supplied.

Precedence decides which candidate is used. It never lowers the bar for classification — everything still goes through the same ten-condition test regardless of which tier it came from.

## No benchmark laundering

A weak claim stays weak no matter how it's worded. "AI saves 90% of content cost" cannot become "AI can significantly reduce content production cost" just because the second version sounds more defensible — the certainty of a claim has to match its evidence, not its prose quality. See `references/validation-rules.md` for the full rule and worked examples.

## What it deliberately does not do

- Does not write client reports, business cases, or decks
- Does not build commercial models, payback calculations, or savings scenarios
- Does not invent a benchmark value, date, source, or scope that wasn't in the supplied material
- Does not let a weak claim become Client-facing just because someone labeled it that way

See `SKILL.md` for the full boundary statement and reasoning.

## How it fits with other skills

This skill is a dependency, not a replacement. The typical flow:

```text
Raw benchmark claims → Benchmark Evidence Validator → benchmark_register.csv
                                                              │
                                  (rows where allowed_use = Client-facing
                                   AND evidence_status = Primary verified)
                                                              ▼
                                        FSOMA Transformation Report Skill
                                        (or any other client-facing deliverable)
```

Being usable and being usable *for a given commercial line* are two different checks. See `references/fsoma-commercial-impact-map.md` for which FSOMA commercial-impact category (AI productivity, cost reduction, spend recovery, tooling rationalisation, revenue enablement, risk and compliance reduction, working-capital / cash impact, or adoption and transformation confidence) each benchmark type can support, and what it explicitly cannot. Downstream skills should read `benchmark_register.csv` and filter on **both** `allowed_use = Client-facing` **and** `evidence_status = Primary verified` before citing any figure externally — the ten-condition test already ties these together for any correctly classified row, but checking both is cheap defense-in-depth against a hand-edited or stale register. Rows marked `Internal challenge only` are for the team's own use — sanity-checking assumptions, not client copy. `Exclude` rows, and anything sitting in the benchmark sourcing backlog, should never be surfaced externally at all.

## Outputs

Every run produces four files:

| File | Purpose |
|---|---|
| `benchmark_register.csv` | Machine-readable register, all rows, for downstream skills and tooling |
| `benchmark_register.md` | Human-readable register, grouped by allowed_use |
| `excluded_benchmarks.md` | Every excluded claim, with the reason, the missing evidence, and what would promote it |
| `validation_notes.md` | Run summary: counts, breakdowns, every case where a user's label was overridden, and an FSOMA commercial-impact mapping table for every register row |

## Repository structure

```text
benchmark-evidence-validator-skill/
├── SKILL.md                                    # The skill definition Claude reads
├── README.md                                   # This file
├── RELEASE_NOTES.md                            # Version history
├── references/
│   ├── benchmark-schema.md                     # Field-by-field schema definition
│   ├── validation-rules.md                     # Ten-condition test, evidence_status logic, no-laundering rule
│   ├── allowed-use-rules.md                    # Client-facing / Internal / Exclude decision logic
│   ├── output-examples.md                      # Worked example of all four outputs, incl. Mode 2 and fallback wording
│   ├── recognised-source-catalogue.md          # Source categories — never automatic proof
│   ├── default-industry-benchmark-register.csv # Persistent Mode 2 register (machine-readable)
│   ├── default-industry-benchmark-register.md  # Persistent Mode 2 register (human-readable)
│   ├── benchmark-sourcing-backlog.md           # Unapproved candidates — research-lead visibility only
│   └── fsoma-commercial-impact-map.md          # Maps benchmarks to FSOMA's 8 commercial-impact categories
└── examples/
    ├── example-benchmark-register.csv
    └── excluded-benchmarks-example.md
```

## Using this skill

Point Claude at raw benchmark material and ask for it to be validated, sourced, or turned into a register — Claude will invoke this skill automatically when the context matches (see the `description` field in `SKILL.md` for the exact triggers). It works standalone; it does not require any other skill or connector to run, though its output is designed to be consumed by report-writing skills like the FSOMA Transformation Report skill.

## Status

v1.2.1 source release. See `RELEASE_NOTES.md` for version history.
