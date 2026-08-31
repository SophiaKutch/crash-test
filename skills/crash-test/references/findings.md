# Findings: Classes, Severity, Halting

## The three classes

Every finding is exactly one of these. The class determines how much the engineer should trust it, so misclassifying is worse than missing.

### GAP — the plan is silent

A field of the compiled spec cannot be filled from the plan. **Objective**: the engineer can verify it by reading their own document.

```
GAP · G4 — plan specifies a success toast, says nothing about undo.
   > "Clicking Archive archives the selected rows and shows a success toast."
   Cannot determine what happens when the wrong rows are archived.
   Steps 4–5 are unknowable until specified.
```

Do not fill a gap with something plausible. The plausible thing is what the engineer would have written if they had thought about it, and the whole point is that they did not. Filling gaps silently is the single most damaging thing you can do in this skill — it converts a finding into a hidden assumption.

Gaps dominate early runs. That is the expected shape, not a sign you cast badly.

### CONFLICT — two things cannot both be true

Two parts of the plan contradict, or the plan contradicts the codebase, an established convention, or a prior decision. **Objective**: verifiable, with two citations.

```
CONFLICT — "hidden from the default list view" implies a non-default view exists.
   > "Archived rows are hidden from the default list view."
   No such view is defined anywhere in the plan, and none exists in src/list/.
   As written, archiving is neither verifiable nor reversible.
```

Always cite both sides. A conflict with one citation is a prediction wearing a costume.

### PREDICTION — the plan is complete and you still expect failure

The step is fully specified, you can quote it, and you believe a human fails anyway. **Subjective.** This is the only class that depends on trusting simulated behaviour, and it must always carry a confidence level and the reasoning that produced it.

```
PREDICTION · G3 · confidence: medium
   > "Label the button 'Archive'."
   Marcus arrives from a tool where archive meant permanent delete. He may
   avoid the button entirely rather than risk it.
   Basis: [competitor-reviews] two G2 reviews describe exactly this confusion.
   Weakest finding in this run — engineer's call.
```

Confidence levels:

- **high** — the mechanism is well established and the evidence is specific to this dummy's stated traits
- **medium** — the mechanism is plausible and there is some supporting prior art
- **low** — you are reasoning from first principles with no external evidence. Say so. Consider asking the engineer to play the dummy instead of asserting it.

Never present a prediction with the same confidence as a gap. The engineer's willingness to trust this skill depends on you being visibly honest about which findings are facts about their document and which are your guesses about people.

### VERIFIED — a subclass, and the strongest thing you can produce

Some gate failures can be *checked* rather than predicted, when a running implementation or a rendered artifact exists: contrast ratio, tap target size, tab order, focus visibility, whether text overflows at a stated width, whether a label exceeds its container. Run the check, report the number, mark it `VERIFIED`.

Never blur `VERIFIED` and `PREDICTED`. A measured 2.9:1 contrast ratio and a hunch that a label reads oddly are not the same kind of claim.

## Severity

**Blocking** — the dummy cannot correctly proceed, *or* the plan's silence makes downstream steps indeterminate. Only blockers go to the engineer for a ruling in Phase 3.

**Friction** — the dummy reaches the goal and pays a cost: extra clicks, a moment of doubt, a recoverable wrong turn. Logged in the run, revisited at convergence, never allowed to crowd out a blocker.

The distinction exists because attention is the scarce resource. A run that surfaces 30 findings gets skimmed; a run that surfaces 4 blockers and files 26 frictions gets acted on.

## Halting

A walk halts at the first **blocking** finding for that dummy. Do not narrate past it.

This is not a stylistic preference. Past a blocker you are inventing: you do not know what the recovered state looks like, so every subsequent step is fiction dressed as analysis. Halting is also what makes the frontier real — steps beyond a blocker are genuinely unknowable until the engineer rules, and their ruling is what advances reach.

When you halt, always report:

- the step number and the total (`step 3 of 7`)
- which steps became unreachable as a result
- which other dummies are blocked by the same finding
- what unblocking would reveal

Continue with the remaining dummies. A dummy who reaches step 7 is doing useful work while another is stuck at step 3.

## Caps and honesty

Cap findings at roughly 12 per run. If you hit the cap, say so and say what you dropped:

```
Capped at 12 findings. 4 friction items in Job B not reported this run:
labelling in the filter row, two ordering questions, one empty-state copy gap.
Will surface after the current blockers are ruled on.
```

Silent truncation reads as "we covered everything." That is the one failure mode that makes the coverage table actively misleading.
