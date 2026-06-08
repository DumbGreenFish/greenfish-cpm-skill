# Greenfish Compose Multiplatform Skill

*Kotlin Multiplatform development skill.*

![Kotlin](https://img.shields.io/badge/Kotlin-7F52FF?style=flat-square&logo=kotlin&logoColor=white)
![Compose Multiplatform](https://img.shields.io/badge/Compose_Multiplatform-4285F4?style=flat-square&logo=jetpackcompose&logoColor=white)
![Material 3](https://img.shields.io/badge/Material_3-757575?style=flat-square&logo=materialdesign&logoColor=white)
![DI: Koin](https://img.shields.io/badge/DI-Koin-F76D3C?style=flat-square)

## Overview

This skill is a practical, everyday cheatsheet for building Compose Multiplatform apps, put together through trial and error. It focuses on keeping things organized by sticking to a straightforward MVI setup, putting one class in each file, and using Koin annotations to skip manual dependency wiring. It also includes some basic sanity checks to keep the code clean, like moving magic numbers into token objects, keeping mock data out of production code, and handling adaptive layouts without overcomplicating things.

## Tech stack

- **Kotlin Multiplatform** - shared logic and UI across targets.
- **Compose Multiplatform** + **Material 3** - declarative, themeable UI.
- **Koin** (annotations + KSP) - compile-time-safe dependency injection.
- **MVI** - one unidirectional state flow per screen.

## Getting started

Clone the project into your "~/.claude/skills" or "C:\\Users\{Your user}\.claude\skills" directory for Claude Code. 
If you use a different LLM client, you should specify its own path.

## Conventions

The codebase follows a fixed set of conventions so every screen looks and behaves consistently:

- One function or class per file.
- No magic numbers in UI - every `dp`, alpha, and duration is a named token.
- No mock data in production.
- Adaptive navigation: bottom bar below 600 dp, sidebar at 600 dp and up.

> [!NOTE]
> The complete, authoritative rules - with rationale and examples - live in [`SKILL.md`](SKILL.md).

## Resources

- [Compose Multiplatform](https://www.jetbrains.com/compose-multiplatform/)
- [Koin Annotations](https://insert-koin.io/)
- [Material 3](https://m3.material.io/)

## Contributing

Found a bug or have a suggestion? Feel free to open an issue or submit a pull request!
