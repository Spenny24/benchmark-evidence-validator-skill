# Benchmark Register Schema

Every row produced by this skill uses exactly these fifteen fields. Do not add, remove, or rename fields — downstream skills (including the FSOMA Transformation Report skill) key off this exact header row.

## Field reference

| # | Field | Required for Client-facing | Description |
|---|-------|:---:|---|
| 1 | `benchmark_id` | — | Unique identifier for the row. See ID scheme below. |
| 2 | `benchmark_name` | — | Short human-readable label, e.g. "Content production cycle time reduction." Not part of the ten-condition test, but always populate it — an unnamed row is hard to audit later. |
| 3 | `metric` | Yes | What is being measured, e.g. "Cycle time," "Cost per asset," "Rework rate." |
| 4 | `value_or_range` | Yes | The number itself. A single value ("32%") or a range ("20–35%"). See formatting below. |
| 5 | `unit` | Yes | The unit the value is expressed in: "%", "days", "$ per asset," "hours/week," etc. |
| 6 | `source_title` | Yes | The exact title of the report, article, or document the claim comes from. |
| 7 | `source_organisation` | Yes | Who published it — the organisation, not an individual author unless the claim is personally attributed (e.g. a named analyst's LinkedIn post). |
| 8 | `source_url` | Yes | A working link to the source. If the source is a PDF or file with no public URL, note that in `notes` and leave this blank — a blank `source_url` fails condition 3 and the row cannot be Client-facing, which is correct: an unlinkable source can't be independently checked by a client. |
| 9 | `publication_date` | Yes | When the source was published, `YYYY-MM-DD` where the source specifies a full date, `YYYY-MM` or `YYYY` where it only specifies a month or year. Do not back-fill a specific date the source doesn't give. |
| 10 | `industry_scope` | — | What industry or sector the benchmark applies to. Not part of the ten-condition test, but leaving it blank should push `functional_scope` scrutiny higher — a benchmark with no stated scope at all is a signal to look harder before trusting it. |
| 11 | `functional_scope` | Yes | What function or process area the benchmark applies to, e.g. "Content production," "Creative review," "Campaign approval." |
| 12 | `geography_scope` | — | Geographic applicability, e.g. "Global," "North America," "UK only." Not part of the ten-condition test, but always populate if the source states it. |
| 13 | `allowed_use` | Yes (must equal Client-facing) | One of: `Client-facing`, `Internal challenge only`, `Exclude`. See `allowed-use-rules.md`. |
| 14 | `evidence_status` | Yes (must equal Primary verified) | One of: `Primary verified`, `Secondary/internal`, `Draft/unverified`. See `validation-rules.md`. |
| 15 | `notes` | — | Free text: caveats, why a condition failed, what would need to change to promote the row, anything a future reader needs to know before relying on this row. Never leave this blank for a row that isn't Client-facing — the reason it isn't belongs here. Can be tidied for readability but must not upgrade the claim's certainty — see the no-benchmark-laundering rule in `validation-rules.md`. |

## ID scheme

Use `BM-###` with a three-digit, zero-padded, sequential number scoped to the register being built (`BM-001`, `BM-002`, ...). If the user is adding rows to an existing register, continue the existing sequence rather than restarting at 001 — check the highest existing ID first.

`BM-###` IDs are assigned only to rows that enter `benchmark_register.csv`. Do not assign benchmark IDs to claims that are excluded outright, resolved only with fallback wording, or otherwise absent from the register. Those claims are labelled Claim 1, Claim 2, etc. by original submission order in `excluded_benchmarks.md` and `validation_notes.md`. See "Benchmark ID scope" in `SKILL.md` for the full convention.

## Formatting conventions

**value_or_range**: write a single figure as a bare number with its sign if relevant ("32%", "-18%"). Write a range with an en dash and no spaces around it ("20-35%" is acceptable if an en dash isn't available; "20–35%" is preferred). Never average a range down to a single point — if the source gives a range, the register gives a range.

**publication_date** precision: match the source. If the source only says "2024," the field is `2024`, not `2024-01-01`. Inventing a day or month is fabricating a field, even if it looks tidier.

**source_organisation** vs. **source_title**: these are always two different things. "Forrester" is an organisation; "The Total Economic Impact of Content Operations Platforms" is a title. A row with the organisation name in the title field and nothing in the organisation field is incomplete, not correct.

**Multiple scopes**: if a benchmark applies across more than one industry, function, or geography, list them separated by a semicolon within the field ("Retail; CPG"), rather than creating duplicate rows — duplicate rows for the same underlying evidence make the register harder to audit, not easier.
