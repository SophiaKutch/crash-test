# Cast — Group Accountability Plant

Status: ☑ approved (by non-objection, runs 2–4)

> **Read this before trusting any finding derived from this cast.**
> This product has no users, no analytics, no support tickets, and no reviews.
> Almost every trait below is `[INVENTED ⚠️]` — the agent made it up. This was
> the weakest input in the entire run and it was never corrected. Treat the cast
> as a set of hypotheses about people, not a description of them.

---

## Jules — "recruited everyone, checks it daily"

**Job:** *"Get my friends to actually put their phones down with me."*

| Axis | Value | Provenance |
|---|---|---|
| Motivation | Chose it, evangelised it | `[INVENTED ⚠️]` |
| Frequency | Multiple times daily | `[INVENTED ⚠️]` |
| Stakes | Socially invested — it was their idea | `[INVENTED ⚠️]` |

**Here to break:** anything that assumes the group is as motivated as its founder.

**What Jules actually found:** the comparability problem, twice. Private self-set
limits made his goal unverifiable (run 2). Absolute self-set limits made it
visible that the group wasn't playing the same game (run 4). Both fixed — the
second by expressing limits as % tightened rather than minutes.

---

## Sam — "joined because Jules asked, doesn't actually want to change"

**Job:** *"Not be the reason everyone's annoyed at me."*

| Axis | Value | Provenance |
|---|---|---|
| Motivation | Social obligation, not self-improvement | `[INVENTED ⚠️]` |
| Frequency | Opens it when nudged | `[INVENTED ⚠️]` |
| Prior model | Has abandoned two habit apps already | `[INVENTED ⚠️]` |

**Here to break:** every place the design relies on goodwill.

**The most valuable dummy in this cast.** Sam broke the design three separate
times by completing it without changing his behaviour at all: once through
self-set limits with no floor, once through gaming the baseline window, and once
by coasting on a generous opening limit through several escalation steps. Every
structural fix in this product traces back to Sam. Apps like this live or die on
their least-motivated member.

---

## Priya — "phone is a work tool, constantly interrupted"

**Job:** *"Join in without getting punished for doing my job."*

| Axis | Value | Provenance |
|---|---|---|
| Context | Calls, Maps, work Slack, client DMs all day | `[INVENTED ⚠️]` |
| Motivation | Wants in, won't accept unfair losses | `[INVENTED ⚠️]` |
| Volume | Heavy legitimate use | `[INVENTED ⚠️]` — **the single trait most worth verifying.** Forest and Flora both ship per-user app selection, which implies the complaint exists in this category, but no specific reviews were read. |

**Here to break:** the gap between "used a phone" and "was distracted."

**What Priya found:** unscoped limits (fixed by self-set app selection), and the
ratchet's lack of a floor (fixed by declared personal floors).

---

## Worst case — Dev, "joined late, plant already dying"

**Job:** *"Work out whether this is still worth caring about."*

| Axis | Value | Provenance |
|---|---|---|
| Hard constraint | Arrived day 4 of a failing group, plant near death | `[INVENTED ⚠️]` |
| Everything else | Typical, moderately willing | `[INVENTED ⚠️]` |

**Here to break:** the despair spiral, and whether a newcomer inherits other
people's damage.

**What Dev found:** no recovery path (fixed), inherited damage (fixed by neutral
entry), the rejoin-resets-the-ratchet exploit (fixed by attaching history to the
person), and rarity being illegible on a widget with no comparison surface
(fixed by intrinsic escalation).

---

## Axis coverage

| Axis | Spanned? | By |
|---|---|---|
| Motivation | ✅ | Jules (chose) ↔ Sam (dragged) |
| Frequency | ✅ | Jules (daily) ↔ Sam (rare) |
| Context / interruption | ✅ | Priya |
| Stakes & emotional state | ✅ | Jules (invested) ↔ Dev (already lost) |
| Prior mental model | ⚠️ thin | Sam only |
| Data volume | n/a | not a volume-driven product |

## Engineer edits

**None.** The cast was presented twice and accepted by non-objection. The
`[INVENTED ⚠️]` traits were never corrected, which is the largest known
calibration risk in this run.

The highest-value thing to do before building: find one real friend group that
has tried a shared accountability app and check whether Sam exists as described.
If real groups don't contain a Sam, several structural decisions in the spec were
made to defeat a person who isn't there.
