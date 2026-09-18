# Vello Interview Synthesis — Verified Findings

*Source: 6 discovery interviews (P01–P06), Kestrel Park. P01–P03 requesters, P04–P05 providers, P06 community admin.*

## Verified Themes

### Strong

**1. Personal vouching from someone you know outweighs any formal signal (certificates, ratings).**
Raised by 4/6 directly (P01, P02, P05, P06), plus 1 indirectly (P03/D — absence-driven, see Corrections).
> "if you gave me the choice between a man with a certificate who I've never seen before and a man with no certificate who Priya has used for two years, I'm taking Priya's man. Every time." — P01
Contradicting: P02 wants this *and* demands formal DBS/ID checks for childcare — the preference is domain-conditional, not universal.

**2. Reliability ("turning up") matters more than safety credentials.**
Raised by 4/6 (P01, P03, P04, P05).
> "Turning up. That's ninety per cent of it. Half the trade won't ring you back." — P05
Contradicting: P03 wanted a specific *time*, not just reliable attendance — a related but distinct scheduling complaint.

### Medium

**3. Star ratings and reviews are actively distrusted.**
Raised by 3/6 (P01, P02, P04).
> "I read them but I don't believe them. Everyone's five stars. The only reviews I trust are the bad ones and there are never any bad ones." — P02
Contradicting: P04 doesn't distrust reviews in the abstract — the objection is a specific unfair review with no recourse (overlaps Theme 5).

**4. "Neighborhood" is social overlap, not geographic distance.**
Raised by 4/6 (P01, P04, P05, P06).
> "It's not really distance, it's whether the paths cross. Priya's four blocks over and that's the same place to me... The estate on the other side of the park... might as well be another town, and it's closer than Priya." — P01
Contradicting: none direct; P06 confirms the same fuzziness from the admin side, no one argues for a hard boundary.

**5. No mechanism exists for a provider to respond to or repair a single negative incident.**
Raised by 3/6 (P04, P05, P06).
> "There was nothing to challenge it with. There's a button, I pressed it, nothing happened. That review is still there and it's the only one I've got." — P04
Contradicting: P05's real-world version of the same failure (broken shelf, mirror) *was* resolved informally in minutes — the informal system isn't broken, mediated platforms remove the repair path.

**6. Formal vetting is strongly desired for childcare specifically — in tension with Theme 1.**
Raised by 1/6 strongly (P02), 1/6 partially (P01).
> "If somebody is going to be alone with my son, I want to see a check. I want to see a DBS, I want to see references I can call, I want to see ID." — P02
Contradicting: the same participant hired his own cleaner with zero formal check, on a neighbor's word alone ("I suppose the checks are for when you don't have a Denise"). Stated preference and revealed behavior diverge.

**7. Disconnected residents are most exposed to being overcharged.**
Raised by 3/6 (P02, P03/D, P06).
> "They're the ones who get charged four hundred pounds for a job worth eighty." — P06
Contradicting: none direct; P06 and D independently use near-identical figures, which corroborates the pattern but isn't independent proof of one incident.

### Thin (noted, not load-bearing)

- Providers economically depend on serving multiple neighborhoods (P04 strong, P05 supporting) — a single-neighborhood restriction would cost P04 "half my income."
- Delegated/proxy booking for a resident who won't use the app (P03/D only, one household).
- Provider-side fee-structure and onboarding-friction complaints (P04 only).
- Desire for a persistent per-household service-history record (P03/D only).

---

## Corrections I Made

Two corrections came out of the audit, and both are worth stating plainly rather than quietly fixing, because they're the kind of error that's easy to make again.

First, I stated P04's gender as fact when the transcript never supports it — no pronoun referring to P04 appears anywhere in that interview, in either the dialogue or the header notes, unlike P05 ("interviewed in his van") or P06 ("a list she maintains"). I'd pattern-matched off the other providers in the set and filled the gap without noticing I'd done it. I made a softer version of the same mistake with P01, inferring "she" from a single line — "I remember saying to my husband" — which makes female likely but isn't the same as the document stating it. Both are now corrected to gender-neutral language. This matters beyond tidiness: if this synthesis feeds into personas or scenario writing, an invented attribute quietly becomes a "fact" that shapes design decisions no one chose to make.

Second, I'd ranked Theme 1 as "raised by 5/6" in a way that implied five equally-weighted endorsements. Pulling every relevant line per participant showed that's not what the data supports: P01, P02, P05, and P06 each make direct, comparative statements ("I'm taking Priya's man, every time"; "it doesn't tell you anything about whether the man can plaster"). P03/D does not — P03 mentions one social-vouching channel without a comparison, and D explicitly falls back to reviews and "gut feeling" specifically *because* she has no local vouch available. That's evidence people want vouching when they don't have it, which is different from evidence that they rate it above alternatives when they do. I've kept the theme at Strong — four independent, comparative sources earn that — but corrected the count language so "5/6" doesn't read as more uniform than it is. Overstating consensus here would matter directly: it's the theme most likely to justify a specific design bet, and the bet should be sized to what four strong data points support, not what five implies.

---

## Problem Statements

**1. Trust signal gap for requesters without an established local network.**
Requesters who lack a long-tenured local social network — new arrivals, residents in transient buildings, or people booking on someone else's behalf — have no reliable way to judge whether an unfamiliar provider will be safe, competent, and fair. The signal that actually resolves this for people who have it — a specific known person's direct vouch — isn't available to them, and the substitutes on offer (star ratings, generic reviews, formal certifications) are ones they describe as uninformative or answering the wrong question. This matters because the gap isn't evenly distributed: it falls hardest on people already most exposed to harm, and the failure mode isn't hypothetical — two independent participants describe residents being charged roughly five times a fair price on jobs they didn't feel able to question.

**2. No mechanism for a provider to contest or repair a single negative incident.**
Providers whose livelihood depends on accumulated reputation, and the informal moderators who vouch for them, have no way to respond to, contextualize, or recover from an isolated negative outcome once it's visible — the available systems treat every incident as a permanent, binary, unmediated public mark. This matters because it's inconsistent with how these same incidents actually resolve when handled person-to-person, where a bad outcome gets fixed in a short conversation and the relationship continues — meaning the mediated version produces a worse result for the identical underlying event, and it's already pushing providers to avoid review-based platforms altogether.

---

## Data Entities & States

Limited to entities tied to Strong/Medium themes — thin themes aren't load-bearing enough to spec against yet.

| Entity | Key attributes | Driven by |
|---|---|---|
| **Vouch** | voucher_id, vouchee_provider_id, relationship strength/tenure to the requester, is_voucher_known_to_viewer (per-viewer, not global) | Theme 1 |
| **Community / Neighborhood** | member-declared boundary (not a fixed geofence), overlapping membership, per-user "my area" distinct from any official boundary | Theme 4 |
| **Provider Profile** | service categories, service area (must support multiple, non-contiguous neighborhoods), formal_credentials as a separate object from vouches | Themes 1, 6 |
| **Credential / Verification record** | type (DBS, ID, insurance, reference), issuing check, visibility flag — explicitly not merged into vouch strength or trust score | Theme 6 |
| **Booking / Job** | scheduled_time (exact, not a window), price_quote shown pre-booking, recurring_flag, status | Themes 2, 7 |
| **Dispute / Resolution** | linked booking_id, provider_response field, resolution_state kept separate from the public review, escalation path | Theme 5 |
| **Review** | weighted below vouches, includes provider-response field, carries job-scope context (e.g., what was and wasn't included) | Themes 3, 5 |
