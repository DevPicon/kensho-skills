---
name: compose-actions-contract
description: Encourage Jetpack Compose and Compose Multiplatform APIs to group related UI event callbacks into a domain-specific Actions data class instead of passing many individual lambdas. Use when designing or refactoring composables, screen contracts, ViewModel wiring, or child composable APIs that are becoming noisy, hard to scan, or callback-heavy.
license: MIT
compatibility: Designed for OpenAI Codex, Claude Code, and similar Agent Skills compatible coding agents working in Kotlin, Jetpack Compose, or Compose Multiplatform codebases.
metadata:
  author: devpicon
  version: "1.0"
---

# Compose Actions Contract

## Overview

Use this skill when a composable API is accumulating too many callback parameters and the function signature is becoming harder to read, harder to maintain, or harder to pass through multiple UI layers.

Primary goal:
Group related callbacks into a dedicated, domain-specific `Actions` data class so composable contracts stay legible and stable.

Preferred outcome:
- Screen and section APIs are easier to scan.
- State and events are clearly separated.
- Parent composables pass fewer parameters.
- ViewModel wiring is centralized and predictable.
- Child composables receive a coherent event contract instead of a long callback list.

This is guidance, not dogma. Keep direct callbacks when that is clearly simpler.

## Activation Criteria

Use this skill when:
- A Jetpack Compose or Compose Multiplatform API is becoming callback-heavy or hard to scan.
- A composable, route, or screen contract would become clearer by grouping related callbacks into a domain-specific `Actions` type.
- The same callback set is forwarded through multiple UI layers.
- The task is a focused API design or refactor of composable event contracts.

Do not use this skill when:
- The main task is unrelated to composable API design.
- A tiny component only has 1-2 obvious callbacks and grouping would add ceremony.
- The callbacks are unrelated enough that one actions holder would hide intent.
- The project already has a stronger local pattern that should not be displaced.

## Resources

Read [references/EXAMPLES.md](references/EXAMPLES.md) for quick decision examples, naming guidance, and refactoring patterns that complement this skill.

## Core philosophy

Prefer this:

```kotlin
data class DiceResultActions(
    val onRollDice: () -> Unit,
    val onResetResult: () -> Unit,
    val onOpenHistory: () -> Unit,
)

@Composable
fun DiceResultScreen(
    state: DiceResultState,
    actions: DiceResultActions,
    modifier: Modifier = Modifier,
) {
    // ...
}
```

Instead of this:

```kotlin
@Composable
fun DiceResultScreen(
    state: DiceResultState,
    onRollDice: () -> Unit,
    onResetResult: () -> Unit,
    onOpenHistory: () -> Unit,
    modifier: Modifier = Modifier,
) {
    // ...
}
```

Why:
- `state` answers "what the UI shows".
- `actions` answers "what the UI can do".
- The composable signature reads like a screen contract instead of an event dump.

Target principle:
Use a domain-specific `Actions` type whenever the number of callbacks or the semantic grouping makes the API meaningfully clearer.

## Activation criteria

Use this skill when:
- A composable has 3 or more callbacks and the signature is getting noisy.
- A screen-level composable forwards many callbacks into multiple child composables.
- A state holder already exists and a matching `Actions` contract would clarify the API.
- The same callback group is passed through several layers.
- A refactor is needed to make screen contracts more maintainable.

Do not use this skill when:
- A tiny reusable composable only needs 1 callback.
- A simple component has 2 obvious callbacks and an `Actions` class would add ceremony.
- The callbacks are unrelated enough that grouping them would hide intent.
- The project already follows a stronger local convention that would be broken by this pattern.

Heuristic:
- `1-2 callbacks`: usually keep them direct.
- `3-5 callbacks`: strongly consider `Actions`.
- `6+ callbacks`: default to `Actions` unless there is a very strong reason not to.

## Naming conventions

Always prefer domain-specific names.

Preferred:
- `DiceResultActions`
- `CharacterCreationActions`
- `ModelSetupActions`
- `GameSessionActions`

