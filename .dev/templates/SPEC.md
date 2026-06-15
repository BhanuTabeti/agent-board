# Spec: <title>

- **Created:** YYYY-MM-DD
- **Owner:** <session-id>

## Context

<the problem, and why it's worth solving now>

## Goals / Non-goals

- **Goals:**
- **Non-goals:**

## Design

<the approach: the modules, their responsibilities, how data flows between them>

## Testing strategy — fill this in BEFORE implementing

<!-- This section is what makes the work test-first. If you can't fill it in, the design isn't ready. -->

- **Pure units** (logic with no I/O — parsing, mapping, rules, calculation): <list them;
  these get tested directly with literal inputs and expected outputs>
- **I/O seam** (network, credentials, files, db, time): <how it's pushed to the edges and
  hidden behind an interface so the pure units stay pure>
- **Hermetic setup:** <the fixture/harness that severs network, real credentials, and
  global state for every test>
- **What the tests must pin down:** <key behaviors and edge cases>

## Module contracts — freeze these before coding

<!-- For each non-trivial module: signature + I/O rules + one worked example.
     Commit these as stubs first; write the tests against them; then implement. -->

### `<module>`

- **Signature:** `<fn(args) -> result>`
- **Rules:** <how inputs map to outputs; edge cases>
- **Example:** `<a concrete input>` → `<its expected output>`

## Alternatives considered

- <option> — <why not>

## Scale / cost notes

<!-- How does this hold up at 100x the data? Where's the cost? Optional, but reviewers value it. -->

-

## Decisions log

### YYYY-MM-DD — <topic>

- **Decided / Why / Impact**
