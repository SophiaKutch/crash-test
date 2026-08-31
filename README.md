# crash-test

**Crash stand-in users into your plan before you build it.**

> You bring a plan. `crash-test` researches it, casts stand-in users, crashes them
> into the plan, and every wreck becomes a decision you rule on — then it re-runs
> to see what your ruling broke.

An agent skill for the gap between *"I have an idea"* and *"I'm writing code"* —
where most product decisions get made accidentally, by whoever typed first.

---

## The problem

Plans read as complete because prose hides holes.

You write four bullets in a ticket. They're clear, they're reviewable, everyone
nods. Then you're three days into the implementation and you discover the plan
never said what happens when the list is empty. Or that the archive action can't
be undone. Or that the button label means "permanently delete" to the users who
came from your competitor. Or that the endpoint caps at 100 IDs and your bulk
action sends 400.

None of those are bugs. Code review won't catch them, because the code correctly
implements a plan that was never finished. They surface in QA at best, in a
support ticket at worst, and the fix costs fifty times what it would have cost as
an edit to a markdown file.

**And it's worse when you hand the plan to an AI.** A coding agent will happily
build all four bullets and silently make forty micro-decisions you never saw —
what the toast says, what happens on partial failure, whether selection survives
a scroll. Each one plausible. Some wrong. None written down, none agreed to, and
all of them now load-bearing.

### You and the AI are holding two different plans

That's the deeper problem. The same four bullets produce one implementation in
your head and a different one in the agent's, and **neither of you knows where the
two diverge.** You find out by reading the diff — which is late, and which shows
you the code rather than the decision behind it.

The usual fixes both fail. Specifying everything up front doesn't work, because
the decisions you'd need to write down are precisely the ones you haven't thought
of yet; that's what makes them expensive. And reviewing after the fact doesn't
work either, because by then you're evaluating an implementation instead of
choosing between options, and the cost of changing your mind has gone up
tenfold.

`crash-test` makes the divergence **enumerable before either of you commits to
anything.** Every `GAP` it reports is a place the agent would otherwise have
guessed on your behalf. Every ruling you make is a place you took the wheel. And
every disagreement — including the ones where you overrule the agent outright —
gets written into the decision log with your reasoning next to its recommendation.

Overruling is a first-class outcome, not a failure state. Agreement here doesn't
mean the agent deferring to you or you accepting its proposal; it means neither of
you is operating on an assumption the other didn't see. When the walk converges,
you have one plan you both read the same way, and a record of how you got there —
so the next session picks up the reasoning instead of quietly re-deciding it.

What you end up with is a plan neither of you would have written alone, where you
can point to every place you disagreed and say what you chose.

## What it looks like

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

See [`examples/bulk-archive.md`](examples/bulk-archive.md) for a full two-run
session on a four-bullet plan — it finds two gaps, two conflicts, a
partial-failure state nobody considered, and one tradeoff made explicit.

## Install

**As a Claude Code plugin:**

```
/plugin marketplace add SophiaKutch/crash-test
/plugin install crash-test@crash-test
```

**Try it before installing:**

```bash
git clone https://github.com/SophiaKutch/crash-test.git
claude --plugin-dir ./crash-test
```

**Or drop it into a single project** — copy `skills/crash-test/` into that
project's `.claude/skills/` directory. Nothing else in the repo is required at
runtime; `examples/` and `README.md` are documentation.

Works with any agent that reads `SKILL.md` files. No dependencies, no build step,
no scripts — it's markdown all the way down, so you can read every instruction it
follows before you trust it with your plan.

## Usage

```
/crash-test
```

Then point it at a plan. It accepts anything: a markdown spec, a Jira ticket, a
design doc, a PR description, or three paragraphs typed into the chat.

```
/crash-test  docs/plans/bulk-archive.md
/crash-test  CHD-1234
/crash-test  "Let users archive multiple records at once from the list view"
```

### What happens, in five phases

