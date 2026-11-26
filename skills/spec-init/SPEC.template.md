# SPEC — <project name>

<!--
The contract the osteoblast checks every change against. Keep it short and true.
Humans own this file: agents may PROPOSE edits, never apply them unasked.
Every line should be something an agent can check by reading code or running a command.
-->

## Product
- **What it is:** <one sentence>
- **Who it's for:** <the user>
- **Type:** <saas-web | mobile-rn | mobile-native | content-site | api | cli>
- **Stack:** <languages, frameworks, database, hosting>

## Core flows (must never break)
<!-- One line each: what a real user does, and what they see when it works. -->
1. <flow> — works when: <observable result>
2. <flow> — works when: <observable result>

## Rules (non-negotiable)
<!-- Each rule: what, and the reason. A rule without a reason gets argued with. -->
- **R1** <rule> — because <what broke or would break>
- **R2** <rule> — because <...>

## Budgets
<!-- Numbers an agent can measure. Delete the ones that don't apply. -->
- Bundle / app size: <e.g. main JS chunk under 300 KB gzipped>
- Speed: <e.g. API p95 under 300 ms; cold start under 2 s>
- Cost: <e.g. no new paid service without approval>

## Checks (commands that must pass)
<!-- Deterministic gates. The osteoblast runs these before reading any code. -->
- `<type-check command>`
- `<lint command>`
- `<test command, if any>`

## Features
<!--
One section per feature, or a link to wherever feature specs live
(e.g. docs/specs/*.md). Each needs the same four parts.
-->
### <feature name>
- **Goal:** <the end state a user can see, not the activity>
- **Done when:**
  - [ ] <something an agent can run or look at>
- **Not in scope:** <what must NOT be built or touched>
- **Constraints:** <files, tables or behavior that must not change>

## Decisions log
<!-- Where deviations from this spec get recorded (file path), so drift is visible. -->
- <path to the file where deviations are recorded>
