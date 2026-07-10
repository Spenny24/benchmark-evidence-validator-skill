# FSOMA Commercial-Impact Benchmark Map

This file maps benchmark metrics to the eight commercial-model categories used by the FSOMA Transformation Report skill. It answers a question the core register does not: once a benchmark clears the evidence bar, which part of a commercial case is it actually allowed to support?

A benchmark being `Client-facing` and `Primary verified` means the number itself can be trusted. It does not mean the number can be used anywhere in a commercial model. A verified 35% productivity gain is real evidence of productivity. It is not evidence of revenue growth, cash-flow improvement, or reduced compliance risk, even though a confident narrative could stretch it to cover all three. This file exists so that stretch never happens quietly.

## How this fits with the rest of the skill

This is an additional classification layer, not a replacement for anything in `validation-rules.md` or `allowed-use-rules.md`. The sequence for any benchmark headed into FSOMA work is:

1. Run the full ten-condition test and assign `evidence_status` / `allowed_use` as normal. Nothing in this file changes that outcome, and nothing in this file grants an exception to it.
2. Only once a row is usable at all (`Client-facing` for external use, `Internal challenge only` for internal sanity-checking) does the question in this file apply: which FSOMA commercial-impact category can this specific benchmark support, and what does the client need to already have on hand before it can be used there.
3. A benchmark that is fully verified but assigned to the wrong commercial category is still a misuse. Evidence quality and category fit are two separate checks, and a benchmark has to clear both before it goes into a commercial case line.

Do not use a benchmark outside its commercial-impact category, even when the underlying evidence is strong. A well-sourced productivity benchmark does not become weaker evidence when it is misapplied to a revenue line, but the resulting commercial case claim is still wrong.

## The eight categories

### 1. AI productivity

**Benchmark types that belong here:** hours-per-output or labour-hours-per-task reduction, cycle-time reduction, throughput increase, task-completion-rate improvement, output-per-FTE, automation-coverage percentage. Anything measuring how much faster or how much more a person or team produces with a tool, workflow, or process change.

**What it can support:** labour productivity gains, FTE reallocation or capacity uplift narratives, throughput-based efficiency lines in the commercial model.

**What it cannot support:** revenue growth, cost reduction (freed-up time is not the same as reduced spend unless headcount or budget actually shrinks), compliance or risk reduction, cash-flow timing.

**Required client baseline:** the client's own current cycle time, hours-per-task, or throughput figure for the same process, so the benchmark's percentage or multiple has a real starting point instead of a national average.

**Misuse examples:** taking a "35% faster content production" benchmark and writing it straight into the model as a "35% cost reduction" line without confirming headcount or budget actually goes down; citing a productivity benchmark to argue "faster campaigns mean more sales" with no revenue-linked KPI in sight.

### 2. Cost reduction

**Benchmark types that belong here:** direct unit-cost reduction (cost per asset, cost per campaign, cost per ticket), reduction in outsourced or agency spend, reduction in rework or waste cost.

**What it can support:** hard-dollar cost-line reduction in the commercial model, before/after cost-per-unit comparisons.

**What it cannot support:** revenue enablement, productivity claims where hours saved were never converted into an actual reduced spend line, adoption confidence.

**Required client baseline:** the client's own current cost-per-unit or total spend on the relevant line item, ideally from actual invoices or financials rather than an internal estimate.

**Misuse examples:** using a vendor's "90% cost reduction" claim to size total content-budget savings without a verified, client-specific unit-cost baseline; treating a benchmark on "cost per word" as if it covers total programme cost.

### 3. Spend recovery

**Benchmark types that belong here:** SaaS licence waste recovery, unused-seat reclamation, contract renegotiation savings, duplicate-tool spend elimination, maverick-spend recovery.

**What it can support:** one-time or recurring recovered-spend line items tied to identified, named contracts or tools.

**What it cannot support:** labour productivity, revenue enablement, ongoing operating-cost reduction beyond the specific recovered line.

**Required client baseline:** the client's actual tool or vendor inventory and current spend per tool, ideally from procurement or finance records naming the specific contracts in scope.

**Misuse examples:** applying an industry "average SaaS waste rate" (for example 30%) across a client's entire software budget without confirming which specific tools or seats are actually redundant.

### 4. Tooling rationalisation

**Benchmark types that belong here:** tool-consolidation savings, licence-count reduction, integration or maintenance-overhead reduction from a smaller tool stack.

**What it can support:** the commercial case for retiring or consolidating named tools, projected savings from a smaller stack.

**What it cannot support:** labour productivity gains from the tools that remain, revenue impact, compliance risk.