| Phase | What it does | What you do |
|---|---|---|
| **0 · Recon & compile** | Researches in parallel — your codebase, how shipped products solve this, API and platform limits, any real evidence about your users. Then compiles your plan into a step-by-step spec where every step must state what's visible, what actions exist, what the copy literally says, and what happens next. | Read the compiled spec. Correct any misreadings — a misread step invalidates everything downstream of it. |
| **1 · Cast** | Proposes 3–4 **dummies**: stand-in users spanning the axes that actually break products — frequency of use, motivation, working context, data volume, and the mental model they arrive with. Plus exactly one worst case. Every trait is tagged with where it came from. | Edit the cast. Fix anything tagged `[INVENTED ⚠️]` — you almost certainly know the real answer, and this is the cheapest, highest-leverage checkpoint in the whole run. |
| **2 · Walk** | Each dummy attempts their job against the spec, step by step, evaluated against four gates. Halts at the first blocker. | Nothing — read the wrecks. |
| **3 · Rulings** | Blockers ranked by how many dummies they block and what they cost to fix, each with a recommendation and a citation. | Rule on each one: **accept**, **substitute** your own fix, or **overrule** the finding entirely. All three are valid and all three get recorded. |
| **4 · Re-walk** | Applies your rulings and re-runs the whole cast. Reports newly reachable steps, and regressions your ruling just created. | Rule again. Repeat 2–4 until it converges. |
| **5 · Converge** | Writes the artifacts. | Ship a plan you actually agree with. |

### It only asks about blockers

A simulated user can generate nits forever. Non-blocking friction gets logged and
revisited at the end; only things that genuinely stop a dummy interrupt you. A run
that surfaces four blockers gets acted on. A run that surfaces thirty findings
gets skimmed.

### The frontier

A dummy blocked at step 4 makes steps 5–12 **unknowable** — you physically cannot
find the checkout problem until the signup problem is fixed. So the walk halts at
blockers and never narrates past them, and depth of reach only advances when you
rule on something. That gives you a progress bar for plan quality:

```
                    Job A      Job B      Job C
Marcus (daily)      7/7 ✅     6/6 ✅     2/5 🚧
Dana (quarterly)    7/7 ✅     4/6 🚧     —
Priya (mobile)      5/7 🚧     6/6 ✅     —
Worst case (10k)    7/7 ✅     6/6 ✅     1/5 🚧
```

### Modes

| Flag | Effect |
|---|---|
| `--advisory` | Predictions never block convergence — only gaps and conflicts do. Use this if you don't want to argue with a simulated person. |
| `--dev-cast` | The user is a **developer** reading your API, types, and error messages. Same four gates. Use for refactors, queues, schemas, internal libraries — anything with no end-user surface. |
| `--verified-only` | Runs only the gates that can be mechanically checked: contrast ratio, tap targets, tab order, focus visibility, overflow. Marked `VERIFIED`, never `PREDICTED`. |

## What you get

Three artifacts, written alongside your plan and meant to be committed:

| File | Contents |
|---|---|
| `walkable-spec.md` | The converged spec. Every step, every state, no blanks. The shared source of truth. |
| `cast.md` | The approved dummies and the provenance of every trait. |
| `decision-log.md` | Every ruling: the failure that forced it, what the agent recommended, what you decided, and **where the two of you differed and why.** |

That last column is the point of the whole exercise. The output isn't a plan the
agent wrote or a plan you wrote — it's one plan where every point of disagreement
was surfaced, decided by you, and written down. Six months later, "why isn't this
a modal?" has an answer.

## Why "simulated users" isn't hand-waving

The obvious objection: an AI imagining that a fictional person got confused is
worthless. Correct — so the mechanism is built to make that the *minority* of what
it produces.

**Every verdict must quote the line of your plan it judges.** No quote means it
isn't a prediction, it's a gap. That single rule makes the whole run auditable: you
can check each finding against your own text.

Findings sort into three classes, and only one is speculation:

| Class | What it is | Objective? |
|---|---|---|
| **GAP** | The plan is silent. A required field in the spec is blank. | **Yes** — verify it against your own doc |
| **CONFLICT** | Two parts of the plan contradict, or contradict your codebase | **Yes** — cited on both sides |
| **PREDICTION** | The plan is clear and complete; the agent still expects a human to fail | **No** — always tagged with a confidence level |

