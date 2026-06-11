# Career Learnings

A running log of durable, generalizable insights from my career. Entries are
domain-grouped, not chronological. Client-specific context is stripped — only
the pattern that carries forward is kept.

Maintained in git. Updated by the Discussion Agent after sessions where a
finding clears the bar below.

---

## What belongs here

Add an entry if it is:
- **New to me** — something I didn't know or hadn't thought about before
- **Generic** — applies beyond the specific client, product, or situation it came from

That's it. Simple learnings, obvious-in-hindsight learnings, and theories all belong here.
The only thing to strip out is client-specific detail — take the generic pattern, not the example.

---

## Format per entry

```
### [Short, memorable title]
**Principle** — one sentence that captures the insight
**Insight** — 2–3 sentences: the nuance, the mechanism, or the why
**Signal** — what to look for that tells you this pattern is in play
```

---

## Product

<!-- Add learnings about product design, prioritization, sequencing, feature scoping -->

---

## Business & Markets

### When volume is concentrated in one customer, top-line trends are almost always about that customer
**Principle** — Aggregate trend lines in concentrated businesses often track the largest entity, not the platform.
**Insight** — A 16.7% overall funding decline was almost entirely explained by one merchant dropping $2M. The rest of the base showed mixed movement with no clear signal. The top-line number communicated a platform-wide problem; merchant-level decomposition showed it was a single-entity timing issue. Whether that drop was seasonality, pipeline timing, or something structural — the raw data alone couldn't say.
**Signal** — When you see a top-line metric move, check what share of the movement is explained by the top 1–2 entities. If it's >80%, the trend isn't really a trend.
**Analysis** — Merchant-level pivot comparison of two 8-day funding windows, with day-by-day breakdown for top 15 merchants.
**What's missing** — No root cause on the top entity specifically. No seasonality baseline from the same period the prior year. No qualitative input from the merchant about pipeline state.
*Jun 2026*

---

## Customer Behavior

### The offer screen may be where approved customers exit — but we don't know why
**Principle** — The largest observed dropout stage in a consumer lending funnel was after offer presentation, but the data doesn't tell you the cause.
**Insight** — ~31% of approved customers dropped after seeing their offer. This could mean the gap between expectation and actual terms is the problem — but it could equally mean customers were comparison shopping, the UX was confusing, or they got distracted. A funnel analysis shows where the drop happens, not why. It can't distinguish between a pricing problem, a UX problem, and a research behavior.
**Signal** — If your biggest dropout stage is post-offer, don't immediately assume it's a pricing problem. You need qualitative data or session recordings before redesigning terms.
**Analysis** — Stage-level funnel breakdown across 27K unsecured consumer loan applications across two channels (in-app and pre-approved), over a 4-month window.
**What's missing** — No exit surveys, session recordings, or A/B tests on offer presentation. Entirely observational.
*Jun 2026*

### Pre-approved customers may have higher expectations — or they may just be a different population
**Principle** — Higher approval rate doesn't imply higher conversion; channel self-selection confounds the comparison.
**Insight** — Pre-approved customers had ~4x the approval rate of in-app customers but lower contract sign rates (~39% vs ~52%). One interpretation is that pre-approval raises expectations and the offer disappoints. But these are fundamentally different populations — different intent, potentially already comparing offers elsewhere. You can't isolate the expectation effect without controlling for who these customers are.
**Signal** — When comparing conversion rates across acquisition channels, always ask whether the populations are actually comparable before drawing conclusions about the channel design.
**Analysis** — Channel-split funnel analysis comparing in-app vs pre-approved cohorts across the same time period.
**What's missing** — No customer-level attribute controls (credit score, income, existing relationship). No intent data.
*Jun 2026*

### APR thresholds may create conversion cliffs — but the threshold needs more scrutiny
**Principle** — An observed sign rate drop at a specific APR band is a signal worth investigating, not a proven causal lever.
**Insight** — Contract sign rates dropped from ~57% to ~41% above 17.99% APR for pre-approved personal loans. The cliff pattern is sharp enough to warrant attention. But high-APR offers may also come with different loan amounts, different merchant contexts, or different customer risk profiles — any of which could explain the dropout. Without controlling for those variables, APR is an observed correlate, not a proven driver.
**Signal** — When you see a behavioral cliff at a round number, ask whether that number is doing the work or whether it's a proxy for something else (loan size, credit profile, merchant type).
**Analysis** — Funnel segmented by APR bands across two channels, combined with rank-order model evaluation by APR segment.
**What's missing** — No multivariate analysis controlling for FICO, loan amount, and merchant type simultaneously. No randomized pricing test.
*Jun 2026*

