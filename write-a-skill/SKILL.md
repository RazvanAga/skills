---
name: write-a-skill
description: Create new agent skills with proper structure, progressive disclosure, and bundled resources. Use when user wants to create, write, or build a new skill.
---

# Writing Skills

A skill is a directory Claude loads on demand. Claude sees only the `description` up front and reads the full `SKILL.md` when a request matches. This is **progressive disclosure**: keep `SKILL.md` lean, and push rarely-needed detail into separate files that Claude opens only when needed.

## Quick start

A skill can be a single `SKILL.md` — frontmatter plus instructions, no scripts. Here is a complete one:

```md
---
name: grill-me
description: Interview the user relentlessly about a plan or design until reaching shared understanding, resolving each branch of the decision tree. Use when user wants to stress-test a plan, get grilled on their design, or mentions "grill me".
---

Interview me relentlessly about every aspect of this plan until we reach a
shared understanding. Walk down each branch of the design tree, resolving
dependencies between decisions one-by-one. For each question, give your
recommended answer. Ask one question at a time. If a question can be
answered by exploring the codebase, do that instead of asking.
```

That's the whole skill. Most skills should start this small and grow only when a real need appears.

## Naming & location

- One directory per skill, named in **kebab-case** (`write-a-skill`, not `WriteASkill`).
- The frontmatter `name:` **must match the directory name**.
- Personal skills live flat at `~/.claude/skills/<name>/SKILL.md`; project skills at `.claude/skills/<name>/SKILL.md`. Don't nest skills inside category folders — discovery expects them one level deep.
- The invocation name comes from `name:`, not the path.

## Structure

```
skill-name/
├── SKILL.md           # required: frontmatter + instructions
├── REFERENCE.md       # optional: detailed docs loaded on demand
├── EXAMPLES.md        # optional: usage examples
└── scripts/           # optional: utility scripts
    └── helper.js
```

## Workflow

1. **Scope it** — infer what you can from the request and codebase; ask the user only about what's genuinely ambiguous (use cases, whether it needs scripts, reference material to bundle).
2. **Draft** — write `SKILL.md` using the template below. Keep it concise.
3. **Split if needed** — move long or rarely-used detail into sibling files (see *When to split*).
4. **Review** — run the checklist, then confirm with the user that it covers their cases.

## SKILL.md template

```md
---
name: skill-name
description: What it does. Use when [specific triggers].
---

# Skill Name

## Quick start
[Minimal working example]

## Workflows
[Step-by-step processes; checklists for complex tasks]

## Advanced features
[Link to separate files: See [REFERENCE.md](REFERENCE.md)]
```

## Writing the description (the critical part)

The description is **the only thing Claude sees** when deciding whether to load the skill. It's surfaced alongside every other installed skill, so it must let Claude distinguish this one.

**Goal** — tell Claude two things:

1. What capability this skill provides.
2. When to trigger it (specific keywords, contexts, file types).

**Format**

- Max 1024 chars; write in the third person.
- First sentence: what it does. Second sentence: `Use when [specific triggers]`.
- YAML gotcha: if the description starts with a special character or contains a `:` followed by a space, wrap the whole value in double quotes so the frontmatter parses.

**Good**

```
Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

**Bad**

```
Helps with documents.
```

The bad one gives Claude no way to tell this from any other document skill.

## When to add scripts

Add a utility script when the operation is deterministic (validation, formatting), the same code would otherwise be regenerated every run, or errors need explicit handling. Scripts save tokens and improve reliability versus generated code.

## When to split files

Split content out of `SKILL.md` when it exceeds ~100 lines, covers distinct domains (e.g. finance vs. sales schemas), or holds advanced features that are rarely needed. Keep references **one level deep** — `SKILL.md` points to a file; that file shouldn't chain to another.

## Review checklist

- [ ] Description includes triggers ("Use when…")
- [ ] `name:` matches the directory; directory is kebab-case
- [ ] `SKILL.md` under ~100 lines
- [ ] No time-sensitive info
- [ ] Consistent terminology
- [ ] Concrete example included
- [ ] References one level deep