Avoid:
- `Actions`
- `Handlers`
- `Callbacks`
- `ScreenActions`
- `UiActions` when a better domain name exists

Default naming rule:
- Start from the state holder name.
- Replace `State` with `Actions`.

Examples:
- `DiceResultState` -> `DiceResultActions`
- `CharacterCreationState` -> `CharacterCreationActions`
- `ModelSetupState` -> `ModelSetupActions`

If there is no paired state class, still use a domain name:
- `ProfileEditorActions`
- `ChatComposerActions`

Keep callback names verb-first and user-intent focused:
- `onRetry`
- `onDismiss`
- `onConfirmDelete`
- `onItemSelected`
- `onSaveDraft`

Avoid vague event names:
- `onAction`
- `onEvent`
- `onClick1`

## Parameter ordering conventions

Prefer this parameter order:

1. `state`
2. `actions`
3. `modifier`

Example:

```kotlin
@Composable
fun ModelSetupScreen(
    state: ModelSetupState,
    actions: ModelSetupActions,
    modifier: Modifier = Modifier,
)
```

Rationale:
- State is the primary input.
- Actions define interaction points.
- Modifier stays last among common non-trailing parameters, following standard Compose style.

For small stateless building blocks without state, use judgment:
```kotlin
@Composable
fun RetryBanner(
    actions: RetryBannerActions,
    modifier: Modifier = Modifier,
)
```

Do not place `modifier` before `actions` unless the local codebase has an explicit convention that requires it.

## Refactoring patterns

### Pattern 1: Collapse a long callback list into one contract

Before:

```kotlin
@Composable
fun Screen(
    state: ScreenState,
    onBack: () -> Unit,
    onRetry: () -> Unit,
    onDismiss: () -> Unit,
    onConfirm: () -> Unit,
    onItemSelected: (String) -> Unit,
    onDelete: (String) -> Unit,
    modifier: Modifier = Modifier,
)
```

After:

```kotlin
data class ScreenActions(
    val onBack: () -> Unit,
    val onRetry: () -> Unit,
    val onDismiss: () -> Unit,
    val onConfirm: () -> Unit,
    val onItemSelected: (String) -> Unit,
    val onDelete: (String) -> Unit,
)

@Composable
fun Screen(
    state: ScreenState,
    actions: ScreenActions,
    modifier: Modifier = Modifier,
)
```

### Pattern 2: Stop threading callbacks individually through parents

Before:

```kotlin
@Composable
fun ParentScreen(
    state: ParentState,
    onSave: () -> Unit,
    onCancel: () -> Unit,
    onNameChanged: (String) -> Unit,
    onAvatarClick: () -> Unit,
) {
    ChildForm(
        name = state.name,
        onNameChanged = onNameChanged,
        onSave = onSave,
        onCancel = onCancel,
    )

    AvatarSection(
        avatarUrl = state.avatarUrl,
        onAvatarClick = onAvatarClick,
    )
}
```

After:

```kotlin
data class ParentActions(
    val onSave: () -> Unit,
    val onCancel: () -> Unit,
    val onNameChanged: (String) -> Unit,
    val onAvatarClick: () -> Unit,
)

@Composable
fun ParentScreen(
    state: ParentState,
    actions: ParentActions,
) {
    ChildForm(
        name = state.name,
        onNameChanged = actions.onNameChanged,
        onSave = actions.onSave,
        onCancel = actions.onCancel,
    )

    AvatarSection(
        avatarUrl = state.avatarUrl,
        onAvatarClick = actions.onAvatarClick,
    )
}
```

### Pattern 3: Introduce an actions contract during screen extraction

When extracting a large screen into smaller composables:
- Keep one top-level screen `Actions` contract.
- Pass only the specific callbacks needed by children.
- Introduce nested action classes only when a child section is itself substantial.

## Nested actions guidance

Nested action classes are allowed for large screens with clear subdomains.

Example:

