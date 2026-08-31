# Walkable Spec — Group Accountability Plant

Compiled from: a one-paragraph idea (no prior plan)
Runs completed: 4 · Status: **converged**

> Every field below is filled from the engineer's rulings or from cited research.
> `[STRAWMAN]` marks material the agent authored that the engineer accepted.
> Remaining blanks are listed under **Open** at the bottom — they are not
> silently filled.

## Shape of the product

| Decision | Value | Ruling |
|---|---|---|
| What is measured | Actual OS social-media screen time, per person | D5 (reverses D1) |
| Signal needed from platform | Binary "crossed own declared limit", plus laddered buckets | D8, D13 |
| How a slip lands | Partial watering — shared fate, proportional | D2, D17 |
| Limit setting | Self-set (time + apps), group-visible, locked per season | D3, D4 |
| Floor | Escalating ratchet + declared personal floor | D8, D14 |
| Arc | Seasons: 7 → 14 → 21 → 30 days | D6 |
| Feedback surface | Home/lock-screen widget. No feed, no gallery. | D7 |
| Progression | Intrinsic plant escalation by seasons survived | D10 |
| Streak | Accumulating count of seasons survived | D9 |

## Jobs under test

| ID | Job (user's words) | Dummies |
|----|--------------------|---------|
| A | "Get my friends to actually put their phones down with me" | Jules |
| B | "Not be the reason everyone's annoyed at me" | Sam |
| C | "Join in without getting punished for doing my job" | Priya |
| D | "Work out whether this is still worth caring about" | Dev |

---

## Step 1 — Create group

| Field | Content |
|---|---|
| `visible` | Group name field, generated invite link, member list (1 member) |
| `actions` | Create group, copy/share invite link |
| `copy` | **OPEN** — see Open Copy below |
| `next` | Founder sees a "waiting for friends" state with the invite link persistent. Plant does not exist yet. |

**Source:** `[STRAWMAN]`, accepted

---

## Step 2 — Friends join

| Field | Content |
|---|---|
| `visible` | Member list filling in; each member's declared limit once set |
| `actions` | Join via link, leave group |
| `copy` | **OPEN** |
| `next` | Season can start once ≥2 members have set limits. |

**Escalation history attaches to the person, not the membership** (D15). A member who leaves and rejoins retains their ratchet. A genuine new member entering at season 5 starts at the group's current average tightening, not at zero.

---

## Step 3 — Each sets a daily limit

| Field | Content |
|---|---|
| `visible` | Per-person: app selection, time limit, declared personal floor. All three visible to the whole group. |
| `actions` | Select apps, set limit, set floor, confirm |
| `copy` | **OPEN** — highest-value copy in the app |
| `next` | Limits lock for the duration of the season. |

- Self-set for both time allowed and which apps count (D3, D4)
- **Primary display is "% tightened since season one"**, absolute minutes secondary (D12) — everyone's `−20%` is comparable; `4h` and `30m` are not
- **Declared personal floor** (D14): *"I won't go below 45 min, this is my job."* Group-visible and consented to at season start.

---

## Step 4 — Season starts

| Field | Content |
|---|---|
| `visible` | Seed planted, day 1 of N, health 60 |
| `actions` | — |
| `copy` | **OPEN** |
| `next` | Daily loop begins. Season 1 = 7 days. |

**Season 1 cannot kill the plant** — 60 health cannot drain in 7 days at −6/day. Deliberate: new groups feel the mechanic before it can hurt them.

---

## Step 5 — A normal day (ambient)

| Field | Content |
|---|---|
| `visible` | **Widget only.** Plant at current stage and posture, day N of M, count of members currently under limit today. |
| `actions` | None required. Opening the app is never necessary. |
| `copy` | **OPEN** |
| `next` | Rolls at local midnight per member's own timezone. |

The no-app-opens constraint is a first-class requirement, not a preference. No feed. No gallery. Social discussion happens in whatever group chat the friends already use.

---

## Step 6 — Threshold crossed

| Field | Content |
|---|---|
| `visible` | Widget reflects one fewer member under limit today |
| `actions` | None — this is not something the user does in the app |
| `copy` | **OPEN** |
| `next` | Counted against today's water. No individual notification — see D7 rationale. |

**Platform mechanism:** `DeviceActivityMonitor.eventDidReachThreshold`. Laddered events at 30/60/90/120/150/180 min give a coarse bucket from purely binary signals, used for headroom detection (D13). No protected quantity ever leaves the device.

⚠️ **Two unverified assumptions gate this step. See Never Verified.**

---

## Step 7 — Day resolves

| Field | Content |
|---|---|
| `visible` | Widget: health delta reflected in plant posture |
| `actions` | — |
| `copy` | **OPEN** |
| `next` | `water = 10 × (under_limit / total_members)` ; `decay = −6` ; net applied |

| Under limit (of 5) | Water | Net |
|---|---|---|
| 5/5 | +10 | **+4** |
| 4/5 | +8 | +2 |
| 3/5 | +6 | 0 |
| 2/5 | +4 | −2 |
| 0/5 | 0 | **−6** |

Normalised by group size so difficulty does not scale with recruiting.
**Recovery:** 3 consecutive 5/5 days → +10 bonus (D3 recovery ruling).

---

## Step 8 — Group awareness

| Field | Content |
|---|---|
| `visible` | Widget shows the count under limit today; the app shows per-member status |
| `actions` | — |
| `copy` | **OPEN** — hardest copy in the app; must not read as blame |
| `next` | — |

Attribution is present because shared fate requires it. Partial watering keeps a single slip from being catastrophic, which is what makes attribution tolerable rather than corrosive.

---

## Step 9 — Plant dying (health 20–39)

| Field | Content |
|---|---|
| `visible` | Visibly failing plant on the widget |
| `actions` | Recovery is available and stated: 3 consecutive full days heals +10 |
| `copy` | **OPEN** — must always answer "what would help" |
| `next` | Continues to decay or recovers |

**The only notification in the product** fires on entry to this band. Never "you've used 80% of your limit."

---

## Step 10 — Health bands

| Band | State | Widget |
|---|---|---|
| 80–100 | Thriving | flowering, upright |
| 60–79 | Healthy | upright |
| 40–59 | Wilting | drooping |
| 20–39 | Dying | visibly failing — notification |
| 1–19 | Critical | bare |
| 0 | Dead | — |

---

## Step 11 — Season ends

| Field | Content |
|---|---|
| `visible` | Outcome, seasons-survived count incremented if survived |
| `actions` | Start next season |
| `copy` | **OPEN** |
| `next` | Survived → next season is longer and limits must tighten. **Failed → escalation pauses**, limits repeat (D16). Failure does not compound. |

---

## Step 12 — Progression

| Seasons survived | Stage |
|---|---|
| 1 | sprout |
| 2–3 | leafed seedling |
| 4–5 | flowering |
| 6+ | mature, elaborate |

Intrinsic escalation, not rarity (D10). Stage is legible on the widget with no comparison to other groups. No collection view — a gallery is an app open, and app opens are the thing this product exists to reduce.

---

## Step 13 — Next season

| Field | Content |
|---|---|
| `visible` | New limits proposed: previous limit tightened, or snapped down to unused headroom bucket if it was never approached (D13) |
| `actions` | Confirm, adjust down further, invoke declared floor |
| `copy` | **OPEN** |
| `next` | Returns to Step 4 |

Escalation stops at each member's declared personal floor.

---

## Reach — final

| Dummy | Job | Reach |
|---|---|---|
| Jules | A | 13/13 ✅ |
| Sam | B | 13/13 ✅ |
| Priya | C | 13/13 ✅ |
| Dev | D | 13/13 ✅ |

---

## Open

### Open copy
Every `copy` field above is unspecified. This is the largest remaining body of
work and it is where most of this product's success actually lives. Three are
load-bearing:

1. **Step 3** — how a limit is framed at the moment of setting it
2. **Step 8** — reporting a slip without assigning blame
3. **Step 9** — a dying plant that always answers "what would help"

### Open decisions
- **D17 partial watering** — proposed and walked, awaiting explicit confirmation
- Timezone handling when a group spans zones (Step 5 `next`)
- Whether limits can be adjusted mid-season (currently: no)
- What the founder sees for the days before anyone accepts (Step 1 `next`)
- Group dissolution — what happens to accumulated seasons when a friend group drifts apart
- Whether escalating difficulty implies eventual guaranteed failure, and whether that is intended
