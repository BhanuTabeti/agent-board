# .dev/ — the board

The shared workspace for everyone in the repo, human or agent. Plain Markdown:
greppable, diffable, reviewable in a PR. Read it on start; write to it on finish.

## State is the folder

A task's status is the directory it lives in — there's no separate status field to drift
out of sync:

```
.dev/
├── tasks/
│   ├── backlog/   not started
│   ├── active/    in progress (a task here with an Owner is claimed)
│   └── done/      complete — acceptance criteria pass
├── epics/         a workstream of many tasks: <slug>/epic.md + its own tasks/
├── specs/         design docs — the "why & what", including the testing strategy
├── handover/      dated session summaries — how context survives across sessions
└── templates/     copy these to create new entries
```

Change a task's state with `git mv`. To claim a task, move it to `active/` and put your
session-ID in its `Owner`. Check the owner before touching someone else's active task.

## The units

- **Task** (`templates/TASK.md`) — one deliverable; acceptance criteria written as tests.
- **Epic** (`templates/EPIC.md`) — a handful of related tasks, with a decisions log. It
  lists its task files; their state is read from the folders, not duplicated here.
- **Spec** (`templates/SPEC.md`) — for anything non-trivial; its testing strategy and
  module contracts are filled in before code.
- **Handover** (`templates/HANDOVER.md`) — written at the end of a working session.

## The rhythm

- **Start:** read `active/` and the latest handover; open or create a task.
- **During:** append to the task's session log after each meaningful chunk; record
  non-obvious choices in the decisions log.
- **End:** move the task if its tests pass; write a handover (`YYYY-MM-DD-<topic>.md`).

Nothing here is deleted — `done/` and `handover/` are the project's memory.