```kotlin
data class CharacterCreationActions(
    val form: CharacterFormActions,
    val avatar: CharacterAvatarActions,
    val footer: CharacterFooterActions,
)

data class CharacterFormActions(
    val onNameChanged: (String) -> Unit,
    val onClassSelected: (CharacterClass) -> Unit,
)

data class CharacterAvatarActions(
    val onRandomizeAvatar: () -> Unit,
    val onOpenAvatarPicker: () -> Unit,
)

data class CharacterFooterActions(
    val onBack: () -> Unit,
    val onCreateCharacter: () -> Unit,
)
```

Use nested actions when:
- The screen has distinct functional sections.
- Child sections are large enough to deserve their own contracts.
- Grouping improves readability at the call site.

Do not over-engineer:
- Avoid nesting just to follow a pattern mechanically.
- Avoid `ParentActions( header, body, footer )` if each group only contains one trivial callback.
- Prefer a flatter `Actions` class when that is easier to read.

Rule:
If nesting makes the call site clearer, use it. If nesting makes readers jump across too many tiny types, simplify it.

## ViewModel integration examples

### Basic ViewModel wiring

```kotlin
class DiceResultViewModel : ViewModel() {
    val state: StateFlow<DiceResultState> = TODO()

    fun rollDice() = Unit
    fun resetResult() = Unit
    fun openHistory() = Unit
}

@Composable
fun DiceResultRoute(
    viewModel: DiceResultViewModel,
    modifier: Modifier = Modifier,
) {
    val state = viewModel.state.collectAsState().value

    val actions = DiceResultActions(
        onRollDice = viewModel::rollDice,
        onResetResult = viewModel::resetResult,
        onOpenHistory = viewModel::openHistory,
    )

    DiceResultScreen(
        state = state,
        actions = actions,
        modifier = modifier,
    )
}
```

### With parameters captured from composition

If callbacks capture changing values from composition, create the actions object with `remember` keyed to the relevant values.

```kotlin
@Composable
fun CharacterCreationRoute(
    viewModel: CharacterCreationViewModel,
    navigator: Navigator,
) {
    val state = viewModel.state.collectAsState().value

    val actions = remember(viewModel, navigator) {
        CharacterCreationActions(
            form = CharacterFormActions(
                onNameChanged = viewModel::updateName,
                onClassSelected = viewModel::selectClass,
            ),
            avatar = CharacterAvatarActions(
                onRandomizeAvatar = viewModel::randomizeAvatar,
                onOpenAvatarPicker = navigator::openAvatarPicker,
            ),
            footer = CharacterFooterActions(
                onBack = navigator::goBack,
                onCreateCharacter = viewModel::createCharacter,
            ),
        )
    }

    CharacterCreationScreen(
        state = state,
        actions = actions,
    )
}
```

### If the ViewModel already exposes intent methods

Prefer passing stable, named methods rather than wrapping everything in anonymous lambdas without need.

Preferred:
```kotlin
val actions = ModelSetupActions(
    onRetry = viewModel::retry,
    onDismissError = viewModel::dismissError,
    onModelSelected = viewModel::selectModel,
)
```

Less preferred when not necessary:
```kotlin
val actions = ModelSetupActions(
    onRetry = { viewModel.retry() },
    onDismissError = { viewModel.dismissError() },
    onModelSelected = { id -> viewModel.selectModel(id) },
)
```

## Stability considerations

Default recommendation:
- Use an immutable `data class` for actions.
- Use `val` properties only.

Example:
```kotlin
data class GameSessionActions(
    val onPause: () -> Unit,
    val onResume: () -> Unit,
    val onQuit: () -> Unit,
)
```

Optional:
- Use `@Immutable` only if the project already uses Compose stability annotations and this is part of the local convention.

Example:
```kotlin
@Immutable
data class GameSessionActions(
    val onPause: () -> Unit,
    val onResume: () -> Unit,
    val onQuit: () -> Unit,
)
```

Do not introduce `@Immutable` just because this skill exists. Match the project.