### Contract signing behavior shows a wide distribution — which may indicate two distinct populations
**Principle** — Aggregate TAT metrics hide the shape of the distribution; median and mean diverging sharply is a signal to look at the distribution itself.
**Insight** — SAC contracts had a median signing time of 19 minutes but a mean of 32.8 hours. That divergence suggests a distribution with a fast majority and a long stall tail. Whether this reflects two distinct behavioral populations or one population with variable urgency is unclear from the data alone. The implication — if it is two populations — is that re-engagement strategies would look completely different for each group.
**Signal** — When median and mean diverge significantly on a behavioral metric, plot the full distribution before acting on the average.
**Analysis** — Descriptive statistics on time-from-approval-to-signature for ~450 SAC contracts over a 6-week window.
**What's missing** — No segmentation of the long-tail group. No clustering or survival analysis to validate whether two populations actually exist.
*Jun 2026*

### Longer SAC terms correlated with higher sign rates — but merchant mix wasn't controlled
**Principle** — Observed conversion lift from longer promotional terms may reflect merchant differences as much as customer preference.
**Insight** — SAC-18M (74.7% sign rate) outperformed SAC-12M (68.6%) and Standard (65.7%). The direction is intuitive — longer interest-free windows are more attractive. But merchants offering 18M terms may attract a different customer profile entirely. Similarly, payment deferment showed almost no effect (71.1% non-deferred vs 69.5% deferred) — which suggests the interest-free window is what customers value, not delayed payment onset. But again, which merchants offer deferment matters before drawing that conclusion.
**Signal** — When comparing product configurations, check whether the merchants or customers using each configuration are comparable before attributing the difference to the configuration itself.
**Analysis** — Conversion rate breakdown by SAC term length and deferment flag across ~8K approved applications.
**What's missing** — No merchant-controlled comparison. No A/B test with the same merchant offering different SAC terms to different customers.
*Jun 2026*

---

## Fintech & Lending

### Document submission may be the conversion gate — but it's also a selection effect
**Principle** — High approval rates conditional on document submission could reflect underwriting quality or could reflect that motivated customers self-select into submitting.
**Insight** — 80.9% of applicants who submitted documents got approved; without docs, 60.5% were declined. The intuitive read is that the underwriting works and the product problem is just getting docs submitted. But customers who submit documents are likely more motivated and better-prepared — their approval rate might be higher independent of what the docs actually contain. Disentangling selection effect from document quality effect would require seeing underwriting decisions on the non-submitters.
**Signal** — When a completion step (doc upload, consent, verification) shows high downstream conversion, ask whether it's causal or whether it's selecting for a certain type of customer.
**Analysis** — Funnel segmented by document request and submission status across 21K applications, split by channel (Lyon vs Partner Direct).
**What's missing** — No analysis of outcomes when docs are submitted but poor quality. No counterfactual on what would happen if non-submitters were nudged to submit.
*Jun 2026*

### Aggregate model metrics can hide segment-level failures
**Principle** — A model that performs well in aggregate can be completely non-functional in a specific segment, and you'll only see it if you cut the data that way.
**Insight** — A line assignment model showed clear monotonic rank ordering at APR < 17% (loan amounts spread $26K–$30K across deciles) but completely flat output at APR > 17% (all deciles clustered at $20–22K). In aggregate, the model looked fine. The failure only surfaced when evaluated by APR band. Models trained on a broad population often underperform in tails or segments that weren't well-represented in training data.
**Signal** — Evaluate models by the segments that matter for business decisions, not just aggregate metrics. If a segment has different economics or behavior, it needs its own evaluation slice.
**Analysis** — Rank-order analysis of loan amounts by customer rank decile, split by APR band, from a consumer loan funnel dataset.
**What's missing** — No analysis of what drove the model's failure at high APR — whether it was data sparsity, feature distribution shift, or label quality issues.
*Jun 2026*

---

## Distribution & Channels

<!-- Add learnings about channel partners, aggregators, merchant behavior, go-to-market -->

---

## Teams & Execution

<!-- Add learnings about cross-functional work, alignment, specs, and delivery -->

---
