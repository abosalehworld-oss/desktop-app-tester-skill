# Phase 13: Automated Offensive Security Testing 🔴

> **Objective:** Replace manual penetration testing with automated AI-driven offensive security tools.
> The AI agent installs, configures, runs, and analyzes results from real security testing tools
> that actively attack the application — not just read its code. This is the DYNAMIC testing layer
> that complements the static analysis performed in Phases 1-12.

---

## ⛔ GATE 0 OF THIS PHASE — RULES OF ENGAGEMENT (BEFORE ANY ACTIVE COMMAND)

> This phase performs ACTIVE testing. Before the first scanner/fuzzer/attack command you MUST:
> 1. Print the **Rules of Engagement** block from `00-agent-integrity-protocol.md` §6 and have the USER fill it.
> 2. Confirm the environment. Default = **local or staging**. Production active scans, load tests,
>    brute force, injection tools at elevated risk levels → a SEPARATE explicit "yes" naming the command.
> 3. Scope = only the targets the user listed. Never scan CDNs, payment/OAuth providers or other third parties.
> 4. Never upload the user's code, binaries or data to third-party online scanners without explicit consent naming the service.
> 5. Ask before installing tools globally; prefer official container images or project-local installs (Phase 0 §8).
>
> **Execution mode B/C (no shell):** do not pretend. Give the user the exact commands, mark each check
> `USER-RUN (pending)`, wait for the pasted output, then analyze it. This is a valid, honest completion path.
>
> **No permission → no active testing.** Perform the static/local-only checks of this phase and record
> the rest as `NOT RUN — no authorization` in the gate report (this is not a failure of yours).

---

## ⚠️ IMPORTANT: This Phase is NON-NEGOTIABLE

> This phase uses **real security tools** that perform **actual attacks** against your app binary.
> It is NOT static analysis — it is dynamic offensive testing.
> **Do NOT skip this phase silently.** Offer it, obtain the Rules of Engagement, then run it inside scope.
> Do NOT claim "tools are unavailable" without real attempts (mode A) — or, in mode B/C, hand the user the commands.

---

## 🚨 ANTI-CHEATING RULES FOR THIS PHASE

### Rule OT-A: MANDATORY `run_command` PROOF
> Every tool execution MUST be performed via an actual `run_command` / terminal tool call.
> You MUST show the **real terminal output** — not text you wrote from memory.
> **Writing fake tool output = CHEATING.** If you cannot run the command, mark as MANUAL_CHECK.
> The user can verify by checking the tool call history in the conversation.

### Rule OT-B: HONEST TOOL COVERAGE (replaces "maximum 2 MANUAL_CHECK")
> In execution mode A (you have a shell), try at least one alternative install path (package manager,
> official container, official release binary) before marking a tool unavailable, and show the real errors.
> If more than 2 tools stay unavailable, report it plainly and give the user the commands to run themselves.
> In mode B/C, every tool check is `USER-RUN (pending)` until the user pastes real output.
> **Never mark PASS for a tool that did not run. Never write tool output you did not receive.**

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
> You MUST also check the tool's official release page / `--version` (and `ref-freshness-registry.md`) to verify
> you are using the most current version. Scanning with outdated databases = scanning blind.
> **If the database update fails, document the error and mark the check as PARTIAL.**

### Rule OT-E: MANDATORY FALSE POSITIVE TRIAGE (BEFORE ANY FIX)
> ⚠️ **This is the MOST CRITICAL safety rule in this phase.**
> Automated security tools produce **FALSE POSITIVES** (fake alerts). Blindly fixing a false
> positive can **BREAK the application** — disable login, corrupt data, or block real users.
>
> **BEFORE fixing ANY finding from ANY tool, you MUST complete this triage for EACH finding:**
>
> **Step 1 — Classify the finding:**
> ```
> | Finding ID | Tool | Verdict | Confidence | Reason |
> |------------|------|---------|------------|--------|
> | OT1-F1 | Trivy | TRUE_POSITIVE / FALSE_POSITIVE / UNCERTAIN | HIGH/MEDIUM/LOW | Why? |
> ```
>
> **Step 2 — MANDATORY WEB SEARCH per finding (ANTI-STALE-KNOWLEDGE):**
> Your training data is OUTDATED. You MUST NOT classify findings based on memory alone.
> For EACH finding, run a web search:
> ```
> search_web("[CVE-ID or vulnerability name] false positive [tool-name] [current-year]")
> search_web("[vulnerability name] [framework/library version] exploit proof of concept [current-year]")
> ```
> - If the internet says this CVE is **confirmed exploitable** in the detected version → TRUE_POSITIVE
> - If the internet says this is a **known false positive** for this tool/version → FALSE_POSITIVE
> - If no clear internet consensus exists → UNCERTAIN (present to user with extra caution)
> **DO NOT trust your training data over live internet results. Your knowledge may be 1-2 years stale.**
>
> **Step 3 — For each TRUE_POSITIVE or UNCERTAIN finding, explain in PLAIN LANGUAGE:**
> ```
> 🔴 FINDING: [Tool] reported [vulnerability name]
> 📍 WHERE: [file:line or endpoint]
> 👶 EXPLAIN LIKE I'M 5: [How can a hacker exploit this? What damage can they do?]
> 🔧 PROPOSED FIX: [What exactly will you change in the code?]
> ⚠️ FIX SIDE EFFECTS: [Will this fix break any feature? Will users notice anything different?]
> 🤔 FALSE POSITIVE RISK: [What is the probability this is a false alarm? Why?]
> 🌐 WEB SEARCH EVIDENCE: [What did the internet say about this CVE/vulnerability?]
> ```
>
> **Step 4 — Present the FULL triage table to the user and STOP.**
> ```
> ⛔ STOP — Do NOT fix anything until the user reviews this triage and says "fix" or "skip" for each finding.
> ```
>
> **FORBIDDEN behaviors:**
> ```
> ❌ NEVER fix a finding without completing the triage table first
> ❌ NEVER say "I fixed 5 vulnerabilities" without user approval on EACH one
> ❌ NEVER assume the user understands security jargon — ALWAYS use plain language
> ❌ NEVER hide the false positive probability — the user NEEDS this to make a decision
> ❌ NEVER batch-fix all findings at once — present them individually for approval
> ❌ NEVER classify a finding as FALSE_POSITIVE without a web search proving it
> ❌ NEVER classify a finding as TRUE_POSITIVE without a web search confirming it
> ```
>
> **WHY THIS EXISTS:** The user operating this skill may NOT be a cybersecurity expert.
> They rely on YOUR analysis to decide what to fix. If you fix a false positive, you may
> break their application. If you skip a true positive, you leave them vulnerable.
> Your training data may be outdated — a vulnerability you "remember" as harmless may now
> have a working exploit. The web search requirement ensures you use CURRENT intelligence.
> **Your triage is the user's ONLY defense. Take it seriously.**

