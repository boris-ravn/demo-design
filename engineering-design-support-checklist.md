# Engineering → Design Support Checklist

Grown from Monday's phase → contribution → failure-mode table (see
[deliverable-1.md](deliverable-1.md)), then verified against what
actually happened later in the week rather than left as theory.

- **Discover: name data/feasibility constraints early, not solutions.**
  The right question at this phase is "can we even infer which building
  an address is in?" — feasibility framing only, no solutioning. The
  live failure mode to watch for: assuming a clean data source exists
  without checking. This week's own entity model has to treat
  "neighborhood" as a member-declared boundary rather than a fixed
  geofence, precisely because no clean boundary data source exists to
  assume. See [deliverable-1.md](deliverable-1.md) and the Community /
  Neighborhood entity in [deliverable-2.md](deliverable-2.md).

- **Define: offer categories of tradeoff, not a vendor pick.**
  Right shape: compare approach categories (documents-based check vs.
  admin-vouching) and their rough cost/compliance shape, with no vendor
  named. The failure mode we actually made this week: ID-verification
  vendor selection got placed in Discover, before there was scope,
  compliance posture, or budget to select a vendor against. It had to
  be moved out. See the second pushback in
  [deliverable-1.md](deliverable-1.md).

- **Architect: give design a menu of feasible infra patterns before a
  flow assumes a capability that doesn't exist.**
  Confirmed this week: the flow drafted from the brief invented a
  standalone "Admin Desk" nav entry that doesn't exist in the real
  product — the actual entry point is buried under Profile → Staff
  tools. The flow had to be rebuilt against the live prototype, not the
  brief's copy. See [deliverable-3.md](deliverable-3.md).

- **Design: audit every generated component against the real design
  system before calling it done.**
  Token lint → accessibility scan → reference comparison — never accept
  "it renders" as "it's done." This is the literal audit method used
  this week (`vello-fidelity-review`), and it's what caught 7 real
  drift fixes plus a systemic Badge contrast gap that a
  render-only check would have missed. See
  [FIDELITY-AUDIT.md](project/FIDELITY-AUDIT.md).

- **Validate/Handoff: keep a living unresolved-values log instead of
  letting undocumented gaps rot silently in code.**
  When a value doesn't trace to a token, or the system's own docs fail
  their own stated standard, it gets written down as a named question
  for a designer — not silently guessed, not silently ignored. See
  [UNRESOLVED-QUESTIONS.md](project/UNRESOLVED-QUESTIONS.md), which
  exists specifically for this.

- **Cross-cutting habit confirmed this week: verify a visual/screenshot
  read against real computed DOM styles before trusting it.**
  A confident-looking read can still be wrong at the pixel level: the
  header IconButtons looked undersized/sub-44px on inspection, but
  measuring the actual computed size (39.45px rendered ÷ the phone
  mockup's 0.8966 scale transform) showed they're exactly 44px logical
  — that claim was a false positive, only caught by inspecting real DOM
  values instead of trusting the visual read. The same discipline
  applied to a doc claim, not just a screenshot: Badge's docs assert
  "every variant meets 4.5:1," and computing it directly showed that's
  false for 3 of 8 variants. Treat a confident, specific-sounding AI or
  doc claim as something to check, not a fact. See
  [deliverable-4.md](deliverable-4.md), finding #4, and
  [UNRESOLVED-QUESTIONS.md](project/UNRESOLVED-QUESTIONS.md).
