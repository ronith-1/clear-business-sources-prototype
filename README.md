# CLEAR Business Sources — UI Prototype

**Live prototype:** https://ronith-1.github.io/clear-business-sources-prototype/

## Context

CLEAR is a third-party due-diligence report run on businesses applying for loans — it checks criminal records, liens, bankruptcies, UCC filings, and ~14 other categories. Alongside the applicant, the report returns **other businesses connected to the application** — through a shared owner, frequent transactions, or an unclear relation — and each of those carries the same full set of checks.

This prototype explores how to represent those associated businesses in the reviewer dashboard without overwhelming it. The guiding constraints:

- Up to ~30 associated businesses can come back, but typically only 1–3 are flagged.
- Reviewers only need to scrutinize the flagged ones — clean businesses stay concealed behind a click.
- The UI follows a consistent rhythm: **summary → where data needs scrutiny → deep dive**.
- Every associated business is a full peer of the applicant: same inputs, same output sections, same deep-dive drawer.

## The five navigation variations

Open the **Prototype** button at the bottom-right for the control panel. It holds everything:

- **Nav variation** — A through E.
- **Scenario** — `Typical · 2 flagged / 14` or `Heavy · 6 flagged / 22`, so the crowded states (chip overflow, long tab strips, scrolling lists) are reachable.
- **Owner subtext** — shows or hides the owner names under each business name, across every variation. Pure visibility: the line is always rendered and a class on `<body>` hides it.

| Variation | Pattern | Idea |
|---|---|---|
| **A · Temp tabs** | Entity tab strip on the card | Main business + flagged businesses are pinned tabs; picking a clean business from the "+N more" menu opens it as a dismissable tab. One metaphor (tabs) everywhere. |
| **B · Breadcrumb** | Tabs + drill-in | Flagged businesses stay as tabs; picking a clean business replaces the card content with a breadcrumb back to the applicant. |
| **C · Triage** | "Other Business Sources" banner | No entity tabs. A banner summarises `2 of 14 flagged` with clickable chips; expanding it shows a triage list (flagged first, with the sections that tripped), and clicking a row drills in with a breadcrumb. |

### Breadcrumb (B and C)

Both read `← Blue Oak Technologies LLC / Pacific Crest Catering LLC` — applicant as a link, current business in bold. C originally carried a third level, "Associated businesses", which landed on the applicant exactly like the first crumb; the two differed only in whether the banner list was left open. Back now returns you to the applicant in the state you left it, so that behaviour survives without a level of its own.
| **D · Banner switcher** | Banner *is* the navigation | No tab strip, no breadcrumb. The banner persists on every entity and the header rewrites to `Viewing · Marina Holdings LLC`. The applicant is the first row in the list, so leaving and returning are the same gesture, and the list stays open across switches. |
| **E · Banner + tabs** | Banner replaces the dropdown | Flagged stay pinned as tabs (as in A), but the banner replaces the "+N more" dropdown as the discovery surface. It persists and stays open while you pick, so opening several businesses in a row is one click each. |

D and E are a single-axis test: **does a pinned tab strip earn its space once the banner is persistent?** D says no; E says flagged deserve permanent surfacing and the banner handles the long tail. The cost of E is redundant representation — a flagged business appears both as a tab and as a chip.

In every variation, clicking any output row opens the **deep-dive drawer** scoped to the selected business — section tabs, per-check summary panel, and record tables.

## Interactions to try

1. Click a flagged tab (Marina Holdings / Coastal Freight) — the whole card swaps to that business's inputs and outputs.
2. Open "+12 more businesses" (A/B) or the banner (C/D/E) — the concealed clean businesses, each with its owners + outcome.
3. In A, open several businesses in a row — each one leaves the menu as it opens. Open all of them and "+0 more" becomes **Collapse all**.
4. In D, expand the banner and switch between three businesses without it ever closing.
5. Switch the scenario to **Heavy** and look at C/D/E — the chips cap at three plus a `+N` that opens the list rather than navigating.
6. Click the **Criminal Record** or **Liens** row on a flagged business — scoped drawer with real-looking records.

## Notes

- Single self-contained file (`index.html`) — no build step, no dependencies beyond Google Fonts. Open locally or serve statically.
- All data is **mock** and generated from one `BUSINESSES` array at the top of the `<script>`; each business derives from a shared 18-section template with per-section overrides, so reshaping scenarios is a few lines.
- Styling follows the HVOne design tokens (Inter, `#553EF1` interactive accents, 11px overline labels, 4/8pt spacing grid). The top-nav segmented control matches Figma spec `13873:8838`.
- Business rows carry **owner names** as a subtext, toggleable from the panel. CLEAR does not reliably return *why* two businesses are linked, but it does return who owns them, so the relation line was replaced rather than restored. Flagged rows add a second line — the sections that tripped — which is unaffected by the toggle.
- Each business shows its status **once**. For the selected business that is the `Status:` chip beside "Attempted at"; its row, header and breadcrumb stay bare so the same fact is not repeated three ways.
- Open product question (deliberately deferred): whether to group associated businesses by owner ("what else does Person B own?") instead of a flat flag-first list. With the subtext on, the repetition is visible — Priya Anand appears on five businesses in the typical scenario.
