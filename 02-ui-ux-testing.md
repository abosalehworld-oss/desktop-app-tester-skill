# Phase 2: UI/UX Testing 🎨

> **Objective:** Systematically test every visual element, user interaction, and accessibility
> feature of the desktop application. Desktop UX has unique challenges: multi-window management,
> keyboard navigation, system tray, multi-monitor support, and OS-native look & feel.

---

## 📋 UI/UX CHECKS

### CHECK U1: Window Management & Layout
```
WHAT TO CHECK:
  ❑ Does the main window have proper minimum size constraints?
  ❑ Does resizing work correctly? (no clipped content, no broken layouts)
  ❑ Is the window position remembered between sessions?
  ❑ Does maximizing/restoring work correctly?
  ❑ Does the app handle multi-monitor setups? (window on removed monitor)
  ❑ Is there a proper splash screen or loading indicator?
  ❑ Does window z-ordering work correctly? (dialogs stay on top)
  ❑ Is fullscreen mode supported and toggleable?

COMMON BUGS:
  🐛 Window opens at 0,0 coordinate or off-screen
  🐛 Content clips when window is resized smaller
  🐛 Dialog appears behind parent window
  🐛 App crashes when monitor is disconnected while running
  🐛 Maximizing on ultrawide monitor stretches UI badly
  🐛 No minimum window size — UI breaks at small sizes

CITATION REQUIRED: Show window initialization code and layout constraints
```

### CHECK U2: Controls & Interactive Elements
```
WHAT TO CHECK:
  ❑ Do all buttons have visible hover/pressed states?
  ❑ Are all form fields properly labeled?
  ❑ Do dropdown/combobox items render correctly?
  ❑ Are scrollbars visible and functional?
  ❑ Do text inputs have proper placeholder text?
  ❑ Are loading states shown for async operations?
  ❑ Is there visual feedback for every user action?
  ❑ Do tooltips appear for icon-only buttons?

COMMON BUGS:
  🐛 Button appears clickable but does nothing (missing event handler)
  🐛 Text field accepts input but doesn't validate on blur
  🐛 Dropdown shows empty when data hasn't loaded
  🐛 No loading indicator during file import (app appears frozen)
  🐛 Right-click context menu missing or incomplete
  🐛 Double-click and single-click behavior conflict

CITATION REQUIRED: Show at least 3 UI component implementations with checks
```

### CHECK U3: Keyboard Navigation & Shortcuts
```
WHAT TO CHECK:
  ❑ Can the entire app be operated with keyboard only? (Tab order)
  ❑ Are common shortcuts implemented? (Ctrl+S, Ctrl+Z, Ctrl+C/V, F1)
  ❑ Are shortcuts documented or discoverable?
  ❑ Does Tab/Shift+Tab navigate in logical order?
  ❑ Does Enter trigger the default/primary button in dialogs?
  ❑ Does Escape close dialogs and cancel operations?
  ❑ Are keyboard shortcuts conflict-free?

COMMON BUGS:
  🐛 Tab order is random/illogical
  🐛 Focus trap — cannot Tab out of a control
  🐛 Ctrl+S doesn't save (most common user complaint)
  🐛 Escape doesn't close dialog
  🐛 No keyboard shortcut for common actions
  🐛 Shortcut conflicts with OS-level shortcuts
  🐛 Focused element not visually highlighted

CITATION REQUIRED: Show tab order and keyboard handler implementations
```

### CHECK U4: Typography & Theming
```
WHAT TO CHECK:
  ❑ Are fonts consistent across the application?
  ❑ Is text readable at all DPI settings? (100%, 125%, 150%, 200%)
  ❑ Does the app respect OS theme? (Light/Dark mode)
  ❑ Are colors accessible? (contrast ratio ≥ 4.5:1 for text)
  ❑ Is text truncation handled properly? (ellipsis, not clipping)
  ❑ Are text wrapping and overflow handled?
  ❑ Are custom fonts bundled and loaded correctly?

COMMON BUGS:
  🐛 Text overflows container at high DPI
  🐛 App ignores system dark mode (white eye-burn at night)
  🐛 Font size too small at 100% DPI (12px minimum for body text)
  🐛 Text truncated without ellipsis (user can't tell there's more)
  🐛 Custom font not found — falls back to ugly system default
  🐛 Hardcoded colors ignore theme changes

CITATION REQUIRED: Show theme/styling implementation and DPI handling
```

