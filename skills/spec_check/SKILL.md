---
name: spec_check
description: >-
  Run the spec_check: check the current change against the project's SPEC.md and report
  a PASS / FAIL / PASS WITH GAPS verdict with evidence. Use when the user types
  /spec_check, asks "is this on spec?", or before opening a pull request.
  Arguments (optional): what to check (a branch, PR number or paths) and which feature
  spec to check against.
---

# spec_check

Launch the `spec_check` agent (`spec-and-cleanup:spec_check`) on the change and relay its report.

1. **Work out the target** from `$ARGUMENTS`. Default: the current branch against the
   default branch, plus uncommitted and untracked files. If the user named a feature
   spec, PR or paths, pass them through as given.
2. **Launch the spec_check as a subagent** with a brief that contains only: the repo
   path, the target, the feature spec to use (if named) and, if the user gave one, the
   stated goal of the change in their words. Do not pass your own summary of the work,
   your reasoning, or what you expect it to find: the review is only worth something if
   the reviewer has not seen the builder's account.
3. **Show the report in full**: verdict first, then the OUT OF SPEC grid as a table,
   every row and every column in the agent's wording. Do not soften, drop, merge or
   re-rank rows. If you believe a row is wrong, say so separately and say why; do not
   remove it.
4. **Ask the user to rule on each row** (they may answer for several rows at once):
   - **Agree** — it is out of spec; it should be fixed.
   - **Spec is wrong** — the code is right; the spec should change.
   - **Not an issue** — the finding is wrong or does not matter; say why.
   Take their feedback in their own words as well.
5. **Change nothing until they have ruled**, then act only on what they approved:
   fix "Agree" rows if they ask for the fix; for "Spec is wrong" rows, show the exact
   `SPEC.md` edit and make it only on their yes; for "Not an issue" rows, offer to
   record the ruling where the project logs decisions so the same row is not raised
   again. If the project has no `SPEC.md`, offer `/spec-init`.
