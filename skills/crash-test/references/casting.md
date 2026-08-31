# Casting the Dummies

A dummy is a **stand-in**, not a person. It exists to be destroyed against the plan so the plan gets stronger. The name is deliberate: nobody mistakes a crash-test dummy for a driver, and nobody should mistake these for users.

Bad casting is the fastest way to make this skill worthless. Four dummies who are all variations of "a reasonable person using the product as intended" will walk the happy path four times and find nothing.

## Rule 1 — Behaviour and context, never demographics

Do not assign age, gender, ethnicity, or job title as an explanatory trait. Two reasons, and the second matters as much as the first:

1. It invites stereotyping, and personas built that way encode assumptions about people that nobody in the room actually verified.
2. **It does not predict anything.** Demographics tell you nothing about whether someone notices a button. Frequency of use, motivation, device, interruption, data volume, and prior tooling tell you everything.

A role is admissible only when it carries a real behavioural implication, and then state the implication rather than the role: not *"a front-desk receptionist"* but *"does this 40 times a day, always mid-conversation with someone else."*

## Rule 2 — Span the axes that break products

Pick traits so the cast disagrees with itself. Each dummy should sit at a different extreme on at least two axes:

| Axis | Extremes | What it breaks |
|---|---|---|
| **Frequency** | 40×/day ↔ once a quarter | Muscle memory vs. total re-orientation. Power users need speed and are hurt by confirmations; returners need context and are hurt by their absence. Both cannot be optimised at once, and that tension is a real finding. |
| **Motivation** | chose the tool ↔ mandated to use it | Mandated users do not explore, do not read, and abandon at the first ambiguity. They surface every place the plan relies on goodwill. |
| **Context** | calm and focused ↔ interrupted, mobile, one-handed, in front of a customer | Kills transient feedback, timed undo, multi-step flows, and anything below the fold. |
| **Data volume** | 3 rows ↔ 10,000 rows | Kills one-at-a-time interactions, unpaginated lists, unfiltered pickers, and any plan that says "select the rows." |
| **Prior mental model** | arrives from a named competitor ↔ arrives from a spreadsheet ↔ arrives from nothing | Kills labels. This is where most G3 failures come from. |
| **Stakes** | mistake is trivial ↔ mistake is expensive, public, or regulated | Kills missing confirmations, absent undo, and unclear destructive actions. |

## Rule 3 — Exactly one worst case

One dummy in every cast is the hardest case that is still real. Choose the one that matters for this plan:

- Screen reader or keyboard only
- 10,000 rows, or a name field with 200 characters, or a translation 3× longer than English
- Failing network, mid-flight session expiry, a second tab open on the same record
- The account with permissions that are almost but not quite sufficient

One, not three. A cast of worst cases produces a wall of findings that gets ignored wholesale.

## Rule 4 — Goals in their words

The goal is what the person came to do, expressed the way they would say it. It must not name a product feature.

| Bad | Good |
|---|---|
| "Use the bulk archive feature" | "Get last quarter off my list" |
| "Filter the report by date range" | "Find out whether we did better than last month" |
| "Complete the onboarding flow" | "See whether this thing is worth my time" |

If the goal names a feature, you have smuggled the plan's assumptions into the test and G1 can never fail. Any goal you cannot restate without naming a feature is a finding in itself.

## Rule 5 — Tag every trait with provenance

Each trait carries where it came from:

- `[support-tickets]`, `[analytics]`, `[competitor-reviews]`, `[user-interviews]` — real evidence, cite it
- `[repo]` — inferred from the codebase, with `file:line`
- `[prior-art]` — how a comparable product's users behave, with a URL
- `[engineer]` — the engineer told you, in this session
- `[INVENTED ⚠️]` — you made it up because no evidence existed

`[INVENTED]` is allowed. Silently invented is not. The engineer reads the cast to find those tags, and correcting them is the highest-leverage thing they do in Phase 1 — they usually know the real answer and would never have volunteered it unprompted.

## Rule 6 — Present one line per dummy, not a table

The full trait tables belong in `cast.md`, not in the conversation. A table of six traits across four dummies is thirty cells to review before the engineer has seen a single finding — and at that point they have no basis for judging any of it. It reads as homework and it gets skipped.

In conversation, one line each: name, defining trait, goal. Then ask a single answerable question — *"which of these four doesn't exist among your users?"*

The checkpoint that actually works comes **after the first walk**, when each dummy has done something concrete. *"Sam set a four-hour limit and coasted — do your users do that, or did I build a strawman?"* is answerable in a sentence. *"Correct the cast"* is not.

If the cast is never corrected, stamp it: `[INVENTED ⚠️]` traits stand unverified, and any structural decision made to defeat a dummy may be defending against nobody. Never let silence read as approval.

## Rule 7 — The engineer can play a dummy

When a walk hits a contested step and your confidence is low, ask: *"What would Marcus actually do here?"* Take their answer as canon and record it as `[engineer]`.

This is the pressure valve for your worst weakness. You over-comply and under-fumble; real users are lazier, more habitual, and less curious than you predict. The engineer's tacit knowledge of their own users is not in your training data. Use it on the steps where it matters, not on every step.

## Cast size

Three or four. Two cannot generate a tradeoff. Five or more produces overlapping findings and a Phase 3 the engineer will not finish reading.
