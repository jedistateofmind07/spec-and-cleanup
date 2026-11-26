# remodel

Two helpers for Claude Code that keep an AI-built project honest and tidy.

When you build with AI, two things go wrong over time. The project drifts away from
what you asked for, and it fills up with leftovers: unused code, the same thing written
twice, slow pages, notes that are no longer true. remodel gives you one helper for each
problem.

The name comes from bone. Your body rebuilds bone all the time: one kind of cell builds,
another clears away what is no longer needed. These two helpers do the same for a
codebase.

## What you get

| Helper | What it does for you | How you run it |
|---|---|---|
| **Spec check** (the "osteoblast") | Reads your written plan and tells you whether the latest change matches it. Gives a clear verdict: pass, fail, or pass with gaps. | `/spec_check` |
| **Cleanup crew** (the "osteoclast") | Looks through the project for things you no longer need or that cost too much, measures how fast it loads, and gives you a list of what to remove or fix. | `/cleanup_crew` |
| **Spec starter** | Writes the first draft of your plan (a file called `SPEC.md`) so the other two have something to check against. | `/spec-init` |

## What makes it safe

- **Neither helper changes anything on its own.** They only read and report.
- **Nothing is removed until you say yes.** The cleanup report is a list; you approve it
  row by row.
- **Reports are in plain English.** Every row says what the thing is, why it should go,
  and what you will notice if it does. You do not need to read code to make the call.
- **Bugs are listed apart** from cleanup, so they do not get lost.
- **Anything the helper could not check is listed**, so silence never looks like a pass.

## What the cleanup crew looks for

- Code, files and tools that nothing uses any more
- The same logic written in more than one place
- Features and safety nets built for cases that never happen
- Slow or wasteful spots: how big the download is, how long each main page (or each
  app screen) takes to show its data, and slow database requests
- Docs, notes and comments that are no longer true

## Install

You need [Claude Code](https://claude.com/claude-code).

```bash
claude plugin marketplace add jedistateofmind07/remodel
```

```bash
claude plugin install remodel@remodel
```

## How to use it

1. Open your project in Claude Code and run `/spec-init` once. Read the `SPEC.md` it
   drafts and correct anything that is wrong. This is your plan.
2. Before you ship a change, run `/spec_check`. Fix what it flags, or update the plan if
   the plan is what changed.
3. Every so often, run `/cleanup_crew`. Read the report and approve the rows you agree
   with. Approved cuts are made, then spec-checked.

You can point either helper at one part of the project, for example
`/cleanup_crew the api folder` or `/spec_check PR 42`.

## Limits to know about

- The helpers are only as good as your `SPEC.md`. A vague plan gives vague checks.
- Out of the box the cleanup crew can read files and run read-only commands. It cannot
  open a browser or query your database by itself, so some speed checks end up under
  "Not checked" unless your project gives it a command-line way to take them.
- This is an early version (0.1). Expect rough edges.

---

## Technical details

**Layout**

```
.claude-plugin/plugin.json        plugin manifest
.claude-plugin/marketplace.json   lets this repo act as its own marketplace
agents/osteoblast.md              spec conformance reviewer (subagent)
agents/osteoclast.md              cleanup scout (subagent)
skills/spec_check/SKILL.md        /spec_check — runs the osteoblast
skills/cleanup_crew/SKILL.md      /cleanup_crew — runs the osteoclast, applies approved rows
skills/spec-init/                 /spec-init and the SPEC.md template
SPEC.md                           this plugin's own spec
```

**Agents.** Both are subagents with the tools `Read, Grep, Glob, Bash` and a hard
read-only rule in their prompts: no edits, commits, pushes, deploys or database writes.
A measuring command may write only to a temporary folder outside the repository.

- `osteoblast` starts with no knowledge of how the change was made and judges the diff
  against `SPEC.md`: each "done when", out-of-scope work, project rules, and spec drift.
  Every finding cites a file and line or a command and its output. Verdict: `PASS`,
  `FAIL` or `PASS WITH GAPS`.
- `osteoclast` sweeps for dead, duplicated, over-built, slow and stale items. "Unused"
  must be proven by searching imports, string and dynamic references, config, routes,
  scheduled jobs, scripts, CI and database functions. Speed is measured, not guessed:
  bundle sizes from a build into a temp folder, load time per core flow (cold and warm),
  requests per page, and database statistics and query plans where available. Each row
  carries a confidence level; technical proof goes in a separate Evidence section.

**Skills.** The skills are thin wrappers that launch the agent, show its report
unchanged, and (for `/cleanup_crew`) apply only the rows the user approves, then run a
spec check on the result.

**Local development**

```bash
claude plugin marketplace add /path/to/remodel
```

```bash
claude plugin validate /path/to/remodel
```

## License

MIT
