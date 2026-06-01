---
name: kmp-task-independent-review
description: Perform a strict independent engineering review of completed Kotlin Multiplatform work, checking scope compliance, architectural correctness, requirement compliance, regressions, test quality, documentation consistency, and future readiness. Use when reviewing a finished task as a senior or principal engineer.
license: MIT
compatibility: Designed for Agent Skills compatible coding agents reviewing completed Kotlin Multiplatform, Android, and iOS code changes.
metadata:
  author: devpicon
  version: "1.0"
---

# KMP Task Independent Review

Use this skill when the task is to review completed work independently. This skill reviews. It does not implement.

## Activation Criteria

Use this skill when:
- The primary task is to review completed Kotlin Multiplatform work rather than implement it.
- The user expects an independent engineering verdict with findings, severity, and closure criteria.
- The request involves checking scope compliance, regressions, architecture, tests, or documentation consistency.
- The change spans `commonMain`, `androidMain`, `iosMain`, or their contracts and needs a skeptical senior review.

Do not use this skill when:
- The main task is to write or modify production code.
- The main task is runtime validation on devices.
- The request is broad brainstorming or product ideation.
- There is no concrete implementation, diff, or completed work to review.

## Resources

Read [references/EXAMPLES.md](references/EXAMPLES.md) when you need verdict examples, severity patterns, or closure criteria wording.

## Objective

Produce a rigorous engineering review that challenges assumptions and determines whether the delivered task is acceptable as-is.

Review posture:
- independent
- skeptical
- technically grounded
- precise
- non-emotional

## Non-Goals

Do not:
- rewrite code
- make feature changes
- soften findings with vague language
- speculate without technical basis
- approve work because it is “good enough” when blockers remain

## Review Workflow

1. Read the original task request, acceptance criteria, and any architecture or tracking documents.
2. Read the implementation diff or changed files without assuming the chosen approach is correct.
3. Identify declared scope, implicit constraints, and backward-compatibility obligations.
4. Compare implementation against requirements, not against intent claimed in comments or summaries.
5. Evaluate shared-vs-platform placement, dependency boundaries, state handling, and failure behavior.
6. Inspect tests for coverage quality, not just presence.
7. Check whether docs, task notes, and architecture records match the delivered behavior.
8. Call out hidden debt, shortcuts, and risks introduced by the change.
9. Produce a verdict with blockers, required fixes, and non-blocking recommendations.

## Severity Model

- `S0`: release-blocking correctness or safety failure
- `S1`: major functional, architectural, or compatibility issue
- `S2`: meaningful quality gap, missing case, or weak test coverage
- `S3`: minor improvement or polish recommendation

## Required Review Dimensions

### 1. Scope Compliance

Check:
- was the requested task fully implemented
- was any unauthorized scope added
- were promised follow-ups hidden inside this change

### 2. Correctness Review

Check:
- functional correctness
- state transitions
- concurrency assumptions
- nullability and error handling
- platform parity where required

### 3. Edge Cases

Check:
- missing dependency behavior
- repeated invocation
- stale state reuse
- offline mode
- partial initialization
- platform-specific failure paths

### 4. Test Quality

Check:
- whether tests cover the changed logic directly
- whether success and failure paths are both covered
- whether tests assert behavior instead of implementation trivia
- whether platform-specific code lacks any platform-specific verification

### 5. Documentation Consistency

Check:
- architecture docs match the new behavior
- task tracking reflects actual completion state
- limitations and trade-offs are documented

### 6. Architectural Risks

Check:
- inappropriate platform leakage into shared code
- missing seams for likely replacement points
- shortcuts that will harden into long-term debt
- regression risk from altered contracts or defaults

### 7. Future Readiness

Check:
- whether the design can support the next obvious iteration without rewrite
- whether contracts are stable enough for Android and iOS evolution
- whether deterministic MVP behavior remains understandable

## Do / Do Not

Do:
- cite concrete files, classes, functions, or behaviors
- separate blockers from recommendations
- explain the technical reason each finding matters
- distinguish missing evidence from confirmed defects

Do not:
- say “looks good” without testing the claim against requirements
- accept stale docs when behavior changed
- treat test existence as proof of correctness
- confuse architectural taste with actual risk

## PASS / PARTIAL / FAIL Rules

- `PASS`: requirements are met, no blockers remain, residual concerns are minor
- `PARTIAL`: substantial work is correct, but one or more required fixes remain before closure
- `FAIL`: core requirements are unmet, regressions exist, or the implementation is not safe to accept

## Expected Output

Always use this structure:

```text
1. Scope Compliance
- PASS/PARTIAL/FAIL: <assessment>

2. Correctness Review
- [S1] <finding>

3. Edge Cases
- [S2] <finding or "No material gaps found">

4. Test Quality
- [S2] <finding>

5. Documentation Consistency
- [S2] <finding>

6. Architectural Risks
- [S1] <finding>

7. Final Verdict
- PASS/PARTIAL/FAIL: <one-sentence decision>

8. Required Fixes
- <must-fix item with closure criterion>

9. Non-blocking Recommendations
- <optional improvement>
```

## Closure Criteria

A review is complete only when:
- findings are actionable
- severity is assigned
- blockers are separated from recommendations
- closure criteria are explicit
- the verdict matches the evidence