When to use `remember`:
- Use `remember` when recreating the actions object every recomposition would be unnecessary and the inputs are stable.
- Especially useful at route or screen-container level.
- Key `remember` with the values actually captured by the callbacks.

Example:
```kotlin
val actions = remember(viewModel) {
    SettingsActions(
        onRefresh = viewModel::refresh,
        onSignOut = viewModel::signOut,
    )
}
```

When `remember` may be unnecessary:
- The actions object is tiny.
- The codebase does not optimize around object recreation here.
- The callbacks must capture frequently changing values anyway.

Do not add `remember` mechanically. Use it intentionally.

## Anti-patterns

### Generic naming

Bad:
```kotlin
data class Actions(
    val onClick: () -> Unit,
)
```

Why it is bad:
- The type name carries no domain meaning.
- Searchability and readability suffer.

### Ceremony for tiny components

Bad:
```kotlin
data class IconButtonActions(
    val onClick: () -> Unit,
)

@Composable
fun TinyButton(
    actions: IconButtonActions,
)
```

Why it is bad:
- One callback does not justify another type unless there is a very specific reuse reason.

Preferred:
```kotlin
@Composable
fun TinyButton(
    onClick: () -> Unit,
)
```

### Mutable actions holder

Bad:
```kotlin
data class ScreenActions(
    var onRetry: () -> Unit,
)
```

Why it is bad:
- Mutable function references make the contract harder to reason about.

### Mixing state into actions

Bad:
```kotlin
data class ScreenActions(
    val canRetry: Boolean,
    val onRetry: () -> Unit,
)
```

Why it is bad:
- `canRetry` belongs in state, not actions.

### God-object actions

Bad:
```kotlin
data class AppActions(
    val onBack: () -> Unit,
    val onRetry: () -> Unit,
    val onSignOut: () -> Unit,
    val onDeleteAccount: () -> Unit,
    val onOpenChat: () -> Unit,
    val onSelectTheme: (Theme) -> Unit,
    // many more unrelated events
)
```

Why it is bad:
- The grouping is too broad to be meaningful.
- Use screen or feature boundaries.

### Nested actions without real structure

Bad:
```kotlin
data class ScreenActions(
    val header: HeaderActions,
    val body: BodyActions,
    val footer: FooterActions,
)
```

When each nested class only contains one trivial callback, this often harms readability rather than helping it.

## Decision checklist

Use an `Actions` class if most answers are yes:
- Are there 3 or more callbacks?
- Are the callbacks part of one screen or feature contract?
- Would the function signature become easier to scan with one `actions` parameter?
- Will the callback group be forwarded through multiple composables?
- Is there already a related `State` type that suggests a matching `Actions` type?
- Would this make ViewModel or route wiring more coherent?

Keep direct callbacks if most answers are yes:
- Does the composable only have 1 or 2 callbacks?
- Is the component tiny and generic?
- Would a new type add more ceremony than clarity?
- Are the callbacks simple and obvious at the call site already?

## Before and after examples

### Example 1: Screen-level API

Before:

```kotlin
@Composable
fun CharacterCreationScreen(
    state: CharacterCreationState,
    onBack: () -> Unit,
    onNameChanged: (String) -> Unit,
    onClassSelected: (CharacterClass) -> Unit,
    onRandomizeAvatar: () -> Unit,
    onCreateCharacter: () -> Unit,
    modifier: Modifier = Modifier,
)
```

After:

```kotlin
data class CharacterCreationActions(
    val onBack: () -> Unit,
    val onNameChanged: (String) -> Unit,
    val onClassSelected: (CharacterClass) -> Unit,
    val onRandomizeAvatar: () -> Unit,
    val onCreateCharacter: () -> Unit,
)

@Composable
fun CharacterCreationScreen(
    state: CharacterCreationState,
    actions: CharacterCreationActions,
    modifier: Modifier = Modifier,
)
```

### Example 2: Nested feature sections

Before:

