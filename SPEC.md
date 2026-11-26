# SPEC — spec-and-cleanup

<!-- DRAFT written by Claude on 2026-11-24. Robbie owns this file: edit or strike anything. -->

## Product
- **What it is:** a Claude Code plugin that keeps an AI-built codebase on spec (spec_check) and lean (cleanup_crew).
- **Who it's for:** someone building a SaaS or consumer app mostly through coding agents, across several projects.
- **Type:** cli (Claude Code plugin: markdown agents and skills, no runtime code)
- **Stack:** Markdown + JSON manifests; distributed as a git repo that is its own plugin marketplace.

## Core flows (must never break)
1. Install — works when: `claude plugin marketplace add <repo>` then `claude plugin install spec-and-cleanup@spec-and-cleanup` succeed and `/spec_check` is available in a new session.
2. `/spec-init` in a project with no spec — works when: a `SPEC.md` is written at the repo root from the template, with unconfirmed items listed, and nothing else is changed.
3. `/cleanup_crew` — works when: the user sees a report grid (what, where, why, impact, how sure) and nothing in the repo has changed until they approve rows.
3b. `/spec_check` on a branch — works when: the spec_check runs in a fresh context and returns a report in its documented format with a verdict line.
4. `/spec_check` in a project with no `SPEC.md` — works when: the review still completes from fallback sources, says it is weaker, and recommends `/spec-init`.

## Rules (non-negotiable)
- **R1** The spec_check never edits, creates or deletes files, and never commits, pushes, deploys or writes to a database — because a reviewer that changes things cannot be trusted as a gate.
- **R2** No agent or skill in this plugin edits a project's `SPEC.md` without the user asking — because a spec the agents can rewrite is as unreliable as the code.
- **R3** `/spec-init` never overwrites an existing `SPEC.md` — because the spec holds human decisions.
- **R4** `/spec_check` passes the spec_check the target, the spec and the user's own stated goal only, never the builder's reasoning or expected result — because the review must be independent.
- **R5** Every spec_check finding carries evidence (file:line or command output) and a plain-words impact — because unverifiable findings get ignored, then all findings do.
- **R6** Nothing project-specific in the plugin: no project names, paths, stacks or rules from any one app — because it must work on any repo.
- **R8** Nothing is removed or fixed by the cleanup_crew flow until the user has seen the plain-English report grid and approved the rows — because the owner must understand each cut and its impact first.
- **R9** The cleanup_crew never proposes cutting anything the project's spec lists as a rule, core flow or constraint — because lean must not cost correctness.
- **R10** `/spec_check` shows every out-of-spec item as a plain-English grid row with its impact, and the user rules on each row (agree / spec is wrong / not an issue) before anything is fixed or the spec is edited — because the owner decides what "on spec" means.
- **R7** `/spec_check` relays findings without dropping, softening or re-ranking them — because the caller is usually the builder.

## Budgets
- spec_check report: about 60 lines, longer only for spec-drift edits.
- `SPEC.md` produced by `/spec-init`: under about 150 lines.
- Plugin size: each agent or skill file under 120 lines.

## Checks (commands that must pass)
- `claude plugin validate .`

## Features
### spec_check agent (v0.1)
- **Goal:** a change can be checked against the project's spec and the user gets a verdict they can act on.
- **Done when:**
  - [ ] `agents/spec_check.md` exists with tools limited to Read, Grep, Glob, Bash
  - [ ] its report format includes: verdict, an OUT OF SPEC grid (what, where, what the spec says, what was built, impact, blocks, how sure), done-when, checks, spec may be wrong, rules checked, not checked, evidence
  - [ ] it defines FAIL and PASS WITH GAPS
  - [ ] it has a fallback for projects with no `SPEC.md`
  - [ ] on a seeded test repo it catches: an unmet done-when, an out-of-scope change, a broken rule; and passes a clean change (NOT YET BUILT — see evals)
- **Not in scope:** fixing what it finds; style or leanness review; security review.
- **Constraints:** must stay read-only (R1).

### /spec-init skill (v0.1)
- **Goal:** any project can get a first `SPEC.md` drafted from its own code and docs.
- **Done when:**
  - [ ] `skills/spec-init/SKILL.md` and `SPEC.template.md` exist
  - [ ] the template has: Product, Core flows, Rules, Budgets, Checks, Features, Decisions log
  - [ ] the skill says to fill from evidence only and list inferred rules as "Unconfirmed"
- **Not in scope:** writing feature specs; keeping the spec in sync afterwards.

### /spec_check skill (v0.1)
- **Goal:** one command runs the spec_check on the current change.
- **Done when:**
  - [ ] `skills/spec_check/SKILL.md` exists and launches the spec_check as a subagent
  - [ ] it follows R4 and R7 and changes nothing
- **Not in scope:** blocking `gh pr create` automatically; hooks.

### cleanup_crew agent + /cleanup_crew skill (v0.1)
- **Goal:** the user can see everything worth cutting, in plain English, and decide row by row before anything is removed.
- **Done when:**
  - [ ] `agents/cleanup_crew.md` exists with tools limited to Read, Grep, Glob, Bash
  - [ ] its report is a grid with columns: what, where, remove or fix, why, impact, how sure
  - [ ] it sets no cap on the number of findings
  - [ ] `skills/cleanup_crew/SKILL.md` shows the full report and waits for the user's approval before any change
  - [ ] approved cuts are applied one commit per row, or per group of rows that depend on each other, and checked with `/spec_check`
- **Not in scope:** running on a schedule (planned); cutting anything the spec protects.

### Planned, not built
- scheduled cleanup_crew runs; project-type checklists; eval suite under `evals/`; Jev router.

## Decisions log
- `README.md` status table, until a notes file is needed.
