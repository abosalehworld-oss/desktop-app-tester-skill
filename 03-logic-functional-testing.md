# Phase 3: Logic & Functional Testing 🧠

> **Objective:** Verify that all business logic, state transitions, data validation,
> and computational functions work correctly under all conditions — including edge cases.
> Logic bugs are the most common category and the hardest to find through casual testing.

---

## 📋 LOGIC CHECKS

### CHECK L1: Input Validation
```
WHAT TO CHECK:
  ❑ Are ALL user inputs validated? (text fields, file pickers, CLI args)
  ❑ Is validation done on both client-side AND processing-side?
  ❑ Are boundary values handled? (min/max lengths, numeric limits)
  ❑ Are special characters handled? (unicode, emoji, RTL text, null bytes)
  ❑ Are empty/null inputs handled gracefully?
  ❑ Is type coercion safe? (string to number, date parsing)
  ❑ Are file path inputs sanitized? (path traversal prevention)

COMMON BUGS:
  🐛 Text field accepts 1 million characters (no max length)
  🐛 Numeric field accepts negative values when they make no sense
  🐛 Date picker allows future dates for birth date
  🐛 File path input not sanitized — path traversal possible
  🐛 Pasted text bypasses character validation
  🐛 Unicode RTL override character breaks display

CITATION REQUIRED: Show at least 4 validation implementations or violations
```

### CHECK L2: State Machine & Workflow Logic
```
WHAT TO CHECK:
  ❑ Are there clear state definitions? (enum, constants)
  ❑ Are invalid state transitions prevented?
  ❑ Is the current state always visible to the user?
  ❑ Can the user undo/redo state changes?
  ❑ Are concurrent state modifications handled?
  ❑ Is state persisted across app restarts?

COMMON BUGS:
  🐛 Button enabled in wrong state (e.g., Submit before form is valid)
  🐛 State gets stuck — no way to recover without restarting
  🐛 Two operations modify state simultaneously → corrupted data
  🐛 App crash during state transition leaves data inconsistent
  🐛 State not saved when app is force-closed
  🐛 Undo doesn't restore previous state correctly

CITATION REQUIRED: Show state management pattern and transition guards
```

### CHECK L3: Calculation & Data Processing
```
WHAT TO CHECK:
  ❑ Are floating-point calculations handled correctly? (use Decimal for money)
  ❑ Are date/time operations timezone-aware?
  ❑ Are sorting algorithms stable and correct?
  ❑ Are search/filter operations accurate?
  ❑ Are aggregation functions correct? (sum, average, count)
  ❑ Are rounding rules consistent?

COMMON BUGS:
  🐛 0.1 + 0.2 ≠ 0.3 (floating-point arithmetic)
  🐛 Currency displayed as $10.1 instead of $10.10
  🐛 Date calculation ignores daylight saving time
  🐛 Sorting is case-sensitive unexpectedly (Z before a)
  🐛 Search doesn't find partial matches
  🐛 Division by zero not handled

CITATION REQUIRED: Show calculation logic and edge case handling
```

### CHECK L4: CRUD Operations
```
WHAT TO CHECK:
  ❑ Create: Can new records be created with all required fields?
  ❑ Read: Does list/detail view show correct data?
  ❑ Update: Are changes saved and reflected immediately?
  ❑ Delete: Is deletion confirmed? Can it be undone?
  ❑ Are duplicate records prevented?
  ❑ Are batch operations (multi-select delete/edit) handled?
  ❑ Is data integrity maintained after each operation?

COMMON BUGS:
  🐛 Duplicate records created on double-click
  🐛 Deleted record still appears in list until refresh
  🐛 Update silently fails but shows success message
  🐛 Batch delete hangs on large selections
  🐛 Creating record with all-spaces name succeeds
  🐛 Edit form doesn't load current values (shows defaults)

CITATION REQUIRED: Show CRUD implementations for at least 2 entities
```

