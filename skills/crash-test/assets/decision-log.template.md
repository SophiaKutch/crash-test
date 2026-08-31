# Decision Log — <feature name>

This is the deliverable. Not a plan the agent wrote, not a plan the engineer
wrote — one plan where every point of divergence was surfaced, ruled on by the
engineer, and recorded.

Status: ☐ converging · ☐ converged
Runs completed: <n>

---

## D<n> — <the decision, stated as a sentence>

**Forced by:** Run <n>, <Dummy>, step <s>, Gate <Gx>, class `<GAP|CONFLICT|PREDICTION>`

> "<the plan line that was judged, quoted verbatim>"

**Agent recommended:** <what you proposed>
**Citation:** `[prior-art]` <product, url> · `[lens]` <principle>

**Engineer ruled:** <what they decided>

**Divergence:** ☐ none — recommendation accepted · ☐ substituted · ☐ overruled

**Reason for divergence:** <in the engineer's words, when they differed. This
field is why the log exists — it is the record of where the agent's model of the
product was wrong, and why.>

**Cascade:** <what this ruling invalidated or newly opened>

---

## Accepted with eyes open

Findings the engineer chose not to fix. Each needs a reason; "accepted" without a
reason is indistinguishable from "missed".

| ID | Finding | Class | Why accepted | Revisit when |
|---|---|---|---|---|
| | | | | |

---

## Never verified

Things neither of you could establish. State them plainly rather than letting a
clean coverage table imply they were checked.

| Question | Why unresolved | Cheapest way to find out |
|---|---|---|
| | | |

---

## Convergence

| Criterion | Status |
|---|---|
| Every dummy completes every job | ☐ |
| Zero `GAP`s in the walkable spec | ☐ |
| Zero `CONFLICT`s | ☐ |
| Every `PREDICTION` fixed or explicitly accepted | ☐ |

**Standing caveat:** a full coverage table means the plan is *coherent*. It is
not evidence the product works. Dummies are stand-ins; they over-comply,
under-fumble, and miss habit, laziness, and indifference entirely. Real users
remain unsimulated.
