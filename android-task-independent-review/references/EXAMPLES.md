# Android Review Examples

## Example: PASS

```text
1. Scope Compliance
- PASS: The Android task was completed without unrelated refactors or manifest churn outside the requested feature area.

2. Correctness Review
- No blocking correctness defects found.

3. Concurrency and Lifecycle Safety
- No material gaps found. Model preparation is dispatched off the main thread and cancellation remains ViewModel-scoped.

4. UI and UX Contract
- No material gaps found. Loading, permission denial, and retry states are represented consistently in Compose state.

5. Platform Compatibility
- PASS: API-level checks and permission handling match the supported-device contract.

6. Test Quality
- PASS: ViewModel tests cover success, denial, and unavailable-runtime paths.

7. Documentation Consistency
- PASS: Android rollout limitations and permission requirements are documented.

8. Architectural Risks
- Low: Platform services remain behind a narrow interface seam.

9. Final Verdict
- PASS: Acceptable as submitted.

10. Required Fixes
- None.

11. Non-blocking Recommendations
- Add a regression test for repeated screen entry after process recreation.
```

## Example: PARTIAL

```text
3. Concurrency and Lifecycle Safety
- [S1] Model initialization is triggered from the main thread during first composition, creating jank risk and violating the expected background-load contract.

6. Test Quality
- [S2] No test verifies that repeated recomposition does not re-trigger initialization.

9. Final Verdict
- PARTIAL: Functional behavior is close, but Android threading and proof coverage are not yet acceptable.

10. Required Fixes
- Move initialization to a lifecycle-safe background path and add a regression test that proves it runs once per intended trigger.
```

## Example: FAIL

```text
2. Correctness Review
- [S1] The permission-required path assumes grant success and leaves the screen in a broken state after denial.

5. Platform Compatibility
- [S1] The implementation calls a newer API without guard logic for lower supported versions.

9. Final Verdict
- FAIL: The task introduces Android regressions in both permission behavior and supported-device compatibility.
```

## Anti-Patterns

- Accepting a Compose side effect that can fire navigation repeatedly.
- Treating missing API guards as minor polish.
- Calling a review complete when the denial path is untested and undocumented.
