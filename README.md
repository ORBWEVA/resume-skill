# /resume — Session Continuity Skill for Claude Code

Pick up where you left off. Type `/resume` at the start of a new session and get an instant summary of what was last worked on, what's pending, and what to tackle next.

## What it does

1. Reads recent git commits to show what was last touched
2. Finds active plan files with incomplete steps
3. Surfaces TODO/FIXME/NEXT markers from recent diffs
4. Checks changelogs for unreleased work
5. Looks for explicit session handoff files
6. Presents a clean summary so you can jump back in

## Install

```bash
claude install-skill /path/to/resume-skill
# or
claude install-skill github:ORBWEVA/resume-skill
```

## Usage

```
/resume
```

That's it. Run it at the start of any session where you're continuing previous work.

## No dependencies

This skill uses only git and local files — no MCP servers, no external APIs, no accounts needed.

## Tips for better session continuity

The skill works best when you leave breadcrumbs:

- **Use a PLAN.md** with checkbox items (`- [ ]` / `- [x]`)
- **Keep a CHANGELOG.md** with an `[Unreleased]` section
- **Leave TODO comments** in code for things you'll come back to
- **Write commit messages** that explain intent, not just what changed
- **Create HANDOFF.md** when pausing mid-task (some workflow skills do this automatically)

## License

MIT
