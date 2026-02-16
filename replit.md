# replit.md

## Overview

This is a cross-platform mobile/web application built with **Expo** (React Native). It was scaffolded using `create-expo-app` and uses the default Expo starter template with file-based routing via `expo-router`. The app currently contains a two-tab layout (Home and Explore) with example content demonstrating Expo's capabilities. It targets iOS, Android, and web platforms from a single codebase.

## User Preferences

Preferred communication style: Simple, everyday language.

## System Architecture

### Frontend Architecture

- **Framework:** Expo SDK 54 with React Native 0.81 and React 19
- **Routing:** File-based routing via `expo-router` v6. Routes are defined by the file structure inside the `app/` directory. The `(tabs)` folder creates a tab-based navigation group.
- **Navigation:** `@react-navigation/bottom-tabs` powers the tab bar, with a Stack navigator at the root level for screen management.
- **Styling:** Standard React Native `StyleSheet` API. No CSS-in-JS or utility framework (like NativeWind) is used.
- **Theming:** Built-in light/dark mode support using `useColorScheme` hook and a `Colors` constant file. `ThemedText` and `ThemedView` wrapper components automatically apply theme colors.
- **Animations:** `react-native-reanimated` v4 is used for animations (e.g., parallax scroll header, waving hand emoji).
- **Icons:** Platform-adaptive icon system — SF Symbols on iOS (`expo-symbols`), Material Icons on Android/web (`@expo/vector-icons`). The mapping between symbol names is in `components/ui/IconSymbol.tsx`.
- **TypeScript:** Strict mode enabled. Path alias `@/*` maps to the project root.

### Directory Structure

```
app/                    # File-based routes (expo-router)
  _layout.tsx           # Root layout (Stack navigator + theme provider)
  +not-found.tsx        # 404 fallback screen
  (tabs)/               # Tab group
    _layout.tsx         # Tab navigator configuration
    index.tsx           # Home tab
    explore.tsx         # Explore tab
components/             # Reusable UI components
  ui/                   # Platform-specific UI primitives (icons, tab bar background)
constants/              # App-wide constants (Colors)
hooks/                  # Custom hooks (useColorScheme, useThemeColor)
assets/                 # Static assets (images, fonts)
scripts/                # Utility scripts (reset-project)
```

### Key Design Decisions

1. **File-based routing over manual navigation config:** Expo Router was chosen to simplify route management. Routes are automatically derived from the filesystem, reducing boilerplate. Typed routes are enabled via `experiments.typedRoutes` in `app.json`.

2. **Platform-specific component files:** Components use the `.ios.tsx` / `.tsx` extension pattern (e.g., `IconSymbol.ios.tsx` vs `IconSymbol.tsx`) so Metro bundler automatically picks the right implementation per platform. This avoids runtime platform checks.

3. **Web output as static:** The web build uses Metro bundler with static output (`app.json` → `web.output: "static"`), enabling SSG-compatible web deployment.

4. **No backend:** This is currently a frontend-only project. There is no server, API, or database. If a backend is needed later, it would need to be added separately.

5. **No state management library:** The app uses React's built-in `useState` and props for state. No Redux, Zustand, or other state management is in place.

### Running the App

- `npx expo start` — Starts the Expo dev server
- `npx expo start --web` — Opens the web version
- The app runs on Replit primarily as a web app using `expo start --web`
- Entry point is `expo-router/entry` (configured in `package.json` → `"main"`)

## External Dependencies

### Core Framework
- **Expo SDK 54** — Managed React Native framework
- **React 19.1** / **React Native 0.81** — UI runtime
- **expo-router 6** — File-based routing

### UI & Navigation
- `@react-navigation/bottom-tabs` — Tab navigator
- `@react-navigation/native` — Navigation core
- `react-native-reanimated` — Performant animations
- `react-native-gesture-handler` — Touch gesture system
- `react-native-screens` — Native screen containers
- `react-native-safe-area-context` — Safe area insets

### Expo Modules
- `expo-image` — Optimized image component
- `expo-font` — Custom font loading (SpaceMono)
- `expo-haptics` — Haptic feedback on iOS tab presses
- `expo-blur` — iOS blur effect for tab bar
- `expo-splash-screen` — Splash screen management
- `expo-symbols` — SF Symbols on iOS
- `expo-web-browser` — In-app browser for external links
- `expo-status-bar` — Status bar configuration
- `@expo/vector-icons` — Material Icons fallback for Android/web

### Web Support
- `react-native-web` — React Native components compiled for web
- `react-dom` — React web renderer
- `@expo/metro-runtime` — Metro bundler web runtime

### Dev Tools
- TypeScript 5.9
- ESLint with `eslint-config-expo`
- Jest with `jest-expo` preset
- Babel (required by Metro)

### External Services
- None currently configured. No databases, APIs, authentication, or third-party service integrations are present.