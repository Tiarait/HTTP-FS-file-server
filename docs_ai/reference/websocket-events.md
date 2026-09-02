# Live events WebSocket

Endpoint: **`WS /api/files/events`** (`LiveEventHub`).

Requires **`canRead`**. Push-only — the server sends JSON text frames; the client does not need to send commands (except opening/closing the socket).

The web file manager uses this to refresh listings after remote changes and to show device notices.

**Automation:** connect with the same session cookie as REST — see [api-automation.md](api-automation.md).

---

## Event types (server → client)

| `type` | Fields | When |
|--------|--------|------|
| `list.changed` | `path` | Folder contents changed (upload, delete, rename, move, WebDAV, etc.) |
| `upload.blocked` | `error`, `message` | Upload rejected (often `Error#21` low space) |
| `notice` | `id`, `level`, `on`, optional `percent` | Device notice toggled (battery, storage, …) |
| `battery` | `percent`, `charging` | Phone battery update for status bar |
| `prefs.reset` | `user` | Server cleared that user's web prefs |
| `auth.revoked` | `user` | User sessions invalidated (password change, admin action) |

### Examples

```json
{ "type": "list.changed", "path": "storage/Download" }
```

```json
{ "type": "upload.blocked", "error": "Error#21", "message": "No free space on the disk" }
```

```json
{ "type": "notice", "id": "low_storage", "level": "warn", "on": true }
```

```json
{ "type": "battery", "percent": 42, "charging": false }
```

```json
{ "type": "prefs.reset", "user": "alice" }
```

```json
{ "type": "auth.revoked", "user": "alice" }
```

---

## Server hub API

| Call | Effect |
|------|--------|
| `LiveEventHub.register(session)` | Track socket |
| `unregister(session)` | Remove on close |
| `broadcastListChanged(relativePath)` | `list.changed` |
| `broadcastUploadBlocked(message, error)` | `upload.blocked` |
| `broadcastNotice(…)` | `notice` |
| `broadcastBattery(…)` | `battery` |
| `broadcastPrefsReset(user)` | `prefs.reset` |
| `broadcastAuthRevoked(user)` | `auth.revoked` |

`StorageLiveEvents` calls `broadcastListChanged` after storage mutations.

---

## Separate sockets

| Endpoint | Purpose |
|----------|---------|
| `WS /api/files/status` | Text `online` / `offline` keepalive (Client “check connection” toggle) |
| `WS /api/files/search` | File name search — [api-routes.md](api-routes.md) |
| `WS /api/files/size-ws` | Folder size walk with progress |
| `WS /api/files/download-zip` | [websocket-zip.md](websocket-zip.md) |
| `WS /api/files/transfer` | Binary chunked put/get |

Do not confuse **`/api/files/events`** with **`/api/files/status`**.

---

## Client implementation

Web client: `client_HttpFS/src/composables/useEventsSocket.ts`.

Integrators: parse JSON text frames; reconnect on close; pass session cookie on upgrade when password protection is on.

---

## Related

- [api-routes.md](api-routes.md)
- [api-automation.md](api-automation.md)
