# Benchmark Sourcing Backlog

This file stores candidate benchmarks that were surfaced during validation runs but are not approved for client-facing use. It exists so that a promising-but-unverified claim gets tracked and eventually chased down, instead of being re-discovered and re-litigated from scratch every time it comes up.

## Rules

- **Never client-facing.** Nothing in this file may be used in a client report, commercial model, deck, or external proposal, regardless of how confident the wording sounds. Rows here are here because they didn't clear validation, not because they're waiting for a formatting pass.
- **Research-lead visibility only.** This file is a working list for whoever owns benchmark sourcing, not a citable source in its own right.
- **One promotion path.** A candidate moves into `default-industry-benchmark-register.csv` only after it passes all ten conditions in `validation-rules.md`, and only when a research lead deliberately makes that move — see "Keeping the default register and backlog current" in `SKILL.md`.
- **Update, don't duplicate.** If a validation run finds new information about a claim already listed here, update its row (status, missing evidence, next action) rather than adding a second entry for the same claim.

## Fields

Each row records: `claim`, `possible source`, `source URL if known`, `missing evidence`, `likely use case`, `current status`, `next verification action`.

## Candidates

### AI-assisted review cuts approval time by half

- **Possible source:** VendorCo blog post, which references unnamed "internal PulseWorks research"
- **Source URL if known:** https://example.com/synthetic/pulseworks-blog-tagging (synthetic example URL)
- **Missing evidence:** The underlying PulseWorks study itself — no link, no methodology, no sample size
- **Likely use case:** Creative review / approval workflow transformation narratives
- **Current status:** Secondary/internal — not promotable as-is
- **Next verification action:** Request the underlying study directly from the vendor, or look for independent corroboration of the same figure from a source that isn't the vendor itself

### DAM adoption drives content reuse rate up

- **Possible source:** "DAM Adoption Outcomes Survey," attributed to a benchmarking institute
- **Source URL if known:** Not currently available — original publication not yet located
- **Missing evidence:** A working URL to the source publication, and confirmation of the survey's methodology and sample size
- **Likely use case:** DAM / content operations business case appendices
- **Current status:** Secondary/internal — value can't currently be checked against a primary document
- **Next verification action:** Locate the institute's original publication (search their site, press coverage, or request via analyst relations contact) and confirm the URL and methodology before re-running validation

### Content ops teams cut cycle time by roughly a third

- **Possible source:** None identified — described by the person who raised it as "common industry knowledge"
- **Source URL if known:** None
- **Missing evidence:** Everything — no organisation, title, date, or original study has been identified
- **Likely use case:** None until a source is found; currently not useful even directionally
- **Current status:** Draft/unverified, trending toward Exclude
- **Next verification action:** Ask whoever raised the claim where they originally heard it. If no source surfaces within one research cycle, remove from the backlog rather than let it sit indefinitely as a claim nobody can act on

## Adding a new candidate

Use the same field structure as the entries above. A claim only belongs in this backlog if there's at least a plausible lead to chase (a named vendor, a described-but-unlinked study, a person who might remember where they saw it). A claim with genuinely nothing to go on — no source, no lead, no plausible next action — belongs in that run's `excluded_benchmarks.md` as an outright exclusion, not here.
