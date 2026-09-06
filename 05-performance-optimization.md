# Phase 5: Performance Analysis ⚡

> **Objective:** Identify performance bottlenecks in memory usage, CPU consumption, disk I/O,
> startup time, and UI rendering. Desktop apps are expected to be fast and responsive —
> users will abandon a slow desktop app even faster than a slow web app.

---

## 📋 PERFORMANCE CHECKS

### CHECK P1: Startup Performance
```
WHAT TO CHECK:
  ❑ How long does the app take to show the first usable screen?
  ❑ Are there blocking calls during initialization? (sync I/O, network calls)
  ❑ Is splash screen shown during heavy initialization?
  ❑ Are non-critical services lazy-loaded?
  ❑ Is the dependency injection container resolved efficiently?
  ❑ Are plugins/extensions loaded asynchronously?

TARGETS:
  ✅ Cold start to first usable screen: < 3 seconds
  ✅ Warm start (from minimized): < 500ms
  ⚠️ Any blocking network call during startup = HIGH finding

COMMON BUGS:
  🐛 Database migration runs on every startup (should check version first)
  🐛 All plugins loaded synchronously during splash screen
  🐛 Config file parsed multiple times during init
  🐛 Font loading blocks UI thread
  🐛 Startup tries to reach server — hangs if offline

CITATION REQUIRED: Show startup sequence and timing bottlenecks
```

### CHECK P2: Memory Management
```
WHAT TO CHECK:
  ❑ Are large objects disposed/freed after use?
  ❑ Are images/media loaded at display resolution? (not raw 50MB images)
  ❑ Are event handlers unsubscribed when views close?
  ❑ Are caches bounded? (max size, eviction policy)
  ❑ Is memory usage stable over time? (no slow leak)
  ❑ Are large collections virtualized? (only visible items in memory)
  ❑ Are IDisposable/ICloseable objects properly managed?

COMMON BUGS:
  🐛 Event handler leak — closed window stays in memory
  🐛 Image cache grows unbounded → out of memory after hours
  🐛 String concatenation in loop builds massive strings
  🐛 Large file loaded entirely into memory instead of streaming
  🐛 ListView loads 100K items into memory at once
  🐛 Background timer continues after view disposed

CITATION REQUIRED: Show at least 3 memory management patterns or violations
```

### CHECK P3: CPU & Thread Usage
```
WHAT TO CHECK:
  ❑ Is the UI thread kept below 16ms frame time? (60fps)
  ❑ Are heavy computations offloaded to background threads?
  ❑ Is CPU usage idle when app is not being actively used?
  ❑ Are timers at appropriate intervals? (not polling every 1ms)
  ❑ Are animations hardware-accelerated?
  ❑ Is thread pool usage efficient? (not creating threads per-task)

COMMON BUGS:
  🐛 Busy-wait loop consuming 100% CPU when idle
  🐛 Timer fires every 10ms for a task that needs 1s intervals
  🐛 JSON parsing on UI thread with large payloads
  🐛 Regular expression with catastrophic backtracking
  🐛 Sorting 100K items on UI thread → app freezes
  🐛 Animation jank from layout recalculation

CITATION REQUIRED: Show thread usage patterns and potential CPU bottlenecks
```

### CHECK P4: Disk I/O Performance
```
WHAT TO CHECK:
  ❑ Are file operations buffered? (not one byte at a time)
  ❑ Are database queries optimized? (indexes, query plans)
  ❑ Is file watching efficient? (OS-native watcher, not polling)
  ❑ Are write operations batched? (not writing after every keystroke)
  ❑ Are large files processed in chunks/streams?
  ❑ Is disk space checked before write operations?

COMMON BUGS:
  🐛 Auto-save writes entire file every second (should diff)
  🐛 SQLite query without index scans entire table
  🐛 File watcher triggers on every OS event (too granular)
  🐛 Log file grows without rotation → fills disk
  🐛 Reading 1GB file into memory instead of streaming

CITATION REQUIRED: Show file and database I/O patterns
```

### CHECK P5: UI Rendering Performance
```
WHAT TO CHECK:
  ❑ Are large lists virtualized? (only render visible items)
  ❑ Are images lazy-loaded and cached?
  ❑ Are heavy UI updates batched?
  ❑ Is layout calculation minimized? (avoid forced reflow/measure)
  ❑ Are CSS animations used instead of JS animations? (Electron)
  ❑ Are GPU-intensive operations hardware-accelerated?

COMMON BUGS:
  🐛 Table with 50K rows renders all at once → seconds of lag
  🐛 Every cell update triggers full table re-render
  🐛 Image gallery loads all full-res images at once
  🐛 Electron: DOM manipulation in tight loop → renderer lag
  🐛 Custom drawing without double-buffering → flicker
  🐛 Layout recalculated on scroll

CITATION REQUIRED: Show rendering patterns for lists and heavy UI
```

### CHECK P6: Network Performance
```
WHAT TO CHECK:
  ❑ Are API calls cached appropriately?
  ❑ Are requests batched when possible?
  ❑ Is connection pooling used?
  ❑ Are timeouts set for all network operations?
  ❑ Is bandwidth usage monitored? (not downloading unnecessary data)
  ❑ Are large downloads resumable?

COMMON BUGS:
  🐛 Same API called 50 times for 50 list items (should batch)
  🐛 No timeout on HTTP request → hangs forever if server down
  🐛 Full data downloaded when only count was needed
  🐛 WebSocket reconnects without backoff → server flood
  🐛 Large file download can't resume after interruption

CITATION REQUIRED: Show network call patterns and caching strategy
```

---

## 🚦 PHASE 5 GATE — MANDATORY CHECKLIST

```
PHASE 5 GATE CHECKLIST:
  □ [P1] Startup performance analyzed
  □ [P2] Memory management verified
  □ [P3] CPU and thread usage checked
  □ [P4] Disk I/O performance assessed
  □ [P5] UI rendering performance verified
  □ [P6] Network performance checked
  □ Minimum 8 code citations provided
  □ Files examined list produced
```
