# Walkable Spec — <feature name>

Compiled from: `<path to the original plan>`
Run: <n> · Status: <compiling | walked | converged>

> Every field below is filled **only** from the plan, the codebase, or a cited
> convention. Unfillable fields are marked `GAP` and left unfilled. Do not
> substitute something plausible.

## Jobs under test

| ID | Job (in the user's words) | Dummies attempting it |
|----|---------------------------|-----------------------|
| A  |                           |                       |
| B  |                           |                       |

---

## Step 1 — <short name>

**Entry:** <how the user arrives here>

| Field | Content |
|---|---|
| `visible` | |
| `actions` | |
| `copy` | |
| `next` | |

**Source:** `<plan line, or file:line, or convention + citation>`

**States specified:** empty-first-run ☐ · empty-no-results ☐ · loading ☐ · partial ☐ · error ☐ · permission-denied ☐ · overflow ☐ · stale ☐

**Gaps at this step:**
- `GAP · G<n>` — <what the plan does not say>

---

## Step 2 — <short name>

<repeat>

---

## Branches

Compile each branch as its own numbered path (`3a`, `3b`). Do not collapse.

---

## Reach

| Dummy | Job A | Job B |
|---|---|---|
| | 0/n | — |

Update after every run. Reach is the progress metric: it only advances when the
engineer rules on a blocker.
