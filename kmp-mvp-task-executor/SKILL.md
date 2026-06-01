---
name: kmp-mvp-task-executor
description: Execute scoped implementation tasks in Kotlin Multiplatform projects with a disciplined commonMain-first workflow, Android and iOS parity checks, test updates, architecture traceability, and deterministic delivery. Use when implementing or modifying KMP features, integrations, or refactors for MVP-stage mobile products.
license: MIT
compatibility: Designed for Agent Skills compatible coding agents working in Kotlin Multiplatform repositories with shared, Android, and iOS source sets.
metadata:
  author: devpicon
  version: "1.0"
---

# KMP MVP Task Executor

Use this skill when the request is to implement a task in a Kotlin Multiplatform codebase, not to brainstorm broadly or perform an independent review.

## Activation Criteria

Use this skill when:
- The task is an implementation, bug fix, refactor, or integration in a KMP repository.
- Shared business logic should likely live in `commonMain`.
- Android and iOS parity matters.
- The user expects code changes, tests, and verification.

Do not use this skill when:
- The main task is architectural review only.
- The main task is runtime validation on devices only.
- The request is intentionally open-ended product ideation.

## Resources

Read [references/EXAMPLES.md](references/EXAMPLES.md) when the task needs output formatting examples or KMP-specific decision examples.

## Objective

Deliver the requested implementation with strict scope control, commonMain-first reasoning, test coverage, and documentation traceability.

Priorities:
- Preserve architecture consistency.
- Prefer deterministic logic over speculative intelligence.
- Keep Android and iOS behavior aligned unless the task explicitly requires divergence.
- Leave clear seams for later replacement.

## Workflow

1. Read the task request and restate the requested scope in concrete engineering terms.
2. Read the relevant architecture, task-tracking, and feature documentation before editing code.
3. Identify affected modules, source sets, interfaces, tests, build constraints, and platform dependencies.
4. Check prerequisites and blockers before editing:
   - missing API keys
   - missing models or assets
   - absent platform bridge
   - unrelated failing tests
5. Define the implementation boundary:
   - what is in scope
   - what is explicitly out of scope
   - what must remain backward compatible
6. Default to `commonMain` for domain logic, contracts, and use cases. Push code to platform source sets only when required by platform APIs, SDKs, or packaging constraints.
7. Prefer constructor injection and interfaces at boundaries that may change:
   - model loader
   - repository
   - clock
   - dispatcher
   - network or file access
8. Implement the smallest coherent change that satisfies the task. Avoid opportunistic cleanup unless it is required to complete the requested scope safely.
9. Add or update tests with the change:
   - unit tests first for shared logic
   - platform tests only for platform-specific behavior
   - regression tests for observed bugs
10. Update architecture or task documentation when the implementation changes behavior, dependencies, contracts, or limitations.
11. Run verification relevant to the modified scope.
12. Produce a delivery summary using the required output format.

## CommonMain-First Rules

Default placement:
- `commonMain`: business logic, state transitions, validation, data mapping, contracts, orchestration, deterministic fallback policy
- `androidMain`: Android SDK integration, platform services, AICore access, file/provider specifics
- `iosMain`: bridge/framework calls, Swift or Objective-C boundary adapters, Apple platform lifecycle specifics

Move code out of `commonMain` only when one of these is true:
- it depends on a platform-only API
- binary packaging differs by platform
- threading or lifecycle semantics are inherently platform-specific

If code lands in a platform source set, document why.

## Design Rules

Do:
- prefer interfaces over concrete dependency reach-through
- keep constructors explicit
- pass dispatchers, clocks, and loaders as dependencies when they influence behavior
- model failure states explicitly
- add graceful degradation when external model/runtime dependencies are unavailable
- leave seams for model/runtime replacement
- keep MVP behavior predictable and observable

Do not:
- add abstractions with no current consumer or replacement pressure
- introduce AI or heuristic behavior when deterministic rules solve the task
- hide platform differences behind misleading “shared” APIs
- expand the task into adjacent refactors without necessity
- change architectural direction without documenting the reason

## Constraints

- Optimize for correctness and maintainability over cleverness.
- Prefer straightforward control flow.
- Avoid introducing new dependencies unless required.
- Preserve public contracts unless the task explicitly authorizes breaking changes.
- Keep documentation synchronized with the implementation.

## Failure Modes

Treat these as implementation failures:
- business logic duplicated across Android and iOS without reason
- `commonMain` bypassed for convenience
- tests omitted for changed logic
- documentation left stale after contract changes
- fallback behavior undefined when platform runtime is unavailable
- silent scope creep into unrelated modules

## Boundaries

This skill does not authorize:
- product requirement changes
- large architecture rewrites
- performance optimization beyond the requested scope
- replacing deterministic behavior with LLM-driven behavior unless explicitly requested
- undocumented platform divergence

## Verification Expectations

Run the smallest credible set of checks for the changed area:
- targeted unit tests for shared logic
- platform-specific tests when platform code changed
- build or compile verification for affected modules
- lint or static analysis if already part of normal workflow

If verification cannot run, report exactly why and what remains unverified.

## Expected Output

Return results in this format:

```text
Summary
- <what was implemented>

Files changed
- <path>: <reason>

Tests added/updated
- <test path>: <coverage summary>

Verification results
- PASS/FAIL: <command or check> - <result>

Known limitations
- <remaining constraint or deferred item>

Commits (if applicable)
- <commit hash or "not created">
```

## Closure Criteria

The task is complete only when all are true:
- requested scope is implemented
- boundaries were respected
- tests were added or intentionally justified
- relevant docs were updated or confirmed unchanged
- verification status is explicit
