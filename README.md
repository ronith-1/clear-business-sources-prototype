# CLEAR Business Sources — UI Prototype

**Live prototype:** https://ronith-1.github.io/clear-business-sources-prototype/

## Context

CLEAR is a third-party due-diligence report run on businesses applying for loans — it checks criminal records, liens, bankruptcies, UCC filings, and ~14 other categories. Alongside the applicant, the report returns **other businesses connected to the application** — through a shared owner, frequent transactions, or an unclear relation — and each of those carries the same full set of checks.

This prototype explores how to represent those associated businesses in the reviewer dashboard without overwhelming it. The guiding constraints:

- Up to ~30 associated businesses can come back, but typically only 1–3 are flagged.
- Reviewers only need to scrutinize the flagged ones — clean businesses stay concealed behind a click.
- The UI follows a consistent rhythm: **summary → where data needs scrutiny → deep dive**.
- Every associated business is a full peer of the applicant: same inputs, same output sections, same deep-dive drawer.

## The two variations

Open the **Prototype** button at the bottom-right for the control panel.

| | Pattern | Idea |
|---|---|---|
| **Variation 1 · Grouped menu** | Applicant pill │ one labelled menu | Only the applicant is a pill, reading "Main business". A divider, then a single **Associated businesses** control carrying the total, a divider, and the flagged count in review styling. Opening it groups flagged above clean under marked headings. Picks drill in with a breadcrumb. |
| **Variation 2 · Triage** | "Other Business Sources" banner | No entity pills. A banner summarises `2 of 14 businesses flagged` with chips for the flagged ones; expanding it shows a triage list, flagged first, each with the sections that tripped. Clicking a row drills in with the same breadcrumb. |

### Variation 1 · Grouped menu

- **Pro** — one labelled control states in words what the associated businesses are, so nothing on the strip can be mistaken for a filter bar.
- **Pro** — it costs almost no vertical space, leaving the card's rhythm of inputs → outputs → deep dive intact.
- **Con** — flagged businesses have no standing presence on screen; `2` is the only reminder they exist, and everything else is a click away.

### Variation 2 · Triage

- **Pro** — flagged businesses and the sections that tripped them are visible without opening anything, so the reviewer sees the work before choosing to do it.
- **Pro** — the banner doubles as a progress surface: which businesses were flagged and how many remain is answerable at a glance.
- **Con** — it occupies a large block above the applicant's own results and disappears on drill-in, so the switcher is absent exactly when you want to switch again.

### Earlier explorations

Four earlier passes are kept behind the panel's disclosure rather than deleted: **A · Temp tabs**, **B · Breadcrumb**, **D · Banner switcher**, **E · Banner + tabs**. D is worth reopening if Variation 2's disappearing banner turns out to be the deciding objection — it is the same triage banner made persistent, with the header rewriting to `Viewing · <business>`.

Other panel controls:

- **Scenario** — `Typical · 2 flagged / 14` or `Heavy · 6 flagged / 22`, so the crowded states (chip overflow, long lists) are reachable.
- **Owner subtext** — shows or hides the owner names under each business name.
- **Flag dot** — shows or hides the dot beside flagged businesses. **Off by default**: the status tag already carries the signal. The drawer's section-level flags are unaffected.

Both display switches are pure visibility — the markup is always rendered and a class on `<body>` hides it.

In every variation, clicking any output row opens the **deep-dive drawer** scoped to the selected business — section tabs, per-check summary panel, and record tables.

## Interactions to try

1. In **Variation 1**, open **Associated businesses** — flagged above clean, each with its owners and outcome. Pick one; the card swaps to that business and a breadcrumb returns you.
2. In **Variation 2**, expand the banner for the same list, then use a flagged chip to jump straight there.
3. Switch the scenario to **Heavy** — Variation 1's counts become `22 │ 6`, and Variation 2's chips cap at three plus a `+N` that opens the list rather than navigating.
4. Turn **Flag dot** on and off, and **Owner subtext** off, to see how much each is carrying.
5. Click the **Criminal Record** or **Liens** row on a flagged business — scoped drawer with real-looking records.

## Notes

- Single self-contained file (`index.html`) — no build step, no dependencies beyond Google Fonts. Open locally or serve statically.
- All data is **mock** and generated from one `BUSINESSES` array at the top of the `<script>`; each business derives from a shared 18-section template with per-section overrides, so reshaping scenarios is a few lines.
- Styling follows the HVOne design tokens (Inter, `#553EF1` interactive accents, 11px overline labels, 4/8pt spacing grid). The top-nav segmented control matches Figma spec `13873:8838`.
- Business rows carry **owner names** as a subtext, toggleable from the panel. CLEAR does not reliably return *why* two businesses are linked, but it does return who owns them, so the relation line was replaced rather than restored. Flagged rows add a second line — the sections that tripped — which is unaffected by the toggle.
- The applicant's pill reads **"Main business"**, not its name — the name is already in the page title and the breadcrumb.
- When the flag dot is on, it takes the business's **own status colour**: amber (`--review-text`) for review, red (`--color-fail`) for fail. It was previously hardcoded to fail-red, so a review-status business showed a red dot beside an amber tag.
- Group headings carry their own marker — a red dot on `Flagged (N)`, a green check on `No flags (N)` — in both F's dropdown and the triage list. They are unaffected by the Flag dot switch, which only governs per-business dots.
- F's trigger shows `Associated businesses  14 │ 2` rather than a "Needs Review" tag. The applicant's own `Status: Review` chip was already on screen, and two amber badges reading "Review" a few inches apart described two different things in identical clothes.
- Each business shows its status **once**. For the selected business that is the `Status:` chip beside "Attempted at"; its row, header and breadcrumb stay bare so the same fact is not repeated three ways.
- Open product question (deliberately deferred): whether to group associated businesses by owner ("what else does Person B own?") instead of a flat flag-first list. With the subtext on, the repetition is visible — Priya Anand appears on five businesses in the typical scenario.
