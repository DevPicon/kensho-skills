---
name: mobile-litert-validation
description: Validate LiteRT-LM and on-device model integrations across Android and iOS, including model download, checksum verification, runtime availability, bridge health, offline behavior, latency, memory pressure, main-thread safety, and graceful failure. Use when testing Gemma, Gemini Nano, or similar quantized on-device model integrations on mobile.
license: MIT
compatibility: Designed for Agent Skills compatible coding agents validating Android and iOS on-device AI integrations with LiteRT-LM or equivalent local model runtimes.
metadata:
  author: devpicon
  version: "1.0"
---

# Mobile LiteRT Validation

Use this skill to validate a mobile on-device model integration, not to implement the integration from scratch.

## Activation Criteria

Use this skill when:
- The main task is validating a mobile on-device model integration rather than building it.
- The request involves LiteRT-LM, Gemma, Gemini Nano, or a similar local mobile model runtime.
- The user expects validation evidence such as logs, timing, offline behavior, runtime availability, or failure categorization.
- Real-device behavior, bridge health, or runtime readiness is part of the acceptance criteria.

Do not use this skill when:
- The main task is to implement the integration from scratch.
- The main task is a broad KMP feature implementation or refactor.
- The main task is an architectural review of completed code changes.
- The request can be satisfied by a static code inspection without runtime validation.

## Resources

Read [references/EXAMPLES.md](references/EXAMPLES.md) for validation report examples, blocked-state patterns, and expected diagnostic wording.

## Objective

Determine whether the Android and iOS LiteRT-LM integration is usable, observable, and safe enough for the requested validation target.

Primary validation targets:
- Android LiteRT-LM integration
- iOS LiteRT-LM bridge or framework integration
- Gemma, Gemini Nano, and similar quantized on-device models

## Validation Scope

Validate as applicable:
- model download path
- checksum verification
- runtime or bridge availability
- model load sequence
- airplane mode behavior
- real-device execution
- latency
- memory pressure behavior
- main-thread safety
- graceful failure states
- runtime diagnostics and logs

## Platform Focus

### Android

Check specifically:
- native Kotlin API usage
- coroutine safety
- background-thread execution for model load and inference
- AICore or model availability detection
- model loading diagnostics

### iOS

Check specifically:
- bridge availability
- Swift, Objective-C, or C++ wrapper state
- framework linking and loadability
- SPM vs cinterop integration assumptions
- watchdog risk from blocking work
- memory pressure handling
- UI responsiveness during load and inference

## Workflow

1. Define the validation target:
   - app build
   - branch or commit
   - model/runtime variant
   - device and OS version
2. Confirm prerequisites:
   - correct build installed
   - required model or runtime package available
   - logging enabled
   - test device accessible
3. Verify runtime availability before functional testing.
4. Validate model acquisition:
   - fresh install path
   - repeat launch path
   - checksum or integrity verification path
5. Validate model load behavior:
   - success path
   - unavailable runtime path
   - corrupted or missing asset path
6. Validate offline behavior with airplane mode or equivalent network cutoff when the product claims on-device capability.
7. Measure timing:
   - download duration if applicable
   - load duration
   - first inference latency
   - steady-state inference latency if relevant
8. Observe thread use and UI responsiveness during load and inference.
9. Probe memory pressure or large-model stress if the device and build support it safely.
10. Record blocking defects, evidence, and unblock requirements.
11. Produce a structured validation report.

## Blocking Criteria

Mark the outcome `BLOCKED` when any of these prevent meaningful validation:
- runtime or bridge is unavailable
- build cannot access required model artifact
- framework fails to link or initialize
- app terminates before validation can begin
- device setup is missing and no equivalent target exists
- logs are insufficient to distinguish runtime absence from load failure

## Unblock Requirements

When blocked, state the minimum next action required. Examples:
- provide device build with bridge framework linked
- enable runtime diagnostics for model load
- install required AICore or model package
- fix checksum manifest packaging

## Known Failure Patterns

- checksum mismatch after partial download or stale cached artifact
- model load on main thread causing UI freeze or watchdog risk
- Android coroutine launched on inappropriate dispatcher
- iOS bridge present at compile time but absent at runtime
- inconsistent offline behavior because first-run download was implicit
- successful load with unusable latency on lower-tier devices

## Do / Do Not

Do:
- prefer real-device validation over simulator assumptions when runtime behavior matters
- capture exact device, OS, and model variant
- distinguish unavailability from misconfiguration
- report timing in measurable units
- state whether failure is product-blocking or environment-blocking

Do not:
- declare success based only on build completion
- treat simulator-only evidence as proof of real-device viability
- omit thread or responsiveness observations
- hide blocked states behind generic “test failed” language

## Diagnostics Template

Capture at minimum:
- build identifier
- device model and OS version
- model name and quantization
- runtime or bridge version if known
- relevant logs during download, load, and inference
- whether work executed off the main thread
- timing metrics with units

## Expected Logs

Look for evidence such as:
- runtime availability checks
- model artifact path resolution
- checksum pass or mismatch
- bridge initialization success or failure
- load start and load complete timestamps
- inference start and end timestamps
- explicit fallback or error-state mapping

## Expected Output

Use this format:

```text
Outcome
- PASS / FAIL / BLOCKED: <one-sentence result>

Device tested
- <device, OS, build>

Bridge/runtime availability
- <available/unavailable and evidence>

Validation steps
- <step list>

Observations
- <behavioral observations>

Timing metrics
- <metric>: <value>

Blockers
- <blocking issue or "None">

Recommendation
- <ship / fix / re-test guidance>

Follow-up actions
- <next actions>
```

## Closure Criteria

Validation is complete only when:
- the tested device context is explicit
- availability is confirmed or blocked with evidence
- timing is reported or explicitly unavailable
- failure mode is categorized
- the recommendation is actionable
