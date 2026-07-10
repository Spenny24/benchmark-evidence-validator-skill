# Validation Rules

## The ten-condition Client-facing test

Run every candidate row through all ten conditions, in order, and stop at the first failure — you only need one to know the row isn't Client-facing, though it's worth noting all failures in `notes` rather than just the first, since the user will want the full list when deciding what to go verify.

| # | Condition | Fails if... |
|---|---|---|
| 1 | `source_organisation` is present | Blank, or a vague placeholder like "industry sources" |
| 2 | `source_title` is present | Blank, or just a URL with no actual document title |
| 3 | `source_url` is present | Blank, dead, or paywalled with no way to confirm the content behind it |
| 4 | `publication_date` is present | Blank, or "undated" |
| 5 | `metric` is present | Blank, or too vague to be checkable ("efficiency") |
| 6 | `value_or_range` is present | Blank |
| 7 | `unit` is present | Blank, or ambiguous (a bare "32" with no unit could mean 32% or 32 days) |
| 8 | `functional_scope` is present | Blank |
| 9 | `allowed_use` is Client-facing | The row was otherwise flagged for `Internal challenge only` or `Exclude` in the allowed-use analysis (see `allowed-use-rules.md`) |
| 10 | `evidence_status` is Primary verified | The row is `Secondary/internal` or `Draft/unverified` |

This is a checklist, not a scoring system. Nine out of ten conditions met is a fail, not "almost Client-facing." The whole value of a hard threshold is that it can't be negotiated row by row — if it could, every close call would eventually get waved through and the register would drift back into being unreliable.

## evidence_status decision tree

Ask, in order:

1. **Was the original primary source checked directly** (the actual report, filing, dataset — not a summary of it)? If yes, and the value matches what's checked → `Primary verified`.
2. **Is the source itself referencing or summarizing another document** (a blog post citing a report, a vendor page citing "industry research," an internal deck citing an external stat with no link)? → `Secondary/internal`, *unless* the user also supplies the primary document and the number can be confirmed against it — in that case, verify against the primary document and use `Primary verified` instead, with a note that the primary was supplied separately from where the claim was first seen.
3. **Was the claim found with no traceable source at all**, or only as a number repeated across multiple secondary pages with no common origin? → `Draft/unverified`, or `Exclude` if there isn't even a plausible originating claim to point future verification work at.

**Source checking in practice:** if the user supplies a public URL or document, open and check it directly before classifying — don't infer what a linked source probably says from the claim text alone. If the primary source wasn't supplied or can't be reached, do not assume a secondary summary got it right just because it reads confidently. Default to `Secondary/internal` or lower until the primary is actually in hand.

A source being reputable doesn't skip this tree. A well-known consultancy's own marketing page that cites "our research shows" without naming the underlying study is still secondary at best — the citation is doing the work of a footnote without the footnote.

## Handling user-supplied labels

Users will sometimes hand over data with a label already attached — "this one's Client-facing," a spreadsheet column that says "external," a note that says "verified." Treat these as a starting hypothesis, not a fact:

1. Run the row through the ten-condition test regardless of the supplied label.
2. If it passes, the label was correct — proceed normally, no need to call this out specially.
3. If it fails, override the label. Set `allowed_use` to what the evidence actually supports (`Internal challenge only` or `Exclude`), and in `notes`, state plainly that the user's original label was overridden and list exactly which condition(s) failed.
4. Also add an entry to `validation_notes.md` under an "Overrides" section so the user sees every case where their label didn't hold up, in one place, without having to scan the whole register for it.

Never silently accept a label that doesn't hold up, and never argue the user out of supplying data — just show the evidence trail plainly and let the classification speak for itself. The goal is a register the user can trust precisely because it doesn't just agree with whatever came in.

## No benchmark laundering

Classification tracks the evidence, never the quality of the writeup. It's tempting, when tidying a messy claim into a register row, to smooth the language on the way — this is exactly how a weak claim quietly becomes a stronger-sounding one, without anyone deciding that should happen.

Rewrite the presentation if it helps readability, but the *certainty* of the original claim has to carry through unchanged:

- "AI saves 90% of content cost" stays an unsourced, specific claim. It does not become "AI can significantly reduce content production cost" — that phrasing sounds like a defensible generalisation, which is worse than the original, because it's now harder to spot that nothing backs it.
- "Customers see 10x ROI" stays a vendor marketing claim with no disclosed methodology. It does not become "ROI can be material in early deployment" — that reads like an independent, hedged finding, which misrepresents where it came from.
- "Common knowledge" or "everyone says this" stays exactly that — an unattributed, repeated assertion. It does not become "industry consensus," which implies a body of checked evidence that was never actually assembled.

A practical test: if you show the original claim and your rewritten `notes` entry to someone who didn't see the source, would they estimate the same evidence quality from both? If the rewrite makes the claim sound more credible than the original did, the rewrite has laundered it — go back and describe the weakness plainly instead.

## Common judgment calls

**A number is "well known" or "everyone in the industry says this."** That's exactly the pattern that produces `Draft/unverified` or `Exclude` rows — wide repetition without a traceable origin is the default failure mode this skill exists to catch. Ask for the original source before treating it as anything stronger than a claim worth investigating.

**The client provided the number themselves** (their own internal KPI, their own reported figure). This is real evidence, but it's about the client, not an external benchmark to compare them against. If the user wants it in the register anyway (e.g. as a baseline to benchmark other evidence against), classify it `Secondary/internal` at best, since it's a single internal data point rather than a verified external reference, and never `Primary verified` — there's nothing external to verify it against.

**Two sources give different numbers for what looks like the same benchmark.** Don't average them or pick the more favorable one. Create separate rows, each sourced and dated correctly, and note the discrepancy in both rows' `notes` fields so the reader sees the conflict rather than a single number that quietly picked a side.
