# Phase 1: Architecture Review 🏗️

> **Objective:** Analyze the project's foundational structure, dependencies, design patterns,
> and code organization. Architectural problems are the ROOT CAUSE of most bugs — a bad
> architecture makes bugs inevitable.

---

## 🔍 PRE-CHECK: Project Discovery

Before analyzing architecture, gather this information:

### 1.1 Project Structure Map
```
ACTION: List the ENTIRE project directory tree (at least 3 levels deep)
OUTPUT: Complete tree showing all folders and files
LOOK FOR:
  - Is there a clear separation of concerns? (views, models, services, etc.)
  - Are test directories present? (tests/, unit_tests/, integration_tests/)
  - Is there a consistent naming convention?
  - Are there configuration files for different environments? (dev, staging, prod)
```

### 1.2 Dependency Audit
```
ACTION: Read the dependency file (*.csproj / package.json / Cargo.toml / pom.xml / CMakeLists.txt / pubspec.yaml / Gemfile / requirements.txt)
CHECK EACH DEPENDENCY FOR:
  ❑ Is it still maintained? (last update within 12 months)
  ❑ Are there known vulnerabilities? (check version numbers)
  ❑ Is the version pinned or floating?
  ❑ Are there conflicting dependencies?
  ❑ Are dev dependencies properly separated from production dependencies?
  ❑ Is the minimum runtime/SDK version appropriate?
  ❑ Are there unnecessary/unused dependencies?
  ❑ Are native/platform-specific dependencies properly handled?
```

### 1.3 Entry Point Analysis
```
ACTION: Read the main entry point file:
  - Electron: main.js/main.ts
  - WPF/WinForms: App.xaml.cs / Program.cs
  - Qt: main.cpp / main.py
  - JavaFX: Main class extending Application
  - Tauri: main.rs + App.tsx
  - Flutter Desktop: main.dart
  - SwiftUI: @main struct

CHECK:
  ❑ Is initialization properly ordered? (services before UI)
  ❑ Are there blocking calls on the main/UI thread during startup?
  ❑ Is error handling set up at the global level?
  ❑ Are environment configurations properly loaded?
  ❑ Is dependency injection / service locator configured?
  ❑ Is single-instance enforcement implemented? (prevent multiple app copies)
  ❑ Are command-line arguments parsed safely?
```

---

## 📋 ARCHITECTURE CHECKS

### CHECK A1: Design Pattern Consistency
```
WHAT TO LOOK FOR:
  - Does the project follow a consistent architecture pattern?
    □ MVVM (Model-View-ViewModel) — WPF, Avalonia, SwiftUI
    □ MVC (Model-View-Controller)
    □ MVP (Model-View-Presenter)
    □ Clean Architecture (Domain/Data/Presentation layers)
    □ Flux/Redux — Electron
    □ Component-based — Qt, JavaFX
    □ ELM Architecture — Tauri

  - Is the pattern applied CONSISTENTLY across ALL features?
    (Common bug: Feature A uses MVVM, Feature B uses spaghetti code)

  - Are layer boundaries respected?
    (Common bug: View layer directly accessing database, skipping ViewModel)

CITATION REQUIRED: Show at least 2 examples of pattern usage (or violation)
```

### CHECK A2: Folder Structure Convention
```
EXPECTED STRUCTURES BY FRAMEWORK:

Electron:
  src/
  ├── main/           (main process — Node.js)
  │   ├── index.ts    (entry point)
  │   └── ipc/        (IPC handlers)
  ├── renderer/       (renderer process — Chromium)
  │   ├── components/ (UI components)
  │   ├── pages/      (screen-level views)
  │   └── store/      (state management)
  ├── shared/         (shared types/utils)
  └── preload/        (preload scripts)

WPF / WinForms (.NET):
  ProjectName/
  ├── Models/
  ├── Views/          (XAML + code-behind)
  ├── ViewModels/
  ├── Services/
  ├── Data/           (repositories, DB context)
  ├── Helpers/
  └── Resources/      (images, strings, styles)

Qt (C++):
  src/
  ├── ui/             (.ui files)
  ├── widgets/        (custom widgets)
  ├── models/
  ├── controllers/
  ├── services/
  └── resources/      (.qrc files)

Tauri:
  src/                (frontend — React/Vue/Svelte)
  src-tauri/
  ├── src/
  │   ├── main.rs
  │   └── commands/   (Tauri commands)
  ├── Cargo.toml
  └── tauri.conf.json

JavaFX:
  src/main/
  ├── java/com/example/
  │   ├── controllers/
  │   ├── models/
  │   ├── services/
  │   └── App.java
  └── resources/
      ├── fxml/       (FXML layouts)
      ├── css/
      └── images/

RED FLAGS:
  🔴 All files dumped in one folder
  🔴 No separation between UI and business logic
  🔴 Models defined inside UI files
  🔴 Database calls made directly from UI event handlers
  🔴 No dedicated folder for services/repositories
  🔴 Main process and renderer process code mixed (Electron)
```

### CHECK A3: Dependency Injection
```
WHAT TO CHECK:
  ❑ Is there a DI framework? (Microsoft.Extensions.DI / Autofac / Dagger / Spring / get_it)
  ❑ Are dependencies injected or hardcoded?
  ❑ Are singletons used appropriately? (not overused)
  ❑ Can dependencies be mocked for testing?

COMMON BUGS:
  🐛 Service creates its own database connection instead of receiving one (untestable)
  🐛 Singleton holding UI state across windows (stale data)
  🐛 Tight coupling between unrelated modules
  🐛 Circular dependencies between services
  🐛 Lazy singletons that throw on first access without error handling

CITATION REQUIRED: Show how dependencies are created and provided
```

