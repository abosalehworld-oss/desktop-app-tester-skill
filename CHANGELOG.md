# Changelog

All notable changes to the Desktop App Tester Skill are documented here.

## [1.2.0] - 2026-09-08

### Added
- **Phase 13: Automated Offensive Security Testing** (`13-automated-offensive-testing.md`)
  - Code signing verification (Sigcheck/codesign)
  - DLL hijacking and library injection testing
  - Dependency vulnerability scanning (Trivy/OWASP Dependency-Check)
  - Binary security analysis (ASLR, DEP, CFG)
  - Sensitive data exposure testing
  - Auto-update security verification
  - 25 minimum citations from real tool outputs
  - Tool unavailability protocol with manual fallback
- Sentry validation renumbered to Phase 14
- Total minimum citations raised from 91 to 116
- Total phases raised from 13 to 14

## [1.0.0] - 2026-09-06

### Added
- Initial release of the comprehensive desktop app testing skill
- 13 structured review phases with mandatory gates and STOP points
- Anti-laziness enforcement system with 6 built-in mechanisms:
  - Proof-of-work citations (91 minimum total across all phases)
  - Mandatory phase gate checklists
  - Mandatory STOP points between phases
  - Cross-reference verification matrix in final phase
  - Fresh-Eyes re-analysis (4-layer reminder system for second independent pass)
  - Hacker Mindset R6 post-fix verification from attacker's perspective
- Full security coverage: DLL injection, IPC exploits, privilege escalation, code signing
- Mandatory web search for current-year CVEs in Phase 4 (Security Audit)
- Support for 12 desktop frameworks:
  - Electron
  - WPF
  - WinForms
  - .NET MAUI
  - Qt (C++/Python)
  - JavaFX
  - GTK
  - Tauri
  - Flutter Desktop
  - SwiftUI (macOS)
  - Avalonia
  - PyQt / PySide
- 150+ real-world desktop bug patterns in `ref-common-bugs-database.md`
- Phase 11: Structured Remediation with sprint-based fixes
- Phase 12: Distribution & Commercial Readiness (installer, signing, auto-update, licensing)
- Phase 13: Pre-Delivery Sentry Validation (real testing with step-by-step guidance)
- Executive summary with health score (0-100) and release recommendation
- Severity classification system (Critical / High / Medium / Low)
- Framework auto-detection from project files
- Bilingual support (English + Arabic)
