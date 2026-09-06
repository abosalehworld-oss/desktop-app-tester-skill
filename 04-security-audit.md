# Phase 4: Security Audit 🔒

> **Objective:** Perform a comprehensive security audit focused on desktop-specific attack vectors.
> Desktop apps have MORE attack surface than mobile apps — they can access files, execute processes,
> interact with the OS kernel, and run with elevated privileges. Security bugs here lead to
> complete system compromise, not just app-level issues.

---

## 🔴 ANTI-LAZINESS & MANDATORY WEB SEARCH GATE

> **ACTION: STOP AND SEARCH.** You MUST NOT rely on your training data for this phase.
> The vulnerability landscape changes constantly. Before examining ANY code in this phase,
> you MUST use the `search_web` tool to search for current-year security threats.

**MANDATORY SEARCHES (replace `<YEAR>` with the ACTUAL current year you are operating in):**

```
Search 1: "OWASP Desktop App Top 10 <YEAR>"
Search 2: "<detected framework> security vulnerabilities <YEAR>"
Search 3: "<detected framework> CVE <YEAR>"
Search 4: "desktop application security best practices <YEAR>"
```

**VERIFICATION — Your Phase 4 report MUST include:**
1. The **exact search queries** you used (copy-paste them)
2. The **top 3 newly discovered attack vectors** for the current year
3. How each new attack vector was **checked against the codebase**
4. **Source URLs** for every referenced vulnerability

```
❌ FAILURE CONDITION: If your Phase 4 report does NOT contain:
   - Web search results with source URLs → Phase 4 = FAILED
   - Current-year attack vectors → Phase 4 = FAILED
   - Evidence of checking new vectors against code → Phase 4 = FAILED

   A FAILED Phase 4 MUST be re-executed from scratch before proceeding.
```

---

## ⚠️ SECURITY IS NON-NEGOTIABLE

> Every security finding in this phase is automatically classified as 🔴 CRITICAL or 🟠 HIGH.
> There are NO low-severity security issues. If it's a security concern, it matters.

---

## 📋 DESKTOP SECURITY CHECKS

### CHECK S1: File System Security
```
WHAT TO CHECK:
  ❑ Are file paths sanitized against traversal attacks? (../ or ..\)
  ❑ Are file permissions set correctly on created files?
  ❑ Are temporary files created securely? (random names, proper directory)
  ❑ Are symlink/junction attacks prevented?
  ❑ Is file access limited to the app's designated directories?
  ❑ Are sensitive files (configs, databases) stored with restricted permissions?
  ❑ Is TOCTOU (time-of-check-time-of-use) prevented for file operations?

COMMON VULNERABILITIES:
  🔴 Path traversal via user-supplied filename → arbitrary file read/write
  🔴 Temp file with predictable name → symlink attack → overwrite system file
  🔴 App creates files with world-readable permissions → other users/apps read secrets
  🔴 Config file stores passwords in plaintext
  🔴 Log files contain sensitive data accessible by other users

SEVERITY: 🔴 CRITICAL for file traversal, 🟠 HIGH for permissions
CITATION REQUIRED: Show ALL file I/O operations with path handling
```

### CHECK S2: Process & Command Execution
```
WHAT TO CHECK:
  ❑ Does the app execute external commands? (shell, system calls)
  ❑ Are command arguments sanitized against injection?
  ❑ Are child processes started with minimal privileges?
  ❑ Are spawned process outputs sanitized before display?
  ❑ Is there a whitelist of allowed commands?
  ❑ Are pipes/redirections handled safely?

COMMON VULNERABILITIES:
  🔴 Command injection via unsanitized user input in system() or exec()
  🔴 Shell=true with user input → attacker runs arbitrary commands
  🔴 Spawned process inherits parent's elevated privileges
  🔴 Process output displayed in UI without sanitization → XSS (Electron)
  🔴 DLL search order hijacking → malicious DLL loaded

SEVERITY: 🔴 CRITICAL
CITATION REQUIRED: Show ALL process/command execution code
```

