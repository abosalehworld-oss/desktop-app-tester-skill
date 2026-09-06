# Phase 7: API & Network Resilience 🌐

> **Objective:** Test all network-dependent functionality for robustness, error handling,
> and offline behavior. Desktop apps MUST work gracefully when network is unavailable,
> slow, or intermittent — unlike web apps, users expect desktop apps to work offline.

---

## 📋 NETWORK CHECKS

### CHECK N1: API Communication Layer
```
WHAT TO CHECK:
  ❑ Is there a centralized HTTP client/service? (not scattered fetch calls)
  ❑ Are API base URLs configurable? (not hardcoded)
  ❑ Is authentication handled centrally? (token injection, refresh)
  ❑ Are request/response models typed? (not raw JSON manipulation)
  ❑ Is API versioning handled?
  ❑ Are content types set correctly?

COMMON BUGS:
  🐛 Each screen creates its own HTTP client instance
  🐛 API base URL hardcoded, can't switch environments
  🐛 Token refresh race condition → multiple refresh requests
  🐛 No request deduplication → same request sent 5 times
  🐛 Response not validated against expected schema

CITATION REQUIRED: Show HTTP client setup and request patterns
```

### CHECK N2: Error Handling & Retry Logic
```
WHAT TO CHECK:
  ❑ Are HTTP errors handled by status code? (4xx vs 5xx)
  ❑ Is there retry logic with exponential backoff?
  ❑ Are retries limited? (max attempts)
  ❑ Are non-retryable errors identified? (400, 401, 403)
  ❑ Are timeout values set for all requests?
  ❑ Is the user notified of persistent failures?

COMMON BUGS:
  🐛 All errors show generic "Something went wrong"
  🐛 Infinite retry loop on 400 (client error — won't change)
  🐛 No timeout → request hangs forever
  🐛 Retry on 401 without token refresh → infinite loop
  🐛 Rate limiting (429) not handled → app gets blocked

CITATION REQUIRED: Show error handling for at least 3 API calls
```

### CHECK N3: Offline Mode & Data Sync
```
WHAT TO CHECK:
  ❑ Does the app detect network availability?
  ❑ Can core features work offline?
  ❑ Is offline data cached locally? (SQLite, file-based cache)
  ❑ Is data synced when connectivity returns?
  ❑ Are sync conflicts handled? (local edit vs server edit)
  ❑ Is the user informed of offline status?

COMMON BUGS:
  🐛 App crashes immediately when network unavailable
  🐛 No offline indicator — user thinks actions are saved
  🐛 Sync creates duplicates when reconnecting
  🐛 Offline cache never expires → stale data shown
  🐛 Conflict resolution deletes user's local changes

CITATION REQUIRED: Show offline detection and caching strategy
```

### CHECK N4: WebSocket & Real-time Connections
```
WHAT TO CHECK:
  ❑ Is WebSocket connection authenticated?
  ❑ Is automatic reconnection implemented?
  ❑ Is reconnection backoff exponential? (not hammering server)
  ❑ Are heartbeat/ping-pong messages implemented?
  ❑ Is message ordering guaranteed?
  ❑ Are stale connections detected and refreshed?
  ❑ Is message queue handled during disconnection?

COMMON BUGS:
  🐛 WebSocket reconnects every 100ms → server overload
  🐛 Messages lost during brief disconnection
  🐛 No heartbeat → connection silently drops after NAT timeout
  🐛 WebSocket never reconnects after sleep/wake
  🐛 Real-time data shows after page/view already closed

CITATION REQUIRED: Show WebSocket setup and reconnection logic
```

### CHECK N5: Download & Upload Handling
```
WHAT TO CHECK:
  ❑ Are large downloads resumable?
  ❑ Is download progress shown to user?
  ❑ Are downloads cancellable?
  ❑ Is disk space checked before download?
  ❑ Are upload sizes validated before sending?
  ❑ Are partial upload/download failures handled?
  ❑ Is download integrity verified? (checksum/hash)

COMMON BUGS:
  🐛 100MB download can't be resumed → starts over on failure
  🐛 No progress indicator for upload → user cancels thinking it's stuck
  🐛 Download fills disk → app crashes
  🐛 Upload of 5GB file attempted without size check → OOM
  🐛 Downloaded file corrupted, no hash verification

CITATION REQUIRED: Show download/upload implementations
```

### CHECK N6: Proxy & Corporate Network Support
```
WHAT TO CHECK:
  ❑ Does the app respect system proxy settings?
  ❑ Can proxy be configured manually? (host, port, auth)
  ❑ Does the app work behind corporate firewalls?
  ❑ Are proxy credentials stored securely?
  ❑ Is proxy authentication handled? (NTLM, Basic, Kerberos)
  ❑ Does the app handle SSL inspection/MITM proxies?

COMMON BUGS:
  🐛 App ignores system proxy → fails in corporate environments
  🐛 No proxy authentication support → 407 errors
  🐛 SSL inspection proxy breaks certificate pinning
  🐛 Proxy credentials stored in plaintext

CITATION REQUIRED: Show proxy configuration handling
```

---

## 🚦 PHASE 7 GATE — MANDATORY CHECKLIST

```
PHASE 7 GATE CHECKLIST:
  □ [N1] API communication layer reviewed
  □ [N2] Error handling and retry logic verified
  □ [N3] Offline mode and data sync checked
  □ [N4] WebSocket/real-time connections tested
  □ [N5] Download/upload handling verified
  □ [N6] Proxy and corporate network support checked
  □ Minimum 10 code citations provided
  □ Files examined list produced
```