### CHECK A4: Window & Dialog Architecture
```
WHAT TO CHECK:
  ❑ Is window management centralized? (window factory/manager pattern)
  ❑ Are dialogs modal when they should be? (blocking parent interaction)
  ❑ Is the main window lifecycle properly managed?
  ❑ Are child windows properly disposed when closed?
  ❑ Is there a mechanism for inter-window communication?
  ❑ Are window positions/sizes persisted across sessions?

COMMON BUGS:
  🐛 Opening same dialog multiple times (no guard)
  🐛 Memory leak from undisposed child windows
  🐛 Parent window frozen because dialog blocks UI thread
  🐛 Data not refreshed in parent after dialog save
  🐛 Window opens off-screen on multi-monitor setups
  🐛 System tray icon not cleaned up on app exit

CITATION REQUIRED: Show window creation and lifecycle management
```

### CHECK A5: Configuration & Environment Management
```
WHAT TO CHECK:
  ❑ Are there separate configs for dev/staging/production?
  ❑ Are API base URLs environment-specific?
  ❑ Are secrets properly handled? (NOT hardcoded, see Security Phase)
  ❑ Is the app build configuration properly set up? (Debug vs Release)
  ❑ Are feature flags implemented?
  ❑ Where are user preferences stored? (registry, config file, SQLite)

RED FLAGS:
  🔴 Production API URL hardcoded in source code
  🔴 No environment switching mechanism
  🔴 Debug flags left enabled in release builds
  🔴 Console.WriteLine / console.log / print statements everywhere
  🔴 Same config used for all environments
  🔴 Config files with world-readable permissions
```

### CHECK A6: Code Modularity & Coupling
```
WHAT TO CHECK:
  ❑ Can a feature be modified without touching other features?
  ❑ Are there shared components that could break multiple features if changed?
  ❑ Is the codebase monolithic or modular?
  ❑ Are there clear boundaries between modules?
  ❑ Is code duplication minimized?

MEASUREMENT:
  - Count imports: If a single file imports from 10+ different modules,
    it's a coupling red flag
  - Count file length: Files over 500 lines likely violate single responsibility
  - Count class methods: Classes with 20+ methods are doing too much

CITATION REQUIRED: Show the most coupled file (most imports) and explain why
```

### CHECK A7: Process & Thread Architecture
```
WHAT TO CHECK:
  ❑ Is the main/UI thread kept free of heavy computation?
  ❑ Is there a proper threading model? (thread pool, async/await, worker threads)
  ❑ Are background tasks properly managed? (cancellation, progress reporting)
  ❑ Is inter-process communication (IPC) properly structured? (Electron main↔renderer)
  ❑ Are there proper synchronization mechanisms? (mutexes, semaphores, locks)

COMMON BUGS:
  🐛 Heavy file I/O on the UI thread (app freezes)
  🐛 Database queries on the UI thread
  🐛 No cancellation token for long-running operations
  🐛 Race condition when multiple threads access shared state
  🐛 Electron: renderer process accessing Node.js APIs directly (security risk)

CITATION REQUIRED: Show threading/async pattern and potential issues
```

---

## 🚦 PHASE 1 GATE — MANDATORY CHECKLIST

You MUST complete ALL items before proceeding to Phase 2:

```
PHASE 1 GATE CHECKLIST:
  □ [A1] Design pattern identified and consistency verified
  □ [A2] Folder structure analyzed with RED FLAGS noted
  □ [A3] Dependency injection reviewed
  □ [A4] Window/dialog architecture mapped
  □ [A5] Configuration/environment management checked
  □ [A6] Code modularity and coupling assessed
  □ [A7] Process/thread architecture reviewed
  □ Project Structure Map produced
  □ Dependency Audit completed
  □ Entry Point Analysis completed
  □ Minimum 8 code citations provided
  □ Files examined list produced
```

### Gate Report Format:
```
═══════════════════════════════════════════════════════
  ✅ PHASE 1 COMPLETE: Architecture Review
  📊 Findings: [X] Critical | [Y] High | [Z] Medium | [W] Low
  📋 Gate Status: [PASSED/FAILED] ([checked]/12 items)

  🏗️ Architecture Health Score: [X/10]
  Pattern: [Detected Pattern]
  Modularity: [High/Medium/Low]
  Dependency Health: [Good/Warning/Critical]
═══════════════════════════════════════════════════════
```

### ⛔ STOP POINT
**DO NOT proceed to Phase 2 until:**
1. All gate items are checked
2. Report is output to user
3. User acknowledges (or you've presented findings and are continuing the review)

---

## 🔗 WHAT FEEDS INTO LATER PHASES
- Architecture issues → Phase 3 (Logic bugs often stem from bad architecture)
- Window/dialog issues → Phase 2 (UI/UX depends on window management)
- Dependency issues → Phase 4 (Vulnerable dependencies are security risks)
- Coupling issues → Phase 5 (Tight coupling causes performance problems)
- Thread architecture → Phase 5 (Threading bugs cause performance issues)