### CHECK S3: DLL/Library Injection & Loading
```
WHAT TO CHECK:
  ❑ Are dynamic libraries loaded from absolute paths?
  ❑ Is the DLL search order hardened? (SetDllDirectory, manifest)
  ❑ Are native library dependencies signed/verified?
  ❑ Are plugin/extension loading mechanisms secure?
  ❑ Is code signing verified before loading external code?

COMMON VULNERABILITIES:
  🔴 DLL search order hijacking — app loads malicious DLL from current directory
  🔴 Plugin system loads unsigned code → arbitrary code execution
  🔴 Library loaded from user-writable path
  🔴 No integrity check on loaded native modules

SEVERITY: 🔴 CRITICAL
CITATION REQUIRED: Show ALL dynamic library loading code
```

### CHECK S4: Inter-Process Communication (IPC)
```
WHAT TO CHECK:
  ❑ Are IPC channels authenticated? (named pipes, sockets, COM)
  ❑ Is data exchanged over IPC validated and sanitized?
  ❑ Are IPC endpoints restricted to authorized processes?
  ❑ Electron: Is contextBridge used instead of nodeIntegration?
  ❑ Electron: Are IPC messages validated in main process?
  ❑ Tauri: Are commands properly exposed with minimal scope?

COMMON VULNERABILITIES:
  🔴 Electron: nodeIntegration=true → renderer can execute system commands
  🔴 Named pipe accessible by any process → unauthorized data access
  🔴 IPC messages not validated → injection attacks via IPC
  🔴 Electron: webSecurity disabled → XSS leads to system compromise
  🔴 COM object registered without authentication → hijackable

SEVERITY: 🔴 CRITICAL for Electron nodeIntegration, 🟠 HIGH otherwise
CITATION REQUIRED: Show ALL IPC channel definitions and message handlers
```

### CHECK S5: Data Protection & Encryption
```
WHAT TO CHECK:
  ❑ Are passwords/tokens stored using OS keychain? (Windows Credential Manager / macOS Keychain / Linux Secret Service)
  ❑ Is sensitive data encrypted at rest?
  ❑ Are encryption keys hardcoded? (MUST NOT be)
  ❑ Is the encryption algorithm modern? (AES-256, not DES/RC4)
  ❑ Are database files encrypted? (SQLCipher for SQLite)
  ❑ Is clipboard data cleared after sensitive copy?
  ❑ Are memory-resident secrets cleared when no longer needed?

COMMON VULNERABILITIES:
  🔴 API key hardcoded in source code
  🔴 Password stored in plaintext config file
  🔴 SQLite database with user data completely unencrypted
  🔴 Encryption key derived from predictable value
  🔴 Sensitive data left on clipboard indefinitely
  🔴 Secrets visible in process memory dumps

SEVERITY: 🔴 CRITICAL for hardcoded secrets, 🟠 HIGH for missing encryption
CITATION REQUIRED: Show ALL credential/secret storage and encryption code
```

### CHECK S6: Network Security
```
WHAT TO CHECK:
  ❑ Are ALL network connections using HTTPS/TLS?
  ❑ Is certificate validation enabled? (not disabled for debugging)
  ❑ Is certificate pinning implemented for critical connections?
  ❑ Are API keys sent in headers, not URL parameters?
  ❑ Is the app vulnerable to man-in-the-middle attacks?
  ❑ Are WebSocket connections authenticated?
  ❑ Are download integrity checks performed? (hash verification)

COMMON VULNERABILITIES:
  🔴 TLS certificate validation disabled (common dev leftover)
  🔴 API key sent as URL query parameter → logged in server access logs
  🔴 Auto-update downloads update over HTTP without signature verification
  🔴 WebSocket connection without authentication → data injection
  🔴 No proxy support → corporate users can't use app

SEVERITY: 🔴 CRITICAL for TLS bypass, 🟠 HIGH for missing pinning
CITATION REQUIRED: Show ALL network connection configurations
```