---

## 🛠️ TOOL INSTALLATION

Before running any tests, install the required tools:

### Windows
```powershell
# Sigcheck - Microsoft Sysinternals (code signing verification)
Invoke-WebRequest -Uri "https://live.sysinternals.com/sigcheck.exe" -OutFile sigcheck.exe

# OWASP Dependency-Check — download the release ZIP manually from the official releases page
# https://github.com/jeremylong/DependencyCheck/releases  (the /latest URL is an HTML page, not a ZIP)
# Ask the user before downloading/executing binaries (Phase 0 §8)

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
  spctl --assess --type exec -vv /path/to/app.app
  codesign -dv --verbose=4 /path/to/app.app   # check "flags=0x10000(runtime)" = Hardened Runtime
  xcrun stapler validate /path/to/app.app      # notarization ticket stapled?
Analyze:
  - Unsigned executables or DLLs
  - Expired certificates
  - Self-signed vs CA-signed
  - Timestamp presence (for long-term validity)
  - EV certificate vs standard
Coverage target: 5 cited findings from sigcheck/codesign output — cite EVERY real finding the tool produced; if it produced fewer,
  quote the raw summary line proving the count (Phase 0 §5). Never invent or split findings to reach the target.
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
Coverage target: 4 cited findings — cite EVERY real finding the tool produced; if it produced fewer,
  quote the raw summary line proving the count (Phase 0 §5). Never invent or split findings to reach the target.
```

### [OT3] Dependency Vulnerability Scan
```
Action: Scan all dependencies for known CVEs
Commands:
  # Trivy filesystem scan
  trivy fs --scanners vuln,secret /path/to/project
  # OWASP Dependency-Check
  dependency-check --project "AppName" --scan /path/to/project
  # .NET
  dotnet list package --vulnerable --include-transitive
  # Electron
  npm audit --json
  npx @doyensec/electronegativity -i .     # Electron security misconfiguration scanner
  osv-scanner scan source -r .             # multi-ecosystem CVE scan (no account)
  # Tauri
  cargo audit
Analyze:
  - Critical CVEs (CVSS >= 9.0)
  - High CVEs (CVSS >= 7.0)
  - Outdated framework versions
  - Electron version vulnerabilities
Coverage target: 5 cited findings — cite EVERY real finding the tool produced; if it produced fewer,
  quote the raw summary line proving the count (Phase 0 §5). Never invent or split findings to reach the target.
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
Coverage target: 4 cited findings — cite EVERY real finding the tool produced; if it produced fewer,
  quote the raw summary line proving the count (Phase 0 §5). Never invent or split findings to reach the target.
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
Coverage target: 4 cited findings — cite EVERY real finding the tool produced; if it produced fewer,
  quote the raw summary line proving the count (Phase 0 §5). Never invent or split findings to reach the target.
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
Coverage target: 3 cited findings — cite EVERY real finding the tool produced; if it produced fewer,
  quote the raw summary line proving the count (Phase 0 §5). Never invent or split findings to reach the target.
```

---

## 🔧 REMEDIATION WITHIN THIS PHASE

> ⚠️ **You MUST complete Rule OT-E (False Positive Triage) BEFORE fixing ANY finding.**
> Do NOT skip the triage. Do NOT fix findings without explicit user approval for EACH one.

After the user approves specific findings for fixing (via Rule OT-E triage), for each approved fix:
1. Show the tool output that identified the vulnerability
2. Apply the fix
3. Re-run the specific tool to verify the fix worked
4. Apply Rule OT-C (Hacker Mindset) to verify the fix doesn't create new problems
5. Document before/after results

---

## ⚠️ TOOL UNAVAILABILITY PROTOCOL

> In mode B/C (no shell) every check below is `USER-RUN (pending)` — give exact commands, wait for real output.

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