```kotlin
@Composable
fun ModelSetupScreen(
    state: ModelSetupState,
    onModelSelected: (String) -> Unit,
    onEndpointChanged: (String) -> Unit,
    onApiKeyChanged: (String) -> Unit,
    onTestConnection: () -> Unit,
    onSave: () -> Unit,
    onCancel: () -> Unit,
)
```

After:

```kotlin
data class ModelSetupActions(
    val form: ModelSetupFormActions,
    val footer: ModelSetupFooterActions,
)

data class ModelSetupFormActions(
    val onModelSelected: (String) -> Unit,
    val onEndpointChanged: (String) -> Unit,
    val onApiKeyChanged: (String) -> Unit,
    val onTestConnection: () -> Unit,
)

data class ModelSetupFooterActions(
    val onSave: () -> Unit,
    val onCancel: () -> Unit,
)

@Composable
fun ModelSetupScreen(
    state: ModelSetupState,
    actions: ModelSetupActions,
    modifier: Modifier = Modifier,
)
```

### Example 3: Tiny component that should stay simple

Preferred direct callback:

```kotlin
@Composable
fun RetryChip(
    onRetry: () -> Unit,
    modifier: Modifier = Modifier,
)
```

Not preferred:

```kotlin
data class RetryChipActions(
    val onRetry: () -> Unit,
)

@Composable
fun RetryChip(
    actions: RetryChipActions,
    modifier: Modifier = Modifier,
)
```

## Edge cases

### Composable has unrelated callbacks

If callbacks belong to different concerns, do not force them into one object just because there are many. Split by child section or keep direct callbacks where that reads better.

### Shared reusable components

For highly reusable library-style components, prefer the smallest clear API. A reusable primitive often benefits from direct callbacks even when app screens benefit from `Actions`.

### Navigation-only callbacks

If a screen mostly emits navigation intents, an `Actions` contract is still appropriate when there are enough of them. Do not create a generic `NavigationActions`; keep the feature domain in the name.

### Optional actions

If an interaction is truly optional, nullable callbacks may be acceptable:

```kotlin
data class HelpCardActions(
    val onOpenDocs: (() -> Unit)?,
    val onDismiss: () -> Unit,
)
```

But prefer explicit state-driven visibility over large sets of nullable callbacks.

### Preview usage

For previews, `Actions` objects are often convenient because they provide one place for stub handlers:

```kotlin
private val PreviewActions = DiceResultActions(
    onRollDice = {},
    onResetResult = {},
    onOpenHistory = {},
)
```

### Compose Multiplatform

The same contract pattern applies in Compose Multiplatform. This is architectural guidance, not Android-specific framework guidance.

## Output expectations for the coding agent

When applying this skill:
- Refactor callback-heavy composable APIs into a domain-specific `Actions` data class when it improves readability.
- Preserve behavior exactly unless the user asked for a functional change.
- Prefer the naming rule `State` -> `Actions` when a paired state class exists.
- Prefer parameter order `state`, `actions`, `modifier`.
- Keep `Actions` immutable with `val` properties.
- Use `data class` by default.
- Mention `@Immutable` only if the codebase already uses Compose stability annotations.
- Add `remember` for actions objects only when it is justified by captured inputs or local project practice.
- Avoid introducing `Actions` wrappers for tiny 1-2 callback composables unless there is a strong reuse reason.
- Use nested action classes only when they clarify large screen structure.
- Prefer direct method references from state holders or ViewModels when readable.
- Do not introduce generic names like `Actions`, `Handlers`, or `Callbacks`.
- Keep the result idiomatic to the existing project style.

If refactoring:
1. Create the new `...Actions` type near the related screen contract or in the location already used by the project.
2. Update the composable signature.
3. Update all call sites.
4. Simplify callback forwarding through parent composables.
5. Verify no state fields were accidentally moved into the actions holder.
6. Keep the public API more readable than before.

Success criteria:
- The resulting composable contract is easier to scan.
- The event API is grouped by feature meaning.
- The code does not add unnecessary ceremony.
- Readability improves at both declaration and call sites.
