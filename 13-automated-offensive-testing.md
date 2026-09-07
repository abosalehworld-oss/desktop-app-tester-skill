# Phase 13: Automated Offensive Security Testing 🔴

> **Objective:** Replace manual penetration testing with automated AI-driven offensive security tools.
> The AI agent installs, configures, runs, and analyzes results from real security testing tools
> that actively attack the application — not just read its code. This is the DYNAMIC testing layer
> that complements the static analysis performed in Phases 1-12.

---

## ⚠️ IMPORTANT: This Phase is NON-NEGOTIABLE

> This phase uses **real security tools** that perform **actual attacks** against your app binary.
> It is NOT static analysis — it is dynamic offensive testing.
> **Do NOT skip this phase.** Do NOT claim "tools are unavailable" without attempting installation first.

---

## 🚨 ANTI-CHEATING RULES FOR THIS PHASE

### Rule OT-A: MANDATORY `run_command` PROOF
> Every tool execution MUST be performed via an actual `run_command` / terminal tool call.
> You MUST show the **real terminal output** — not text you wrote from memory.
> **Writing fake tool output = CHEATING.** If you cannot run the command, mark as MANUAL_CHECK.
> The user can verify by checking the tool call history in the conversation.

### Rule OT-B: MAXIMUM 2 MANUAL_CHECK ALLOWED
> You may mark **at most 2 checks** as MANUAL_CHECK due to tool unavailability.
> If 3 or more tools fail to install, you MUST troubleshoot or try alternative installation
> methods (pip, npm, brew, choco, scoop, docker, direct download).
> **Claiming all tools are unavailable = PHASE INVALID. Start over.**

### Rule OT-C: HACKER MINDSET R6 ON ALL FIXES
> If you fix any finding in this phase, you MUST apply Rule R6 (Hacker Mindset Verification):
> 1. Can the fix be bypassed?
> 2. Does the fix create a new vulnerability?
> 3. Would a penetration tester find this fix adequate?
> **No R6 verification = fix is NOT accepted.**

### Rule OT-D: MANDATORY DATABASE FRESHNESS
> Before running ANY scan, you MUST update the tool's vulnerability database first.
> Run these update commands and show the output:
> ```
> # Trivy database
> trivy --download-db-only
> # OWASP Dependency-Check DB
> dependency-check --updateonly
> ```
> You MUST also run a **web search** for "latest [tool-name] version [current-year]" to verify
> you are using the most current version. Scanning with outdated databases = scanning blind.
> **If the database update fails, document the error and mark the check as PARTIAL.**

---

## 🛠️ TOOL INSTALLATION

Before running any tests, install the required tools:

### Windows
```powershell
# Sigcheck - Microsoft Sysinternals (code signing verification)
Invoke-WebRequest -Uri "https://live.sysinternals.com/sigcheck.exe" -OutFile sigcheck.exe

# OWASP Dependency-Check
Invoke-WebRequest -Uri "https://github.com/jeremylong/DependencyCheck/releases/latest" -OutFile dependency-check.zip

# PE-bear / PE-sieve (binary analysis)
# Download from: https://github.com/hasherezade/pe-bear/releases
# Download from: https://github.com/hasherezade/pe-sieve/releases

# Trivy (vulnerability scanner)
choco install trivy  # or scoop install trivy
```

### macOS/Linux
```bash
# Trivy
brew install trivy  # macOS
apt install trivy   # Debian/Ubuntu

# OWASP Dependency-Check
brew install dependency-check
```

### Cross-Platform (Electron/Tauri/Flutter Desktop)
```bash
# npm audit for Electron
npm audit --json

# cargo audit for Tauri
cargo install cargo-audit
cargo audit

# For .NET
dotnet list package --vulnerable
```

---

## 📋 PHASE 13 CHECKS

### [OT1] Code Signing Verification
```
Action: Verify all executables and DLLs are properly signed
Commands:
  # Windows
  sigcheck.exe -e -u -s "C:\path\to\app\directory"
  # macOS
  codesign --verify --deep --strict /path/to/app.app
  spctl --assess --type exec /path/to/app.app
Analyze:
  - Unsigned executables or DLLs
  - Expired certificates
  - Self-signed vs CA-signed
  - Timestamp presence (for long-term validity)
  - EV certificate vs standard
Minimum citations: 5 findings from sigcheck/codesign output
```

### [OT2] DLL Hijacking / Library Injection Test
```
Action: Test for DLL search order hijacking vulnerabilities
Commands:
  # List DLLs loaded by the application
  listdlls.exe [app-name]  # Sysinternals
  # Check for writable DLL directories in PATH
  # Check if app loads DLLs from current directory
  procmon.exe /BackingFile log.pml  # Monitor file access
Analyze:
  - DLLs loaded from user-writable paths
  - Missing DLLs that could be planted
  - Unsigned third-party DLLs
  - DLL side-loading opportunities
Minimum citations: 4 findings
```

