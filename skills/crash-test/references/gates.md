# The Four Gates

Every step of a walk is evaluated against four gates, in order. They come from the **cognitive walkthrough**, an established usability inspection method (Wharton, Rieman, Lewis & Polson, *The Cognitive Walkthrough Method: A Practitioner's Guide*, 1994). Using a real method rather than improvised critique is what makes the output defensible.

Evaluate gates **in order** and stop at the first failure. A user who never forms the goal (G1) cannot fail to notice the button (G2) — reporting both is noise, and it inflates finding counts without adding information.

---

## G1 — Goal

> Will the dummy try to achieve the right thing at this step?

This is about the user's *intent*, formed before they look at the screen. It fails when the product's model of the task diverges from the user's model of their own task.

**Failure signatures**

- The step assumes the user knows a concept the product invented (`workspace`, `pipeline`, `case`) and never taught.
- The user's goal is framed in time (*"what changed while I was gone"*) and the product is framed in objects (*"the records list"*).
- The step is the second half of a task the user thinks of as one thing, so they never look for it.
- The user does not know this step exists at all, and nothing upstream implied it.

**What to quote:** the upstream step that was supposed to set up this intention, or the absence of one.

**Typical fix shapes:** rename to the user's noun; collapse the step into the one before it; add a signpost upstream; default the choice so no goal is required.

---

## G2 — Notice

> Will the dummy notice that the correct action is available?

Purely about perception, not comprehension. The control exists — can they see it?

**Failure signatures**

- The action is the same visual weight as five others.
- It lives behind a hover, a right-click, a kebab menu, or a keyboard shortcut with no visible entry point.
- It is below the fold at the dummy's actual viewport, especially the mobile one.
- Meaning is carried by colour alone, which fails for the worst-case dummy.
- It appears only after a state change the dummy has no reason to trigger.

**What to quote:** the `visible` and `copy` fields of the step's compiled spec.

**Note:** several G2 failures are *verifiable*, not predicted — contrast ratio, tap target size, position at a stated viewport. When you can check it, check it, and mark it `VERIFIED`.

---

## G3 — Associate

> Will the dummy connect that action to the outcome they want?

They can see the control. Do they believe it does the thing? This gate is where microcopy lives, and it is the single most common real failure in engineer-authored plans.

**Failure signatures**

- The label is a system verb (`Process`, `Submit`, `Sync`, `Run`) rather than an outcome.
- Two controls plausibly do the job and nothing distinguishes them.
- The label collides with a different meaning in the tool the dummy arrived from (`Archive` meaning delete; `Save` meaning publish).
- The consequence is destructive or irreversible and the label does not say so.
- The label is accurate to the implementation and meaningless to the user.

**What to quote:** the literal `copy` string. If the plan does not specify the string, that is a `GAP` — and a very common one. Plans that say "add an archive button" have not specified the button.

**Typical fix shapes:** name the outcome, not the mechanism; state the scope (*"Archive 40 selected"*); add the consequence to the label or a confirm step.

---

## G4 — Feedback

> Will the dummy see that it worked?

The action was taken correctly. Does the system close the loop?

**Failure signatures**

- Success is signalled by something disappearing.
- A toast is the only confirmation, and the dummy's context makes them miss it — interrupted, scrolling, on mobile, using a screen reader.
- The operation is slow and the plan specifies no interim state.
- The operation is reversible in theory but the plan never says how, or the undo window is shorter than the dummy's attention.
- The result is invisible from where they are standing, so they cannot verify it without navigating away.
- Failure and success look the same.

**What to quote:** the `next` field. `next` fields that only describe the happy outcome are the standard G4 gap.

**Typical fix shapes:** persistent confirmation over transient; show the result in place; specify the interim state; make undo durable rather than timed; state what the user sees when it fails.

---

## Gates and severity

A gate failure is **blocking** when the dummy cannot correctly proceed, or when the plan's silence makes every downstream step unknowable. It is **friction** when they get there but pay a cost.

G1 and G3 failures are usually blocking — a wrong intention or a wrong control means the wrong thing happens. G2 is often friction, unless the control is genuinely undiscoverable. G4 failures block whenever the missing feedback makes the *next* step's preconditions indeterminate: if the plan never says what the post-save screen shows, you cannot walk the post-save screen.

Only blocking failures go to the engineer for a ruling. Friction is logged in the run and revisited at convergence.