### CHECK L5: File Operations & I/O Logic
```
WHAT TO CHECK:
  ❑ Are file operations wrapped in try/catch?
  ❑ Are file handles properly closed/disposed?
  ❑ Is file locking handled? (file open by another process)
  ❑ Are large files handled efficiently? (streaming vs loading all into memory)
  ❑ Is the file format validated before processing?
  ❑ Are temporary files cleaned up?
  ❑ Is the operation atomic? (no partial writes on failure)

COMMON BUGS:
  🐛 App crashes on file larger than available RAM
  🐛 File locked after operation — can't be deleted/moved
  🐛 Partial write on crash leaves corrupted file
  🐛 Temp files accumulate and fill disk space
  🐛 File encoding mismatch (UTF-8 vs ANSI)
  🐛 Path with spaces or special characters causes error

CITATION REQUIRED: Show file I/O patterns and error handling
```

### CHECK L6: Concurrency & Race Conditions
```
WHAT TO CHECK:
  ❑ Are shared resources protected by locks/mutexes?
  ❑ Is the UI updated safely from background threads?
  ❑ Are async operations awaited properly? (no fire-and-forget)
  ❑ Are database transactions used for multi-step operations?
  ❑ Is the app safe from TOCTOU (Time-of-check-to-time-of-use)?
  ❑ Are cancellation tokens propagated through async chains?

COMMON BUGS:
  🐛 UI update from background thread crashes app
  🐛 Two saves simultaneously → data corruption
  🐛 File existence checked then opened → deleted between check and open
  🐛 Progress bar goes backwards (race condition in update)
  🐛 Cancelling operation doesn't actually cancel (fire-and-forget)
  🐛 Deadlock when two locks acquired in different order

CITATION REQUIRED: Show concurrency handling and potential race conditions
```

### CHECK L7: Import/Export & Data Interchange
```
WHAT TO CHECK:
  ❑ Does import handle malformed data gracefully?
  ❑ Does export produce valid output format?
  ❑ Are all import formats documented and tested?
  ❑ Is progress shown for large import/export operations?
  ❑ Is import idempotent? (re-importing same data doesn't create duplicates)
  ❑ Are encodings handled correctly? (UTF-8, BOM)
  ❑ Is CSV injection prevented? (formulas in Excel-opening CSVs)

COMMON BUGS:
  🐛 Import crashes on malformed CSV (missing column)
  🐛 Export loses special characters (encoding issue)
  🐛 Re-importing creates duplicate records
  🐛 Large import freezes UI (no progress, no cancel)
  🐛 Export to Excel creates formula injection vulnerability
  🐛 Date format changes between import and export

CITATION REQUIRED: Show import/export logic and validation
```

### CHECK L8: Print & Report Generation
```
WHAT TO CHECK:
  ❑ Does print preview match actual print output?
  ❑ Are page margins and headers/footers correct?
  ❑ Does report generation handle empty data sets?
  ❑ Are generated PDFs/reports accessible?
  ❑ Do charts/graphs render correctly in reports?
  ❑ Is pagination correct for multi-page reports?

COMMON BUGS:
  🐛 Print cuts off content at page boundaries
  🐛 Report shows stale data (not refreshed before generation)
  🐛 Empty report shows error instead of "No data"
  🐛 PDF generation fails silently
  🐛 Charts don't scale correctly in print
  🐛 Non-ASCII characters garbled in PDF output

CITATION REQUIRED: Show print/report generation logic if present
```

---

## 🚦 PHASE 3 GATE — MANDATORY CHECKLIST

```
PHASE 3 GATE CHECKLIST:
  □ [L1] Input validation verified for all user inputs
  □ [L2] State machine and workflow logic verified
  □ [L3] Calculations and data processing checked
  □ [L4] CRUD operations verified
  □ [L5] File operations and I/O logic checked
  □ [L6] Concurrency and race conditions assessed
  □ [L7] Import/export functionality verified
  □ [L8] Print/report generation tested (if applicable)
  □ Minimum 12 code citations provided
  □ Files examined list produced
```
