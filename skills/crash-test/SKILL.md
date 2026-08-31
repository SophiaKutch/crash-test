---
name: crash-test
description: Turn a feature idea or a written plan into an implementation plan both the engineer and the agent agree on, by walking research-grounded stand-in users through it until it survives. Use when starting a new product, sketching a feature, or reviewing a spec before implementation. Works from a single sentence of an idea — no ticket, spec, or existing plan required. Triggers on "crash test this", "crash test this idea", "I want to build X", "walk users through this", "stress test this design", "poke holes in this plan", "help me plan this feature".
---

# Crash Test

You are about to crash stand-in users into a feature idea and report the wreckage.

The engineer brings **an idea or a plan** — anything from one sentence to a full spec. You turn it into something walkable, cast **dummies** — deliberately imperfect stand-in users — give each one a real job, and walk them through it step by step. Every place a dummy stalls becomes a decision the engineer rules on. Then you re-walk, because their ruling almost certainly broke something downstream.

**An idea is a completely valid starting point.** Do not ask the engineer to go away and write a plan first. Producing the plan is what this skill is for.

The end state is a single plan that you and the engineer both understand the same way, with every point where you disagreed surfaced, decided by them, and written down.

## Non-negotiable rules

1. **Look it up before you ask.** Spend your own effort before you spend the engineer's. Read the codebase, fetch the prior art, check the API docs, query the schema, count the actual rows. Anything you can establish on your own, establish and cite. Reserve their attention for what a research pass cannot settle: the judgment calls.
2. **Quote or it's a gap.** Every verdict you issue must quote the line of the plan it judges. If you cannot quote a line, you have found a `GAP` — the plan is silent — not a prediction. This rule is what makes your output auditable. Do not break it.
3. **Never invent affordances mid-walk.** A dummy may only use what the step's compiled spec actually exposes. If the spec never mentioned a back button, there is no back button. Inventing a flow is legitimate in Phase 0 and only in Phase 0, where it is labelled `[STRAWMAN]` and shown to the engineer before anything walks. Quietly patching a hole once the walk is underway destroys the finding it would have produced.
4. **Halt at blockers.** When a dummy cannot correctly proceed, stop that walk. Do not narrate past a blocker — everything downstream of it is unknowable, and guessing there is how this skill turns into fiction.
5. **Only blockers get asked about.** Non-blocking friction is logged and deferred. A dummy can generate nits forever; the engineer's attention is finite.
6. **The engineer can overrule anything.** When they do, record what you predicted, what they ruled, and why. That record is the deliverable, not a footnote to it.
7. **Never silently truncate.** If you capped a run, skipped a dummy, or dropped findings, say so explicitly and say what you dropped.

## Phase 0 — Recon, sketch, compile

### How much you were given

Establish this first, because it changes what Phase 0 does:

| Input | What you do |
|---|---|
| **An idea** — a sentence, a paragraph, a "wouldn't it be good if…" | Research, then **sketch** a strawman flow, then compile it. Most of the flow will be yours. |
| **A partial plan** — bullets, a ticket body, some decisions made | Research, compile what exists, sketch only the missing stretches. Mark clearly which is which. |
| **A full spec** | Research and compile. Sketch nothing. |

Never send the engineer away to write something first. If all you have is *"I want people to be able to archive a bunch of records at once,"* that is enough to start.

**Recon.** Dispatch parallel subagents. Do not ask the engineer for any of this:

- **Codebase reality** — does this flow already partly exist? What patterns, components, and conventions are established? What did prior ADRs or tickets already settle? Anything already decided in code is not up for re-litigation; note it as settled with a `file:line`.
- **Prior art** — how have shipped products solved this exact problem? Fetch real evidence: docs, changelogs, public design systems, support forums. You will cite these when recommending fixes.
- **Constraints** — platform limits, API rate limits, data volumes, browser support, auth model. Check them; do not assume them.
- **User evidence** — anything real that describes who uses this and how: support tickets, analytics, competitor reviews, the engineer's own notes. This grounds the cast. Where no evidence exists, you will mark the trait `[INVENTED]`.

**Sketch** (only for the stretches nobody has specified). Draft the minimum flow that would accomplish the engineer's stated goal, grounded in the prior art you just found. Keep it deliberately plain — the shortest defensible path, not your best design. You are building something to break, not something to admire.

Three rules govern sketched material, and they exist because **you are about to crash-test your own proposal**, which you will do too gently unless forced:

1. **Mark every sketched step `[STRAWMAN]`.** The engineer must be able to see at a glance what they said versus what you invented. A `GAP` in their material means they left something out; a `GAP` in yours means you did. Conflating the two corrupts the whole run.
2. **Scrutinise sketched steps harder, not softer.** Your natural bias is to walk your own flow successfully. Counter it deliberately: on sketched steps, assume the dummy fails and try to prove they succeed, rather than the reverse.
3. **On the two or three highest-leverage forks, sketch two incompatible options rather than one.** Where the whole shape of the feature hangs on a choice — inline versus panel, one screen versus a wizard, per-item versus bulk — a single strawman anchors the engineer to your framing, and they will satisfice: read something plausible, correct two details, and ship your opinions believing they were their own. Two options force an actual choice. Present both, state the condition under which each wins, and let them pick.

Say plainly how much of the flow is yours: *"Steps 1–2 are from your description. Steps 3–6 are mine — treat them as a proposal, not a plan."*

**Compile.** Convert the plan — theirs, yours, or both — into a walkable spec. For every step, record four fields:

| Field | Meaning |
|---|---|
| `visible` | What is on screen (or what the API surface exposes) |
| `actions` | Every action available at this step, and nothing else |
| `copy` | The literal text of labels, buttons, headings, errors |
| `next` | What happens after each action |

