# Worktrees — parallel work without collisions

Git worktrees let several agents (or you and an agent) work different streams at once,
each on its own branch in its own directory, without stepping on a shared tree. Use this
whenever work splits into **independent, parallel-safe streams**. For a single linear
task, just work in the main tree — worktrees aren't worth the overhead.

> **Lesson baked in:** in earlier projects the worktrees worked great but were never
> cleaned up — dozens of orphaned worktrees and branches piled up across two different
> directories, and "did we merge back to main?" became a recurring question. This guide
> fixes that with **one canonical location** and a **mandatory cleanup** step.

## One canonical location

All worktrees live under `.worktrees/` (gitignored). Never create them anywhere else.
One worktree per task, named after the task slug.

## Set up a worktree

```bash
# from the repo root, for task <slug>
git worktree add .worktrees/<slug> -b <slug>
cd .worktrees/<slug>
# install deps if the stack needs them, then work the task here
```

The lead session dispatches a sub-agent to work inside `.worktrees/<slug>`, pointing it
at the task file `.dev/tasks/active/<slug>.md`. The sub-agent works only there and
reports back through that task file's session log — never directly to other agents.

## Merge back

When the task's tests pass and it's reviewed:

```bash
cd <repo-root>
git merge <slug>          # or open a PR from the branch
```

## Clean up — mandatory, every time a task closes

Leaving a worktree behind is how this rots. As soon as a stream is merged (or abandoned):

```bash
git worktree remove .worktrees/<slug>
git branch -d <slug>      # use -D if abandoning unmerged work
```

## Check for orphans

Periodically — and before wrapping up any session that used worktrees:

```bash
git worktree list                          # everything currently registered
git worktree prune                         # drop entries whose dirs are gone
# branches already merged into the default branch — safe to delete:
DEFAULT=$(git symbolic-ref --short refs/remotes/origin/HEAD 2>/dev/null | sed 's@^origin/@@'); DEFAULT=${DEFAULT:-main}
git branch --merged "$DEFAULT" | grep -vE "^\*| $DEFAULT$"
```

If `git worktree list` shows anything that isn't actively in use, remove it.

## Rules

- One canonical root: `.worktrees/`. No second location.
- One worktree per task, named after the task slug.
- A worktree exists only while its task is in `active/`. Task closed → worktree removed,
  branch merged or deleted.
- Sub-agents in worktrees coordinate only through `.dev/` task files — never agent-to-agent.