### CHECK S7: Authentication & Authorization
```
WHAT TO CHECK:
  ❑ Are login credentials transmitted securely?
  ❑ Are tokens stored securely? (not in plain file)
  ❑ Is session timeout implemented?
  ❑ Is token refresh handled automatically?
  ❑ Are local admin functions protected? (not just hidden UI)
  ❑ Is biometric/OS-level auth supported for sensitive operations?
  ❑ Is brute-force protection implemented?

COMMON VULNERABILITIES:
  🔴 Auth token stored in plain JSON file readable by other apps
  🔴 No session expiry → stolen token works forever
  🔴 Admin features hidden but accessible by modifying config
  🔴 Password sent over unencrypted channel
  🔴 "Remember me" stores actual password, not token

SEVERITY: 🔴 CRITICAL for token exposure, 🟠 HIGH for missing timeout
CITATION REQUIRED: Show auth flow, token storage, and session management
```

### CHECK S8: Privilege & Permission Management
```
WHAT TO CHECK:
  ❑ Does the app request only necessary permissions?
  ❑ Does the app run with minimal privileges? (not requiring admin)
  ❑ Are elevated operations isolated? (UAC prompts when needed, not at startup)
  ❑ Is there privilege escalation protection?
  ❑ Are file system ACLs respected?
  ❑ Is the app's installation directory protected from modification?

COMMON VULNERABILITIES:
  🔴 App requires admin rights to run when it shouldn't
  🔴 Installer sets world-writable permissions on app directory → DLL hijacking
  🔴 App runs privileged service that accepts unauthenticated commands
  🔴 Self-update mechanism runs as SYSTEM without validation
  🔴 User can modify app binaries in install directory

SEVERITY: 🔴 CRITICAL for privilege escalation, 🟠 HIGH for unnecessary privileges
CITATION REQUIRED: Show privilege requirements and elevation requests
```

### CHECK S9: Logging & Information Disclosure
```
WHAT TO CHECK:
  ❑ Are sensitive data excluded from logs? (passwords, tokens, PII)
  ❑ Are log files stored with restricted permissions?
  ❑ Are error messages user-friendly? (no stack traces shown to user)
  ❑ Is debug logging disabled in release builds?
  ❑ Are crash reports sanitized before transmission?
  ❑ Does the app expose internal paths or versions unnecessarily?

COMMON VULNERABILITIES:
  🟠 Log file contains plaintext passwords
  🟠 Stack traces shown to user in error dialogs
  🟠 Crash report contains user's file paths and username
  🟠 Debug console accessible in release build (Electron DevTools)
  🟠 Error messages reveal database schema or API structure

SEVERITY: 🟠 HIGH
CITATION REQUIRED: Show logging configuration and error display code
```

### CHECK S10: Update & Code Integrity
```
WHAT TO CHECK:
  ❑ Is the auto-update mechanism using HTTPS with signature verification?
  ❑ Are update packages signed? (code signing certificate)
  ❑ Is the update server authenticated? (certificate pinning)
  ❑ Can the app detect tampered binaries?
  ❑ Is rollback supported for failed updates?
  ❑ Are third-party plugins/extensions sandboxed?

COMMON VULNERABILITIES:
  🔴 Auto-update downloads over HTTP → man-in-the-middle code injection
  🔴 Update package not signed → attacker replaces with malware
  🔴 No integrity check on application binaries → modified app runs normally
  🔴 Plugin system executes arbitrary code without sandboxing
  🔴 Update server compromise → malware pushed to all users

SEVERITY: 🔴 CRITICAL for unsigned updates, 🟠 HIGH for missing integrity
CITATION REQUIRED: Show update mechanism and signature verification code
```

---

## 🚦 PHASE 4 GATE — MANDATORY CHECKLIST

```
PHASE 4 GATE CHECKLIST:
  □ [S1] File system security verified
  □ [S2] Process/command execution checked
  □ [S3] DLL/library injection assessed
  □ [S4] IPC security verified
  □ [S5] Data protection and encryption reviewed
  □ [S6] Network security checked
  □ [S7] Authentication and authorization verified
  □ [S8] Privilege management assessed
  □ [S9] Logging and information disclosure checked
  □ [S10] Update and code integrity verified
  □ Web search results included with URLs
  □ Current-year attack vectors checked
  □ Minimum 15 code citations provided
  □ Files examined list produced
```

### ⛔ STOP POINT — SECURITY IS CRITICAL
**If ANY 🔴 CRITICAL security finding is discovered, it MUST be flagged as a
RELEASE BLOCKER in the Phase 10 final report. No exceptions.**