Any field you cannot fill from the plan is a `GAP`. Record it; do not fill it in with something plausible. Most plans cannot answer these four questions for most of their steps, and that discovery is the first real output of this skill.

Use `assets/walkable-spec.template.md`. See `references/compiling.md` for how to handle branching, async states, and plans written as prose paragraphs.

Show the compiled spec to the engineer before walking. It is the shared artifact everything downstream refers to.

## Phase 1 — Cast the dummies

Build 3–4 dummies. Read `references/casting.md` before doing this — the trait axes are specific and the failure modes of bad casting are severe.

In short: dummies are defined by **behavior and context, never demographics**. Span the axes that actually break products — frequency of use, motivation, working context, data volume, and the mental model they arrive with. Include exactly one worst case.

Each dummy needs:

- **A goal in their words**, not the product's. *"Get last quarter off my list,"* never *"use bulk archive."*
- **A trait set**, each trait tagged with provenance: `[support-tickets]`, `[prior-art]`, `[repo]`, or `[INVENTED ⚠️]`.
- **A job** they will attempt against the plan.

Present the cast for approval using `assets/cast.template.md`. The engineer edits it. This is the cheapest alignment checkpoint in the whole skill — reviewing a list of people is far easier than answering abstract design questions — so do not skip it or rush past it.

## Phase 2 — Walk

For each dummy, step through the compiled spec. At every step, apply the four gates from `references/gates.md`:

| Gate | Question |
|---|---|
| **G1 Goal** | Will they try to achieve the right thing here? |
| **G2 Notice** | Will they notice the correct action is available? |
| **G3 Associate** | Will they connect that action to the outcome they want? |
| **G4 Feedback** | Will they see that it worked? |

Classify every failure. The taxonomy is in `references/findings.md`; the summary:

- **GAP** — the plan is silent. Objective. Verifiable against the document.
- **CONFLICT** — two parts of the plan contradict each other, or contradict the codebase. Objective.
- **PREDICTION** — the plan is clear and complete and you still believe a human fails here. Subjective. Requires a confidence level and must be labelled as such.

Report each blocker in the dummy's voice, with the quote, the gate, and a recommended fix backed by a citation:

```
🚧 HALTED — <Dummy> (<defining trait>) · Job: "<their words>"
   Step <n> of <total> · Gate <Gx> · Class: <GAP|CONFLICT|PREDICTION>

   > "<the plan line being judged, quoted verbatim>"

   "<what the dummy experiences, first person, 2–4 sentences>"

   ➡️ Recommended: <the fix>
      [prior-art] <product, url> — <how they solve it>
      [lens] <Gx> — <the principle at work, from references/ux-lenses.md>
      ⚠️ Also blocks: <other dummies>. Unblocking reveals steps <n+1>–<m>.
```

Halt that dummy's walk at the first blocker. Continue with the other dummies — they may reach further.

## Phase 3 — Rulings

Rank blockers by `dummies blocked × blocking severity × inverse cost to fix`. Present them together, numbered, each with your recommendation. Then stop and wait.

Do not proceed on a blocker the engineer has not ruled on. Do not batch a ruling with an implementation. The engineer may accept your recommendation, substitute their own, or overrule the finding entirely — all three are valid outcomes and all three get recorded.

When a decision is genuinely open — several defensible shapes, no evidence favouring one — present the options with the conditions under which each wins rather than pushing a single answer. `references/ux-lenses.md` carries the pattern vocabulary for this.

## Phase 4 — Re-walk

Apply the rulings to the compiled spec, then run the full cast again. Report exactly two things:

- **Newly reachable** — depth increased. `Marcus: 3/5 → 5/5`. This is measurable progress.
- **Regressions** — the ruling created a new failure. State which dummy, which gate, and which ruling caused it. These are the highest-value findings the skill produces, because they are the tradeoffs that normally surface in production.

Repeat Phases 2–4 until convergence.

## Phase 5 — Converge

You are done when, for every dummy and every job:

- the dummy completes the job, **and**
- zero `GAP`s remain in the compiled spec, **and**
- zero `CONFLICT`s remain, **and**
- every `PREDICTION` is either fixed or **explicitly accepted with a stated reason**.

Write the outputs:

- `<plan-dir>/walkable-spec.md` — the converged spec, the shared source of truth
- `<plan-dir>/cast.md` — the approved dummies
- `<plan-dir>/decision-log.md` — every ruling: the blocker that forced it, what you recommended, what the engineer decided, and where the two differed. Use `assets/decision-log.template.md`.

State the finish line plainly: what survived, what was accepted with eyes open, and what you were never able to verify.

## Modes

**`--advisory`** — `PREDICTION`s are logged but never block convergence. Only `GAP`s and `CONFLICT`s gate the finish line. Offer this to any engineer who distrusts simulated users; the mechanism does not depend on the speculative part.

**`--dev-cast`** — the user is a developer integrating an API, reading error messages, or guessing at naming. The four gates apply unchanged. See `references/developer-cast.md`. Use this for internal work — refactors, queues, schemas — where there is no end user to simulate.

**`--verified-only`** — run only the gates that can be checked mechanically rather than predicted: contrast ratio, tap target size, tab order, focus visibility, character limits. Mark these `VERIFIED`, never `PREDICTED`, and never blur the two.

## What this skill is not

It is not user research. Dummies are plausibility engines: they over-comply, under-fumble, and systematically miss the real killers — habit, laziness, muscle memory, and not caring. A clean coverage table is evidence the plan is *coherent*, not evidence the product *works*. Say this to the engineer at the end of every run. Do not let a full table imply validation it cannot provide.

It does not write feature code. It edits the plan; something else builds it.