### [OT3] Dependency Vulnerability Scan
```
Action: Scan all dependencies for known CVEs
Commands:
  # Trivy filesystem scan
  trivy fs --security-checks vuln /path/to/project
  # OWASP Dependency-Check
  dependency-check --project "AppName" --scan /path/to/project
  # .NET
  dotnet list package --vulnerable --include-transitive
  # Electron
  npm audit --json
  npx electron-builder --check-updates
  # Tauri
  cargo audit
Analyze:
  - Critical CVEs (CVSS >= 9.0)
  - High CVEs (CVSS >= 7.0)
  - Outdated framework versions
  - Electron version vulnerabilities
Minimum citations: 5 findings
```

### [OT4] Binary Security Analysis
```
Action: Analyze compiled binary for security weaknesses
Commands:
  # Windows PE Analysis
  # Check ASLR, DEP, CFG, SafeSEH
  dumpbin /headers app.exe  # MSVC
  pe-bear app.exe  # Visual analysis
  # Check for debug symbols in release
  sigcheck -a app.exe
Analyze:
  - ASLR (Address Space Layout Randomization) enabled?
  - DEP (Data Execution Prevention) enabled?
  - CFG (Control Flow Guard) enabled?
  - SafeSEH enabled?
  - Debug symbols stripped?
  - Stack canaries present?
Minimum citations: 4 findings
```

### [OT5] Sensitive Data Exposure Test
```
Action: Search for sensitive data in the installed application
Commands:
  # Search for hardcoded secrets
  findstr /s /i "password api_key secret token private" *.exe *.dll *.config *.json
  # Check app data directories for plaintext storage
  dir /s "%APPDATA%\[AppName]" 
  # Check registry entries
  reg query "HKCU\Software\[AppName]" /s
Analyze:
  - Passwords in config files
  - API keys in binary strings
  - Sensitive data in registry (unencrypted)
  - Temp files with sensitive content
  - Log files with PII
Minimum citations: 4 findings
```

### [OT6] Auto-Update Security
```
Action: Verify auto-update mechanism is secure
Check:
  - Update channel uses HTTPS?
  - Update packages are digitally signed?
  - Signature verified before applying update?
  - Rollback mechanism exists?
  - Update server certificate pinned?
Minimum citations: 3 findings
```

---

## 🔧 REMEDIATION WITHIN THIS PHASE

Unlike Phases 1-12, you MAY fix Critical and High findings discovered in this phase immediately.
For each fix:
1. Show the tool output that identified the vulnerability
2. Apply the fix
3. Re-run the specific tool to verify the fix worked
4. Document before/after results

---

## ⚠️ TOOL UNAVAILABILITY PROTOCOL

If a tool genuinely cannot be installed (e.g., restricted environment, OS mismatch):
1. **Document exactly what you tried** and the error message
2. **Perform manual equivalent checks** using grep, find, and file analysis
3. **Mark the check as PARTIAL** in the gate report
4. **Never mark as PASS** if the tool couldn't run — mark as MANUAL_CHECK

---

## 📊 PHASE 13 GATE REPORT

```
╔══════════════════════════════════════════════════════╗
║  PHASE 13 GATE — Automated Offensive Testing         ║
╠══════════════════════════════════════════════════════╣
║ [OT1] Code Signing:          [PASS/FAIL/PARTIAL]    ║
║ [OT2] DLL Hijacking:         [PASS/FAIL/PARTIAL]    ║
║ [OT3] Dependency Vulns:      [PASS/FAIL/PARTIAL]    ║
║ [OT4] Binary Security:       [PASS/FAIL/PARTIAL]    ║
║ [OT5] Sensitive Data:        [PASS/FAIL/PARTIAL]    ║
║ [OT6] Auto-Update Security:  [PASS/FAIL/PARTIAL]    ║
╠══════════════════════════════════════════════════════╣
║ Tools Successfully Run:      [N/6]                   ║
║ Critical Findings:           [N]                     ║
║ Findings Fixed:              [N]                     ║
║ Remaining Risks:             [N]                     ║
╠══════════════════════════════════════════════════════╣
║ VERDICT: [PROCEED TO PHASE 14 / BLOCK — FIX FIRST]  ║
╚══════════════════════════════════════════════════════╝
```

> ⛔ **STOP — Do NOT proceed to Phase 14 until the user confirms.**

---

## 📌 MINIMUM CITATION REQUIREMENTS

| Check | Minimum Citations |
|-------|-------------------|
| OT1 - Code Signing | 5 |
| OT2 - DLL Hijacking | 4 |
| OT3 - Dependencies | 5 |
| OT4 - Binary Security | 4 |
| OT5 - Sensitive Data | 4 |
| OT6 - Auto-Update | 3 |
| **Total** | **25** |

> Every citation MUST include: tool name, exact output snippet, file/component affected, and severity.
