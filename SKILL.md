# Compose Multiplatform — Working Conventions

## Architecture: MVI

Every screen follows this structure under `ui/<feature>/`:

- `*State` — immutable data class; initializes with empty/default values, never hardcoded sample data
- `*Intent` — sealed class for user actions
- `*ViewModel` — holds `StateFlow<*State>`, processes intents; annotated `@KoinViewModel`
- `*View` — Composable that observes state and dispatches intents

One function (or class) per file. No exceptions.

## Dependency Injection (Koin)

Use annotation-based Koin: `@Module`, `@KoinViewModel`, component scanning on the root package. No manual `module { }` blocks or `factory`/`single` wiring unless annotations are unavailable. KSP generates type-safe builders at compile time.

## Theme and Content Color

Never wrap a theme root in `Surface` just to set content color. Use `CompositionLocalProvider`:

```kotlin
CompositionLocalProvider(LocalContentColor provides colorScheme.onSurface) {
    content()
}
```

`Surface` adds an implicit background layer. Use it only when you explicitly want a background drawn. `CompositionLocalProvider` is the right primitive for propagating color context.

## Naming by Context

Do not repeat what the surrounding package or class already says. In package `navigation.animation`, a function called `navAnimationColors()` is redundant — `animationColors()` is enough. Before naming a symbol, check what the enclosing scope already implies; only add a qualifier if the name would be genuinely ambiguous without it.

## Design Tokens

All animation constants (durations, spring stiffness, press scales) go in a single `*Animation` object. All extended color tokens go in a single `*Colors` object. Never inline magic numbers in UI code — if a value is used once, it still belongs in the token object.

M3 state layer opacity: 8% hover, 12% press. Active nav items: `lerp(containerColor, onContainerColor, stateAlpha)`. Inactive: `onSurface.copy(alpha = stateAlpha)`.

## No Mock Data in Production

`State` classes and `ViewModel`s initialize with empty collections and default values. Hardcoded sample or demo data belongs in test helpers or `@Preview` parameters only — never inline in production `State` or `ViewModel` files. The app is a working product, not a prototype.

## No Magic Numbers in UI

Every numeric constant used in layout, animation, or color logic must be named. No `Modifier.padding(12.dp)` without a token; no `alpha = 0.08f` without a constant. Name it where it lives conceptually (animation object, color object, shape object).

## Adaptive Layout

- Compact (width < 600 dp): bottom navigation
- Wide (width ≥ 600 dp): sidebar navigation + vertical divider + content in a `Row`
- Both branches use `Scaffold` so window insets are handled automatically
- `currentWindowAdaptiveInfo()` has no `commonMain` alternative yet; suppress deprecation where needed

## Android Specifics

Lock phones (smallestScreenWidthDp < 600) to portrait in `Activity.onCreate()`. Tablets and foldables rotate freely.

## Antipatterns

- Do not use `Surface` as a theme wrapper for content color propagation
- Do not put mock/sample data inline in `State` or `ViewModel`
- Do not repeat package/class context in symbol names
- Do not use magic numbers anywhere in UI code — no inline dp, alpha, duration values
- Do not wire Koin manually when annotations are available
- Do not write multiple functions or classes in a single file
