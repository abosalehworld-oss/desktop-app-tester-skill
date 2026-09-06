# Phase 8: State & Data Management 💾

> **Objective:** Verify the application's state management architecture, local data storage,
> configuration handling, and data flow integrity. Desktop apps uniquely manage persistent
> local state across sessions, including databases, config files, and user preferences.

---

## 📋 STATE & DATA CHECKS

### CHECK D1: State Architecture
```
WHAT TO CHECK:
  ❑ Is there a clear state management pattern? (MVVM bindings, Redux store, signals)
  ❑ Is state properly scoped? (window-level vs app-level vs user-level)
  ❑ Are state changes reactive? (UI updates automatically when data changes)
  ❑ Is state mutation controlled? (immutable patterns, action dispatchers)
  ❑ Can state be inspected for debugging? (dev tools, state dumps)
  ❑ Is state serializable? (for crash recovery, session restore)

COMMON BUGS:
  🐛 State shared between windows causes unintended side effects
  🐛 UI doesn't update when underlying data changes (stale view)
  🐛 State mutation from multiple sources → inconsistent data
  🐛 State lost on window close (should persist)
  🐛 Undo/redo doesn't work because state isn't immutable

CITATION REQUIRED: Show state management pattern and data flow
```

### CHECK D2: Local Database Management
```
WHAT TO CHECK:
  ❑ Is the database engine appropriate? (SQLite, LevelDB, embedded DB)
  ❑ Are database connections managed properly? (connection pool, single instance)
  ❑ Are migrations versioned and reversible?
  ❑ Is the database file stored in the correct OS location?
  ❑ Is the database encrypted? (SQLCipher for sensitive data)
  ❑ Are queries parameterized? (SQL injection prevention)
  ❑ Is database backup/recovery implemented?

COMMON BUGS:
  🐛 SQLite database locked — multiple write connections
  🐛 No migration system → database schema can't be updated
  🐛 Database stored in app directory → lost on update/reinstall
  🐛 Raw SQL with string concatenation → SQL injection
  🐛 Database not backed up → data loss on corruption
  🐛 Database grows unbounded → fills disk

CITATION REQUIRED: Show database setup, queries, and migration handling
```

### CHECK D3: Configuration & Settings
```
WHAT TO CHECK:
  ❑ Where are settings stored? (registry, JSON, XML, INI, TOML)
  ❑ Are settings validated when loaded? (corrupt file handling)
  ❑ Are default values provided for missing settings?
  ❑ Can settings be reset to defaults?
  ❑ Are settings backed up before migration?
  ❑ Is settings file format human-readable? (for manual editing)
  ❑ Are settings changes applied immediately or on restart?

COMMON BUGS:
  🐛 Corrupt config file → app crashes on startup (no fallback)
  🐛 New setting added in update → old config missing key → crash
  🐛 Settings saved only on clean exit → lost on crash
  🐛 Settings stored in registry → hard to backup/migrate
  🐛 Concurrent settings write → file corruption
  🐛 Settings file with sensitive data not encrypted

CITATION REQUIRED: Show settings load/save logic and error handling
```

### CHECK D4: Cache Management
```
WHAT TO CHECK:
  ❑ Is caching strategy defined? (LRU, TTL, size-limited)
  ❑ Are cache entries invalidated correctly?
  ❑ Is cache size bounded? (max disk/memory usage)
  ❑ Can the user clear the cache? (settings option)
  ❑ Is the cache location OS-appropriate?
    - Windows: %LOCALAPPDATA%\AppName\Cache
    - macOS: ~/Library/Caches/AppName
    - Linux: ~/.cache/AppName
  ❑ Are cache files cleaned up on uninstall?

COMMON BUGS:
  🐛 Cache grows indefinitely → fills disk after months
  🐛 Stale cache shows outdated data after server update
  🐛 No way to clear cache → user must manually delete files
  🐛 Cache stored in wrong location → not cleaned by OS tools

CITATION REQUIRED: Show caching implementation and eviction logic
```

### CHECK D5: Data Integrity & Transactions
```
WHAT TO CHECK:
  ❑ Are multi-step operations wrapped in transactions?
  ❑ Is data validated before persisting?
  ❑ Are foreign key constraints enforced?
  ❑ Is referential integrity maintained across entities?
  ❑ Are orphaned records cleaned up?
  ❑ Is data export/import maintaining integrity?

COMMON BUGS:
  🐛 Partial save on crash → database in inconsistent state
  🐛 Delete parent record → orphaned child records remain
  🐛 Concurrent edit → last writer wins, no conflict detection
  🐛 Import creates references to non-existent records
  🐛 Data validation only in UI → can be bypassed via config edit

CITATION REQUIRED: Show transaction usage and data validation
```

### CHECK D6: Session & Window State
```
WHAT TO CHECK:
  ❑ Is window position/size restored on restart?
  ❑ Are open documents/tabs restored after crash?
  ❑ Is user session (login state) persisted securely?
  ❑ Is recent files list maintained?
  ❑ Are UI customizations persisted? (toolbar layout, column widths)
  ❑ Is the working directory/last used path remembered?

COMMON BUGS:
  🐛 Window always opens at default position (ignoring last position)
  🐛 All unsaved work lost after crash (no auto-save/recovery)
  🐛 Session cookie expires while app is open → silent failure
  🐛 Recent files list shows deleted/moved files
  🐛 Column width customization lost on restart

CITATION REQUIRED: Show session persistence and window state management
```

---

## 🚦 PHASE 8 GATE — MANDATORY CHECKLIST

```
PHASE 8 GATE CHECKLIST:
  □ [D1] State architecture reviewed
  □ [D2] Local database management verified
  □ [D3] Configuration and settings checked
  □ [D4] Cache management assessed
  □ [D5] Data integrity and transactions verified
  □ [D6] Session and window state persistence checked
  □ Minimum 8 code citations provided
  □ Files examined list produced
```
