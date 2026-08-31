# UX Lenses

The vocabulary you cite when recommending a fix. Name the principle, do not lecture: one line of principle attached to a concrete failure teaches more than a paragraph of theory, and the engineer is reading this in the middle of their own work.

Every recommendation in a walk should carry a `[lens]` tag pointing at one of these.

---

## Hierarchy and attention

**One primary action per view.** If two things are equally emphasised, neither is primary and the user must decide what the screen is for. Most G2 failures are really hierarchy failures: the action was visible and simply lost among peers.

**The squint test.** Blur the layout in your mind's eye. What survives? If the primary action does not survive, hierarchy is carried by detail the user never processes. Applies just as well to a compiled spec: if the `visible` field lists nine items of equal weight, the step has no hierarchy.

**Never encode meaning in colour alone.** Fails for the worst-case dummy, fails in greyscale, fails for the 8% of men with colour vision deficiency. Colour plus shape, position, or text.

**Progressive disclosure.** Show what the frequent case needs; keep the rest reachable. Its failure mode is the opposite of clutter — burying something that most users need every time.

---

## Cognitive load

**Recognition over recall.** Do not require users to remember something the interface could show them. Any step whose `visible` field is missing information the user needed from an earlier step is a recall failure.

**Don't make them count, compute, or convert.** If the plan requires the user to work out how many rows are selected, or convert a date range in their head, the product should have done it.

**Interaction cost.** Count the actions the dummy's goal requires. A goal that takes 40 clicks for a 400-row user is not solved by making each click nicer. High-volume dummies exist to find exactly this.

**Match the user's model, not the schema's.** Product nouns invented for the data model (`entity`, `record`, `case`) leak into labels and cause G1 failures. Name things the way the user names them.

---

## Affordance and microcopy

**Label the outcome, not the mechanism.** `Process` and `Submit` describe the code. `Send to insurance` and `Archive 40 rows` describe what happens.

**State the scope of a bulk action in the control.** `Archive` versus `Archive 40 selected`. The second lets the user catch an error before it happens, which is worth more than any confirmation dialog.

**Destructive actions announce themselves.** Irreversibility belongs in the label or in a confirm step — not discovered afterwards.

**Errors: what happened, why, what to do now.** All three. `Something went wrong` has none. Blame the system, never the user, and never expose a stack trace, an internal ID, or anything about someone else's data.

**Empty states are onboarding.** "No results" is a dead end. An empty state should say what goes here, why it is empty, and offer the action that fills it. And distinguish *never used* from *your filter matched nothing* — they need different copy and are almost always conflated.

---

## Feedback and recovery

**Every action gets a visible response.** Something disappearing is not confirmation.

**Transient feedback fails interrupted users.** A toast assumes the user is watching. The interrupted dummy, the mobile dummy, and the screen reader dummy all miss it. Prefer feedback that persists or that shows the result in place.

**Undo beats confirm.** A confirmation dialog taxes every correct action to prevent a rare wrong one; undo taxes only the mistake. But timed undo is not undo for an interrupted user — if the window is shorter than their attention, it does not exist.

**Show the result where they are.** If the user must navigate elsewhere to confirm the thing worked, the loop is not closed.

**Optimistic updates need a rollback story.** If the plan shows success before the server confirms, it must specify what the user sees when the server disagrees.

---

## Density and structure

**Density is a decision, not a default.** Frequent users want more per screen; occasional users want fewer things and more explanation. A cast spanning frequency will surface this tension as an explicit tradeoff — which is the correct outcome. It cannot be resolved by splitting the difference; it needs a ruling.

**Scale to the worst realistic dataset, not the demo one.** Any list, picker, or table specified without pagination, search, or virtualisation is a finding waiting for the high-volume dummy.

**Structure first, styling later.** Information architecture — what is grouped with what, what is one level down, what gets a URL — determines more of the experience than any visual choice, and it is far more expensive to change after implementation.

---

## Known pattern space

When a decision is genuinely open, present the shapes and the conditions under which each wins, rather than asserting one:

| Shape | Wins when | Costs |
|---|---|---|
| **Inline edit** | high frequency, small edits, context matters | limited room, awkward validation, fights with row density |
| **Detail panel** | medium frequency, several fields, list context still needed | split attention, extra fetch, cramped on mobile |
| **Full page** | rare, complex, many fields, needs its own URL | loses context, heavier navigation |
| **Modal** | short, blocking, genuinely needs a decision now | no URL, no deep link, hostile on mobile, easily abused |
| **Wizard** | rare, high-stakes, strict ordering, novice users | slow and infuriating for repeat users |
| **Bulk + preview** | high volume, repetitive, consequential | needs a real selection model and a real undo |

Cite prior art alongside the shape. "Linear and Notion both use inline for high-frequency edits" is an argument; "inline is better" is an opinion.
