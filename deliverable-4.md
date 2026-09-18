# Grace Lin Provider Detail — Human vs AI Audit

## Screen

Grace Lin's provider detail ("Neighbor") screen — math/SAT tutor profile with trust signals, stats, and a booking CTA, audited against the Vello Design System.

---

## My Pass (No AI)

**Visual hierarchy** — Eye lands in this order: (1) "Book Grace" button — large, high-contrast fill, bottom of screen; (2) the stat block (rating/price/response time) — pulls attention purely from size relative to its surroundings; (3) the "Available" label — pulls the eye by color alone, not size or position.

**Token consistency** — "Available" label's color looks like it might not be a correctly-applied token, but flagging as suspicious, not confirmed.

**Accessibility** — Subtitle text under the name ("Math & SAT tutoring") looks low-contrast against the background — weakest-confidence observation at a glance. Both icon-only buttons at the top (back arrow, heart/like) look smaller than a comfortable tap target, with no visible button boundary around either. Every interactive element on this screen (back, heart, message icon, Book Grace) should be a real `<button>`, not a clickable `<div>`. Focus order should follow the screen's visual/reading order top-to-bottom — back arrow, then heart, then page content, then "Book Grace" last, since it sits in a sticky footer at the bottom. (Corrected myself here: first assumed focus order should prioritize the most important action first; that's wrong — order should match visual position, not importance, so keyboard/screen-reader users build a predictable mental map of the page.)

