# crash-test

**Crash stand-in users into your plan before you build it.**

An agent skill for the gap between "I have an idea" and "I'm writing code" — where
most product decisions get made accidentally, by whoever typed first.

You bring a plan. `crash-test` researches the problem, casts **dummies** —
deliberately imperfect stand-in users — gives each one a real job, and walks them
through your plan step by step. Every place a dummy stalls becomes a decision you
rule on. Then it re-walks, because your ruling almost certainly broke something
downstream.

The output is not a plan the agent wrote or a plan you wrote. It's one plan where
every point of disagreement was surfaced, decided by you, and written down.

```
🚧 HALTED — Marcus (400 rows, interrupted) · Job: "get last quarter off my list"
   Step 3 of 5 · Gate G4 · Class: GAP

   > "Clicking Archive archives the selected rows and shows a success toast."

   "I selected about forty rows and hit Archive. One of them was this month's —
    I saw it a half-second too late. Now what? The plan tells me a toast
    appeared. It doesn't tell me there's any way back."

   ➡️ Recommended: undo affordance on the archive action.
      [prior-art] Gmail persistent undo bar; Notion 30-day trash
      [lens] G4 — undo beats confirm; transient feedback fails interrupted users
      ⚠️ Blocks all 4 dummies. Steps 4–5 unknowable until specified.
```

---

## Why "simulated users" isn't hand-waving

The obvious objection: an AI imagining that a fictional person got confused is
worthless. Correct — so the mechanism is built to make that the *minority* of
what it produces.

Before anything walks, your plan is compiled into an explicit spec where every
step must answer four questions: what's **visible**, what **actions** exist, what
the **copy** literally says, and what happens **next**. Most plans can't answer
those for most of their steps. Every blank is a finding, and it's a finding about
your document — not a guess about people.

**Every verdict must quote the line of your plan it judges.** No quote means it
isn't a prediction, it's a gap. That rule makes the whole run auditable: you can
check each finding against your own text.

Findings sort into three classes, and only one is speculation:

| Class | What it is | Objective? |
|---|---|---|
| **GAP** | The plan is silent. A field in the spec is blank. | Yes — check it against your doc |
| **CONFLICT** | Two parts of the plan contradict, or contradict the codebase | Yes — cited on both sides |
| **PREDICTION** | Plan is clear and complete; the agent still expects failure | No — tagged with confidence |

In practice the first two dominate. And if you distrust the third entirely, run
`--advisory`: predictions get logged, only gaps and conflicts gate the finish
line. The mechanism doesn't depend on the speculative part.

## The frontier

A dummy blocked at step 4 makes steps 5–12 **unknowable**. You physically cannot
find the checkout friction until the signup friction is fixed.

So the walk halts at the first blocker and never narrates past it. Depth of reach
*is* the frontier, and it advances only when you rule on something. That gives
you a progress metric for design quality, which is unusual:

```
                    Job A      Job B      Job C
Marcus (daily)      7/7 ✅     6/6 ✅     2/5 🚧
Dana (quarterly)    7/7 ✅     4/6 🚧     —
Priya (mobile)      5/7 🚧     6/6 ✅     —
Worst case (10k)    7/7 ✅     6/6 ✅     1/5 🚧
```

## Built on a real method

The four gates are the **cognitive walkthrough** (Wharton, Rieman, Lewis &
Polson, 1994), a usability inspection method that predates all of this by three
decades:

| Gate | Question | What it teaches |
|---|---|---|
| **G1 Goal** | Will they try the right thing here? | Mental models, goal formation |
| **G2 Notice** | Will they see the action is available? | Discoverability, hierarchy |
| **G3 Associate** | Will they connect it to their goal? | Affordance, labelling, microcopy |
| **G4 Feedback** | Will they see that it worked? | System status, error recovery |

Every finding is tagged with the gate that failed. That's how the UX principles
land — diagnostically, attached to a concrete failure in your own plan, rather
than as theory you'd skim.

## Install

As a Claude Code plugin:

