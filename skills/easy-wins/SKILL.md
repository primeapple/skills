---
name: easy-wins
description: Find small, evidence-backed codebase changes with outsized user or maintainer benefit, rank them by impact, risk, recency, cost, and entropy reduction, then implement the chosen fix and open a PR.
disable-model-invocation: true
---

# Easy wins

Find one bounded change that makes the whole system simpler, safer, faster, or harder to regress. Prefer finishing work over starting another cleanup thread.

## Operating rule

A proposal is an **easy win** only when all are true:

- scope is concrete: named files, symbols, dependency, asset, test, or migration step;
- the benefit reaches users or many maintainers, not only the author;
- evidence supports the problem and the proposed fix;
- change can be reviewed and tested in one small PR;
- risk is understood and has a verification plan;
- the result reduces entropy: fewer concepts, dependencies, paths, exceptions, stale facts, or ways for the old state to return.

Do not turn this into a general architecture review. Produce fixable candidates, not themes such as “improve modularity” or “add more tests.”

## Run

### 1. Establish the search area

Inspect before proposing:

1. Working tree, current branch, project instructions, package manifests, build/test/lint commands, and existing contribution rules.
2. Recent history (`git log`, roughly the last 30–90 days). Give extra attention to files and subsystems changed repeatedly or most recently.
3. Open PRs/issues when repository tooling makes that cheap. Inspect open items and recently closed items; avoid proposing work already in flight or just completed.
4. Code and configuration around the hottest areas. Use repository-native search and analysis tools; do not guess from filenames alone.

Use the smallest useful scan. Widen it only when the first pass produces no candidate with evidence.

### 2. Hunt for high-leverage finish lines

Look for these signals, in roughly this order:

- dead dependencies, scripts, configuration, code, exports, feature flags, assets, and generated artifacts;
- an old migration with an obvious remaining step, compatibility shim, dual path, or fallback;
- a dependency that is substantially behind, has a security/maintenance cost, or can be removed with native/platform code;
- duplicated or unreachable behavior in recently changed hot paths;
- missing tests at a high-value boundary, especially a regression test for a real bug or a frequently modified path;
- impure seams whose small pure extraction makes behavior easier to verify without adding abstraction;
- slow or flaky tooling with a local, measurable fix;
- stale comments, documents, examples, or tests that encode implementation details instead of user-visible behavior.

For removal claims, prove absence across imports/references, scripts, configuration, dynamic loading, generated files, and documented commands. For migration claims, identify both the old and new paths and the exact final deletion or switch. For dependency upgrades, check changelog/release notes and compatibility rather than treating version age as proof of value.

### 3. Score candidates

Create 3–5 candidates, then rank them. Prefer a mix of categories (dependency, deletion, migration, tests, tooling, purity, docs) when each candidate clears the evidence bar. Do not add a weak candidate just to fill a category. Score each dimension 1–5:

- **Impact**: user benefit, reliability, security, performance, or maintainer reach.
- **Reach**: how many users, runs, packages, or contributors benefit.
- **Confidence**: strength of direct evidence that the problem and fix are real.
- **Recency**: relevance to code changed recently or an active workstream.
- **Finishability**: bounded scope and likelihood of one small PR completing it.
- **Entropy reduction**: concepts, dependencies, paths, exceptions, or stale facts removed; extra credit when the old state cannot grow back.
- **Cost**: expected implementation and review effort; higher is worse.
- **Risk**: regression, compatibility, migration, or rollout risk; higher is worse.

Show every estimate and also calculate a transparent ranking score:

`leverage = impact × reach × confidence × finishability × entropy reduction`

Divide leverage by `cost × risk`. Apply recency as a moderate multiplier, not a gate. Use judgment when the numbers mislead, and explain the override. Tie-break in this order: confidence, entropy reduction, reach, finishability, then recency. A recent change is a relevance signal, not permission to make a risky change. Do not recommend a high-risk item merely because its impact is large.

### 4. Present proposals

Show no more than five. Lead with the top recommendation. Each proposal must contain:

- **Title**: action + concrete target.
- **Evidence**: commands, history, references, or measurements and what they establish.
- **Change**: exact files/symbols/dependency and the smallest viable diff.
- **Benefit**: who gains and how this lowers system entropy.
- **Scores**: impact, reach, confidence, recency, finishability, entropy reduction, cost, risk.
- **Verification**: tests, static checks, benchmark, build, or runtime check that would catch failure.
- **Residual risk**: what remains uncertain and how to contain it.
- **PR shape**: expected title, files touched, and whether a migration note or release note is needed.

State clearly when evidence is incomplete. “Possibly unused” is an investigation lead, not a proposal to delete.

Ask the user to choose one candidate. Do not edit code or create a branch during scanning. If the working tree is dirty, scan and report candidates but refuse implementation until a clean baseline exists.

### 5. Execute the chosen candidate

After the user chooses:

1. Restate scope, success checks, and risk in one short plan. Proceed without another approval unless the chosen change differs materially from the proposal or has irreversible production/data impact.
2. Fetch the latest default branch, create a focused branch from it, and inspect the exact code. Preserve public behavior unless the proposal explicitly changes it.
3. Write the smallest change. Add or adjust tests at the observable behavior boundary. Remove implementation-detail tests only when surviving tests still cover the contract.
4. Run focused checks first, then the repository’s required lint, type, build, and test commands. Report failures as failures; do not weaken checks to get green.
5. Re-scan for reintroduction: old imports, compatibility paths, references, docs, lockfile entries, generated output, and dead tests. For removals, confirm the removed thing is absent and the build still works.
6. Commit one coherent change, push the branch, and open a PR using repository conventions. PR title and body must contain a small **What** and **Why**, plus evidence, verification, risk, and entropy reduction. Mark the description as **AI generated**. Never bundle a second cleanup into the PR.

If no candidate clears the evidence and risk bar, report “no easy win found” and show the strongest near-misses with the evidence they lack. Do not force a daily proposal.

Stop after the PR opens. Human handles review and follow-up.

Done means: chosen scope is implemented, evidence-backed checks pass, the old path cannot silently remain or regrow where removal was the goal, and the PR is open or the exact remaining blocker is reported.

## Quality bar

Prefer a boring deletion, completed migration, targeted regression test, or measurable tooling fix over a broad refactor. Prefer native code only when it is smaller, clearer, supported by the project’s runtime matrix, and behaviorally equivalent. Treat comments and documentation as code: delete stale facts, but keep rationale that the code cannot reveal.

Never propose security-sensitive, data-destructive, public API, or major dependency changes as an “easy win” without explicit risk, compatibility evidence, and a rollback or rollout plan. If no candidate clears the bar, say so plainly rather than manufacturing momentum.