**Missing states** — No-reviews/new-provider state (what replaces the rating block at zero reviews); unavailable state (what changes — badge, CTA — when the provider isn't currently available); error state (what shows if the profile fails to load).

**Additional observation** — The "Available" label's color reads as visually similar to the color used for the "Reject" action in the Wednesday moderation-queue flow. If a warm/reddish hue is used for both a positive status (available) and a negative action (reject) elsewhere in the same app, that's a semantic color consistency concern worth checking against the system, beyond just "is this the right token."

---

## Your Pass (AI)

**Hierarchy**
- *Medium* — Trust pill ("On Vello since 2025 · Background-checked") renders at body-copy weight, contradicting the system's "trust is visible, never fine print" principle.
- *Medium* — Rating shown as a bare number with no amber stars, weakening the intended scan hierarchy.
- *Low* — Price, rating, and response time share identical size/weight despite unequal decision importance.

**Token consistency**
- *High* — "Available" badge uses accent/persimmon instead of Badge's `success` variant (distance/status variant misuse).
- *Medium* — "14 min walk" badge uses success/brand-green styling instead of the documented `neutral` treatment for distance metadata.
- *Medium* — Header IconButtons (back, heart) use the `ghost` variant (transparent border) while the message icon on the same screen uses `secondary` (visible border) — a real style inconsistency, not a size defect.
- *Medium* — Combined trust pill exceeds Badge's three-word guideline and fuses two distinct facts into one non-standard composition.
- *Low* — The one legitimate use of persimmon in the system (favorite/save state) is unused, while persimmon is misapplied elsewhere.

**Accessibility**
- *High* — "Available" badge text-on-tint contrast, initially cited against the Color page's 3.3:1 warning.
- *Open* — Focus order and `aria-pressed` on the heart icon can't be confirmed from a screenshot.

**Missing states**
- Zero-review/new-provider fallback, unavailable state, empty-offerings state, loading/skeleton state, error state, and a hard content clip under the sticky footer with no scroll-affordance signal (fade/peek).

---

## What Each Pass Caught Uniquely

**What I caught that the AI didn't:**
- The subtitle text contrast concern — confirmed later as a real, borderline-passing value, not a false alarm.
- The blanket semantic requirement that every interactive element be a real `<button>`, not a `<div>`.
- The focus-order principle itself (visual order over importance-order), including my own self-correction — the AI only flagged focus order as "unverifiable," it didn't state the underlying rule.
- The cross-flow color echo between "Available" and the moderation queue's "Reject" — outside the scope of a single-screen audit, only visible if you've seen both flows.
- My token-consistency note was a hunch ("suspicious, not confirmed"); the AI's was backed by the specific component doc and variant name.

**What the AI caught that I missed entirely:**
- The specific wrong-variant diagnosis on both badges (`accent` instead of `success`, brand-green instead of `neutral`), not just "this color looks off."
- The internal inconsistency between the back/heart icons' `ghost`-variant styling and the message icon's `secondary`-variant styling on the same screen.
- The trust pill's length/word-count violation and its two-facts-in-one-badge composition.
- The unused favorite/save persimmon state as the flip side of the "Available" badge's misuse.
- Three additional missing states I didn't list: empty-offerings, loading/skeleton, and the hard content clip under the sticky footer.

---

## The Pushback I Made

I pushed back on the "Available" badge contrast finding because it looked like the AI was citing two different parts of the docs against each other — the Color page's line about `--accent` on white being 3.3:1, versus the Badge page's flat claim that "every variant meets 4.5:1 for its text on its tint." Those can't both be treated as true of the same pairing without someone explaining which one actually applies here. I asked it to pick a side: either this is Badge's own calibrated accent-on-tint pairing (in which case the contrast finding is wrong and it's just a wrong-variant issue), or it's a raw persimmon value bypassing the component (in which case the finding stands, but say so plainly).

It didn't just restate a citation to get out of it — it actually worked out the WCAG contrast math for the darkest documented persimmon shade (`--coral-700`) against the accent tint and got roughly 4.1:1, short of 4.5:1 even in the best-case documented scenario. Net result at the time: the finding survived the challenge — it didn't walk it back, it sharpened it into "this is a raw/uncalibrated color, not Badge's documented-safe pairing." Friday's DOM inspection of the live prototype updated that conclusion again: the actual computed colors (`#C5421F` on `#FCE3D9`) turned out to be exactly `--coral-700` on `--accent-tint` — Badge's own documented accent pairing, not a bypass. So the finding survived a second time, but the diagnosis flipped: this isn't implementation drift, it's a gap in the design system's own accent variant, which doesn't clear 4.5:1 even when used exactly as specified.

Smaller thing: I also caught that it said Accent-usage defines "exactly four" sanctioned persimmon uses but only actually named three good ones — the fourth item it counted was itself a "don't do this" example mislabeled under the same heading. It agreed and corrected the count to three without any pushback needed.

---

## Resolved Findings (Final List)

Ranked by severity, both passes merged, disagreements settled where possible.

### High
1. **"Available" badge: wrong variant, and the variant itself fails contrast.** Confirmed on both counts, but the diagnosis changed after inspecting the live prototype: the rendered colors (`#C5421F` on `#FCE3D9`) are exactly `--coral-700` on `--accent-tint` — Badge's own documented accent-variant pairing, not a raw/bypassed color as first suspected. Semantically it should still be `success`, not `accent`, for a live status. But even used exactly as specified, that pairing computes to ~4.1:1, short of 4.5:1 — a gap in the design system's own accent variant, not implementation drift.

### Medium
2. **Subtitle text ("Math & SAT tutoring") sits at the contrast floor.** Confirmed via calculation — `--text-muted` on `--color-bg` computes to ~4.5:1, technically passing but with essentially zero margin, which is why it reads as low-contrast at a glance. Human instinct was correct; this needs a safer color if there's any risk of rendering variance.
3. **"14 min walk" badge uses success/brand-green instead of `neutral`.** Confirmed via computed styles (background `--green-50`, text `--text-brand`) — distance/duration metadata is explicitly a `neutral`-variant case.
4. **Header IconButtons (back, heart) use a different variant than the message icon.** Confirmed — back/heart use `ghost` (transparent border), the message icon uses `secondary` (visible border). A real style inconsistency; the earlier claim that these were undersized/sub-44px was a false positive — the real component measures exactly 44px logical (39.45px rendered ÷ the phone mockup's 0.8966 scale transform).
5. **Combined trust pill is oversized and fuses two facts into one badge.** Confirmed — exceeds the three-word guideline; "since 2025" and "background-checked" are two separate claims wearing one badge.
6. **Trust pill's visual weight matches ordinary body copy.** Confirmed — contradicts the "trust is visible, never fine print" principle; the pill should stand apart from the paragraph above it.
7. **Content under the sticky footer is hard-clipped with no scroll-affordance signal.** Confirmed as observed — no fade or peek cue tells the user "What Grace offers" continues below the fold.

### Low
8. **Rating shown as a bare number, no amber stars.** Confirmed — breaks Rating component fidelity and weakens scan hierarchy, though not an accessibility failure per the system's own accessibility note (the numeric value is the documented accessible content).
9. **Stat block has no size/weight differentiation** despite unequal importance across price, rating, and response time.
10. **Favorite/save persimmon state is unused** — the one legitimate persimmon use case in the whole system, sitting idle while persimmon is misapplied on the "Available" badge instead.

### Unresolved / Needs Code or Cross-Flow Check
11. **Every interactive element should be a real `<button>`/`<a>`, not a clickable `<div>`.** Can't be verified from a screenshot — flagged for code inspection.
12. **Focus order should follow visual/reading order** (back → heart → page content → "Book Grace" last, sticky footer) rather than importance. Implementation guidance, not a currently-observed defect — verify in code.
13. **"Available" vs. "Reject" color echo across flows.** Flagged only by the human pass, based on having seen the separate moderation-queue flow; not independently verifiable within a single-screen audit. If confirmed, this elevates the badge's wrong-variant issue from a local misuse to a system-wide semantic color collision (same hue signaling opposite meanings in two flows).

### Missing States (design coverage gaps, not currently-visible bugs)
14. No zero-review/new-provider fallback (Rating's own spec calls for a "New" badge instead of a score).
15. No unavailable-state design (badge/CTA change when `available: false`).
16. No empty-offerings state for "What Grace offers."
17. No loading/skeleton state.
18. No error state for a failed profile load.
