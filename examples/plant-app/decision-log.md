# Decision Log — Group Accountability Plant

Neither the agent's plan nor the engineer's plan. One plan, with every point of
divergence surfaced, ruled on by the engineer, and recorded.

Status: ☑ converged (1 item awaiting confirmation)
Runs completed: 4 · Dummies: 4 · Jobs: 4 · Final reach: 13/13 on all

---

## D1 — What gets measured

**Forced by:** Phase 0 recon — verified platform constraint, before any walk

> Apple, `DeviceActivityReport`: *"To protect the user's privacy, your extension
> runs in a sandbox. This sandbox prevents your extension from making network
> requests or moving sensitive content outside the extension's address space."*

**Agent presented:** two incompatible options — A1 real OS screen time (truthful,
needs Apple entitlement approval, platform-asymmetric) vs. A2 declared sessions
(ships anywhere, measures intention not behaviour, trivially gamed).

**Engineer ruled:** A2.

**Divergence:** none — chose from offered options.

**Cascade:** eliminated the `FamilyControls` entitlement dependency. Later
reversed by D5.

---

## D2 — How a slip lands on the group

**Forced by:** Phase 0

**Agent presented:** B1 shared fate (the engineer's original idea) vs. B2 shared
reward, noting that Flora — the closest successful precedent — chose B2:
*"if you successfully stay off the phones together, each of you will win an
additional tree."*

**Engineer ruled:** B1 shared fate.

**Divergence:** none, but the engineer chose against the prior-art precedent
knowingly. Recorded because it is the product's core differentiator and the
source of most of its social risk.

---

## D3 — Resolving the inaction-dominance conflict

**Forced by:** `CONFLICT` found during compilation, before any dummy walked —
under A2 + B1, damage could only occur during a voluntary session, so the optimal
strategy was to never play. The mechanic punished the behaviour it existed to
encourage.

**Agent recommended:** decay — the plant loses health passively, sessions are the
only way to water it.

**Engineer ruled:** decay, **plus** a month-long season arc and *"if someone
exceeds their screen time limit for the day, the plant doesn't get watered."*

**Divergence:** substituted and extended. The engineer's addition was better than
the recommendation — decay alone left the reward loop undefined; the daily
watered/not-watered cycle supplied it.

**Cascade:** silently reversed D1 (see D5). Also introduced the feedback
asymmetry and cold-start problems that produced D6.

---

## D4 — Limit setting

**Forced by:** Run 2 — Jules halted at step 3 (G1, `GAP`: limits invisible to the
group) and Priya halted at step 3 (G3, `GAP`: which apps count unspecified). Two
of four dummies dead at the same step for independent reasons.

**Agent recommended:** group-visible limits locked at season start; per-person app
selection.

**Engineer ruled:** self-set for both time allowed and which apps count, *"but
encouraged to reduce time on social media apps."*

**Divergence:** **substituted, and the ruling answered a different question than
the finding asked.** Jules's blocker was about *visibility*, not about who
chooses. The agent proceeded on self-set + group-visible + locked, and flagged the
assumption. It was not corrected.

**Cascade:** "encouraged" had no mechanism behind it, which produced the most
important finding in the run — see D5.

---

## D5 — The floor

**Forced by:** Run 3 — Sam **completed all 13 steps** and reported: *"We survived
the season, the group got a streak, Jules is thrilled, and I unlocked a fern. My
screen time is exactly what it was in January."* Classified `CONFLICT`: nothing in
the design required or verified a reduction.

**Agent recommended:** baseline-relative limits (measure 2 weeks, then set limits
as a % reduction from each person's own baseline).

**Engineer ruled:** accepted.

**Cascade — three regressions, all found in run 4:**
1. Baselines need *quantities*, which Apple's sandbox blocks. This partially
   reversed D1's platform feasibility.
2. Sam gamed the measurement window — inflated his baseline, kept his headroom.
   Goodhart's law with a two-week window. The fix chosen to stop Sam did not stop
   Sam.
3. A 14-day dead start before the group could play at all — a worse cold-start
   problem than the one D6 had just solved.

**Superseded by D8.**

---

## D6 — Season length

**Forced by:** Run 2 — Sam halted at step 4 (G1, `GAP`): a 30-day co-signed
commitment with no experienced benefit. *"I've already quit two apps like this."*

**Agent recommended:** a 7-day first season; 30-day seasons unlock after one
completed. Cited: Forest converts on a 25-minute ask; Duolingo's streak begins at
day one.

**Engineer ruled:** accepted and extended — *"I like the idea of slowly building
seasons to be longer and longer."* Progressive escalation: 7 → 14 → 21 → 30.

**Divergence:** none. Extension improved it.

---

## D7 — Feedback surface (no-phone constraint)

**Forced by:** engineer constraint, mid-run — *"I don't want this app to encourage
phone usage (you shouldn't need to be on your phone for the app to work)."*

This **invalidated the agent's own recommendation from the previous message**,
which had proposed a daily-perking plant to fix the feedback asymmetry — that
required opening the app daily. Withdrawn.

`CONFLICT` created: B1 shared fate requires people to *know* when the plant
suffers; knowing requires a surface; every surface is a phone pull.

**Agent recommended:** home/lock-screen widget as the primary and ambient surface;
notifications only on state transitions that matter; the social layer deliberately
pushed out of the app into the group chat friends already use. No feed.

**Engineer ruled:** accepted.

**Strategic consequence recorded:** DAU becomes an anti-metric. Success means
fewer opens over time. Season completion rate and cross-season group survival are
the honest measures.

---

## D8 — Escalating limits (supersedes D5)

**Forced by:** run 4 — Sam completed again via the baseline exploit, and the
baseline requirement collided with Apple's sandbox.

**Agent recommended:** escalating limits — self-set in season 1, mandatory
tightening thereafter. **This was the agent withdrawing its own D5 recommendation
from the previous round.** Ungameable because the ratchet is relative to your own
prior declared commitment, needs no measurement window, and needs no quantities.

**Engineer ruled:** accepted.

**Cascade — two fixed for free, one regression:**
- The 14-day dead start disappeared entirely
- Platform feasibility restored: binary threshold events suffice
- **Regression:** absolute self-set limits are incomparable, so Jules's problem
  returned in a new form → D12

---

## D9–D16 — Accepted without divergence

| ID | Decision | Forced by |
|---|---|---|
| **D9** | Streak is an accumulating count of seasons survived, not a breakable chain | Run 3, Sam, step 11 — `PREDICTION` high confidence: a multi-season chain destroyed by one person is the highest-anxiety mechanic in consumer software |
| **D10** | Intrinsic plant escalation by seasons survived, not rarity | Run 3, Dev, step 12 — `CONFLICT`: rarity is comparative by definition, and D7 removed comparison. A collection view is also an app open, contradicting D7. |
| **D11** | Decay/healing table (see spec) | Run 5 — engineer requested a proposal |
| **D12** | Primary display is "% tightened since season one"; minutes secondary | Run 4, Jules, step 3 — `CONFLICT` created by D8 |
| **D13** | Laddered thresholds (30/60/90/120/150/180 min) detect unused headroom; limits snap down if never approached | Run 4, Sam — closes the coast without any quantity leaving the device |
| **D14** | Declared, group-visible personal floor; escalation stops there | Run 4, Priya — the ratchet had no bottom and her work has one |
| **D15** | Escalation history attaches to the person, not the membership | Run 4, Dev — leave-and-rejoin reset the ratchet |
| **D16** | Escalation pauses after a failed season; limits repeat | Run 4 — failure should not compound into impossibility |

---

## Awaiting confirmation

### D17 — Partial watering

**Forced by:** proposing the D11 numbers. All-or-nothing watering scales badly
with group size: five members at 90% individual daily reliability water the plant
on only 59% of days (0.9⁵); six members, 53%. A group of well-intentioned people
watches the plant decline for reasons that feel arbitrary, and **recruiting more
friends makes it worse.**

**Agent recommends:** `water = 10 × (members_under_limit / total_members)`,
normalised so difficulty does not scale with group size.

**Status:** walked and converged on this basis, but **this modifies the D2 shared-fate
ruling** and has not been explicitly confirmed. Confirm or reject before building.

---

## Never verified

Stated plainly rather than letting a converged spec imply these were checked.

| Question | Why unresolved | Cheapest way to find out |
|---|---|---|
| Can `DeviceActivityMonitor` make network requests when a threshold fires? | Apple documents the sandbox restriction explicitly for the *report* extension and says nothing either way about the *monitor* extension. | Half-day spike. **This is the critical path — the product's viability rests on it.** If the answer is no, the signal can still reach app storage and sync on next open: degraded, not dead. |
| Is there a low cap on simultaneously monitored `DeviceActivityEvent`s? | Not documented in what was read. D13's laddered thresholds depend on registering ~6 per person. | Same spike. |
| Does iOS expose historical screen-time data pre-install? | Unknown. Would have made an ungameable retrospective baseline possible. Moot under D8. | Same spike. |
| Will Apple grant the `FamilyControls` entitlement for a social game? | The framework is documented as *"Authorize your app to provide parental controls."* Approval is required before App Store submission and the use case is not what it is framed for. | Submit the request early. Note in review notes that the app requests only binary threshold events, never usage quantities — a far narrower ask than the framework grants. |
| Does "Sam" exist in real friend groups? | Zero user evidence. Every cast trait is `[INVENTED ⚠️]` and was never corrected. | Talk to one friend group that has tried a shared accountability app. |

---

## Open — deferred, not dropped

Logged during runs and never blocking; none were silently discarded.

- **All `copy` fields in the spec.** The largest remaining body of work. Three are
  load-bearing: setting a limit (step 3), reporting a slip without blame (step 8),
  and a dying plant that always answers "what would help" (step 9).
- Timezone handling for groups spanning zones
- Whether limits can be adjusted mid-season (currently: no)
- What the founder sees before anyone accepts the invite
- What happens to accumulated seasons when a friend group drifts apart
- Whether escalating difficulty implies eventual guaranteed failure, and whether
  that is intended (a roguelike arc) or a defect
- Season-end restart flow
- Notification copy for the "entering dying" transition

---

## Convergence

| Criterion | Status |
|---|---|
| Every dummy completes every job | ☑ 13/13 × 4 |
| Zero `GAP`s in the walkable spec | ☑ structural — `copy` fields remain open and are listed |
| Zero `CONFLICT`s | ☑ |
| Every `PREDICTION` fixed or explicitly accepted | ☑ |

### Accepted with eyes open

| Finding | Why accepted |
|---|---|
| Sam may churn rather than comply once D13 closes his headroom | Not a spec defect. A product whose mechanic actually bites will lose people who never wanted it to bite. The alternative is a product that doesn't work. |
| B1 shared fate carries social risk Flora deliberately avoided | It is the differentiator. Partial watering (D17) is the cushion. |

**Standing caveat.** A full coverage table means this plan is *coherent*. It is not
evidence the product works. Jules, Sam, Priya and Dev are stand-ins — they
over-comply, under-fumble, and miss habit, laziness and indifference entirely.
Every trait describing them was invented. **No real person has seen this.**
