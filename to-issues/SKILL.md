---
name: to-issues
description: Break a plan, spec, or PRD into independently-grabbable issues on the project issue tracker using tracer-bullet vertical slices. Use when user wants to convert a plan into issues, create implementation tickets, or break down work into issues.
---

# To Issues

Break a plan into independently-grabbable issues using vertical slices (tracer bullets).

Issue tracker — where issues live. Default to **GitHub issues** via the `gh` CLI (`gh issue create`) unless the user specifies another tracker.

## Process

### 0. Pre-flight: is there something to break down?

You need a plan, spec, or PRD to slice into issues. If one is **not** already in the conversation context, not passed as an argument, and no `docs/PRD-*.md` exists in the repo, give the user a one-time heads-up — *"I don't see a PRD or plan to break down (no `docs/PRD-*.md`, nothing in context). Want to run `to-prd` first, or point me at the source?"* — and continue based on their answer. This is a soft nudge, not a gate. Skip it whenever a plan/PRD is already available (in context, as an argument, or on disk).

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes an issue reference (issue number, URL, or path) as an argument, fetch it from the issue tracker and read its full body and comments.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. Issue titles and descriptions should use the project's domain glossary vocabulary, and respect ADRs in the area you're touching (both live under `docs/` if present).

### 3. Draft vertical slices

Break the plan into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices may be 'HITL' or 'AFK'. HITL slices require human interaction, such as an architectural decision or a design review. AFK slices can be implemented and merged without human interaction. Prefer AFK over HITL where possible.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories this addresses (if the source material has them)

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

### 5. Publish the issues to the issue tracker

Each slice is labeled by its **type**: `hitl` or `afk`. Ensure both labels exist before publishing — `gh issue create --label` fails on a missing label — so run `gh label create hitl --force` and `gh label create afk --force` first (`--force` is a no-op if the label already exists).

Publish issues in **dependency order** (blockers first), so the real issue numbers returned by `gh issue create` can fill the "Blocked by" field of later issues. For each approved slice, publish with `gh issue create`, passing `--title`, `--body` from the template below, and `--label hitl` or `--label afk`.

In each issue body, instruct the implementing agent to finish by creating a commit whose message describes what was achieved (the issue title is a fine default).

Report the created issue numbers and URLs back to the user.

<issue-template>
## Parent

A reference to the parent issue on the issue tracker (if the source was an existing issue, otherwise omit this section).

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it here and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- A reference to the blocking ticket (if any)

Or "None - can start immediately" if no blockers.

</issue-template>

### 6. Write the issues to the repo

After publishing, write each slice to its own file under `docs/issues/`, named `NNN-<slug>.md` — where `NNN` is a zero-padded sequence number in dependency order and `<slug>` is a kebab-case version of the title. For example: `001-install-dependencies.md`, `002-walking-skeleton.md`.

Each file uses the same issue-body template as above, with a link to the published GitHub issue at the top (e.g. `Issue: <url>`).

Just write the files; do NOT stage or commit them — the user commits docs changes manually (e.g. `git commit -m "docs"` once both `to-prd` and `to-issues` have run). Numbers are sequential within this breakdown, so regenerating a plan may overwrite existing files in `docs/issues/`.

Do NOT close or modify any parent issue.