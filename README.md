# Claude Skills

My personal collection of [Claude Code](https://docs.anthropic.com/en/docs/claude-code) skills — curated, handpicked, and adapted to my own workflow as a solo/indie developer.

Each skill is a self-contained directory with a `SKILL.md` (and optional reference files / scripts). Claude Code loads a skill's `description` to decide when to invoke it, then reads the full `SKILL.md` on demand.

## Skills

| Skill | Description |
|-------|-------------|
| [`grill-me`](grill-me/) | Interview relentlessly about a plan or design until reaching shared understanding. |
| [`handoff`](handoff/) | Compact the current conversation into a handoff document for another agent. |
| [`improve-codebase-architecture`](improve-codebase-architecture/) | Find deepening/refactoring opportunities to make a codebase more testable and navigable. |
| [`to-issues`](to-issues/) | Break a plan or PRD into independently-grabbable tracker issues. |
| [`to-prd`](to-prd/) | Turn the current context into a PRD and publish it to the issue tracker. |
| [`write-a-skill`](write-a-skill/) | Author new skills with proper structure and progressive disclosure. |

## Install

Clone into your Claude Code skills directory so each skill sits at `~/.claude/skills/<skill-name>/SKILL.md`:

```bash
git clone https://github.com/<your-username>/claude-skills.git ~/.claude/skills
```

Or cherry-pick individual skills by copying their directory into `~/.claude/skills/`.

Restart Claude Code (or start a new session) to pick up newly added skills.

## Adding a skill

1. Create a directory named after the skill (kebab-case).
2. Add a `SKILL.md` with YAML frontmatter (`name`, `description`) followed by the instructions.
3. Keep `SKILL.md` concise; split long reference material into separate files in the same directory.
4. Add a row to the table above.

The `write-a-skill` skill documents the conventions in detail.

## Layout

```
.
├── README.md
├── LICENSE
├── .gitignore
└── <skill-name>/
    └── SKILL.md          # required
    └── REFERENCE.md      # optional supporting files
```

## License

[MIT](LICENSE)
