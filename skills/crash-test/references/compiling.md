# Compiling a Plan into a Walkable Spec

Prose cannot be walked. Before any dummy moves, the plan becomes an explicit sequence of steps, each answering four questions. Compilation is not bureaucracy — it is where most of the findings come from, because a plan that reads well usually cannot answer these questions at all.

## The four fields

| Field | Question it answers | Gate it feeds |
|---|---|---|
| `visible` | What is on screen right now? | G1, G2 |
| `actions` | Every action available here — and nothing else | G2 |
| `copy` | The literal text of every label, button, heading, and message | G3 |
| `next` | For each action, what happens? Including when it fails. | G4 |

Fill each field **only** from the plan, the codebase, or an established convention you can cite. Every unfillable field is a `GAP`.

## The `copy` field is where plans break

Engineer-authored plans almost never specify strings. "Add an archive button" does not say what the button says. "Show an error" does not say what the error says.

Do not paper over this. If the plan does not give you the string, the `copy` field is a `GAP` and G3 cannot be evaluated at that step. This is usually the largest single category of findings in a first run, and it is genuinely valuable: microcopy is where most real-world G3 failures live, and it is the cheapest thing in the world to fix at plan time.

## Handling common plan shapes

**Prose paragraphs.** Extract steps in the order a user would encounter them, not the order the plan discusses them. Plans are usually organised by implementation concern (data model, then endpoints, then UI); users experience the reverse. Reordering into user sequence frequently reveals that the plan never specifies an entry point at all.

**Branches.** Compile each branch as its own path with its own step numbers (`3a`, `3b`). Do not collapse branches into a single step — the whole reason a branch exists is that different users go different ways, which is exactly what the cast is for.

**Async and latency.** Any step whose `next` is not instantaneous needs an interim state in `visible`. If the plan does not specify one, that is a `GAP` at G4. "Then it saves" is not a specification of what the user looks at while it saves.

**Existing flows.** When the plan modifies something that already exists, compile from the **code**, not the plan. The plan describes the delta; the dummy experiences the whole. Read the existing implementation and cite `file:line` in `visible`, `actions`, and `copy`. This is where recon pays for itself, and where you will find conflicts between what the plan assumes exists and what actually does.

**Plans with no UI.** Use `--dev-cast` and see `references/developer-cast.md`. The four fields become: what the developer can see (types, docs, errors), what calls are available, what things are literally named, and what comes back.

## Required states

For every step, check whether the plan specifies these. Missing ones are `GAP`s, and they are the states engineers skip most reliably:

- **empty — never used** (first run) versus **empty — no results** (a filter matched nothing). Different situations, different copy, constantly conflated.
- **loading** — and whether it is a spinner, a skeleton, or optimistic
- **partial** — some data arrived, some failed
- **error** — retryable versus terminal, and what the user does next
- **permission denied** — including the case where it is denied *after* they started
- **overflow** — 10,000 rows, a 200-character name, a translation 3× longer
- **stale** — a second tab, an expired session, data changed underneath them

Do not report all seven as separate findings for every step; that buries the blockers. Check them, list the missing ones compactly per step, and escalate only the ones a dummy actually hits.

## Output

Write to `assets/walkable-spec.template.md`'s shape and show it to the engineer **before** walking. Two reasons:

1. They will correct compilation errors immediately, and a misread step invalidates every finding downstream of it.
2. It is the artifact both of you refer to for the rest of the session. When you and the engineer disagree later, you disagree about a specific numbered step with a quoted line — not about a vibe.
