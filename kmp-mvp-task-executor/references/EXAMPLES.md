# Examples

## Example: scope framing

Request:
Implement model download status handling for shared setup flow.

Good scope statement:
- In scope: shared download state model, repository contract, Android and iOS adapters, tests, setup-flow documentation.
- Out of scope: UI redesign, download resume support, telemetry pipeline changes.

Poor scope statement:
- Improve setup flow and clean up related code.

## Example: commonMain-first decision

Place in `commonMain`:
- checksum validation policy
- download state machine
- fallback decision tree
- user-facing error classification

Place in `androidMain`:
- AICore availability query
- Android file storage location

Place in `iosMain`:
- framework bridge invocation
- iOS-specific bundle or sandbox path handling

## Example: acceptable summary

```text
Summary
- Added shared model load coordinator with deterministic fallback when local model metadata is missing.

Files changed
- shared/src/commonMain/.../ModelLoadCoordinator.kt: added orchestration and failure mapping
- shared/src/commonTest/.../ModelLoadCoordinatorTest.kt: added success and fallback coverage
- iosMain/.../LiteRtBridgeAdapter.kt: mapped bridge-unavailable state

Tests added/updated
- ModelLoadCoordinatorTest: load success, checksum failure, bridge unavailable fallback

Verification results
- PASS: shared unit tests
- PASS: Android compile for affected module
- NOT RUN: iOS device validation requires local signing setup

Known limitations
- iOS path uses existing bridge diagnostics and does not add memory-pressure telemetry

Commits (if applicable)
- not created
```

## Anti-Patterns

- Moving logic into `androidMain` because the Android implementation existed first.
- Adding a generic `ManagerFactory` with one implementation.
- Skipping tests because behavior “is simple”.
- Updating code without updating task or architecture notes when contracts changed.
