# Phase 6: Platform Compatibility 🖥️🍎🐧

> **Objective:** Verify the application works correctly across target operating systems
> (Windows, macOS, Linux) and handles OS-specific behaviors, permissions, file systems,
> and system integration correctly.

---

## 📋 PLATFORM CHECKS

### CHECK C1: Operating System Detection & Adaptation
```
WHAT TO CHECK:
  ❑ Does the app detect the current OS correctly?
  ❑ Are OS-specific code paths clearly separated?
  ❑ Are platform-specific dependencies conditionally loaded?
  ❑ Are file paths handled cross-platform? (/ vs \, path separators)
  ❑ Are line endings handled? (LF vs CRLF)
  ❑ Are environment variables accessed correctly per OS?

COMMON BUGS:
  🐛 Hardcoded backslash paths → fails on macOS/Linux
  🐛 Hardcoded "C:\Users" → fails on non-Windows
  🐛 Windows registry code runs on macOS → crash
  🐛 Case-sensitive file system (Linux) vs case-insensitive (Windows) mismatch
  🐛 Home directory resolved incorrectly (~ vs %USERPROFILE%)

CITATION REQUIRED: Show OS detection and platform-specific handling
```

### CHECK C2: File System Compatibility
```
WHAT TO CHECK:
  ❑ Are file paths using OS-agnostic APIs? (path.join, Path.Combine)
  ❑ Are reserved filenames avoided? (CON, PRN, AUX on Windows)
  ❑ Are maximum path lengths handled? (260 chars on older Windows)
  ❑ Are file permissions set correctly per OS?
  ❑ Are symlinks handled safely on all platforms?
  ❑ Is the app data directory OS-appropriate?
    - Windows: %APPDATA%
    - macOS: ~/Library/Application Support/
    - Linux: ~/.config/ or $XDG_CONFIG_HOME

COMMON BUGS:
  🐛 App data stored in app directory → fails without admin on Windows
  🐛 File created with Linux permissions on Windows → ignored
  🐛 Path exceeds 260 chars on Windows → operation fails silently
  🐛 Filename contains : or * → fails on Windows
  🐛 Hidden files (. prefix) behavior differs between OS

CITATION REQUIRED: Show file path construction and storage locations
```

### CHECK C3: UI Framework Platform Behavior
```
WHAT TO CHECK:
  ❑ Do native controls render correctly on all target platforms?
  ❑ Are fonts available on all platforms? (fallback fonts defined)
  ❑ Is DPI/Retina scaling handled? (2x on macOS, varying on Windows)
  ❑ Are keyboard shortcuts OS-appropriate? (Ctrl on Windows/Linux, Cmd on macOS)
  ❑ Are native file dialogs used? (not custom dialogs)
  ❑ Does the menu bar follow OS conventions? (app menu on macOS)
  ❑ Is the window chrome/titlebar appropriate for each OS?

COMMON BUGS:
  🐛 Ctrl+C on macOS (should be Cmd+C)
  🐛 Menu bar inside window on macOS (should be in system menu bar)
  🐛 Windows-style close/minimize buttons on macOS
  🐛 Font rendering different between platforms (anti-aliasing)
  🐛 Dialog button order: OK/Cancel on Windows vs Cancel/OK on macOS
  🐛 System dark mode not detected on Linux

CITATION REQUIRED: Show platform-specific UI adaptations
```

### CHECK C4: System Integration
```
WHAT TO CHECK:
  ❑ Does the app integrate with OS notifications?
  ❑ Is the system tray/menu bar icon functional on all platforms?
  ❑ Are file associations registered correctly?
  ❑ Is the app registered for URL scheme handling? (custom protocols)
  ❑ Does clipboard work across all platforms?
  ❑ Is drag & drop working on all platforms?
  ❑ Are OS-level events handled? (sleep, wake, display change, network change)

COMMON BUGS:
  🐛 System tray icon invisible on Linux (different tray protocols)
  🐛 File association opens new instance instead of existing
  🐛 URL scheme handler doesn't work on Linux (no standard mechanism)
  🐛 Clipboard paste fails with rich content on some platforms
  🐛 App doesn't handle OS sleep/wake → stale connections

CITATION REQUIRED: Show system integration code
```

### CHECK C5: Installation & Uninstallation
```
WHAT TO CHECK:
  ❑ Does the installer work on all target platforms?
  ❑ Are 32-bit and 64-bit builds handled? (ARM on macOS?)
  ❑ Is the uninstaller thorough? (removes all files, registry entries, app data?)
  ❑ Are admin rights requested only when necessary?
  ❑ Is the install path customizable?
  ❑ Are dependencies bundled or checked? (runtime, frameworks)
  ❑ Is the app portable-ready? (can run from USB without installing?)

COMMON BUGS:
  🐛 Installer requires admin but app doesn't need it
  🐛 Uninstall leaves registry entries and app data behind
  🐛 32-bit installer on 64-bit-only app
  🐛 Missing Visual C++ Redistributable → app won't start
  🐛 macOS app not notarized → Gatekeeper blocks it
  🐛 Linux: no .desktop file → doesn't appear in app launcher

CITATION REQUIRED: Show installer/build configuration
```

### CHECK C6: Locale & Internationalization
```
WHAT TO CHECK:
  ❑ Does the app handle different system locales?
  ❑ Are date formats locale-aware? (DD/MM vs MM/DD)
  ❑ Are number formats locale-aware? (1,000.00 vs 1.000,00)
  ❑ Are currency symbols correct per locale?
  ❑ Is RTL text layout supported? (Arabic, Hebrew)
  ❑ Are string resources externalized for translation?
  ❑ Are timezone changes handled?

COMMON BUGS:
  🐛 App crashes with comma decimal separator (parsing "1,5" as 1)
  🐛 Date displayed as MM/DD in European locale
  🐛 Hardcoded English strings mixed with translated UI
  🐛 RTL text breaks layout (overlapping controls)
  🐛 Currency symbol hardcoded as $

CITATION REQUIRED: Show locale/i18n handling
```

---

## 🚦 PHASE 6 GATE — MANDATORY CHECKLIST

```
PHASE 6 GATE CHECKLIST:
  □ [C1] OS detection and adaptation verified
  □ [C2] File system compatibility checked
  □ [C3] UI framework platform behavior tested
  □ [C4] System integration verified
  □ [C5] Installation and uninstallation reviewed
  □ [C6] Locale and internationalization checked
  □ Minimum 8 code citations provided
  □ Files examined list produced
```
