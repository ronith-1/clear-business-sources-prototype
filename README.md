# CLEAR Business Sources — UI Prototype

**Live prototype:** https://ronith-1.github.io/clear-business-sources-prototype/

## Context

CLEAR is a third-party due-diligence report run on businesses applying for loans — it checks criminal records, liens, bankruptcies, UCC filings, and ~14 other categories. Alongside the applicant, the report returns **other businesses connected to the application** — through a shared owner, frequent transactions, or an unclear relation — and each of those carries the same full set of checks.

This prototype explores how to represent those associated businesses in the reviewer dashboard without overwhelming it. The guiding constraints:

- Up to ~30 associated businesses can come back, but typically only 1–3 are flagged.
- Reviewers only need to scrutinize the flagged ones — clean businesses stay concealed behind a click.
- The UI follows a consistent rhythm: **summary → where data needs scrutiny → deep dive**.
- Every associated business is a full peer of the applicant: same inputs, same output sections, same deep-dive drawer.

## The three navigation variations

Use the toggle at the **bottom-right** of the page to switch:

| Variation | Pattern | Idea |
|---|---|---|
| **A · Temp tabs** | Entity tab strip on the card | Main business + flagged businesses are pinned tabs; picking a clean business from the "+N more" menu opens it as a dismissable tab. One metaphor (tabs) everywhere. |
| **B · Breadcrumb** | Tabs + drill-in | Flagged businesses stay as tabs; picking a clean business replaces the card content with a breadcrumb (`← CLEAR Analysis / Business`) back to the applicant. |
| **C · Triage** | "Other Business Sources" banner | No entity tabs. A banner summarises `2 of 14 flagged` with clickable chips; expanding it shows a triage list (flagged first, with the sections that tripped), and clicking a row drills in with a breadcrumb. |

In every variation, clicking any output row opens the **deep-dive drawer** scoped to the selected business — section tabs, per-check summary panel, and record tables.

## Interactions to try

1. Click a flagged tab (Marina Holdings / Coastal Freight) — the whole card swaps to that business's inputs and outputs.
2. Open "+12 more businesses" (A/B) or the banner (C) — the concealed clean businesses, each with relation + outcome.
3. In A, dismiss a temporary tab with its ×.
4. Click the **Criminal Record** or **Liens** row on a flagged business — scoped drawer with real-looking records.

## Notes

- Single self-contained file (`index.html`) — no build step, no dependencies beyond Google Fonts. Open locally or serve statically.
- All data is **mock** and generated from one `BUSINESSES` array at the top of the `<script>`; each business derives from a shared 18-section template with per-section overrides, so reshaping scenarios is a few lines.
- Styling follows the HVOne design tokens (Inter, `#553EF1` interactive accents, 11px overline labels, 4/8pt spacing grid). The top-nav segmented control matches Figma spec `13873:8838`.
- Open product question (deliberately deferred): whether to group associated businesses by owner ("what else does Person B own?") instead of a flat flag-first list — relationship is currently just a metadata field.
