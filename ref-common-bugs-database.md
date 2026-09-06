# 🐛 Desktop Application — Common Bugs Database

> Reference database of 200+ common desktop application bugs categorized by type,
> framework, and severity. Use this as a lookup during analysis phases.

---

## 🔴 CRITICAL BUGS (Security & Data Loss)

### File System Security
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 1 | Path traversal via user input in file open/save | ALL | 4 |
| 2 | DLL search order hijacking (loading from CWD) | WPF, WinForms, Qt(C++) | 4 |
| 3 | Temp file with predictable name → symlink attack | ALL | 4 |
| 4 | World-readable file permissions on config/DB | ALL | 4 |
| 5 | Plaintext password/API key in config file | ALL | 4 |
| 6 | Plaintext credentials in source code | ALL | 4 |
| 7 | Unsigned auto-update over HTTP → MITM code injection | ALL | 4, 12 |
| 8 | Command injection via unsanitized user input | ALL | 4 |
| 9 | SQLite database completely unencrypted with PII | ALL | 4 |
| 10 | Electron: nodeIntegration=true in renderer | Electron | 4 |
| 11 | Electron: webSecurity disabled | Electron | 4 |
| 12 | Installer sets world-writable app directory | ALL | 6, 12 |
| 13 | Plugin/extension loading without verification | ALL | 4 |
| 14 | Registry/config manipulation → privilege escalation | WPF, WinForms | 4 |
| 15 | Unvalidated IPC messages from other processes | Electron, Tauri | 4 |

### Data Loss
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 16 | No auto-save → crash loses all work | ALL | 9 |
| 17 | Partial write on crash → corrupted file | ALL | 3 |
| 18 | Database migration failure → data lost | ALL | 8 |
| 19 | Uninstaller deletes user data | ALL | 6 |
| 20 | Sync conflict resolution deletes local changes | ALL | 7 |

---

## 🟠 HIGH BUGS (Functionality & Performance)

### UI/UX
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 21 | Window opens off-screen on removed monitor | ALL | 2 |
| 22 | App ignores system dark/light mode | ALL | 2 |
| 23 | Content clips/overflows on resize | ALL | 2 |
| 24 | Tab order random/illogical | ALL | 2 |
| 25 | No keyboard shortcut for common actions (Ctrl+S) | ALL | 2 |
| 26 | Dialog appears behind parent window | ALL | 2 |
| 27 | DPI scaling breaks layout (125%, 150%, 200%) | ALL | 2 |
| 28 | No loading indicator for async operations | ALL | 2 |
| 29 | Right-click context menu missing | ALL | 2 |
| 30 | System tray icon persists after crash | Electron, WPF, Qt | 2 |

### Memory & Performance
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 31 | Event handler leak — closed view stays in memory | ALL | 5 |
| 32 | Image cache grows unbounded → OOM | ALL | 5 |
| 33 | Large file loaded entirely into memory | ALL | 5 |
| 34 | Database query on UI thread → app freezes | ALL | 5 |
| 35 | Timer/interval not cleared on view dispose | ALL | 5 |
| 36 | ListView renders all items (no virtualization) | ALL | 5 |
| 37 | Startup blocked by sync network call | ALL | 5 |
| 38 | Busy-wait loop consuming CPU when idle | ALL | 5 |
| 39 | Log file grows without rotation → fills disk | ALL | 5 |
| 40 | Font loading blocks UI thread | ALL | 5 |

### Logic & State
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 41 | Floating-point arithmetic for currency | ALL | 3 |
| 42 | Duplicate records on double-click | ALL | 3 |
| 43 | State lost on window close | ALL | 3 |
| 44 | Undo/redo doesn't work correctly | ALL | 3 |
| 45 | Race condition in concurrent state update | ALL | 3 |
| 46 | Import creates duplicate records | ALL | 3 |
| 47 | File locked after operation completes | ALL | 3 |
| 48 | Date/time ignores timezone/DST | ALL | 3 |
| 49 | Search doesn't find partial matches | ALL | 3 |
| 50 | Division by zero not handled | ALL | 3 |

### Network
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 51 | App crashes when network unavailable | ALL | 7 |
| 52 | No offline mode / no offline indicator | ALL | 7 |
| 53 | No timeout on HTTP requests → hangs forever | ALL | 7 |
| 54 | Infinite retry on client error (400) | ALL | 7 |
| 55 | WebSocket never reconnects after sleep/wake | ALL | 7 |
| 56 | No proxy support → fails in corporate networks | ALL | 7 |
| 57 | Large download can't resume after failure | ALL | 7 |
| 58 | Token refresh race condition | ALL | 7 |
| 59 | SSL certificate validation disabled | ALL | 7 |
| 60 | API key in URL query parameters | ALL | 7 |

### Error Handling
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 61 | Unhandled exception → app disappears silently | ALL | 9 |
| 62 | NullReferenceException from missing config | .NET, Java | 9 |
| 63 | Corrupt config file → app won't start | ALL | 9 |
| 64 | Error dialog shows raw stack trace | ALL | 9 |
| 65 | Background task fails silently | ALL | 9 |
| 66 | Disk full → crash during save | ALL | 9 |
| 67 | Generic "Something went wrong" for all errors | ALL | 9 |
| 68 | Cancelled async operation still runs callback | ALL | 9 |
| 69 | Recursive error in crash handler | ALL | 9 |
| 70 | No crash recovery / auto-save restore | ALL | 9 |

---

## 🟡 MEDIUM BUGS

