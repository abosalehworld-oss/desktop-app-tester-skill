# Phase 9: Error & Crash Prevention 🛑

> ⚡ **REMINDER:** After completing this phase, Phase 10 requires a **MANDATORY FRESH-EYES
> RE-ANALYSIS** where you must re-read ALL critical files as a different AI agent.
> Do NOT forget this when you reach Phase 10. It is the MOST IMPORTANT step.

> **Objective:** Ensure the application handles ALL error conditions gracefully without crashing,
> losing data, or showing cryptic error messages. Desktop app crashes are MORE impactful than
> mobile crashes — users may lose hours of unsaved work.

---

## 📋 ERROR HANDLING CHECKS

### CHECK E1: Global Error Handling
```
WHAT TO CHECK:
  ❑ Is there a global unhandled exception handler?
  ❑ Does it prevent app crash? (catch, log, show user-friendly message)
  ❑ Is crash data logged locally before showing error?
  ❑ Does the app attempt auto-save/recovery on crash?
  ❑ Is the crash report useful for debugging? (stack trace, OS info, app version)
  ❑ Can the user submit crash reports?

COMMON BUGS:
  🐛 Unhandled exception → app disappears with no error message
  🐛 Error dialog shows raw stack trace to non-technical user
  🐛 Crash loses all unsaved data (no recovery attempt)
  🐛 Global handler catches exception but doesn't log it
  🐛 Crash handler itself crashes (recursive error)
  🐛 App restarts after crash but doesn't restore state

CITATION REQUIRED: Show global error handler setup
```

### CHECK E2: Null/Undefined Safety
```
WHAT TO CHECK:
  ❑ Are nullable types used correctly? (C# nullable, TypeScript strict null)
  ❑ Are null checks present before dereferencing?
  ❑ Are optional values unwrapped safely?
  ❑ Are default values provided for potentially null results?
  ❑ Are database query results null-checked?
  ❑ Are configuration values null-checked with fallbacks?

COMMON BUGS:
  🐛 NullReferenceException from missing config value
  🐛 TypeError: Cannot read property of undefined (JavaScript)
  🐛 Optional value force-unwrapped without check (Swift !)
  🐛 Database returns null for deleted record → crash
  🐛 API response missing expected field → crash in parser

CITATION REQUIRED: Show at least 4 null handling patterns or violations
```

### CHECK E3: Resource Exhaustion Handling
```
WHAT TO CHECK:
  ❑ Is disk full condition handled for write operations?
  ❑ Is out-of-memory handled for large operations?
  ❑ Is maximum file size enforced for import/load?
  ❑ Are thread pool limits respected?
  ❑ Are database connection limits handled?
  ❑ Are recursive operations bounded? (stack overflow prevention)

COMMON BUGS:
  🐛 App crashes when disk is full during save
  🐛 Loading 10GB file into memory → OutOfMemoryException
  🐛 Recursive function with no depth limit → StackOverflow
  🐛 Creating unlimited threads → system freeze
  🐛 Database connection pool exhausted → hang

CITATION REQUIRED: Show resource limit handling
```

### CHECK E4: Specific Error Scenarios
```
WHAT TO CHECK:
  ❑ What happens when the database file is deleted while app is running?
  ❑ What happens when a required file is missing?
  ❑ What happens when the config file is corrupted?
  ❑ What happens when the network drops during an operation?
  ❑ What happens when a USB device is removed during file operation?
  ❑ What happens when the user's permissions change while app is running?
  ❑ What happens when the system clock is changed?

COMMON BUGS:
  🐛 Database file deleted → crash, no recovery option
  🐛 Missing DLL/library → cryptic "file not found" error
  🐛 Corrupt config → app won't start, no way to fix
  🐛 Network drop during save → partial data, no rollback
  🐛 Clock change → all token expiry calculations wrong

CITATION REQUIRED: Show error handling for at least 3 failure scenarios
```

### CHECK E5: Async/Promise Error Handling
```
WHAT TO CHECK:
  ❑ Are all async operations properly awaited?
  ❑ Are promise/task rejections caught?
  ❑ Are errors in event handlers caught? (button click, timer tick)
  ❑ Are background task failures reported to the user?
  ❑ Is there a mechanism for async operation timeout?
  ❑ Are cancelled operations handled cleanly? (no ghost callbacks)

COMMON BUGS:
  🐛 Unobserved Task exception in .NET → silent failure
  🐛 Unhandled promise rejection in Electron → process crash
  🐛 Error in timer callback → timer stops firing, no notification
  🐛 Background sync fails silently → data not saved
  🐛 Cancelled operation callback runs → updates wrong state

CITATION REQUIRED: Show async error handling patterns
```

### CHECK E6: User-Facing Error Messages
```
WHAT TO CHECK:
  ❑ Are error messages user-friendly? (not stack traces)
  ❑ Do errors suggest what the user can do? (actionable)
  ❑ Are error codes/IDs included for support reference?
  ❑ Are errors logged with enough detail for debugging?
  ❑ Are error dialogs non-blocking where appropriate?
  ❑ Is there a way to copy error details? (for bug reports)

COMMON BUGS:
  🐛 "An error occurred" with no details or actions
  🐛 Error dialog blocks all interaction → must force-kill app
  🐛 Error message in wrong language (not localized)
  🐛 Same error message for different root causes
  🐛 Error logged but not shown to user → user thinks it worked

CITATION REQUIRED: Show error message implementations
```

### CHECK E7: Graceful Degradation
```
WHAT TO CHECK:
  ❑ Does the app degrade gracefully when optional features fail?
  ❑ Can the app run without internet? (if network is optional)
  ❑ Does the app handle missing optional dependencies?
  ❑ Is there a safe mode / recovery mode?
  ❑ Can the user continue working after a non-fatal error?
  ❑ Are feature flags used for experimental features?

COMMON BUGS:
  🐛 One failed plugin prevents entire app from starting
  🐛 Missing optional icon font → text shows squares everywhere
  🐛 Analytics failure → blocks user operation
  🐛 Non-critical feature error escalates to app crash
  🐛 No way to start app in safe mode after bad configuration

CITATION REQUIRED: Show graceful degradation patterns
```

### CHECK E8: Auto-Save & Recovery
```
WHAT TO CHECK:
  ❑ Is auto-save implemented for user work?
  ❑ How often does auto-save trigger? (time-based or action-based)
  ❑ Is auto-save file stored separately from main file? (no corruption risk)
  ❑ Is crash recovery offered on next launch?
  ❑ Can the user restore from auto-save?
  ❑ Are auto-save files cleaned up after successful save?

COMMON BUGS:
  🐛 No auto-save → 2 hours of work lost on crash
  🐛 Auto-save overwrites main file → both copies corrupted
  🐛 Auto-save files accumulate indefinitely
  🐛 Recovery prompt shows after clean shutdown (false alarm)
  🐛 Auto-save blocks UI thread during write

CITATION REQUIRED: Show auto-save/recovery implementation
```

---

## 🚦 PHASE 9 GATE — MANDATORY CHECKLIST

```
PHASE 9 GATE CHECKLIST:
  □ [E1] Global error handling verified
  □ [E2] Null/undefined safety checked
  □ [E3] Resource exhaustion handling assessed
  □ [E4] Specific error scenarios tested
  □ [E5] Async/promise error handling verified
  □ [E6] User-facing error messages reviewed
  □ [E7] Graceful degradation patterns checked
  □ [E8] Auto-save and recovery verified
  □ Minimum 10 code citations provided
  □ Files examined list produced
```
