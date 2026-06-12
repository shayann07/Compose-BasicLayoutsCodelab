# Basic Layouts in Compose Codelab

Solved MySoothe sample for the Android [Basic Layouts in Compose codelab](https://developer.android.com/codelabs/jetpack-compose-layouts), demonstrating reusable and adaptive Jetpack Compose layouts.

![MySoothe home screen](ScreenShot/Home%20Screen.jpg)

## Overview

This repository contains an educational wellness-themed interface built with Kotlin, Jetpack Compose, and Material 3. The home screen combines a search field, a horizontally scrolling set of circular activity images, a two-row horizontal grid of favorite collections, and adaptive navigation.

Compact windows use a bottom navigation bar. Medium and expanded windows switch to a navigation rail beside the same scrollable home content. The code also includes focused previews for each component and complete portrait and landscape layouts.

## Concepts Demonstrated

- Compose modifiers, alignment, spacing, and baseline padding
- Reusable composables and slot APIs
- `LazyRow` and `LazyHorizontalGrid`
- Material 3 `Surface`, `TextField`, `Scaffold`, navigation bar, and navigation rail
- Adaptive UI with Material window size classes
- Compact portrait and wider landscape layouts
- Custom shapes, typography, bundled fonts, and light/dark color schemes
- Resource-backed strings and drawable data lists
- Component-level and screen-level Compose previews
- Spotless and ktlint formatting configuration

## App Layout

1. `MainActivity` calculates the current `WindowSizeClass`.
2. Compact widths render `MySootheAppPortrait` with a bottom bar.
3. Medium and expanded widths render `MySootheAppLandscape` with a navigation rail.
4. `HomeScreen` vertically composes the search field, body-alignment row, and favorite-collection grid.
5. Static drawable/string pairs provide the visible wellness categories.

## Project Structure

```text
app/src/main/
|-- java/com/codelab/basiclayouts/
|   |-- MainActivity.kt   # Layout components, adaptive shell, and previews
|   `-- ui/theme/         # Colors, shapes, fonts, and Material theme
|-- res/
|   |-- drawable/         # Wellness photography and launcher assets
|   |-- font/             # Kulim Park and Lato fonts
|   `-- values/           # Strings, colors, and Android themes
`-- AndroidManifest.xml

ScreenShot/               # Repository screenshot
ASSETS_LICENSE            # Image attribution details
LICENSE                   # Apache License 2.0
```

## Getting Started

### Prerequisites

- Android Studio with a JDK compatible with Android Gradle Plugin 8.9.0
- Android SDK 35
- An Android 6.0 (API 23) or newer device or emulator

### Build

```bash
git clone https://github.com/shayann07/Compose-BasicLayoutsCodelab.git
cd Compose-BasicLayoutsCodelab
./gradlew assembleDebug
```

On Windows PowerShell, use `./gradlew.bat assembleDebug`.

## Current Status and Limitations

- This is a solved UI codelab, not a functional wellness product.
- The search field is read-only because its value and change callback are fixed.
- Home and Profile navigation items have empty click handlers and do not change screens.
- Activity and collection cards are static display components with no click actions.
- No persistence, networking, navigation graph, business logic, or tests were found.
- Machine-specific `local.properties`, Gradle cache files, and a generated build report are tracked in the repository.

## License and Assets

The source is licensed under the [Apache License 2.0](LICENSE). Image-specific attribution and licensing details are documented in [ASSETS_LICENSE](ASSETS_LICENSE).