**Required client baseline:** the client's current tool count, licence cost, and an overlap map (which tools do the same job) for the function in scope.

**Misuse examples:** using a SaaS-waste benchmark to argue that rationalisation also improves labour productivity, when a smaller tool stack does not by itself make people faster.

### 5. Revenue enablement

**Benchmark types that belong here:** conversion-rate uplift, campaign-performance improvement, speed-to-market advantage tied to a named revenue outcome, personalisation or targeting-accuracy improvement with a stated attribution method.

**What it can support:** revenue-linked commercial case lines, but only where the client already has a revenue-linked KPI (conversion rate, average order value, pipeline velocity, or equivalent) that the benchmark's metric maps onto directly, and an agreed attribution rule connecting the operational change to the revenue outcome.

**What it cannot support:** cost reduction, labour productivity, risk or compliance reduction, working-capital impact.

**Required client baseline:** the client's own current conversion rate (or equivalent revenue KPI) for the process in scope, plus a stated attribution method for how much of any uplift is credited to the change being modelled.

**Misuse examples:** applying a "20% conversion uplift" benchmark to a client with no tracked conversion KPI and no attribution model, then presenting the resulting revenue figure as if it were verified.

### 6. Risk and compliance reduction

**Benchmark types that belong here:** compliance-rework rate reduction, error or defect rate reduction, audit-finding reduction, incident or breach-rate reduction, review-cycle rejection-rate reduction.

**What it can support:** avoided-cost modelling (cost of rework, fines, or remediation avoided), a risk-reduction narrative in the commercial case.

**What it cannot support:** revenue growth, unless a revenue impact from the risk event is separately and directly evidenced (a named instance where a compliance failure caused a quantified revenue loss).

**Required client baseline:** the client's current rework rate, error rate, or incident frequency, and its known or estimated cost per occurrence.

**Misuse examples:** using a compliance-rework benchmark to also claim a revenue-growth benefit ("fewer errors means happier customers means more sales") with no documented, quantified revenue link.

### 7. Working-capital / cash impact

**Benchmark types that belong here:** days-sales-outstanding (DSO) reduction, invoice-cycle-time reduction, payment-term improvement, inventory or work-in-progress reduction tied to a process change.

**What it can support:** cash-flow timing improvements in the commercial model (cash freed up, financing cost avoided).

**What it cannot support:** P&L cost reduction (freeing up cash is not the same as reducing spend), revenue growth, labour productivity.

**Required client baseline:** the client's current DSO, invoice cycle time, or equivalent cash-cycle metric.

**Misuse examples:** presenting a working-capital benchmark as if it reduces operating cost, when it only shifts the timing of cash rather than the total spent.

### 8. Adoption and transformation confidence

**Benchmark types that belong here:** user-adoption rate, change-readiness survey results, time-to-proficiency, stakeholder-satisfaction or NPS shift tied to a transformation programme.

**What it can support:** the qualitative confidence narrative in the commercial case (de-risking the programme, evidencing that comparable transformations landed), sizing an adoption-risk contingency.

**What it cannot support:** any hard-dollar line (cost, revenue, or cash) on its own. Adoption benchmarks measure whether people used the change, not what the change was worth financially.

**Required client baseline:** the client's own current tool-adoption or change-readiness figure where available, or a stated assumption where none exists, flagged explicitly as an assumption.

**Misuse examples:** using an adoption benchmark to size a dollar savings figure directly, for example treating "85% adoption in comparable programmes" as if it means "85% of projected savings will be realised."

## Spend recovery versus tooling rationalisation

Spend recovery covers money already being wasted, duplicated, uncaptured, or misallocated.

Tooling rationalisation covers structural simplification of the tool stack.

If a benchmark relates to unused licences or duplicate seats, classify it as Spend recovery when the saving is reclaiming wasted spend, and Tooling rationalisation when the saving comes from retiring, consolidating, or replacing platforms.

Where both apply, map both categories separately and require separate client baselines.

## Applying more than one category to a single benchmark

Some benchmarks legitimately touch two categories at once, for example an automation benchmark that reports both a labour-hours reduction (AI productivity) and a downstream rework-rate drop (risk and compliance reduction). When that happens, record each category as its own line with its own required client baseline. Do not let evidence for one category stand in for the baseline requirement of the other. A client's cycle-time baseline does not substitute for a rework-rate baseline, even when both numbers come from the same source study.

## What this file does not do

This file does not change `evidence_status` or `allowed_use`, does not add or remove any field in the fifteen-column register schema, does not invent a benchmark, and does not promote a benchmark from the sourcing backlog or the default register into client-facing use. It only decides, for a benchmark that has already cleared the evidence bar, which commercial-model category it is honest to use it in.
