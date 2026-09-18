# Vello Moderation Queue — Flow Verification Notes

## Flow Summary

Admin opens Profile, scrolls to "Staff tools," and taps "Moderation queue." That lands on a shared inbox mixing five case types — ID checks, Flagged reviews, Reported messages, Reported requests, Payment disputes — filterable by type, each row tagged with a severity (High/Medium/Low). Provider ID-verification is just one of those five types, worked through the same queue as everything else, not a dedicated screen.

Opening a case shows a Review report: Reported by / What was reported / Evidence / Account history, then one of three actions. Approve is a single tap — no reason, no confirmation. Reject opens a required, single-select reason picker (Policy violation, Not enough evidence, Duplicate report, Out of scope, Needs legal review), then a Confirm reject / Never mind step. Escalate is a single tap with no reason and no confirmation — it just removes the case from the queue.

---

## What I Caught by Testing the Real Prototype

I didn't just re-derive this flow from the brief a second time — I went and clicked the actual prototype, and it caught three things I would've shipped wrong otherwise.

First, the entry point isn't where I assumed. My first draft invented a standalone "Admin Desk" nav item because that's what the brief's copy implies ("work the admin verification desk"). It isn't there. I had to go find it myself, buried at the bottom of Profile, under a "Staff tools" section that only makes sense once you're looking at it as a logged-in admin — "You're on the Bay Ridge trust & safety team," then a Moderation queue row with an open-case badge. If I hadn't clicked through, I'd have drawn a flow for a screen that doesn't exist.

Second, I reproduced the Escalate bug myself. I opened an ID-check case, tapped Escalate, and the case just vanished from the queue — no confirmation dialog, no reason prompt, no toast, nothing. I was dropped straight into the next case's detail view with zero indication of what had actually happened or where the case went. I only noticed this because I'd just tested Reject on a different case seconds earlier, which *does* stop and ask for a reason before doing anything. Seeing the same three-button row behave two completely different ways, back to back, is what tipped me off — I wasn't looking for a bug, I ran into it.

Third, the confirmed gap. P06's interview describes pulling an already-approved decorator off her list after a single bad job — a live complaint against someone already trusted. I went looking for where that would land in the real queue and it doesn't. I opened a live example of each of the five case types (ID check, Flagged review, Reported message, Reported request, Payment dispute) to check, not just read their labels. Flagged review turned out to be about a single review's *content* — I opened one and it was a privacy complaint about a review naming a minor, filed by the provider. None of the five is "an already-verified provider, reported for how they did the job." That scenario has no case type to become — not just no appeal once it's in the system, no way in at all.

---

## State/API Contract (condensed)

| State | Category | Trigger | API status | UI |
|---|---|---|---|---|
| **Approved** | Case outcome | Admin taps Approve — verified: single tap, no reason, no confirm | `200` `PATCH /moderation/cases/:id/decision {action: approve}` | Toast "Item approved" (verified copy), case removed, queue count decrements |
| **Rejected** | Case outcome | Admin taps Reject → picks one of 5 required reasons → Confirm reject — verified | `200` `PATCH …/decision {action: reject, reason}` | Case removed from queue; reappearance on reapply unverified |
| **Escalated** | Case outcome | Admin taps Escalate — verified: single tap, no reason, no confirm | `200` `PATCH …/decision {action: escalate}` | Case vanishes instantly, no toast observed, destination unknown |
| **Post-verification complaint** | Confirmed gap | Already-verified provider gets a live service complaint (P06's decorator story) | No corresponding endpoint or case type exists in any of the 5 types | No screen represents this scenario at all — checked directly, not inferred |

---

## Open Questions for the Real Team

1. **Does a severity filter actually exist, or is severity display-only?** I found five type tabs (All / ID checks / Reviews / Messages / Requests / Disputes) but no separate control for High/Medium/Low. Before anyone builds against "filterable by type and severity," someone needs to confirm whether severity filtering is a planned feature, a cut feature, or was never in scope — that changes how the queue's information architecture should be spec'd.

2. **Where does an escalated case actually go?** Right now it disappears with no visible handoff. The team needs to decide: does it go to a named senior reviewer, a separate queue, an external system? And does the original admin ever get told the outcome, or is escalation a one-way handoff they lose visibility into? This is a product decision, not just a missing toast.

3. **Is post-verification provider moderation genuinely out of scope, or just not built yet?** If it's intentional — maybe handled entirely outside the app, the way P06 currently does it by hand — that's a legitimate scope call, but it should be a documented one. If it's an oversight, it's a bigger gap than a missing appeal flow: it means there's currently no way for a live complaint about a working provider to reach an admin at all.
