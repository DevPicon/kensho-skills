---
name: ios-task-independent-review
description: Perform a strict independent engineering review of completed iOS work, including Swift or SwiftUI code, UIKit integration, concurrency, lifecycle, memory behavior, framework linking, bridge safety, performance, backward compatibility, and test quality. Use when reviewing a finished iOS task as a senior or principal mobile engineer.
license: MIT
compatibility: Designed for Agent Skills compatible coding agents reviewing iOS application, framework, SDK-integration, or Apple-platform codebases.
metadata:
  author: devpicon
  version: "1.0"
---

# iOS Task Independent Review

Use this skill when the task is to review completed iOS work independently. This skill reviews. It does not implement.

## Activation Criteria

Use this skill when:
- The primary task is to review completed iOS work rather than write it.
- The change affects iOS-only code, Apple-platform modules, or iOS-specific behavior in a KMP project.
- The review must assess Swift concurrency, SwiftUI or UIKit state handling, framework or bridge behavior, memory pressure, main-thread safety, or iOS test coverage.
- The user expects findings, severity, blockers, and closure criteria from an iOS-focused reviewer.

Do not use this skill when:
- The main task is to implement or refactor production code.
- The main task is runtime validation on a device or simulator.
- The review is primarily about shared `commonMain` logic rather than iOS behavior.
- There is no concrete implementation, diff, or completed task to inspect.

## Resources

Read [references/EXAMPLES.md](references/EXAMPLES.md) when you need iOS-specific verdict examples, bridge or framework failure patterns, or closure wording.

## Objective

Produce a rigorous iOS engineering review that checks whether the delivered work is correct, maintainable, platform-safe, and ready to merge.

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
- approve fragile bridge code because it compiles
- hand-wave memory or main-thread issues as future optimization
- give style-only criticism without product or platform risk

## Review Workflow

1. Read the original task, acceptance criteria, product constraints, and any iOS architecture guidance.
2. Read the changed files and diff without assuming the selected approach is valid.
3. Identify the affected iOS surfaces:
   - SwiftUI or UIKit state flow
   - Swift concurrency or callback bridging
   - framework linking and packaging
   - Objective-C, C++, or KMP bridge boundary
   - storage, permissions, background tasks, or app lifecycle
4. Compare implementation against requirements, supported iOS versions, and backward-compatibility expectations.
5. Review state ownership, actor or thread boundaries, lifecycle events, cancellation behavior, and app-resume behavior.
6. Review user-visible failure modes:
   - permission denial
   - offline state
   - unavailable framework or bridge
   - memory pressure
   - invalid input
7. Inspect tests for iOS relevance and credibility.
8. Check docs, task notes, and comments for stale platform claims or hidden assumptions.
9. Produce a verdict with blockers, required fixes, and non-blocking recommendations.

## Severity Model

- `S0`: release-blocking crash, data loss, security, or severe user-safety issue
- `S1`: major functional, bridge, concurrency, performance, or compatibility issue
- `S2`: meaningful quality gap, missing case, weak test coverage, or maintainability risk
- `S3`: minor improvement or polish recommendation

## Required Review Dimensions

### 1. Scope Compliance

Check:
- whether the requested iOS task was fully implemented
- whether unrelated refactors or architectural shifts were folded in
- whether bridge or packaging work exceeded the approved scope

### 2. Correctness Review

Check:
- functional correctness on the iOS path
- SwiftUI or UIKit state correctness
- optionals and error propagation
- framework or bridge initialization behavior
- storage and file-path correctness on Apple platforms

### 3. Concurrency and Main-Thread Safety

Check:
- `MainActor` assumptions and violations
- background work placement
- task cancellation and structured concurrency
- callback-to-async bridging correctness
- UI responsiveness during load-heavy operations
- watchdog-risk behavior from blocking work

### 4. Memory and Lifecycle Behavior

Check:
- retain-cycle risk
- object ownership clarity
- lifecycle behavior across foreground/background transitions
- memory pressure handling where relevant
- repeated initialization or stale singleton state

### 5. Platform Integration

Check:
- supported iOS version handling
- permissions and privacy declarations
- framework linking and packaging consistency
- SPM, CocoaPods, cinterop, or binary bridge assumptions
- device-only capability assumptions hidden behind simulator success

### 6. Test Quality

Check:
- whether changed logic is actually covered
- whether async behavior is tested credibly
- whether bridge-unavailable and failure states are verified
- whether iOS-specific behavior is untested while shared logic alone is covered

### 7. Documentation Consistency

Check:
- whether architecture or feature docs still match iOS behavior
- whether runtime, framework, or device caveats are documented
- whether task tracking reflects real completion

### 8. Architectural Risks

Check:
- SwiftUI, UIKit, and bridge logic coupled too tightly
- framework assumptions leaking across layers
- fragile singleton or static-state patterns
- shortcuts that will harden into long-term platform debt

### 9. Future Readiness

Check:
- whether the design can handle the next obvious iOS extension without rewrite
- whether framework or bridge seams are stable enough for later replacement
- whether test seams exist for future async or runtime changes

## iOS Review Heuristics

Escalate findings when you see:
- bridge or framework initialization assumed successful without explicit failure mapping
- heavy load or inference work performed on the main thread
- `Task` usage with no cancellation or ownership strategy
- retain-cycle risk from closures, delegates, or observer lifetime
- simulator-only evidence treated as sufficient for device-sensitive behavior
- runtime packaging assumptions that differ from compile-time success
- SwiftUI view logic causing repeated side effects or stale state reuse

## Do / Do Not

Do:
- cite concrete files, symbols, behaviors, or platform assumptions
- separate blockers from recommendations
- explain why the iOS platform makes the issue risky
- distinguish compile-time success from runtime safety

Do not:
- treat framework linking or bridge availability as solved because the app builds
- ignore memory ownership issues because the feature is small
- accept main-thread heavy work in interactive flows
- reduce SwiftUI or async issues to style arguments

## PASS / PARTIAL / FAIL Rules

- `PASS`: requirements are met, no blockers remain, and no significant iOS platform risks were found
- `PARTIAL`: substantial work is correct, but one or more iOS-specific fixes are required before closure
- `FAIL`: core requirements are unmet, serious iOS regressions exist, or the implementation is unsafe to accept

## Expected Output

Always use this structure:

```text
1. Scope Compliance
- PASS/PARTIAL/FAIL: <assessment>

2. Correctness Review
- [S1] <finding>

3. Concurrency and Main-Thread Safety
- [S1] <finding or "No material gaps found">

4. Memory and Lifecycle Behavior
- [S1] <finding or "No material gaps found">

5. Platform Integration
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
- iOS-specific blockers are explicit
- closure criteria are concrete
- the verdict matches the evidence
