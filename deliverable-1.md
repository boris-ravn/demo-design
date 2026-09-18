# Vello v1 — Process Debrief

## What Design Owns at Ravn

Running this exercise didn't change what design owns — it clarified it. Design owns the definition of trust, not just its rendering: deciding that "verified" should mean something residents can feel (building-level scoping, shared reviews) rather than a generic star rating was a strategy call no engineering input made for us. Design also owns craft decisions that carry real risk if delegated — how the VerifiedBadge uses shape *and* color so it doesn't read as decoration, how much of a Provider's twenty-two years of craft gets visual space instead of a sterile listing, how Admin's moderation UI asserts authority without feeling punitive. And design owns the final call on what "done" means for a real user — deciding to validate against a 71-year-old resident living alone, not just a demo-friendly persona, was a judgment call about who the product is actually for. Engineering can tell us what's feasible or cheap; it can't tell us what should feel trustworthy to a neighbor.

## What Engineering Contributes Across the Lifecycle

Once I corrected the sequencing, the real shape is: **Discover** — feasibility framing only ("can we even infer which building an address is in?"), no solutioning. **Define** — categories of approach and their rough cost/compliance shape (documents-based check vs. admin-vouching), still no vendor names. **Architect** — concrete infra options (real-time chat pattern, payout processor class) compared on complexity, because IA decisions actually depend on which pattern gets picked. **Design** — flagging which trust signals are cheap (denormalized counters) vs. expensive (live mutual-connection joins) before a component spec locks. **Validate/Handoff** — instrumenting the funnel before the test plan is written, not after launch. The throughline: engineering's contribution gets more concrete as the phases progress, never more concrete than the phase can support.

## One Decision I'd Reframe as a System Decision

The strongest example is verification status. It shows up as a design decision — "what does the badge say?" — but it's actually a data-model decision wearing design's clothes. A Provider isn't just verified-or-not; they move through pending → verified → flagged → suspended, and Admin needs to see *why* a status changed, not just *that* it changed. That forces a state machine with an audit trail, not a boolean `is_verified` flag. If this gets decided at the Design phase, by the badge component, it's already too late — the schema and the Admin-desk history view both depend on a decision that looks cosmetic but is structural.

## Two Pushbacks I Made

**On the token/component count**, I didn't accept the homepage's "170 tokens / 17 components" as fact — I asked for it to be checked against the actual CSS files and bundle manifest. It came back technically correct, but with a catch: 17 is really 16 distinct components, since `VerifiedMark` and `VerifiedBadge` share one source file. Small thing, but it's the difference between citing a number and citing a number I've confirmed.

**On the engineer's-contribution column**, I pushed back because it assumed real backend infra and vendor budget that a from-scratch v1 doesn't have. The fix wasn't cosmetic — it moved ID-verification vendor selection out of Discover entirely, since at that stage there's no scope, compliance posture, or budget to select against. That's the kind of error that looks reasonable until you ask "what could an engineer actually know at this point?"