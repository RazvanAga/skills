---
name: handoff
description: Compact the current conversation into a handoff document for another agent to pick up.
argument-hint: "[current token count, e.g. 59k] and/or what the next session will focus on"
---

## Preflight: continue here, or hand off?

Before writing anything, assess whether handing off is actually the right call. If the user passed a current token count (e.g. `59k`), ground the verdict against the model's context window — a low figure means there's room to keep going. Otherwise judge from what you can observe: how much of the conversation is still live context, and whether you're still tracking the work cleanly or starting to lose the thread.

Open with a one-line verdict and a sentence of reasoning, e.g.:
- **Handoff makes sense** — lots of context behind us. Proceeding.
- **Possibly premature** — plenty of room left; you could implement the next issue or two before handing off. Writing the doc anyway since you invoked the skill.

This is advisory only — always continue and write the document. The goal is to flag premature handoffs so the user can choose to keep going, not to block the handoff.

## Handoff document

Write a handoff document so a fresh agent can continue the work. Save it to the OS temp directory — not the workspace — and state the full path where you saved it (the kickoff prompt references it).

Handoff happens at a clean boundary: the current issue is complete and committed before you hand off. So the document carries only what a fresh agent can't recover from the tracker and git — never re-narrate work that's already durable there:

- **Progress** — completed issues, by number and title, one line each, derived from the tracker and git (`gh issue list --state closed`, `git log`). Point to the closed issues and `docs/issues/` for detail.
- **Decisions not yet written down** — anything settled in conversation that isn't already in a commit, ADR, or issue comment. This is the heart of the handoff.
- **Gotchas** — traps already discovered (flaky test, env quirk, a ruled-out dead end) so the next agent doesn't re-pay for them.
- **Next** — the next issue(s) to pick up, and where to orient first: `docs/` (README, CONTEXT.md, adr/, issues/).
- **Suggested skills** — skills the next agent should invoke.

Redact sensitive information (API keys, passwords, PII).

If the user passed arguments, a token figure like `59k` feeds the preflight above; treat any remaining text as the focus of the next session and tailor the doc accordingly. With no focus text given (e.g. just `100k`), the default focus is the next open issue — the one after the issue you just completed.

## Kickoff prompt

After writing the document, output a short, ready-to-paste prompt the user can hand to the next agent. Point at the handoff doc by path and state the first concrete action. For issue-driven work the default is:

> Read the handoff at `<path>` and implement the next open issue.

Print it outside the document, as the last thing in your reply, so it is easy to copy.
