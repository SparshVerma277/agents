# Product Discovery Report: <topic>

*Date: YYYY-MM-DD · Author: product-discovery skill · Status: Draft for review*

## 1. Executive summary

- <key finding and recommendation> [^citation]
- <competitor delta> [^citation]
- <biggest open question blocking a decision>

## 2. Source and freshness log

| Subagent | Scope | Read | Freshness | Notes |
| :--- | :--- | :--- | :--- | :--- |
| `jira-auditor` | `PMT` epics, `SH`, `IM` | N issues | live | JQL run: … |
| `prd-analyst` | `<group>.md` + PRDs | N of M files | export date | not read: … |
| `market-scout` | Tiers <n> | N pages | undated unless noted | skipped: … |

State what was *not* covered. A silent gap reads as a clean bill of health.

## 3. Capability matrix

| Capability | Internal state | Competitor benchmark | Gap / action |
| :--- | :--- | :--- | :--- |
| <domain> | <state> [^PMT-key or filename] | <claim> [^URL] | <action> |

## 4. Prioritized proposals

### Proposal 1: <title>

- **Problem:** <gap, and how it was observed>
- **Solution:** <scope>
- **Pillars supporting:** <n>/3 — defect pressure / epic delta / parity
- **Surfaces:** Lender Portal / Merchant Portal / Customer Journey
- **Lender configurability:** <needed per-lender, or why not>
- **Competitor reference:** <how the set handles it> [^URL]
- **Existing work:** <epic and ticket keys, or "none found">
- **Funding/conversion impact:** `UNKNOWN — requires input from Sparsh`

## 5. Pillar 1 caveat

Defect pressure (`SH` volume, `IM` clusters) is used as a **proxy** for
conversion leakage. No funding or conversion data source is wired to this
skill. Every impact estimate here is directional, not measured.

## 6. Unverified assumptions and open questions

- `UNKNOWN: <item>` — needs confirmation from **<owner>**
- `UNKNOWN: "Recommended" Jira status semantics` — needs confirmation from **Sparsh**

## 7. Handoff

- Pressure-test first → `DISCUSSION_AGENT.md`
- Ready to document → `PRD_AGENT.md`
