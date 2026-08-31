# The Developer Cast

Invoked with `--dev-cast`. Use it when the plan has no end-user surface — a refactor, a queue, a schema change, an internal library, an API. There is no screen, but there is still a user: the next developer, and they fail in exactly the same four ways.

This mode is what keeps `crash-test` useful for backend and infrastructure work instead of making it a UI-only skill.

## The four fields, translated

| UI field | Developer equivalent |
|---|---|
| `visible` | What a developer can see without reading the implementation: types, signatures, doc comments, README, error messages, log output |
| `actions` | What they can call, configure, or pass at this point — and nothing else |
| `copy` | The literal names. Function names, parameter names, enum values, error strings, config keys, event names |
| `next` | What comes back: return shape, thrown errors, side effects, timing, retry semantics |

## The four gates, translated

**G1 Goal — will they try the right thing?**
Fails when your module's decomposition does not match how the task is thought about. The developer wants to "charge a patient's card"; your API offers `createIntent`, `attachMethod`, `confirm`. They will not guess the sequence. Also fails when a required setup step is invisible: nothing in the API's surface implies that `init()` must run first.

**G2 Notice — will they find the right call?**
Fails when the right function is one of twelve exported names with no grouping, when the correct path is only in a doc page they will not read, or when discovery depends on autocomplete that a broad union type defeats. Ask: with only the type signature and IDE autocomplete, does the correct next call present itself?

**G3 Associate — will they believe it does the thing?**
The richest gate for APIs. Fails on: names that describe implementation (`processRecord`) rather than outcome; two plausible functions with no stated difference (`update` versus `patch` versus `set`); a name that means something else in the ecosystem the developer came from; booleans whose polarity is a coin flip (`skipValidation` versus `validate`); parameters whose units are unstated (`timeout: 30` — seconds or milliseconds?); and destructive operations with mild names.

**G4 Feedback — will they know it worked?**
Fails on: silent failure; a success return that does not confirm what changed; errors that say `500` and nothing actionable; async work that returns before the effect is observable, with no way to await it; retries that are neither documented nor idempotent; and error messages that expose internals or leak data.

## The dummies

Same rules as `references/casting.md` — behaviour and context, never seniority as a shorthand for competence. The axes that matter here:

| Axis | Extremes |
|---|---|
| **Prior ecosystem** | arrives from a library with different conventions ↔ arrives with no reference point |
| **Reading depth** | reads the source ↔ reads only autocomplete and the first code sample |
| **Context** | greenfield integration ↔ patching a production incident at 2am |
| **Failure exposure** | hits only the happy path ↔ hits rate limits, partial failures, retries, races |
| **Volume** | one record ↔ batch of 50,000 |

Include one worst case: the developer holding an incident, reading only the error message, with no time to read source. If your error strings do not survive that dummy, they are decoration.

## Goals in their words

Same rule, same failure mode. A goal must not name your API.

| Bad | Good |
|---|---|
| "Call `bulkArchive` with an ID array" | "Clear out last quarter's records without hammering the API" |
| "Use the migration runner" | "Get this schema change onto prod without downtime" |
| "Handle the webhook" | "Know when a payment actually settled" |

## What this mode finds that a code review does not

A reviewer reads the diff and asks whether it is correct. A dummy reads only the *surface* and tries to accomplish something. That difference reliably surfaces:

- Naming that is accurate to the implementation and misleading from outside
- Required call ordering that is nowhere expressed in the types
- Error messages written for the author rather than the reader
- Migrations whose rollback path was never specified — a `GAP` at G4
- Config keys whose default is undocumented, so nobody knows what happens if omitted
- Operations that are not idempotent and do not say so, which the retrying dummy discovers immediately

## Compliance note

When the plan touches sensitive data, run the worst-case developer against the observability surface specifically: what lands in logs, in error messages, in traces, in analytics, in a bug report they will paste into a ticket. "The error message includes the patient's name" is a G4 finding with a regulatory tail, and it is exactly the kind of thing a dummy reading only the surface will trip over.
