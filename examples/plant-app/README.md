# Worked example — a real session, from one paragraph

This is an unedited-in-substance record of an actual `crash-test` run: four walks,
seventeen decisions, two places the agent withdrew its own recommendation, and one
platform constraint found before a single dummy moved.

It starts from the hardest possible input. No codebase, no users, no analytics, no
ticket — just an idea typed into a chat box:

> *"I want to build an app where groups of friends can interact. The goal of the
> app is some sort of fun game or goal such as 'keeping a plant alive' where it
> keeps users accountable for their social media screen time, and any personal
> interaction where you exceed screen time limits hurts the group."*

The artifacts it produced are in this directory:
[`walkable-spec.md`](walkable-spec.md) · [`cast.md`](cast.md) ·
[`decision-log.md`](decision-log.md)

> **One note on fidelity.** This session **batched rulings** — it asked for four
> decisions at once, then seven. That was a bug in the skill, and this run is what
> exposed it: one ruling later invalidated the assumption two earlier answers were
> resting on. It's fixed — a current run puts one blocker at a time. The rulings
> below are grouped for readability, but a live session would surface them singly.

---

## Phase 0 — the constraint that decides the product

Before any walk, recon found the thing that determines whether this product can
exist. Quoted verbatim from Apple's `DeviceActivityReport` documentation:

> *"To protect the user's privacy, your extension runs in a sandbox. This sandbox
> prevents your extension from making network requests or moving sensitive content
> outside the extension's address space."*

The idea requires **other people to know you exceeded your limit.** On iOS, that is
specifically the thing Apple prevents. You can show a user their own screen time;
you cannot send it to your server or to their friends. The social layer — the entire
premise — is blocked at the platform level.

This is not a design opinion. It's a quote, and it arrived on turn one.

Recon also found that the closest successful precedent disagrees with the idea.
Flora, describing its own social mode:

> *"if you successfully stay off the phones together, **each of you will win an
> additional tree**"*

Shared *reward*, not shared punishment. The most successful product in this exact
space deliberately chose the opposite of "exceeding limits hurts the group." Worth
knowing before you commit — not proof the idea is wrong, but it means the punishment
mechanic needs a defence rather than an assumption.

## The two forks

Rather than sketching one flow and anchoring the engineer to it, the agent offered
two incompatible options on each of the two choices that decide the product's shape:

