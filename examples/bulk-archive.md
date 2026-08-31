# Worked Example — Bulk Archive

A complete two-run session on a plan that looks fine. Four findings in the first
run; three of them are facts about the document, one is speculation. The
engineer's ruling then breaks something that was previously unreachable.

---

## The plan, as written

```markdown
## Bulk archive

- Add checkboxes to each row in the list.
- Add an "Archive" button in the toolbar, enabled when ≥1 row is selected.
- Clicking Archive archives the selected rows and shows a success toast.
- Archived rows are hidden from the default list view.
```

Four bullets. Reviewable in ten seconds. Approved in most standups.

---

## Phase 0 — Recon

Dispatched in parallel, no questions asked of the engineer:

- **Codebase** — list is virtualised at `src/list/rows.tsx:88`; no selection state exists anywhere; no archived/filter view exists in `src/list/`.
- **Prior art** — Gmail (select-all-matching + persistent undo bar), Linear (bulk actions with a scoped label), Notion (trash with a 30-day window). All three make the archived set reachable.
- **Constraints** — archive endpoint accepts max 100 IDs per call (`docs/api/records.md`); the plan does not mention batching.
- **User evidence** — 3 support tickets in the last quarter asking "how do I get something back after archiving it."

## Phase 0 — Compiled spec (excerpt)

| | Step 1 — List | Step 2 — Selected | Step 3 — Archive clicked |
|---|---|---|---|
| `visible` | rows + checkboxes; toolbar | same, button enabled | **GAP** |
| `actions` | check a row; click Archive (disabled) | check more; click Archive | none specified |
| `copy` | `"Archive"` | `"Archive"` | toast text **GAP** |
| `next` | selection state **GAP** | — | "archives the rows and shows a toast" |

Three gaps found before a single dummy moved. This is the normal shape of a first compile.

## Phase 1 — Cast (approved, with one engineer correction)

| Dummy | Defining trait | Job |
|---|---|---|
| **Marcus** | 400 rows, daily, works interrupted `[analytics]` | "Get last quarter off my list" |
| **Dana** | opens it once a quarter `[INVENTED ⚠️ → engineer: correct, ~quarterly]` | "Tidy up before the board meeting" |
| **Priya** | mobile, between appointments `[support-tickets]` | "Clear the two I just finished" |
| **Worst case** | screen reader, 400 rows | "Get last quarter off my list" |

---

## Run 1

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

```
GAP · G2 — Marcus, step 2
   > "Add checkboxes to each row in the list."
   No select-all, no range select, and nothing about whether selection survives
   scrolling a virtualised list (src/list/rows.tsx:88). Marcus's goal needs ~40
   rows. As specified, that is 40 individual clicks.
   [lens] interaction cost — scale to the worst realistic dataset, not the demo one
```

```
CONFLICT — steps 4–5
   > "Archived rows are hidden from the default list view."
   Implies a non-default view. None is defined in the plan, and none exists in
   src/list/. As written, archiving is neither verifiable nor reversible.
   Corroborated: 3 support tickets asking how to recover an archived record.
```

```
PREDICTION · G3 · confidence: medium — Dana, step 2
   > "Add an 'Archive' button in the toolbar"
   Dana comes from a tool where archive meant permanent delete. She may avoid
   the button rather than risk it, and do nothing.
   Basis: [prior-art] two public reviews of that competitor describe this exact
   confusion. Weakest finding in this run — engineer's call.
```

**Reach after run 1**

| Dummy | Job |
|---|---|
| Marcus | 3/5 🚧 |
| Dana | 2/5 🚧 |
| Priya | 3/5 🚧 |
| Worst case | 2/5 🚧 |

Three of four findings are verifiable against the document or the repo. Only the
last one asks the engineer to trust a simulated person.

---

## Phase 3 — Rulings

| # | Blocker | Ruling |
|---|---|---|
| 1 | No undo | **Accepted** — undo in the toast, 10s window |
| 2 | No select-all | **Accepted** — select-all + shift-range; batch at 100/call |
| 3 | Archived set unreachable | **Substituted** — no archived view in v1; undo covers the mistake case. Support tickets revisited post-launch. |
| 4 | `Archive` label ambiguity | **Overruled** — "our users have used this word for four years." Recorded as `[engineer]`. |

---

## Run 2 — the re-walk

```
🚧 NEW — Marcus (interrupted) · Step 4, previously unreachable
   Gate G4 · Class: PREDICTION · confidence: high

   Caused by ruling D1 (10-second undo window).

   "Something needed my attention right as I archived. By the time I looked
    back the toast was gone and so was my chance to fix it."

   ➡️ Persistent undo bar until dismissed or navigation, or archive-with-delay.
      [prior-art] Gmail — undo bar persists
      [lens] G4 — timed undo is not undo for an interrupted user
      ⚠️ Also affects Priya (toast lost on scroll, mobile) and worst case
         (toast not announced to screen readers).
```

```
CONFLICT — introduced by ruling D2
   > select-all + batching at 100/call
   Marcus selects all 400. The archive endpoint caps at 100 IDs
   (docs/api/records.md). The plan specifies no partial-failure state: what does
   he see if batch 3 of 4 fails?
```

Ruling D1 was a reasonable fix that a 400-row interrupted user defeats. Ruling
D2 opened a partial-failure state nobody had considered. Neither was reachable
before the first two blockers were cleared — that is the frontier working.

**Reach after run 2**

| Dummy | Job |
|---|---|
| Marcus | 4/5 🚧 |
| Dana | 5/5 ✅ |
| Priya | 4/5 🚧 |
| Worst case | 4/5 🚧 |

---

## What the session produced

From four bullets that read as complete:

- **2 gaps** — no undo specified; no selection model for a virtualised list
- **2 conflicts** — an implied view that does not exist; a batch limit the plan ignores
- **1 partial-failure state** nobody had thought about
- **1 tradeoff made explicit** — timed versus persistent undo, decided knowingly
- **1 overruled prediction**, recorded with the engineer's reasoning attached

None of it required a running implementation. All of it would otherwise have
surfaced in code review at best, in a support ticket at worst.

And the standing caveat, stated at the end of every real run: a full coverage
table means the plan is coherent. Marcus, Dana, and Priya are stand-ins. Nobody
has tested this with a person yet.