In practice the first two dominate, especially on a first run. And if you distrust
the third entirely, `--advisory` removes it from the finish line. The mechanism
doesn't depend on the speculative part.

## Built on a real method

The four gates are the **cognitive walkthrough** (Wharton, Rieman, Lewis & Polson,
1994), a usability inspection method that predates all of this by three decades:

| Gate | Question | What a failure teaches |
|---|---|---|
| **G1 Goal** | Will they try to do the right thing here? | Mental models, goal formation, naming |
| **G2 Notice** | Will they see the correct action is available? | Discoverability, visual hierarchy |
| **G3 Associate** | Will they connect that action to their goal? | Affordance, labelling, microcopy |
| **G4 Feedback** | Will they see that it worked? | System status, error recovery, undo |

Every finding is tagged with the gate that failed. That's how the UX principles
land — diagnostically, attached to a concrete failure in *your* plan, rather than
as theory you'd skim. If you've ever felt that design feedback was vague or
unfalsifiable, this is the antidote: a named gate, a quoted line, and a cited fix.

The reference material is readable on its own, whether or not you run the skill:

- [`references/gates.md`](skills/crash-test/references/gates.md) — the four gates, their failure signatures, and typical fixes
- [`references/casting.md`](skills/crash-test/references/casting.md) — how to build stand-in users that find things
- [`references/findings.md`](skills/crash-test/references/findings.md) — the finding taxonomy, severity, and halting rules
- [`references/compiling.md`](skills/crash-test/references/compiling.md) — turning a prose plan into something walkable
- [`references/ux-lenses.md`](skills/crash-test/references/ux-lenses.md) — the design principles it cites, plus a pattern-space table
- [`references/developer-cast.md`](skills/crash-test/references/developer-cast.md) — the four gates applied to APIs and internal work

## What this is not

**It is not user research.** Dummies are plausibility engines. They over-comply,
under-fumble, and systematically miss the things that actually kill features —
habit, laziness, muscle memory, and not caring. A full coverage table means your
plan is *coherent*. It is not evidence your product *works*. The skill says so out
loud at the end of every run, and the name is the reminder: nobody mistakes a
crash-test dummy for a driver.

**It does not write feature code.** It edits the plan. Something else builds it.

**It can only walk what you specify.** A vague plan in, vague findings out — with
the mitigation that the vagueness itself gets itemised, which is usually the most
useful thing you learn on a first run.

## Design notes

Two rules do most of the work.

**Look it up before you ask.** Your attention is the scarce resource in this
process, so the agent spends its own first. Anything it can establish without you
— your codebase, prior art, API limits, the schema, the actual row counts — it
establishes on its own and cites. What reaches you is only what genuinely requires
your judgment, which is the part a research pass cannot settle.

**Questions have to be earned.** Planning tools that interview you ask you to
*generate* answers to abstract forks: "how should selection behave?", "what's your
density strategy?" That's the wrong ask for someone who doesn't yet know what they
don't know. `crash-test` never asks a question that doesn't arrive with a concrete
failure narrative attached. People are far better at ruling on a story than at
answering an abstraction — you may not have a density strategy, but you can
immediately tell that Marcus is stuck and that it matters.

## Prior art & credits

- **Cognitive walkthrough** — Wharton, Rieman, Lewis & Polson (1994), *The
  Cognitive Walkthrough Method: A Practitioner's Guide*. The four gates, and the
  practice of evaluating a design one step at a time against a stated user goal.
- **Premortem** — Gary Klein. The stance behind halting at blockers instead of
  narrating a plan that works.
- **Snag lists and shakedown runs** — the construction and aviation practice of
  walking a finished thing looking for defects before it enters service, rather
  than reviewing the drawings and hoping.

## Contributing

Findings about the skill's own findings are especially welcome: a dummy that
produced a confident prediction which turned out to be wrong, or a `GAP` that was
really the agent failing to read the plan properly. Both are calibration bugs, and
both matter more than new features.

## License

MIT — see [LICENSE](LICENSE).
