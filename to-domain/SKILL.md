---
name: to-domain
description: Build and sharpen a project's domain model. Use when the user wants to pin down domain terminology or a ubiquitous language, record an architectural decision (ADR), or when another skill needs to maintain the domain model.
---

# Domain Modeling

Actively build and sharpen the project's domain model as you design. This is the *active* discipline — challenging terms, inventing edge-case scenarios, and writing the glossary and decisions down the moment they crystallise. (Merely *reading* `CONTEXT.md` for vocabulary is not this skill — that's a one-line habit any skill can do. This skill is for when you're changing the model, not just consuming it.)

Run it as a **grilling session**: interview the user relentlessly, one question at a time (the `grill-me` style), but capture the results into the docs as they crystallise — the glossary and any ADRs. The interview surfaces the model; the files make it durable.

## File structure

All project docs live under `docs/`, indexed by `docs/README.md`. A single-context repo:

```
/
└── docs/
    ├── README.md          ← the docs index / map (this skill maintains it)
    ├── CONTEXT.md         ← the glossary / ubiquitous language
    ├── PRD-01.md          ← product specs (to-prd)
    ├── adr/               ← architecture decisions
    │   ├── 0001-event-sourced-orders.md
    │   └── 0002-postgres-for-write-model.md
    └── issues/            ← issue specs (to-issues)
```

If a `docs/CONTEXT-MAP.md` exists, the repo has multiple bounded contexts. It records how the contexts relate; each context keeps its own glossary co-located with its code, because that locality is the whole point of a bounded context:

```
/
├── docs/
│   ├── CONTEXT-MAP.md                 ← how contexts relate (a DDD artifact, NOT the docs index)
│   └── adr/                           ← system-wide decisions
└── src/
    ├── ordering/
    │   ├── CONTEXT.md
    │   └── docs/adr/                   ← context-specific decisions
    └── billing/
        ├── CONTEXT.md
        └── docs/adr/
```

Create files lazily — only when you have something to write. If no `docs/CONTEXT.md` exists, create it when the first term is resolved; if no `docs/adr/` exists, create it when the first ADR is needed.

## The docs index (`docs/README.md`)

`docs/README.md` is the navigation map for the whole `docs/` folder — it lets a reader (human or agent) find what they need without opening every file. **This skill owns it:** create it if missing, keep it accurate.

It documents the *convention*, so it can describe every doc type even before any file of that type exists. Keep it short:

```md
# Project Docs

Map of this project's documentation.

- **[CONTEXT.md](CONTEXT.md)** — the glossary / ubiquitous language. Start here to learn what the domain terms mean.
- **PRD-NN.md** — product requirement docs, one per feature (`PRD-01.md`, `PRD-02.md`, …).
- **[adr/](adr/)** — architecture decision records (`NNNN-slug.md`): why significant, hard-to-reverse choices were made.
- **[issues/](issues/)** — issue specs (`NNN-slug.md`) mirroring the tracker.
```

Don't conflate the index with `CONTEXT-MAP.md`: the index says *where docs live*; the context map (multi-context only) says *how bounded contexts relate*.

## During the session

### Challenge against the glossary

When the user uses a term that conflicts with the existing language in `CONTEXT.md`, call it out immediately. "Your glossary defines 'cancellation' as X, but you seem to mean Y — which is it?"

### Sharpen fuzzy language

When the user uses vague or overloaded terms, propose a precise canonical term. "You're saying 'account' — do you mean the Customer or the User? Those are different things."

### Discuss concrete scenarios

When domain relationships are being discussed, stress-test them with specific scenarios. Invent scenarios that probe edge cases and force the user to be precise about the boundaries between concepts.

### Cross-reference with code

When the user states how something works, check whether the code agrees. If you find a contradiction, surface it: "Your code cancels entire Orders, but you just said partial cancellation is possible — which is right?"

### Update CONTEXT.md inline

When a term is resolved, update `docs/CONTEXT.md` right there. Don't batch these up — capture them as they happen. Use the format in [CONTEXT-FORMAT.md](./CONTEXT-FORMAT.md).

`CONTEXT.md` should be totally devoid of implementation details. Do not treat `CONTEXT.md` as a spec, a scratch pad, or a repository for implementation decisions. It is a glossary and nothing else.

### Offer ADRs sparingly

Only offer to create an ADR when all three are true:

1. **Hard to reverse** — the cost of changing your mind later is meaningful
2. **Surprising without context** — a future reader will wonder "why did they do it this way?"
3. **The result of a real trade-off** — there were genuine alternatives and you picked one for specific reasons

If any of the three is missing, skip the ADR. Use the format in [ADR-FORMAT.md](./ADR-FORMAT.md).