```
/plugin marketplace add SophiaKutch/crash-test
/plugin install crash-test@crash-test
```

Or drop `skills/crash-test/` into `.claude/skills/` in any project.

Try it locally first: `claude --plugin-dir /path/to/crash-test`

## Use

```
/crash-test
```

Then point it at a plan — a markdown spec, a ticket, a design doc, or three
paragraphs in the chat. It runs five phases:

| Phase | What happens |
|---|---|
| **0 · Recon & compile** | Parallel research (codebase, prior art, constraints, real user evidence), then your plan becomes a walkable spec. Gaps surface here. |
| **1 · Cast** | 3–4 dummies proposed, each trait tagged with provenance. You edit the list. Cheapest alignment checkpoint in the run. |
| **2 · Walk** | Each dummy attempts their job. Four gates per step. Halt at the first blocker. |
| **3 · Rulings** | Blockers ranked and put to you, each with a cited recommendation. You accept, substitute, or overrule. |
| **4 · Re-walk** | Reports newly reachable steps and regressions your ruling created. Repeat 2–4. |
| **5 · Converge** | Every dummy finishes, zero gaps, zero conflicts, every prediction fixed or knowingly accepted. |

### Modes

| Flag | Effect |
|---|---|
| `--advisory` | Predictions never block convergence. Only gaps and conflicts gate the finish line. |
| `--dev-cast` | The user is a developer reading your API, types, and error messages. Same four gates. For refactors, queues, schemas — work with no end-user surface. |
| `--verified-only` | Only mechanically checkable gates: contrast, tap targets, tab order, focus, overflow. Marked `VERIFIED`, never `PREDICTED`. |

## What you get

Three artifacts, committed alongside your plan:

- **`walkable-spec.md`** — the converged spec, the shared source of truth
- **`cast.md`** — the approved dummies and their provenance
- **`decision-log.md`** — every ruling: the failure that forced it, what the agent
  recommended, what you decided, and **where the two of you differed and why**

That last field is the point of the whole exercise.

## What this is not

**It is not user research.** Dummies are plausibility engines. They over-comply,
under-fumble, and systematically miss the things that actually kill features —
habit, laziness, muscle memory, and not caring. A full coverage table means your
plan is *coherent*. It is not evidence your product *works*. The skill says this
out loud at the end of every run, and the name is a reminder: nobody mistakes a
crash-test dummy for a driver.

**It does not write feature code.** It edits the plan. Something else builds it.

**It can only walk what you specify.** A vague plan in, vague findings out — with
the mitigation that the vagueness itself gets itemised.

## Design notes

Two rules do most of the work, and both were worth stealing:

**"Finding facts is your job, never the user's."** Anything the agent could look
up, it looks up — codebase, prior art, API docs, schema. Only genuine judgment
calls reach you. This comes from Matt Pocock's
[`grill-me`](https://github.com/mattpocock/skills), which is the closest relative
to this skill and worth using for problems where interrogation fits better than
simulation.

**Questions have to be earned.** `grill-me` asks you to *generate* answers to
abstract forks. `crash-test` never asks a question that doesn't arrive with a
concrete failure narrative attached. People are much better at ruling on a story
than at answering an abstraction — and an engineer who doesn't yet know what they
don't know can still recognise that Marcus is stuck.

## Prior art & credits

- **Cognitive walkthrough** — Wharton, Rieman, Lewis & Polson (1994), *The
  Cognitive Walkthrough Method: A Practitioner's Guide*. The four gates.
- **[`grill-me`](https://github.com/mattpocock/skills)** by Matt Pocock — the
  facts/decisions split, and the frontier idea this skill re-derives from
  simulation depth.
- **Premortem** — Gary Klein. The stance behind halting at blockers rather than
  narrating a plan that works.

## Contributing

Findings about the skill's own findings are especially welcome: cases where a
dummy produced a confident prediction that turned out to be wrong, or where a
`GAP` was really the agent failing to read the plan properly. Both are calibration
bugs and both matter more than new features.

## License

MIT
