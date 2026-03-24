---
name: resume
description: Resume work from the previous session. Reconstructs context from git history, plan files, changelogs, and TODO markers. Use when starting a new session that continues prior work, or when user types /resume. No external dependencies required.
user_invocable: true
---

# Resume Previous Session

Restore context from the last session using only local project state.

## Steps

### 1. Detect current project

- Read the current working directory name and any CLAUDE.md or PROJECT.md in the root
- Identify the project name, stack, and any session conventions

### 2. Check recent git activity

```bash
# Last 10 commits within 7 days
git log --oneline --since="7 days ago" -10 2>/dev/null

# Uncommitted changes
git status --short 2>/dev/null

# Active branches (current branch may indicate in-progress work)
git branch --list 2>/dev/null
```

- Summarize what was last worked on based on commit messages
- Flag uncommitted changes as potential in-progress work
- Note if on a feature branch vs main

### 3. Check for active plan files

Search for plan files with incomplete steps:
```
PLAN.md, .planning/PLAN.md, docs/PLAN.md, TODO.md
```

- If found, extract unchecked items (`- [ ]`)
- Report the plan name and how many steps remain

### 4. Check changelog for recent entries

Look for:
```
CHANGELOG.md, docs/CHANGELOG.md
```

- Read the `[Unreleased]` section for recent changes
- This shows what was completed but not yet released

### 5. Scan for TODO/FIXME/NEXT markers in recent commits

```bash
# Find TODO/FIXME/NEXT comments added in recent commits
git diff HEAD~5..HEAD 2>/dev/null | grep -E "^\+" | grep -iE "(TODO|FIXME|NEXT|HACK|XXX)" || true
```

- These often indicate "I'll come back to this" intentions from the last session

### 6. Check for session handoff files

Some workflows leave explicit handoff notes. Look for:
```
.planning/HANDOFF.md, HANDOFF.md, .session-notes.md, SESSION.md
```

- These are the most explicit form of "what's next" from a prior session

### 7. Present the resume summary

Format output as:

```
## Resuming: {project name}

**Last activity**: {date of most recent commit}
**Branch**: {current branch}

**Recent commits**:
- {commit 1}
- {commit 2}
- {commit 3}

**Uncommitted changes**: {yes/no + file count}

**Active plan**: {plan status or "none found"}
{if plan exists, show remaining unchecked items}

**Recent TODOs added**:
{list any TODO/FIXME/NEXT from recent diffs, or "none"}

**Changelog (unreleased)**:
{unreleased items or "no changelog found"}

---
What would you like to work on?
```

## Edge Cases

- **Not a git repo**: Skip git steps, rely on plan files and changelogs only. Say "This directory isn't a git repo — showing what I can find from project files."
- **No recent activity (>7 days)**: Expand git log to 30 days but warn: "Last commit was {N} days ago — context may be stale."
- **Empty project / no signals**: Say "No recent session context found. What are you working on?" — don't fabricate context.
- **Multiple plan files**: Show all, sorted by modification date (most recent first).
- **Monorepo**: Check if the cwd is a subdirectory and scope git log to that path with `-- .`
