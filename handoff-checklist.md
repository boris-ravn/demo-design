# Design → Engineering Handoff Checklist

What design owes engineering for a faithful build. Every line below is
drawn from a gap this week's audit actually found on the Grace Lin
screen and the moderation-queue flow — not a hypothetical.

1. **Every token used has a name, not a visual match.**
   No "persimmon by eye." The Available badge went from a hand-matched
   `accent` (`coral-700` on `coral-100`) to a named `success` variant
   (`green-700` on `green-100`) via the real `Badge` component — not a
   re-eyeballed hex. See the token-drift table in
   [FIDELITY-AUDIT.md](https://github.com/boris-ravn/vello-assignment/blob/main/FIDELITY-AUDIT.md).

2. **Every UI state is specified, not just the happy path.**
   This week's actual gap list: loading/skeleton, zero-review,
   empty-offerings, unavailable, verification-pending, and
   archived/inactive on the profile screen; plus flow-level state gaps —
   the moderation queue's Escalate action has no confirmation, no toast,
   and an undocumented destination, and there is no case type at all for
   a post-verification complaint against an already-verified provider.
   See [deliverable-3.md](deliverable-3.md) (State/API Contract table),
   [deliverable-4.md](deliverable-4.md) (Missing States, items 14–18),
   and the backlog in
   [UNRESOLVED-QUESTIONS.md](https://github.com/boris-ravn/vello-assignment/blob/main/UNRESOLVED-QUESTIONS.md).

3. **Touch target and semantic role are stated for every interactive
   element, not just its appearance.**
   Every clickable element should be a real `<button>`/`<a>`, not a
   `div` — and don't trust a screenshot's apparent size. The header
   IconButtons looked undersized at a glance; they're actually exactly
   44px logical once you correct for the phone mockup's 0.8966 scale
   transform (39.45px rendered ÷ 0.8966). That correction only came from
   inspecting computed DOM values, not from looking again. See
   [deliverable-4.md](deliverable-4.md), items 11–12 and finding #4.

4. **A named contrast check has run on every text/background pair,
   including badge/tint combinations — not just body text.**
   This week found 3 of 8 Badge variants fail the system's own 4.5:1
   claim: `neutral` (4.319:1), `accent` (4.105:1), and `warning`
   (2.841:1, badly). Only `brand`/`success`, `info`, `danger`, and
   `solid` clear it. See the variant table in
   [UNRESOLVED-QUESTIONS.md](https://github.com/boris-ravn/vello-assignment/blob/main/UNRESOLVED-QUESTIONS.md).

5. **Ambiguous values are logged as an open question for a designer,
   not left for engineering to guess.**
   Three live examples this week, each written down rather than
   silently resolved: whether `--text-muted`'s 4.508:1 margin (a
   hundredth of a point over the floor) is intentional; whether the
   "every variant meets 4.5:1" claim is stale or was never re-verified;
   and what the Book CTA should do when a provider is `unavailable`
   (undocumented — not guessed). See
   [UNRESOLVED-QUESTIONS.md](https://github.com/boris-ravn/vello-assignment/blob/main/UNRESOLVED-QUESTIONS.md).

6. **The state/API contract is documented alongside the flow, not
   implicit.**
   The moderation-queue flow ships with a State/API Contract table —
   trigger, HTTP status, endpoint, and resulting UI per case outcome —
   including the honest gap that Escalate's destination is currently
   "unknown" rather than assumed. See
   [deliverable-3.md](deliverable-3.md).

7. **If the design system's own docs assert something that isn't true,
   that gets reported and fixed — engineering builds trusting the
   documented claim.**
   Badge's docs state "every variant meets 4.5:1 for its text on its
   tint." That's false for `neutral`, `accent`, and `warning` (see #4),
   confirmed via relative-luminance calculation on the docs' own
   rendered colors, not estimated. It's recorded as an open question
   rather than patched over, because the next person to build a Badge
   variant will trust the doc's claim exactly as written. See
   [UNRESOLVED-QUESTIONS.md](https://github.com/boris-ravn/vello-assignment/blob/main/UNRESOLVED-QUESTIONS.md).
