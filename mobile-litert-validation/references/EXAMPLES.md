# Validation Examples

## Android validation success

```text
Outcome
- PASS: Android LiteRT runtime was available, model loaded successfully, and offline inference remained functional on device.

Device tested
- Pixel 8, Android 16, debug build 1.4.0(184)

Bridge/runtime availability
- Available: AICore presence confirmed and LiteRT load logs showed successful runtime initialization.

Validation steps
- Fresh install
- Model availability check
- First load with network enabled
- Airplane mode re-launch
- Prompt inference run x5

Observations
- Load executed on background dispatcher.
- UI remained responsive during load.
- Airplane mode re-launch used cached local model successfully.

Timing metrics
- Model load: 842 ms
- First inference: 1290 ms
- Steady-state inference p50: 611 ms

Blockers
- None

Recommendation
- Accept for continued rollout testing on supported Android devices.

Follow-up actions
- Validate memory behavior on lower-RAM device tier.
```

## iOS bridge unavailable

```text
Outcome
- BLOCKED: iOS validation could not proceed because the LiteRT bridge framework was not available at runtime.

Device tested
- iPhone 15 Pro, iOS 20.0, internal debug build

Bridge/runtime availability
- Unavailable: bridge initialization failed immediately after launch and no model load call was possible.

Validation steps
- Launch app
- Open on-device model screen
- Attempt model initialization

Observations
- App UI stayed responsive, but the bridge health check returned unavailable.
- Logs indicate framework lookup failure rather than checksum or model corruption.

Timing metrics
- Not available due to blocked runtime initialization

Blockers
- Runtime bridge unavailable on device build

Recommendation
- Do not treat this as model failure. Fix framework packaging or runtime loading before further iOS validation.

Follow-up actions
- Confirm framework linking strategy
- Rebuild and re-run bridge availability check
```

## Model load performance regression

```text
Outcome
- FAIL: Model load completed but regressed beyond acceptable latency on the tested build.

Device tested
- Pixel 8 Pro, Android 16, candidate build 1.4.0(190)

Bridge/runtime availability
- Available

Validation steps
- Clear cached model state
- Launch and load model
- Repeat load after warm restart

Observations
- Functional behavior was correct.
- First load latency increased materially relative to baseline.
- No main-thread stall observed.

Timing metrics
- Baseline load: 910 ms
- Current load: 2380 ms
- Regression: +161%

Blockers
- None

Recommendation
- Hold rollout for performance investigation if the product target requires sub-1.5 s initial model readiness.

Follow-up actions
- Compare packaging, decompression path, and runtime initialization logs against baseline build.
```

## Airplane mode verification

```text
Outcome
- PASS: After initial model acquisition, the app completed offline startup and inference in airplane mode.

Device tested
- iPhone 16, iOS 20.0, test build 52

Bridge/runtime availability
- Available

Validation steps
- Launch with network enabled and complete initial model preparation
- Force-close app
- Enable airplane mode
- Relaunch and run prompt inference

Observations
- No network dependency was observed after initial preparation.
- Cached asset path resolved correctly.

Timing metrics
- Offline load: 1.1 s
- First offline inference: 1.6 s

Blockers
- None

Recommendation
- Accept offline readiness for this device tier.

Follow-up actions
- Repeat on a cold-start, lower-memory device.
```

## Blocked Status Rules

Use `BLOCKED`, not `FAIL`, when the integration could not actually be exercised because environment prerequisites were missing or broken before meaningful validation.
