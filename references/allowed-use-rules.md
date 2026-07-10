# Allowed Use Rules

`allowed_use` answers one question: who is safe to show this row to? It is a separate judgment from `evidence_status`, though the two are related — you cannot be Client-facing without also being Primary verified (condition 10), but being Primary verified doesn't automatically make something Client-facing either (it still has to clear the other nine conditions).

## The three values

### Client-facing

Safe to cite in a client deliverable — a report, a deck, a business case — with the source attached. Only assigned when all ten conditions in `validation-rules.md` are true. There is no version of "Client-facing but don't show them the source" — if a row is Client-facing, its source_organisation, source_title, source_url, and publication_date should all be ready to hand over alongside the number, because a client is entitled to ask "where did this come from."

### Internal challenge only

Good enough to use inside the team: to sanity-check whether a client's own numbers look reasonable, to stress-test an assumption, to decide where to dig deeper. Not good enough to say to a client's face, because at least one of the following is true:

- The source is secondary (a summary rather than the primary document)
- A required field is missing (no date, no URL, vague scope, etc.) even though the underlying number is probably right
- The evidence is directionally useful but not precise enough to defend under questioning ("roughly a third of companies report faster cycle times" with no named study)

Rows in this category are genuinely useful — don't undervalue them just because they can't ship externally. They're the internal team's working assumptions, clearly flagged as such.

### Exclude

Doesn't clear the bar for internal use either. Typically because:

- No source can be traced at all, even loosely
- The claim is internally inconsistent or implausible on its face
- The claim duplicates another row with better evidence and adds nothing
- The claim was clearly fabricated, marketing copy dressed up as a statistic, or a rounding of a rounding with no way back to an original figure

Excluded rows still get recorded — in `excluded_benchmarks.md`, not in `benchmark_register.csv` — because the point isn't to pretend the claim was never raised, it's to be explicit that it didn't hold up and say what would need to change for that to be revisited.

## No automatic promotion

Recognised source categories exist so evidence can be evaluated consistently, not so a familiar name can skip the evaluation. See `recognised-source-catalogue.md` for the full list — primary consulting-firm research, analyst-firm research, industry bodies, academic studies, regulators, disclosed-methodology vendor research, platform-provider research with stated sample and scope, and published customer case studies with clear metric definitions. Every one of these can produce a `Client-facing` row, and every one of them can just as easily produce an `Exclude` row, depending on what was actually checked:

- A McKinsey report with an exact metric, value, date, URL, and scope: run it through the ten conditions like anything else. If it clears all ten, `Client-facing`.
- A blog post saying "McKinsey says X%" with no link to the original report: `Secondary/internal` at best, because the primary document was never checked — the McKinsey name on the citation is not a substitute for the citation actually pointing somewhere.
- A vendor's own claim ("customers see 10x ROI") with no disclosed methodology: `Exclude` or `Internal challenge only`, because a company's marketing claim about its own product needs independent or methodologically transparent backing before it's evidence rather than advertising.
- A statistic repeated across several industry blogs with no traceable origin: `Draft/unverified` or `Exclude` — repetition is not corroboration, see the repeated-value case below.

The recognised-category list is there to help spot what *kind* of check a source needs, not to shortcut the check itself.

## Decision logic

For each candidate row, work through these in order:

1. **Can a source be identified at all** (primary or secondary)? No → `Exclude`.
2. **Is the source primary, or can the primary be checked**? No (secondary only, primary not supplied) → provisionally `Internal challenge only`, evidence_status `Secondary/internal`, then re-check condition 9/10 below.
3. **Does every one of the ten conditions hold**? Walk the checklist in `validation-rules.md` explicitly, field by field. All ten true → `Client-facing`. Any single one false → `Internal challenge only` (assuming step 1 and 2 didn't already put it at Exclude).

## The two cases called out most often

**A blog or vendor page summarizing another report.** This is the most common near-miss. The summary usually reads confidently and the number often looks solid, which makes it tempting to treat as done. Resist that: unless the primary report itself is also supplied so the number can be checked directly, this stays `Secondary/internal` and, at most, `Internal challenge only` — it fails condition 10 (evidence_status must be Primary verified) even if every other field is filled in perfectly.

**A value that's repeated across many sites.** Repetition feels like corroboration but usually isn't — most repeated stats trace back to a single original source (or no traceable source at all), just quoted by everyone downstream. If a plausible originating claim exists but can't be directly confirmed, classify `Draft/unverified` and `Internal challenge only` at best. If no originating claim can be found even loosely, this is `Exclude` — three sites agreeing on a number none of them sourced is not stronger evidence than one site with no source, it's the same weak evidence copied three times.
