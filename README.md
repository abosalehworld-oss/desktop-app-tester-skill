# 🖥️ Desktop App Tester Skill

[![Frameworks](https://img.shields.io/badge/Frameworks-Electron%20%7C%20WPF%20%7C%20Qt%20%7C%20Tauri-blue)](SKILL.md)

---

## ✨ What This Skill Does

When installed, your AI agent becomes a **professional desktop app QA engineer** capable of:

- 🏗️ **Architecture Review** — Design patterns, dependencies, IPC, window management, threading
- 🎨 **UI/UX Testing** — Multi-window, keyboard shortcuts, DPI scaling, accessibility, drag & drop
- 🧠 **Logic & Functional Testing** — Business rules, CRUD, file I/O, concurrency, printing
- 🔒 **Security Audit** — DLL injection, IPC exploits, privilege escalation, code signing *(mandatory web search)*
- ⚡ **Performance Analysis** — Startup time, memory, CPU, disk I/O, rendering
- 📱 **Platform Compatibility** — Windows/macOS/Linux, filesystem, i18n
- 🌐 **API & Network Resilience** — Offline mode, proxy, WebSocket, downloads
- 💾 **State & Data Management** — Local DB, config files, cache, session
- 🛑 **Error & Crash Prevention** — Global handlers, auto-save, recovery, crash dumps
- ✅ **Final Delivery + Fresh-Eyes Re-Analysis** — Executive summary, cross-reference matrix, mandatory second pass
- 🔧 **Structured Remediation (Phase 11)** — Sprint-based fixes with Hacker Mindset R6 post-fix verification
- 📦 **Distribution & Commercial Readiness (Phase 12)** — Installer, code signing, auto-update, licensing, store submission
- 🔴 **Automated Offensive Testing (Phase 13)** — AI runs real security tools (Sigcheck, Trivy, PE analysis) with mandatory database freshness and anti-cheating enforcement
- 🛡️ **Pre-Delivery Sentry Validation (Phase 14)** — Sentry setup per framework, real testing, crash reporting, deployment verdict

---

## 🚀 Install in 30 Seconds

```bash
npx skills add https://github.com/abosalehworld-oss/desktop-app-tester-skill
```

Or globally:

```bash
npx skills add https://github.com/abosalehworld-oss/desktop-app-tester-skill -g -y
```

---

## 🎯 Supported Frameworks

| Framework | Detection |
|-----------|-----------|
| **Electron** | `package.json` + electron, `main.js/ts` |
| **WPF** | `*.xaml`, `App.xaml.cs`, `*.csproj` with WPF |
| **WinForms** | `*.Designer.cs`, `Form1.cs` |
| **.NET MAUI** | `MauiProgram.cs`, `*.csproj` with MAUI |
| **Qt (C++/Python)** | `*.pro`, `CMakeLists.txt` with Qt, `*.ui` |
| **JavaFX** | `pom.xml` with javafx, `*.fxml` |
| **GTK** | `meson.build`, `*.glade`, `*.ui` with GTK |
| **Tauri** | `tauri.conf.json`, `src-tauri/` |
| **Flutter Desktop** | `pubspec.yaml` + desktop targets |
| **SwiftUI (macOS)** | `*.swift` with `@main struct`, `*.xcodeproj` |
| **Avalonia** | `*.axaml`, `*.csproj` with Avalonia |
| **PyQt / PySide** | `*.py` with `from PyQt` or `from PySide` |

---

## 🛡️ Anti-Laziness Enforcement System (10 Layers)

This skill is engineered to **force thorough review** even from AI agents that tend to skip steps.

### 1. Proof-of-Work Citations (116 Minimum)
Every finding MUST include exact file path, line numbers, and copied code snippet.

### 2. Mandatory Phase Gates (14 Gates)
Each of the 14 phases has a structured checklist that must be completed with evidence before the AI can proceed.

### 3. Mandatory STOP Points
After each phase, the AI MUST output a structured report and wait for acknowledgment.

### 4. Cross-Reference Verification Matrix
The final phase requires a matrix proving every file was examined — if any phase shows 0 files, the review is invalid.

### 5. Fresh-Eyes Re-Analysis (4-Layer Reminder)
After all phases, the AI performs a second independent pass — proven to catch Critical bugs missed on first pass.

### 6. Hacker Mindset R6
After every fix, the AI verifies from an attacker's perspective that no new vulnerabilities were introduced.

### 7. Anti-Premature-Celebration
AI is blocked from declaring "done" after remediation — offensive testing and Sentry phases still remain.

### 8. 🆕 Mandatory `run_command` Proof (Phase 13)
Every security tool execution MUST be via real terminal commands — fabricating tool output = cheating.

### 9. 🆕 Maximum 2 MANUAL_CHECK (Phase 13)
AI cannot claim all tools are unavailable — at most 2 checks can be manual.

### 10. 🆕 Mandatory Database Freshness (Phase 13)
AI MUST update vulnerability databases before scanning + web search for latest tool versions.

---

## 📂 Skill Structure

```
desktop-app-tester-skill/
├── SKILL.md                              ← Main entry + enforcement rules
├── 01-architecture-review.md             ← Phase 1: Patterns, DI, windows, threads
├── 02-ui-ux-testing.md                   ← Phase 2: Multi-window, DPI, keyboard, a11y
├── 03-logic-functional-testing.md        ← Phase 3: CRUD, file I/O, concurrency
├── 04-security-audit.md                  ← Phase 4: DLL injection, IPC, privileges
├── 05-performance-optimization.md        ← Phase 5: Memory, CPU, startup, rendering
├── 06-platform-compatibility.md          ← Phase 6: Win/Mac/Linux, filesystem
├── 07-api-network-resilience.md          ← Phase 7: Offline, proxy, WebSocket
├── 08-state-data-management.md           ← Phase 8: Local DB, config, cache
├── 09-error-crash-prevention.md          ← Phase 9: Handlers, auto-save, recovery
├── 10-final-delivery-checklist.md        ← Phase 10: Summary + Fresh-Eyes
├── 11-remediation-execution.md           ← Phase 11: Sprint fixes + R6 verification
├── 12-distribution-commercial-readiness.md ← Phase 12: Installer, signing, update
├── 13-automated-offensive-testing.md  ← Phase 13: Sigcheck, DLL test, Trivy, PE analysis
├── 14-pre-delivery-sentry-validation.md  ← Phase 14: Sentry + real testing
└── ref-common-bugs-database.md           ← 150+ desktop bug patterns reference
```

---

## 💬 How to Use with Your AI Agent

### Start a Full Review
```
Review my Electron project using the desktop-app-tester skill.
Start from Phase 1 and follow ALL mandatory gates. Do not skip phases.
My project is at: [path to your project]
```

### Security-Only Review
```
Using the desktop-app-tester skill, run ONLY Phase 4 (Security Audit)
on my WPF project. Check for DLL injection, IPC exploits, and privilege escalation.
```

---

## 🤝 Contributing

Found a common bug pattern not in the database? Have a framework-specific check to add?  
PRs are welcome! Please follow the existing format in `ref-common-bugs-database.md`.

---

## 📜 License

MIT — free to use, share, and modify.

---

## 🌟 Activate on skills.sh

```bash
npx skills add https://github.com/abosalehworld-oss/desktop-app-tester-skill -g -y
```

> Built with ❤️ for desktop developers who care about quality.

---

## 👤 Author

**Mohamed Saleh** 🇪🇬  
Egypt — Mobile & Software Developer  
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Mohamed%20Saleh-blue?logo=linkedin)](https://www.linkedin.com/in/mr-mohamed-saleh/)