### CHECK U5: Accessibility (a11y)
```
WHAT TO CHECK:
  ❑ Are screen readers supported? (ARIA labels, automation peers)
  ❑ Do all images have alt text?
  ❑ Are focus indicators visible?
  ❑ Is high contrast mode supported?
  ❑ Are font sizes adjustable or respect system settings?
  ❑ Are error messages announced to screen readers?
  ❑ Is there adequate color contrast?

COMMON BUGS:
  🐛 Buttons have no accessible name (screen reader says "button")
  🐛 Custom controls not accessible to assistive technology
  🐛 Images used for text (not readable by screen readers)
  🐛 Error states only conveyed by color (inaccessible to colorblind)
  🐛 Focus trapped in hidden element
  🐛 Animations can't be disabled (motion sensitivity)

CITATION REQUIRED: Show accessibility properties or lack thereof
```

### CHECK U6: Menus, Toolbars & Status Bars
```
WHAT TO CHECK:
  ❑ Is there a proper menu bar with standard menus? (File, Edit, View, Help)
  ❑ Do menu items have keyboard accelerators?
  ❑ Are menu items properly enabled/disabled based on context?
  ❑ Is there a toolbar with common actions?
  ❑ Is there a status bar showing app state?
  ❑ Does the system tray icon work correctly?
  ❑ Are right-click context menus contextually appropriate?

COMMON BUGS:
  🐛 Menu item enabled when it shouldn't be (e.g., Save when no document open)
  🐛 No keyboard shortcut for menu items
  🐛 System tray icon doesn't respond to double-click
  🐛 System tray icon persists after app crash
  🐛 Context menu shows same items regardless of context
  🐛 No Help → About dialog

CITATION REQUIRED: Show menu definitions and state management
```

### CHECK U7: Drag & Drop and File Interaction
```
WHAT TO CHECK:
  ❑ Does the app support drag & drop for relevant content types?
  ❑ Is there visual feedback during drag operations?
  ❑ Are drop zones clearly indicated?
  ❑ Does the app handle dropped files of wrong type gracefully?
  ❑ Do file open/save dialogs use proper filters?
  ❑ Are recent files tracked and displayed?
  ❑ Does the app handle file association correctly? (double-click file opens app)

COMMON BUGS:
  🐛 App crashes when wrong file type is dropped
  🐛 No drag visual indicator (user doesn't know drop is possible)
  🐛 File save dialog doesn't suggest proper extension
  🐛 Recent files list shows files that no longer exist
  🐛 Drag & drop handler blocks UI thread on large files
  🐛 App doesn't register file association during install

CITATION REQUIRED: Show drag/drop and file dialog implementations
```

### CHECK U8: Notifications & User Feedback
```
WHAT TO CHECK:
  ❑ Are success/error/warning notifications shown appropriately?
  ❑ Are OS-native notifications used for background events?
  ❑ Do notifications disappear after timeout or require dismissal?
  ❑ Is there undo support for destructive actions?
  ❑ Are confirmation dialogs shown for irreversible actions?
  ❑ Do progress indicators show for long operations?
  ❑ Is there a way to view notification history?

COMMON BUGS:
  🐛 No confirmation before deleting data
  🐛 Notification shows behind other windows
  🐛 Success message shown but operation actually failed
  🐛 No progress indicator for import/export operations
  🐛 Multiple notifications stack and cover content
  🐛 No undo after accidental delete

CITATION REQUIRED: Show notification/dialog implementations
```

---

## 🚦 PHASE 2 GATE — MANDATORY CHECKLIST

```
PHASE 2 GATE CHECKLIST:
  □ [U1] Window management and layout verified
  □ [U2] Controls and interactive elements tested
  □ [U3] Keyboard navigation and shortcuts verified
  □ [U4] Typography and theming checked
  □ [U5] Accessibility reviewed
  □ [U6] Menus, toolbars, and status bars checked
  □ [U7] Drag & drop and file interaction tested
  □ [U8] Notifications and user feedback verified
  □ Minimum 12 code citations provided
  □ Files examined list produced
```

### Gate Report Format:
```
═══════════════════════════════════════════════════════
  ✅ PHASE 2 COMPLETE: UI/UX Testing
  📊 Findings: [X] Critical | [Y] High | [Z] Medium | [W] Low
  📋 Gate Status: [PASSED/FAILED] ([checked]/10 items)

  🎨 UI/UX Health Score: [X/10]
  Accessibility: [Good/Partial/Missing]
  Keyboard Support: [Full/Partial/None]
  Theme Support: [System/Custom/None]
═══════════════════════════════════════════════════════
```
