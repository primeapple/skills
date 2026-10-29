---
name: code-review
description: Review code changes with evidence-backed, actionable findings. Use when the user requests a code review.
---

# Code review

Review only. Implement fixes only when separately requested.

## Review

1. Identify intended behavior from the request, change description, and surrounding code. Honor explicitly requested review scope. Otherwise review staged, unstaged, and untracked changes first; if none exist, review the current branch’s open pull request; if none exists, review the HEAD commit.
2. Inspect tests for potential no-ops: tests that could still pass if the behavior they cover were broken or removed. If you find one, name the test and the uncovered behavior, then suggest a specific mutation test that could verify it. Never perform a mutation test or edit files to try one unless the user confirms first.
3. For structural findings, propose a concrete simplification. Prefer YAGNI and changes that remove concepts, branches, dependencies, exceptions, or stale paths over changes that relocate them. Preparation for a concrete follow-up PR can justify added structure; explain the actual need rather than assuming future reuse.
4. Check comment blocks: flag large or imprecise comments that restate what code does or add no context. Keep precise explanations of why only when rationale is not evident from the code. When the same constraint can be expressed either way, prefer a type over a test over a comment.
5. When dependencies change, read relevant changelog and migration notes, inspect the lockfile diff including transitive changes, and check relevant tests before and after when practical. Recommend isolating unrelated upgrades when doing so makes failures and reverts easier to attribute.
6. Check the verification evidence: tests, build, and applicable manual checks. Distinguish author-reported results from checks you ran; report failures and unverified areas explicitly.

## Findings

Favor a few high-confidence, actionable findings. Order by impact: correctness and security, then structural regressions and missed simplifications, then other improvements.

Label each finding:

- **Must**: required fix; identify the concrete failure, risk, or violated requirement.
- **Should**: recommended improvement with a concrete benefit.
- **Nice to**: optional suggestion.

For each finding, give the file and line when available, evidence, consequence, and smallest useful remedy. Judge structural changes by complexity and entropy removed, not personal taste or arbitrary line limits.

Finish with verification performed and remaining gaps. If no actionable findings remain, say so; passing checks alone do not prove correctness.
