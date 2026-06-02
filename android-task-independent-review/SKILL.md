---
name: android-task-independent-review
description: Perform a strict independent engineering review of completed Android work, including Kotlin, Jetpack, UI, threading, lifecycle, storage, permissions, performance, backward compatibility, and test quality. Use when reviewing a finished Android task as a senior or principal mobile engineer.
license: MIT
compatibility: Designed for Agent Skills compatible coding agents reviewing Android application, library, or platform-integration codebases.
metadata:
  author: devpicon
  version: "1.0"
---

# Android Task Independent Review

Use this skill when the task is to review completed Android work independently. This skill reviews. It does not implement.

## Activation Criteria

Use this skill when:
- The primary task is to review completed Android work rather than write it.
- The change affects Android-only code, Android app modules, or Android-specific behavior in a KMP project.
- The review must assess lifecycle safety, threading, permissions, background work, storage, UI behavior, or Android test coverage.
- The user expects findings, severity, blockers, and closure criteria from an Android-focused reviewer.

Do not use this skill when:
- The main task is to implement or refactor production code.
- The main task is runtime validation on a device or emulator.
- The review is primarily about shared `commonMain` logic rather than Android behavior.
- There is no concrete implementation, diff, or completed task to inspect.

## Resources

Read [references/EXAMPLES.md](references/EXAMPLES.md) when you need Android-specific verdict examples, common failure patterns, or closure wording.

## Objective

Produce a rigorous Android engineering review that checks whether the delivered work is correct, maintainable, platform-safe, and ready to merge.

Review posture:
- independent
- skeptical
- technically grounded
- precise
- non-emotional

## Non-Goals

Do not:
- rewrite production code
- implement missing features
- give vague stylistic criticism without risk
- approve platform-dangerous code because it works on one device
- ignore lifecycle or threading issues because tests happen to pass

## Review Workflow

1. Read the original task, acceptance criteria, product constraints, and any Android architecture guidance.
2. Read the changed files and diff without assuming the chosen solution is valid.
3. Identify the affected Android surfaces:
   - Activity or Fragment lifecycle
   - ViewModel and state flow
   - Compose or View UI
   - coroutines and dispatchers
   - WorkManager, services, or receivers
   - storage, permissions, intents, or background execution
4. Compare the implementation against requirements, API-level support obligations, and backward-compatibility expectations.
5. Review state ownership, lifecycle safety, threading, cancellation, configuration-change behavior, and process-death behavior.
6. Review user-visible failure modes:
   - permission denial
   - offline state
   - missing dependency
   - background restriction
   - invalid input
7. Inspect tests for Android relevance and realism.
8. Check docs, task notes, and comments for stale or misleading platform claims.
9. Produce a verdict with blockers, required fixes, and non-blocking recommendations.

## Severity Model

- `S0`: release-blocking crash, data loss, security, or severe user-safety issue
- `S1`: major functional, lifecycle, concurrency, performance, or compatibility issue
- `S2`: meaningful quality gap, missing case, weak test coverage, or maintainability risk
- `S3`: minor improvement or polish recommendation

## Required Review Dimensions

### 1. Scope Compliance

Check:
- whether the requested Android task was fully implemented
- whether unrelated refactors or behavioral changes were folded in
- whether platform-specific shortcuts exceeded the approved scope

### 2. Correctness Review

Check:
- functional correctness on the Android path
- lifecycle-aware state and effect handling
- nullability and failure-state handling
- intent, navigation, or activity-result correctness
- storage and file-path correctness

### 3. Concurrency and Lifecycle Safety

Check:
- correct dispatcher use
- main-thread vs background-thread boundaries
- cancellation and structured concurrency
- configuration-change resilience
- process-death recovery where relevant
- leak risk from retained context, views, or callbacks

### 4. UI and UX Contract

Check:
- Compose or View state consistency
- duplicate events or repeated navigation
- loading, empty, and error state behavior
- accessibility regressions that are obvious from the implementation
- recomposition or observer misuse when using Compose

### 5. Platform Compatibility

Check:
- API-level guards
- permission model correctness
- behavior under background execution restrictions
- manifest or Gradle configuration consistency
- SDK integration assumptions

### 6. Test Quality

Check:
- whether changed logic is actually covered
- whether coroutine and lifecycle behavior is tested credibly
- whether UI or ViewModel behavior is verified at the right layer
- whether Android-specific code is untested while only shared logic has coverage

### 7. Documentation Consistency

Check:
- whether architecture or feature docs still match Android behavior
- whether rollout limitations, device caveats, or permission assumptions are documented
- whether task tracking reflects real completion

### 8. Architectural Risks

Check:
- Android framework reach-through into layers that should stay decoupled
- tight coupling between UI, platform services, and business logic
- accidental single-activity or single-device assumptions
- shortcuts that will harden into technical debt

### 9. Future Readiness

Check:
- whether the design can handle the next obvious Android extension without rewrite
- whether contracts remain stable across UI evolution
- whether test seams exist for future behavior changes

## Android Review Heuristics

Escalate findings when you see:
- `Context` stored beyond safe lifecycle scope
- blocking I/O or model load on the main thread
- `GlobalScope`, unmanaged jobs, or ad hoc coroutine launching
- permission success path implemented without denial-path UX
- navigation triggered from unstable or repeating UI effects
- one-shot event handling that can replay after rotation or process restoration
- SDK calls without API guards or capability checks

## Do / Do Not

Do:
- cite concrete classes, files, functions, or runtime behaviors
- separate blockers from recommendations
- explain why the Android platform makes the issue risky
- distinguish emulator-only confidence from real-device certainty

Do not:
- treat “works on my phone” assumptions as proof
- ignore lifecycle leaks because the code is small
- accept missing permission or background-restriction handling silently
- reduce Compose, coroutine, or ViewModel issues to style preferences

## PASS / PARTIAL / FAIL Rules

- `PASS`: requirements are met, no blockers remain, and no significant Android platform risks were found
- `PARTIAL`: substantial work is correct, but one or more Android-specific fixes are required before closure
- `FAIL`: core requirements are unmet, serious Android regressions exist, or the implementation is unsafe to accept

## Expected Output

Always use this structure:

```text
1. Scope Compliance
- PASS/PARTIAL/FAIL: <assessment>

2. Correctness Review
- [S1] <finding>

3. Concurrency and Lifecycle Safety
- [S1] <finding or "No material gaps found">

4. UI and UX Contract
- [S2] <finding or "No material gaps found">

5. Platform Compatibility
- [S1] <finding or "No material gaps found">

6. Test Quality
- [S2] <finding>

7. Documentation Consistency
- [S2] <finding>

8. Architectural Risks
- [S1] <finding>

9. Final Verdict
- PASS/PARTIAL/FAIL: <one-sentence decision>

10. Required Fixes
- <must-fix item with closure criterion>

11. Non-blocking Recommendations
- <optional improvement>
```

## Closure Criteria

A review is complete only when:
- findings are actionable
- severity is assigned
- Android-specific blockers are explicit
- closure criteria are concrete
- the verdict matches the evidence
