# agent-board

A small, opinionated baseline for building software with AI coding agents —
**disciplined, test-first, and isolated**. It's the setup I drop into a new repo so an
agent (and I) work the same careful way every time, and so the work stays honest and
reviewable.

Plain Markdown and git. No daemon, no database, no required plugin — read it, grep it,
review it in a PR.

## Why it exists

Good output from AI-assisted development comes from **discipline, not tooling**. This
baseline encodes the few practices that reliably produce clean work and deliberately
leaves everything else out:

- **A shared board** (`.dev/`) — work, decisions, and handovers live as files; a task's
  state is the folder it sits in. Every session reads it on start and writes on finish,
  so context survives across sessions and agents.
- **Test-first by contract** — non-trivial modules freeze their interface as a contract,
  get tests written against it, then get implemented. Tests are hermetic: no network, no
  real credentials.
- **Worktree isolation for parallel work** — independent streams each run in their own
  git worktree under a lead session that owns the plan and the merge, with a mandatory
  cleanup so nothing rots. See [`.dev/WORKTREES.md`](.dev/WORKTREES.md).
- **Honesty over impressiveness** — nothing is "done" without evidence; unproven work is
  labeled as such.

The full operating contract for the agent is [`CLAUDE.md`](CLAUDE.md); the board is
documented in [`.dev/README.md`](.dev/README.md).

## Layout

```
CLAUDE.md          how the agent works — read every session
PROJECT.md         this project's stack, commands, and gotchas
.dev/
  README.md        the board
  WORKTREES.md     parallel-work setup + lifecycle
  templates/       TASK · EPIC · SPEC · HANDOVER
  tasks/ epics/ specs/ handover/
```

## Use it on a new project

**Click "Use this template"** on GitHub to spin up a fresh repo from this — or copy
`CLAUDE.md`, `PROJECT.md`, `.gitignore`, and `.dev/` into an existing repo. Then open
Claude in that repo and send a kickoff message like:

> Read CLAUDE.md and PROJECT.md. Fill in PROJECT.md for this project: \<one line on what
> it is + the stack>. Then create the first task for \<goal> and start the build loop.

Because the files are already present, `CLAUDE.md` governs the session automatically — the
board is set up; you're just starting the first task. When work splits into independent
parallel streams, the agent follows `.dev/WORKTREES.md`.

## Credits

The board-as-protocol idea comes from
[dev-blackboard](https://github.com/Aithu-Snehith/dev-blackboard) — a friend's own take on
this kind of system. The test-first-by-contract discipline, the worktree lifecycle, and
the working principles are distilled from the projects I've built since.

## License

MIT — see [`LICENSE`](LICENSE).
