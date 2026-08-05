# Failed source evidence rows (kyb.html)

Date: 2026-08-05
Status: approved

## Problem

Sometimes a source fails during a KYB check (fetch/eval error). The API still emits an
evidence row for that source, but with no match data. Today the UI has no rendering for
this — every row assumes a completed comparison with a match type and score.

## Data shape

A failed source's evidence row looks like:

```js
{ source, matchType: 'failed', comparedValue, matchedValue: null, score: null, reference: null }
```

- `comparedValue` stays populated — it is the submitted value we attempted to compare.
- `matchedValue`, `score`, `reference` are null: the source returned nothing.
- No per-source failure reason for now. (Future: reason shown on hover of the Failed badge.)

## Rendering (`evidenceRowHtml`)

When `matchType === 'failed'`:

- Row gets a `failed` class: disabled background fill; all text (source name, key, values)
  in the disabled text color.
- Match column: neutral grey **Failed** badge using the existing `match-badge` component.
  Not red — a source outage is "no data", not a mismatch, and must not visually compete
  with genuine verification failures.
- No score meter.
- Matched value renders as an em-dash; compared value still shows, greyed.
- Row is **not expandable**: no caret, no reference panel, non-interactive markup.
  There is nothing behind the expand.

## Counting logic

Failed rows must not inflate match counts:

- `matchedSourceCount` filters `score >= 0.8`; null scores already fall out.
- Guard `Math.round(e.score * 100)` (and `showScore`) against a null score.
- Source-group headers still count the failed row as an evidence item (it is a visible row).

## Mock data

Add one failed row to the existing `RESULT_REVIEW` scenario — `clearCorporateFiling`
failing on Business Address — so the state is visible without a new scenario toggle.

## Out of scope

- Per-source failure reasons and the badge hover tooltip.
- Any change to check-level status computation (a failed source alone does not change
  Pass/Review/Fail).
