# iOS Review Examples

## Example: PASS

```text
1. Scope Compliance
- PASS: The iOS task was completed without unrelated SwiftUI, packaging, or bridge refactors outside the requested area.

2. Correctness Review
- No blocking correctness defects found.

3. Concurrency and Main-Thread Safety
- No material gaps found. Model preparation is off the main thread and UI updates are returned to the expected actor boundary.

4. Memory and Lifecycle Behavior
- No material gaps found. Bridge state is owned explicitly and does not reinitialize unexpectedly on screen redraw.

5. Platform Integration
- PASS: Framework availability checks and runtime failure mapping align with the supported-device contract.

6. Test Quality
- PASS: Async tests cover success, unavailable-bridge behavior, and retry handling.

7. Documentation Consistency
- PASS: iOS runtime caveats and device-only assumptions are documented.

8. Architectural Risks
- Low: The framework boundary remains narrow and replaceable.

9. Final Verdict
- PASS: Acceptable as submitted.

10. Required Fixes
- None.

11. Non-blocking Recommendations
- Add one device-level regression test for repeated foreground-resume behavior.
```

## Example: PARTIAL

```text
3. Concurrency and Main-Thread Safety
- [S1] Initial model load is started from a user-interactive path on the main thread, creating watchdog risk and avoidable UI stall risk.

6. Test Quality
- [S2] No test covers bridge-unavailable behavior at the UI-facing state layer.

9. Final Verdict
- PARTIAL: The core behavior is close, but iOS threading safety and proof coverage remain incomplete.

10. Required Fixes
- Move the heavy initialization path off the main thread and add a regression test for bridge-unavailable state propagation.
```

## Example: FAIL

```text
4. Memory and Lifecycle Behavior
- [S1] The bridge owner retains a closure cycle and will leak across repeated screen presentation.

5. Platform Integration
- [S1] The framework is assumed present at runtime with no failure handling, so device builds can fail after successful compilation.

9. Final Verdict
- FAIL: The task introduces iOS-specific risks in both runtime integration and memory behavior.
```

## Anti-Patterns

- Treating simulator success as proof of device viability.
- Treating framework initialization failure as an edge case rather than a required path.
- Calling a review complete when async cancellation behavior is untested.