| Fork | Options | Ruled |
|---|---|---|
| **What gets measured** | Real OS screen time *(truthful, needs Apple's entitlement)* ↔ declared sessions *(ships anywhere, gameable)* | Declared sessions |
| **How a slip lands** | Shared fate *(the engineer's idea)* ↔ shared reward *(Flora's choice at scale)* | Shared fate |

## A conflict found before anything walked

Compiling the two rulings into a spec surfaced this immediately:

- Sessions are **voluntary**
- Damage can only occur **during** a session
- Therefore starting a session is the only way to incur damage

**The optimal strategy was to never play.** The mechanic punished precisely the
behaviour it existed to encourage, and the most invested member of a group would
converge on doing nothing.

That's objective — it follows from the two rules the engineer picked. No psychology
required, no dummy needed. This is the class of finding that makes compilation worth
doing on its own.

---

## Run 2 — step 3 killed half the cast

```
🚧 HALTED — Priya (phone is a work tool) · Job: "join in without getting punished for doing my job"
   Step 3 of 10 · Gate G3 · Class: GAP

   > step 3 `copy: GAP`

   "It says set a daily limit. A limit on what? Instagram is how I find clients.
    If that counts I'll fail on day one and take four friends down with me. I
    can't pick a number until I know what it's counting, so I closed the app."
```

Jules halted at the same step for an unrelated reason — limits were invisible to
the group, so his goal *("get my friends to actually cut down")* was unverifiable.
**Two of four dummies dead at one step, for two independent reasons.** That step was
the most underspecified in the spec and also the most load-bearing.

## Run 3 — the finding that reshaped the product

```
🚧 COMPLETED — Sam (joined because Jules asked) · Step 13 of 13
   Gate G1 · Class: CONFLICT

   "I set my limit at four hours, which is roughly what I already do. I've never
    once come close to breaking it. We survived the season, the group got a streak,
    Jules is thrilled, and I unlocked a fern. My screen time is exactly what it was
    in January."
```

Not a step failure — the whole design. Self-set limits plus "encouraged to reduce"
meant a group could win every season indefinitely while nobody changed anything.

Note that this finding comes from a dummy **completing** the flow. A run where
everyone succeeds is not automatically a run that went well.

Sam went on to break the product two more times, and every structural decision in
the final spec traces back to him. Apps like this live or die on their
least-motivated member.

## Run 4 — one ruling, three regressions

The fix for Sam was baseline-relative limits: measure two weeks, then set each
person's limit as a percentage reduction from their own baseline. The engineer
accepted it. Re-walking found that it:

1. **Broke platform feasibility.** A percentage needs *quantities*. The threshold-event
   workaround that made this product possible yields only *binary* signals. The
   fix collided with the constraint from Phase 0.
2. **Didn't stop Sam.** *"So the first two weeks decide my limit. Fine — I scrolled
   a bit extra."* Goodhart's law with a two-week window, and it inverts fairness:
   the honest person gets the tightest limit.
3. **Created a 14-day dead start.** Five friends install, and the app does nothing
   for two weeks. Worse cold-start than the problem solved one round earlier.

So the agent **withdrew its own recommendation from the previous round** and proposed
escalating limits instead — self-set in season one, mandatory tightening thereafter.
Ungameable, because the ratchet is relative to your own prior commitment rather than
to a measurement. It also needs no quantities, which put the product back inside
Apple's constraint cleanly, and it deleted the 14-day problem for free.

That ruling then reintroduced a fourth-round version of Jules's original complaint —
absolute self-set limits are incomparable — fixed by displaying *"% tightened since
season one"* as the primary number.

This is what Phase 4 is for. Each fix was correct and each one broke something a
step further out.

---

## Converged

| Dummy | Reach | |
|---|---|---|
| Jules | 13/13 | ✅ |
| Sam | 13/13 | ✅ |
| Priya | 13/13 | ✅ |
| Dev | 13/13 | ✅ |

**17 decisions** recorded with the failure that forced each one. Every unverified
assumption listed rather than buried — including the one the whole product rests on:

> Can `DeviceActivityMonitor` make a network request when a threshold fires? Apple
> documents the sandbox restriction for the *report* extension and is silent on the
> *monitor* extension. Half a day to settle. **Nothing else in the spec matters
> until it is.**

---

## What this run got wrong

The repo asks for findings about the skill's own findings, so:

**The cast was never corrected.** It was offered twice and skipped both times. Every
trait describing Jules, Sam, Priya and Dev is `[INVENTED ⚠️]`, which means several
structural decisions may be defending against a person who doesn't exist. The skill
now stamps this at convergence instead of letting silence read as approval — and the
reason it was skipped was the skill's fault: it presented thirty trait cells and
asked the engineer to "correct the cast" before they'd seen a single finding. That's
homework, not a question. It now asks *"which of these four doesn't exist among your
users?"* and saves the real check for after run 1, when a dummy's behaviour can
actually be judged.

**Rulings were batched.** Four at once, then seven. One later ruling invalidated the
premise of two earlier answers. Fixed — see the note at the top.

**One ruling answered a different question than the finding asked.** The finding was
about who can *see* a limit; the ruling was about who *sets* it. The agent caught it,
but nothing in the skill told it to. Now explicit.

## The standing caveat

A full coverage table means this plan is **coherent**. It is not evidence the product
works. Jules, Sam, Priya and Dev are stand-ins — they over-comply, under-fumble, and
miss the real killers entirely: habit, laziness, muscle memory, and not caring.

No real person has seen any of this.
