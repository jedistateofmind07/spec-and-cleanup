---
name: spec-init
description: >-
  Draft a SPEC.md for the current project: the contract (product, core flows, rules,
  budgets, checks, features) that the osteoblast agent checks changes against. Use when
  the user types /spec-init, asks to "create the spec", or the osteoblast reports that a
  project has no SPEC.md.
---

# spec-init

Write `SPEC.md` at the repository root (the only file this skill creates or changes) from `SPEC.template.md` (next to this file).
If `SPEC.md` already exists, do not overwrite it: report what is missing or stale
against the template and propose edits for the user to approve.

## Steps

1. **Read what the project already says about itself**, in this order, and stop reading a
   source once it stops adding facts: `CLAUDE.md` / `AGENTS.md`, `README`, the package
   manifest (`package.json`, `pyproject.toml`, `Podfile`, `build.gradle`, …), any
   existing spec, decision or notes files the project keeps (search for them; projects
   name these differently), CI workflows, and the route / screen list.
2. **Fill the template from evidence only.**
   - *Rules*: copy the project's existing hard rules and keep each one's reason. Do not
     invent rules. A rule you inferred from code and nobody wrote down goes under an
     "Unconfirmed" list at the bottom for the user to accept or delete.
   - *Core flows*: derive from routes or screens; three to seven flows, each with an
     observable result.
   - *Checks*: only commands that exist in the project (scripts in the manifest, CI
     steps). Run each once and note any that fail today; do not run a command that
     writes files (a build that regenerates output, a formatter in fix mode) — list it
     without running it.
   - *Budgets*: fill only numbers the project states or that you measured just now;
     otherwise leave the hint in place and list it as an open question.
   - *Features*: link to existing feature specs instead of copying them.
3. **Ask the user only what the code cannot answer** — who it is for, which flows matter
   most, budgets — in one batch, after the draft exists.
4. **Report**: the path written, every "Unconfirmed" item and open question, and any
   check that fails today.

Keep the file under about 150 lines. A spec nobody rereads is worse than no spec.
