# CLEAR business sources prototype — Session 2026-07-23

## Resume point
The prototype is complete, hosted, and shared — no code was mid-flight when the
session ended. The work is now in a **feedback/iteration phase**: the user is
testing three navigation variations for a new "associated businesses" feature
and will return with reactions or new variations to try. Next action: whatever
the user asks; likely candidates are in Decisions (⏳ items) and the repo
README's open questions.

## Current state
- **Project root:** `/Users/ronithnair/Experiments/AddingBusinessSourcesInClEAR`
- **`index.html`** — single self-contained prototype (no build step; Google
  Fonts only). Recreates the HyperVerge One (HVOne) dashboard record page for
  the "CLEAR Analysis" module. CLEAR = a third-party due-diligence report on
  loan-applicant businesses (criminal records, liens, bankruptcies, etc.).
- **Live site:** https://ronith-1.github.io/clear-business-sources-prototype/
- **Repo:** https://github.com/ronith-1/clear-business-sources-prototype
  (public, branch `main`, GitHub Pages from main root; push = auto-redeploy)
- The prototype adds "associated businesses" (other companies linked to the
  applicant via owners/transactions, each a full peer with the same 18 output
  sections and its own deep-dive drawer). Three nav variations, switchable via
  the collapsible toggle bottom-right:
  **A** temp tabs · **B** breadcrumb drill-in · **C** "Other Business Sources"
  triage banner.
- All data is mock, generated from the `BUSINESSES` array at the top of the
  `<script>` in `index.html` (18-section template + per-section overrides).
  14 associated businesses; Marina Holdings and Coastal Freight are flagged.
- **`README.md`** — context + variation guide, pushed to the repo.
- New personal skill created this session:
  `~/.claude/skills/beam-out/SKILL.md` (this workflow), subagent-tested twice.

## User directives & preferences
- Works at HyperVerge (`ronith.n@hyperverge.co`); GitHub work account is
  **`ronith-1`** (not `ronithn` — both are logged into `gh`; switch before
  pushing work repos).
- UI must follow the **HVOne design system** — always run the
  `hvone-ui-creator` skill before generating UI for this project.
- Feature title for the triage banner: **"Other Business Sources"** (user-chosen).
- Entity tabs/breadcrumb sit **below** the attempt-tab row, not above.
- Overflow menu opens anchored **under the "+N more businesses" button**.
- Top nav is white-filled with bottom border; its segmented control follows
  Figma spec node `13873:8838` (file `8JeSQR1X9AYqFHSHA4NCuW`) — including
  Inter Medium 500, a deliberate deviation from HVOne's 400/600-only rule.
- Public repo hosting was explicitly approved despite mock-HyperVerge visuals.

## Decisions
- Associated businesses are **full peers** of the applicant (same inputs +
  outputs), not detail under the existing "Affiliations" row.
- Scale target: 1–10 typical, up to 30. Only flagged businesses surface
  prominently; clean ones hide behind a click. Reviewer flow is **read-only**
  (one flag per business, no per-business triage state).
- Relationship (owner / transactions / unknown) is a **metadata field only**;
  ⏳ group-by-owner ("what else does Person B own?") deferred — user wants to
  check with their product manager (also logged in persistent memory).
- Business status is derived from sections (any review ⇒ Review), which makes
  the main business "Review" — ⏳ the original screenshot showed "Pass"
  alongside flagged sections; whether status is independent of section flags
  is an unresolved product question.
- Three variations kept side-by-side deliberately ("build both, compare")
  rather than committing to one.

## Gotchas
- `index.html` was **edited outside the session** at least once (the variation
  toggle gained collapse/expand behavior). Re-read before editing; don't trust
  cached file state.
- In variation A the "+N more" counter shrinks as temp tabs open — correct but
  twitchy; pinning it to the true total is a one-line change if disliked.
- In variation C, chips inside the banner header navigate while the header
  itself toggles the list — dual behavior on one row, flagged as fragile.
- Local preview server: `python3 -m http.server 8742` from the project root
  (Chrome extension can't open `file://` URLs). May still be running.
- Figma asset URLs from the segmented-tab spec expire ~7 days from 2026-07-22
  (none are referenced in code — dividers were rebuilt in CSS).
