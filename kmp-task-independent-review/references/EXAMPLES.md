# Review Examples

## Example: PASS

```text
1. Scope Compliance
- PASS: The requested shared checksum validation and platform adapter wiring were implemented without unrelated expansion.

2. Correctness Review
- No blocking correctness defects found.

3. Edge Cases
- No material gaps found for offline, checksum mismatch, or bridge-unavailable paths.

4. Test Quality
- PASS: Shared tests cover success, invalid checksum, and unavailable runtime fallback.

5. Documentation Consistency
- PASS: Architecture note and task log reflect the new fallback behavior.

6. Architectural Risks
- Low: Current interface seam is sufficient for later model-provider replacement.

7. Final Verdict
- PASS: Acceptable as submitted.

8. Required Fixes
- None.

9. Non-blocking Recommendations
- Add latency histogram logging when device-level validation begins.
```

## Example: PARTIAL

```text
2. Correctness Review
- [S1] iOS bridge-unavailable path returns generic failure, which violates the documented graceful-degradation requirement.

4. Test Quality
- [S2] No regression test proves the fallback state exposed to shared UI state.

7. Final Verdict
- PARTIAL: Core implementation is close, but fallback behavior and proof coverage are incomplete.

8. Required Fixes
- Return the documented bridge-unavailable state from the iOS adapter and add a shared regression test that asserts UI-visible behavior.
```

## Example: FAIL

```text
1. Scope Compliance
- FAIL: The change replaces deterministic shared selection logic with platform-specific heuristics and adds unrelated refactors to repository wiring.

2. Correctness Review
- [S1] Android and iOS now produce different model-selection outcomes for the same inputs.
- [S1] Public contract behavior changed without migration or documentation.

7. Final Verdict
- FAIL: The task is not acceptable because correctness and scope boundaries were both broken.
```

## Anti-Patterns

- “Needs more tests” without naming the missing behavior.
- “Architecture feels off” without identifying the broken boundary.
- Recommending a rewrite instead of isolating the blocking defect.
