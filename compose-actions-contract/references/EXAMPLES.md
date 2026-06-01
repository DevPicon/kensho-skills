# Examples

## Example: use the skill

Request:
Refactor a `SettingsScreen` composable that currently takes 5 callbacks and forwards most of them through two child composables.

Good fit:
- Introduce `SettingsActions`.
- Keep `SettingsState` separate from events.
- Update parent and child call sites to use `actions`.

Why:
- The callback group belongs to one feature contract.
- The signature becomes easier to scan.
- Callback threading through intermediate composables is reduced.

## Example: do not use the skill

Request:
Adjust a small `RetryChip` composable that only exposes `onRetry`.

Poor fit:
- Creating `RetryChipActions` adds ceremony without improving readability.

Preferred:
- Keep `onRetry: () -> Unit` as a direct parameter.

## Example: naming

Preferred:
- `CheckoutActions`
- `ProfileEditorActions`
- `ModelSetupActions`

Avoid:
- `Actions`
- `UiActions`
- `Handlers`

## Example: nested actions

Use nested actions when a large screen has real subdomains:
- `CheckoutActions(form, payment, footer)`

Do not nest when each subgroup only wraps one trivial callback.

## Example: review heuristic

Usually keep direct callbacks:
- 1-2 callbacks

Strongly consider `Actions`:
- 3-5 related callbacks

Default to `Actions` unless there is a strong counterexample:
- 6+ related callbacks
