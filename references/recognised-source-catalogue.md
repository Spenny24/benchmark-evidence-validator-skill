# Recognised Source Catalogue

This is a list of source *categories* the skill recognises as the kind of thing that can, in principle, hold Client-facing evidence. It is not a list of sources that are automatically trusted. Every claim from every category on this list still goes through the full ten-condition test in `validation-rules.md` — the category tells you what kind of checking to do, not whether the checking can be skipped.

## The categories

- **Primary consulting-firm research** — original studies, surveys, or benchmarking reports published by a consulting firm under its own name (e.g. a McKinsey, BCG, or Deloitte report with a named study and methodology).
- **Analyst-firm research** — reports from firms whose core business is independent market and technology analysis (e.g. Gartner, Forrester, IDC).
- **Industry-body reports** — publications from trade associations, standards bodies, or sector coalitions.
- **Academic or peer-reviewed studies** — research published in or through an academic institution or peer-reviewed venue.
- **Regulator or government publications** — official statistics or findings from a government agency or regulatory body.
- **Vendor research with disclosed methodology** — a vendor's own study, where the methodology, sample, and scope are stated clearly enough to evaluate independently of the vendor's commercial interest.
- **Platform-provider research with stated sample and scope** — findings published by a platform or tool provider about usage patterns on their own platform, where the sample and scope are explicit.
- **Published customer case studies with clear metric definitions** — a named customer's documented outcome, where the metric is defined precisely enough to be checked (not just "saw great results").

## Why this list exists, and what it isn't for

The list exists so that, when evidence shows up from one of these source types, there's a clear sense of what "properly checked" looks like for that type — a consulting-firm report needs a named study and a link to it; a vendor claim needs disclosed methodology, not just a confident number on a marketing page.

It is not a shortcut. Being on this list, or coming from a well-known name within one of these categories, never on its own justifies `Client-facing` or `Primary verified`. The ten-condition test in `validation-rules.md` is the only thing that grants those classifications, and it looks at the specific claim in front of you, not the reputation of the category it came from.

## Worked examples

**A McKinsey report with an exact metric, value, date, URL, and scope.** This is primary consulting-firm research, checked directly. If all ten conditions hold, it can be `Primary verified` / `Client-facing`. The category didn't make this happen — having the actual report, with everything the schema needs, did.

**A blog post saying "McKinsey says X%" with no link to the original report.** Same category by association, completely different evidence quality. The primary document was never checked, only a secondary claim about it. This is `Secondary/internal` at best, and fails condition 10 for Client-facing regardless of how the other nine conditions look.

**A vendor claim like "customers see 10x ROI" with no disclosed methodology.** This falls outside "vendor research with disclosed methodology" precisely because the methodology isn't disclosed — it's marketing copy shaped like a statistic. Classify as `Exclude` or, if there's at least a named product and a directional claim worth tracking, `Internal challenge only`. It cannot be Client-facing without independent or methodologically transparent backing.

**A statistic repeated across several industry blogs with no traceable origin.** Repetition across sources is not the same as corroboration by sources — most repeated stats trace back to one unverified original, or to nothing at all. Classify as `Draft/unverified` if a plausible originating claim can be pointed to, or `Exclude` if it can't be traced at all. See the repeated-value case in `allowed-use-rules.md` for the full reasoning.
