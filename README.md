# Basic Layouts in Jetpack Compose (MySoothe)

[![Platform](https://img.shields.io/badge/Platform-Android-3DDC84?style=for-the-badge&logo=android&logoColor=white)](https://developer.android.com)
[![Language](https://img.shields.io/badge/Kotlin-2.0+-7F52FF?style=for-the-badge&logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![UI Framework](https://img.shields.io/badge/Jetpack%20Compose-BOM%202025.02.00-4285F4?style=for-the-badge&logo=jetpackcompose&logoColor=white)](https://developer.android.com/jetpack/compose)
[![Design](https://img.shields.io/badge/Material%203-Window%20Size%20Class-FF6F00?style=for-the-badge&logo=materialdesign&logoColor=white)](https://m3.material.io)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg?style=for-the-badge)](LICENSE)

> A polished implementation of Google's **"Basic Layouts in Compose"** codelab showcasing the **MySoothe** wellness app with adaptive layouts, custom slot APIs, LazyRows, LazyHorizontalGrids, and dynamic NavigationRail/BottomNavigation.

---

## 📖 Overview

The **Compose-BasicLayoutsCodelab** project is an idiomatic demonstration of modern declarative UI engineering using Jetpack Compose and Material Design 3. Built upon Google's reference design for the **MySoothe** holistic wellness application, this repository illustrates how to translate complex UI mocks into modular, testable, and adaptive Compose components.

### 🎯 Educational & Architectural Objectives
- **Declarative Layout Composition**: Transition from legacy View hierarchies to composable layout primitives (`Row`, `Column`, `Box`, `Surface`).
- **Slot API Architecture**: Implement reusable container composables (`HomeSection`) leveraging trailing lambda slot patterns.
- **Performant Lazy Collections**: Handle horizontal scrolling lists (`LazyRow`) and multi-row horizontal grids (`LazyHorizontalGrid`) with efficient recycling.
- **Adaptive Form Factors**: Dynamically switch between **Portrait (Bottom Navigation Bar)** and **Landscape / Tablet (Navigation Rail)** based on `WindowSizeClass`.
- **Material 3 Theming & Typography**: Utilize custom color schemes, shapes, and font families (`Kulim Park` and `Lato`) with baseline padding precision.

---

## 🏗️ Architecture & Component Flow

```mermaid
graph TD
    classDef comp fill:#2D3748,stroke:#4FD1C5,stroke-width:2px,color:#fff;
    classDef state fill:#1A365D,stroke:#63B3ED,stroke-width:2px,color:#fff;
    classDef ui fill:#234E52,stroke:#38B2AC,stroke-width:2px,color:#fff;

    MainActivity["MainActivity<br/>(calculateWindowSizeClass)"]:::state --> WindowDecision{"WindowWidthSizeClass"}

    WindowDecision -->|Compact (Portrait)| PortraitApp["MySootheAppPortrait()"]:::comp
    WindowDecision -->|Medium / Expanded (Landscape)| LandscapeApp["MySootheAppLandscape()"]:::comp

    PortraitApp --> ScaffoldUI["Scaffold(bottomBar = SootheBottomNavigation)"]:::ui
    ScaffoldUI --> HomeScreenP["HomeScreen(Modifier.verticalScroll)"]:::ui

    LandscapeApp --> RowUI["Row { SootheNavigationRail(), HomeScreen() }"]:::ui
    RowUI --> HomeScreenL["HomeScreen(Modifier.verticalScroll)"]:::ui

    HomeScreenP --> SearchBar["SearchBar (TextField + Icons.Default.Search)"]:::comp
    HomeScreenP --> Section1["HomeSection('Align your body')"]:::comp
    HomeScreenP --> Section2["HomeSection('Favorite collections')"]:::comp

    Section1 --> LazyRowUI["LazyRow -> AlignYourBodyElement(Image + Baseline Text)"]:::ui
    Section2 --> LazyGridUI["LazyHorizontalGrid (2 Fixed Rows) -> FavoriteCollectionCard"]:::ui
```

---

## ✨ Core Concepts & Patterns Demonstrated

### 1. Adaptive Screen Form Factors
Using `androidx.compose.material3.windowsizeclass.WindowSizeClass`, the app detects device orientation and window width dynamically:
- **Compact Width (Phone Portrait)**: Renders a `Scaffold` hosting a `SootheBottomNavigation` (`NavigationBar` & `NavigationBarItem`).
- **Medium / Expanded Width (Phone Landscape, Foldable, Tablet)**: Renders a split `Row` hosting a persistent `SootheNavigationRail` (`NavigationRail` & `NavigationRailItem`).

```kotlin
@Composable
fun MySootheApp(windowSize: WindowSizeClass) {
    when (windowSize.widthSizeClass) {
        WindowWidthSizeClass.Compact -> MySootheAppPortrait()
        WindowWidthSizeClass.Medium, WindowWidthSizeClass.Expanded -> MySootheAppLandscape()
        else -> MySootheAppPortrait()
    }
}
```

### 2. Slot API Pattern (`HomeSection`)
Modular container component decoupling section header typography and baseline padding from nested child composables:
```kotlin
@Composable
fun HomeSection(
    @StringRes title: Int,
    modifier: Modifier = Modifier,
    content: @Composable () -> Unit
) {
    Column(modifier = modifier) {
        Text(
            text = stringResource(title),
            style = MaterialTheme.typography.titleMedium,
            modifier = Modifier
                .paddingFromBaseline(top = 40.dp, bottom = 16.dp)
                .padding(horizontal = 16.dp)
        )
        content()
    }
}
```

### 3. Circular Cropped Elements & Baseline Spacing
`AlignYourBodyElement` uses `Modifier.clip(CircleShape)` and `ContentScale.Crop` paired with `paddingFromBaseline` to adhere strictly to Material Design typographic spacing rules.

### 4. Dual-Row Horizontal Lazy Grid
`LazyHorizontalGrid` with `GridCells.Fixed(2)` displays curated wellness cards with balanced horizontal and vertical spacing (`spacedBy(16.dp)`).

---

## 📱 Key Components & Project Structure

```
Compose-BasicLayoutsCodelab/
├── app/
│   ├── src/main/java/com/codelab/basiclayouts/
│   │   ├── MainActivity.kt                # Primary Activity, Layout Composables & Previews
│   │   └── ui/theme/
│   │       ├── Color.kt                   # Taupe & Sand Material 3 color palettes
│   │       ├── Shape.kt                   # Corner radius & surface geometry
│   │       ├── Theme.kt                   # MySootheTheme dark/light configuration
│   │       └── Type.kt                    # Typography using Kulim Park & Lato fonts
│   ├── src/main/res/
│   │   ├── drawable/                      # Body alignment & meditation asset images
│   │   ├── font/                          # TTF font files (Kulim Park, Lato)
│   │   └── values/                        # Color definitions and localized strings
│   └── build.gradle                       # Gradle dependencies, Compose BOM & plugins
```

### Composable Function Index

| Composable | Description | Key Modifiers / APIs Used |
|---|---|---|
| `SearchBar` | Search input bar with lead icon | `TextField`, `heightIn(min = 56.dp)`, `fillMaxWidth` |
| `AlignYourBodyElement` | Circular wellness category card | `Image`, `clip(CircleShape)`, `paddingFromBaseline` |
| `AlignYourBodyRow` | Horizontally scrollable category list | `LazyRow`, `Arrangement.spacedBy(8.dp)` |
| `FavoriteCollectionCard` | Rounded card with image + title | `Surface(shape = medium)`, `Row`, `size(80.dp)` |
| `FavoriteCollectionsGrid` | Two-row horizontal grid of collections | `LazyHorizontalGrid`, `GridCells.Fixed(2)` |
| `HomeSection` | Slot-based section container | Slot API (`content: @Composable () -> Unit`) |
| `HomeScreen` | Full vertical scrollable dashboard | `Column(Modifier.verticalScroll(rememberScrollState()))` |
| `SootheBottomNavigation` | Material 3 bottom app navigation bar | `NavigationBar`, `NavigationBarItem` |
| `SootheNavigationRail` | Material 3 vertical side rail | `NavigationRail`, `NavigationRailItem` |
| `MySootheApp` | Responsive root entry point | `WindowSizeClass`, `calculateWindowSizeClass` |

---

## 🛠️ Technology Stack Matrix

| Layer / Category | Technology / Library | Version / Details |
|---|---|---|
| **Language** | Kotlin | 2.0+ |
| **UI Framework** | Jetpack Compose | BOM `2025.02.00` |
| **Design System** | Material Design 3 | `androidx.compose.material3:material3` |
| **Adaptive Layouts** | Material 3 Window Size Class | `material3-window-size-class:1.3.1` |
| **Iconography** | Material Icons Extended | `material-icons-extended` |
| **Architecture** | Declarative Slot APIs | Slot pattern & stateless composables |
| **Build System** | Gradle (Kotlin DSL / Groovy) | AGP 8.8+, Target SDK 33, Compile SDK 35 |
| **Testing** | Compose UI Test & JUnit4 | `androidx.compose.ui:ui-test-junit4` |

---

## 🚀 Getting Started

### Prerequisites
- **Android Studio Ladybug (2024.2.1+)** or newer.
- **Android SDK 35** with Build-Tools installed.
- **JDK 17 or JDK 21**.

### Build & Run
1. **Clone repository**:
   ```bash
   git clone https://github.com/shayann07/Compose-BasicLayoutsCodelab.git
   cd Compose-BasicLayoutsCodelab
   ```
2. **Open in Android Studio**: Select `File > Open...` and choose the project directory.
3. **Gradle Sync**: Allow Android Studio to sync dependencies and the Compose BOM.
4. **Deploy**: Select an Android Virtual Device (AVD) or physical device running Android 6.0 (API 23) or higher and click **Run (Shift + F10)**.
5. **Interactive Previews**: Open `MainActivity.kt` and toggle Split / Design mode to preview individual components and multi-device layouts (`MySoothePortraitPreview` & `MySootheLandscapePreview`).

---

## 📄 License

This project is licensed under the [Apache License 2.0](LICENSE) — based on Google's Jetpack Compose Codelab sample code.
