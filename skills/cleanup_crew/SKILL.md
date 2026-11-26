---
name: cleanup_crew
description: >-
  Run the cleanup_crew: sweep the project for dead code, duplication, over-building,
  costly spots and stale docs, and show a plain-English report grid of every proposed
  removal or fix with its reason and impact. Nothing is removed until the user approves
  rows from the report. Use when the user types /cleanup_crew or asks "what can we cut?" or
  "clean this up". Arguments (optional): an area, branch or "since <ref>" to limit the
  sweep.
---

# cleanup_crew

1. **Launch the `cleanup_crew` agent** (`remodel:cleanup_crew`) as a subagent with the repo
   path and the scope from `$ARGUMENTS` (default: the whole repository).
2. **Show its report to the user in full**: the grid as a table, every row, every
   column, in the agent's wording. Do not shorten the list, merge rows or drop the
   Impact column. Then stop and ask which rows to apply: all, none, or a list of row
   numbers.
3. **Remove or fix nothing before the user answers.** Approval is per row and comes
   only from the user in this conversation. A row marked "Unsure" needs the user to
   name it; "all" does not include Unsure rows unless the user says so.
4. **Apply only the approved rows**, on a branch, one commit per row (or per linked
   group of rows) so any single cut can be undone on its own. Use the report's Evidence
   section; re-check each item is still unused before deleting it.
5. **Prove nothing broke**: run the project's checks, then run the spec_check
   (`/spec_check`) on the branch. A FAIL means the cut that caused it is reverted and
   reported, not patched around.
6. **Report back** in the same plain English: what was removed, what was skipped and
   why, the measured result (lines, size, speed), and the spec check verdict.