### Platform Compatibility
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 71 | Hardcoded Windows paths (C:\, backslash) | Cross-platform | 6 |
| 72 | Case-sensitive filesystem issues | Cross-platform | 6 |
| 73 | Ctrl+C instead of Cmd+C on macOS | Cross-platform | 6 |
| 74 | Menu bar inside window on macOS | Electron, cross | 6 |
| 75 | Missing .desktop file on Linux | Cross-platform | 6 |
| 76 | Reserved filename (CON, PRN) on Windows | Cross-platform | 6 |
| 77 | Path exceeds 260 chars on Windows | Cross-platform | 6 |
| 78 | App data in wrong OS directory | Cross-platform | 6 |
| 79 | Font not available on all platforms | Cross-platform | 6 |
| 80 | System dark mode not detected on Linux | Cross-platform | 6 |

### Data & State
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 81 | Settings lost on crash (only saved on clean exit) | ALL | 8 |
| 82 | Cache grows indefinitely | ALL | 8 |
| 83 | SQLite database locked — concurrent writes | ALL | 8 |
| 84 | No database migration system | ALL | 8 |
| 85 | Recent files list shows deleted files | ALL | 8 |
| 86 | Window position not saved between sessions | ALL | 8 |
| 87 | Column widths reset on restart | ALL | 8 |
| 88 | Stale cache shows outdated data | ALL | 8 |
| 89 | New config key missing → crash on update | ALL | 8 |
| 90 | Database stored in app directory → lost on update | ALL | 8 |

### Architecture
| # | Bug Pattern | Frameworks | Phase |
|---|------------|------------|-------|
| 91 | No separation between UI and business logic | ALL | 1 |
| 92 | Circular dependencies between services | ALL | 1 |
| 93 | Multiple windows share singleton state → stale data | ALL | 1 |
| 94 | No dependency injection → untestable code | ALL | 1 |
| 95 | Debug flags left in release build | ALL | 1 |
| 96 | Hardcoded API URLs | ALL | 1 |
| 97 | No single-instance enforcement | ALL | 1 |
| 98 | Electron: main/renderer code mixed | Electron | 1 |
| 99 | Console.log/print statements in production | ALL | 1 |
| 100 | Unused dependencies bloating app size | ALL | 1 |

---

## 🔵 LOW BUGS (Code Quality)

| # | Bug Pattern | Phase |
|---|------------|-------|
| 101 | Inconsistent naming conventions | 1 |
| 102 | Missing error logging | 9 |
| 103 | Magic numbers/strings without constants | 3 |
| 104 | Code duplication across features | 1 |
| 105 | Missing type annotations (TypeScript strict) | 1 |
| 106 | Unused imports/variables | 1 |
| 107 | TODO/FIXME comments in production code | 1 |
| 108 | Inconsistent formatting/indentation | 1 |
| 109 | Missing JSDoc/XML doc comments | 1 |
| 110 | Test coverage below 50% | 1 |

---

## 📊 FRAMEWORK-SPECIFIC BUG PATTERNS

### Electron
| # | Bug | Severity |
|---|-----|----------|
| E1 | nodeIntegration enabled in renderer | 🔴 |
| E2 | contextIsolation disabled | 🔴 |
| E3 | webSecurity disabled | 🔴 |
| E4 | DevTools accessible in production | 🟠 |
| E5 | Remote module used (deprecated) | 🟠 |
| E6 | IPC messages not validated in main process | 🔴 |
| E7 | Preload script exposes too many Node APIs | 🟠 |
| E8 | BrowserWindow created without sandbox | 🟠 |
| E9 | Multiple renderer processes share state unsafely | 🟡 |
| E10 | Large DOM causing memory bloat | 🟡 |

### WPF / WinForms (.NET)
| # | Bug | Severity |
|---|-----|----------|
| W1 | Dispatcher.Invoke blocking UI thread | 🟠 |
| W2 | DispatcherTimer used for heavy work | 🟠 |
| W3 | Binding memory leak (missing WeakReference) | 🟠 |
| W4 | XAML resource not found at runtime | 🟡 |
| W5 | Dependency property default value shared across instances | 🔴 |
| W6 | Cross-thread UI update without Dispatcher | 🔴 |
| W7 | Clipboard operations without STA thread | 🟡 |
| W8 | GDI/GDI+ handle leak | 🟠 |
| W9 | COM object not released properly | 🟠 |
| W10 | High DPI not declared in manifest | 🟡 |

### Qt (C++/Python)
| # | Bug | Severity |
|---|-----|----------|
| Q1 | Signal-slot connection memory leak (missing disconnect) | 🟠 |
| Q2 | QObject created without parent → memory leak | 🟠 |
| Q3 | UI update from non-GUI thread | 🔴 |
| Q4 | QThread::terminate used (unsafe) | 🟠 |
| Q5 | Static initialization order fiasco | 🔴 |
| Q6 | Buffer overflow in C++ string handling | 🔴 |
| Q7 | Use-after-free on deleted QObject | 🔴 |
| Q8 | Missing virtual destructor | 🟡 |
| Q9 | QSqlDatabase connection leaked | 🟠 |
| Q10 | QString encoding mismatch (UTF-8 vs Latin1) | 🟡 |

### Tauri
| # | Bug | Severity |
|---|-----|----------|
| T1 | Command handler with overly broad scope | 🟠 |
| T2 | allowlist too permissive | 🟠 |
| T3 | Unsafe Rust code in command handler | 🔴 |
| T4 | Frontend able to invoke dangerous commands | 🔴 |
| T5 | State management across windows not synchronized | 🟡 |

### JavaFX
| # | Bug | Severity |
|---|-----|----------|
| J1 | Platform.runLater not used for UI updates | 🔴 |
| J2 | FXML controller not garbage collected | 🟠 |
| J3 | Scene graph manipulation from background thread | 🔴 |
| J4 | CSS stylesheet not found at runtime | 🟡 |
| J5 | Observable list modification during iteration | 🔴 |
