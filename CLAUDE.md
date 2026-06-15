# CLAUDE.md

Operating contract for any agent — or human — working in this repo. Read it at the
start of every session. It says *how* to work; `PROJECT.md` says *what* the project is.

**The method in one line:** plan on the board, build test-first in small pure units,
isolate parallel work in worktrees, and prove it works before claiming done.

## Working principles

- **Honesty over impressiveness.** Never claim something works without evidence. Label
  untested or aspirational work plainly (`⚠️ NOT VERIFIED`). A smaller honest result
  beats a larger unproven one.
- **Lean by default.** Build the minimal slice that satisfies the task. No speculative
  abstractions, no unrequested features. Ask before adding surface.
- **Bias to action.** When the path is clear, take it — don't stall for permission on
  reversible decisions. Surface only what genuinely needs the human.
- **Enhance in place.** Improve existing structure; don't restructure or rename things
  that weren't asked about.
- **Close the loop.** When you stop or get blocked, end with what you did, what's left,
  and exactly what you need from the human.

## Every session

1. **Orient.** Read `PROJECT.md`. List in-flight work: `ls .dev/tasks/active/` and
   `.dev/epics/*/tasks/active/`. Read the latest handover:
   `ls .dev/handover/*.md 2>/dev/null | sort | tail -1` (handovers are named
   `YYYY-MM-DD-<topic>.md`, so name-sort is chronological).
2. **Get a session ID** (pseudonymous, no PII) to sign your work — distinguishes parallel
   sessions on the board:
   ```bash
   # falls back to host+pid if git identity is unset, so the hash is never shared
   SEED="$(git config user.name)$(git config user.email)"; [ -n "$SEED" ] || SEED="$(hostname)$$"
   echo "$(printf '%s' "$SEED" | shasum -a 256 | head -c 8)-$(date -u +%Y%m%dT%H%M%SZ)"
   ```
3. **Find or create a task.** Match the request to a task in `active/` or `backlog/`.
   If none fits, **create one before writing code** (`.dev/templates/TASK.md` →
   `.dev/tasks/active/<slug>.md`) and confirm the goal. *No work without a task.*
4. **Work it** — see the build loop.
5. **Wrap up.** Append a session-log entry tagged with your session ID, move the task to
   `done/` only if its acceptance criteria pass, and write a handover
   (`.dev/handover/YYYY-MM-DD-<topic>.md`).

## Build loop — test-first

The core discipline. Non-trivial code follows it.

1. **Spec first.** Anything beyond a small change starts from a design spec
   (`.dev/templates/SPEC.md`). Its **Testing strategy** section — which units are pure,
   how I/O is isolated, how tests stay hermetic — is filled in *before* implementation.
2. **Freeze the contract.** For each non-trivial module, write its signature, I/O rules,
   and one worked example as a `CONTRACT` docstring on a stub *before* the logic. The
   contract is what the tests assert against.
3. **Red → Green → Refactor.** Write failing tests against the contract; implement the
   smallest thing that passes; clean up with the tests green.

What keeps it honest:

- **Pure core, thin I/O shell.** Put logic in pure functions and test them with literal
  inputs and literal expected outputs — no mocking the unit under test.
- **Hermetic by default.** Tests touch no network, no real credentials, no global
  state — enforced by a setup fixture. "No network in tests" is a guarantee.
- **One test file per module; small modules.** Hard-to-test usually means too-big.
- **Tests land with their implementation,** in the same commit.

## Definition of done (the verification gate)

A task is done only when, with output you have actually read:

- its acceptance criteria — written as tests — pass;
- the build, typecheck, and lint are green;
- and for anything not covered by an automated test, you ran a real smoke check and
  labeled whatever remains `⚠️ NOT VERIFIED`.

Evidence, not assertion. Reproduce a bug with a failing test before fixing it.

## Parallel work — git worktrees

Default to the main tree. When there is genuinely parallel-safe work — independent
streams that don't touch the same files — isolate each in its own worktree. **Full
setup, dispatch, merge, and cleanup instructions are in
[`.dev/WORKTREES.md`](.dev/WORKTREES.md). Follow them, including the mandatory cleanup —
orphaned worktrees are the #1 way this setup rots.**

The model: the main session is the **lead** — it owns the plan, the board, integration,
and merge. It dispatches one sub-agent per worktree and reviews the result. Sub-agents
coordinate only through `.dev/`, never agent-to-agent. A task in `active/` with an
`Owner` set is claimed — check the owner before touching it.

## Conventions

- **No PII** in tracked files — use the session-ID hash, never real names/emails.
- **The task files are the plan.** No separate plan doc; decompose a spec into one task
  per stream.
- **State is the folder.** A task's status is the directory it sits in
  (`backlog` / `active` / `done`) — there's no status field to drift.
- **Commits report the test result:** a conventional prefix plus a body line like
  `47 tests pass; no network in tests`; flag unit-tested-but-not-live-verified work.

## Optional tooling

Self-contained. If the `superpowers` skill set is installed, its TDD / debugging /
planning skills operationalize the same rules — use them when present.
