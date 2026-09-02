# REST & WebSocket API reference

Base mount: **`/api/files`** (`fmApi` = `api`, `fmDirName` = `files`).

All paths below are relative to `http://HOST:PORT` or `https://HOST:PORT`.

**Automation recipes (curl, cookies, paths):** [api-automation.md](api-automation.md)  
**Path rules:** [storage-paths.md](storage-paths.md)  
**Live events:** [websocket-events.md](websocket-events.md)  
**ZIP socket:** [websocket-zip.md](websocket-zip.md)

---

## Quick index

| Method | Path | Purpose |
|--------|------|---------|
| GET | `/api/files/list` | Folder listing JSON |
| GET | `/api/files/tree` | Tree JSON |
| GET | `/api/files/stat` | One item metadata |
| GET | `/api/files/size` | Folder size (sync walk) |
| GET/HEAD | `/api/files/file` | Download or metadata |
| POST | `/api/files/upload` | Upload bytes |
| POST | `/api/files/create` | New file/folder |
| POST | `/api/files/save` | Overwrite text file |
| POST | `/api/files/delete` | Delete |
| POST | `/api/files/rename` | Rename |
| POST | `/api/files/move` | Move |
| POST | `/api/files/copy` | Copy |
| POST | `/api/files/password` | Change own password |
| GET/PUT/DELETE | `/api/files/prefs` | Per-user web UI prefs |
| WS | `/api/files/search` | Live file search |
| WS | `/api/files/size-ws` | Folder size with progress |
| WS | `/api/files/events` | Live list/notice events |
| WS | `/api/files/status` | Online probe |
| WS | `/api/files/download-zip` | Build & stream ZIP |
| WS | `/api/files/transfer` | Chunked put/get |
| GET | `/info/` | Device info (toggle) — JSON with `?format=json` |
| GET | `/logout` | End session (password on) |
| POST | `/login` | Form login (password on) |

---

## Authentication

| Mode | REST `/api/files/*` | WebDAV |
|------|---------------------|--------|
| **Password off** | Anonymous; rights from global Read/Write / Read-only | Same |
| **Password on** | **Session cookie** after `POST /login` | **HTTP Basic** (username/password) |

The REST API does **not** accept Basic auth for `/api/files/*` — use a session cookie (see [api-automation.md](api-automation.md)).

WebSocket endpoints use the same session as the browser (cookie on the upgrade request).

Guest mode (password on): anonymous users have limited `/files/` access; most `/api/files/*` calls need login.

---

## Common errors

Volume / SAF failures:

```json
{ "error": "VOLUME_UNKNOWN|VOLUME_UNMOUNTED|…", "message": "…" }
```

Permission denied:

```json
{ "error": "Error#9", "message": "Permission denied" }
```

Hidden path (Pro):

```json
{ "error": "Error#66", "message": "File is hidden" }
```

Low disk space:

```json
{ "error": "Error#21", "message": "…" }
```

---

## Path parameter `path`

Used in query strings and JSON bodies. Rules:

- **Logical path** under the server root — no `/files/` prefix.
- URL-encode segments (`2%262` for `2&2`).
- **Empty** `path` = root (volume list in multi-volume mode, or shared folder root in single-folder mode).
- **Multi-volume:** first segment is volume id, e.g. `storage/DCIM/IMG_001.jpg`, `sd:ABCD-1234/Music/track.mp3`.
- See [storage-paths.md](storage-paths.md).

---

## HTTP GET — read

### `GET /api/files/list`

Directory listing for the web UI and scripts.

| Query | Default | Meaning |
|-------|---------|---------|
| `path` | `""` | Logical folder path |
| `type` | `all` | Entry filter (`all`, etc.) |
| `sort` | `name` | `name`, `type`, `size`, `date` |
| `order` | `asc` | `asc` / `desc` |
| `show_parent` | `0` | Include `[..]` when `1` |

**Response:** JSON with `current` (folder meta), `list` (entries), optional `nav` at volume root, `freeBytes`.

Each list entry includes: `name`, `url`, `directory`, `size`, `size_max`, `date`, `modifiedAt`, `inside`, `writable`, `canCreate`, optional `createdAt`, volume fields when applicable.

### `GET /api/files/tree`

Tree-shaped listing.

| Query | Default | Meaning |
|-------|---------|---------|
| `path` | `""` | Root of tree |
| `recursion` | `0` | `1` = recurse |
| `maxLvl` | `3` | Depth limit |
| `lvl` | auto | Starting level |

### `GET /api/files/stat`

Metadata for one path (file or folder).

| Query | Meaning |
|-------|---------|
| `path` | Logical path (required) |

Returns: `name`, `path`, `directory`, `size`, `inside`, `mimeType`, `date`, `modifiedAt`, `createdAt`, `permissions`, `writable`, `canCreate`, optional `volumeId` / `volumeLabel`.

### `GET /api/files/size`

**Synchronous** recursive folder size (can be slow on large trees).

| Query | Meaning |
|-------|---------|
| `path` | Folder logical path |

Returns: `path`, `size`, `size_max`, `files`, `folders`, `sizeComputed: true`.

Prefer **`WS /api/files/size-ws`** for UI/progress; use this GET for simple scripts on small folders.

### `GET|HEAD /api/files/file`

Download or probe a **file**.

| Query | Meaning |
|-------|---------|
| `path` | File logical path (required) |
| `info` | `1` = metadata only (no body); also implied for `HEAD` |

- Supports **`Range:`** for partial content / resume.
- Response headers: `Content-Type`, `Content-Length`; with `info=1`: **`X-File-Size`** (also used for upload resume — partial files named `.downloaded`).
- Streamable media may get `Content-Disposition: inline`.

### Stubs (404)

`GET /api/files`, `/api/files/`, `/api/` → `{ "error": "Empty request" }`.

---

## HTTP POST — mutations

All mutation bodies are **`Content-Type: application/json`** unless noted. Require **write** permission.

### `POST /api/files/delete`

```json
{ "names": ["a.txt", "SubFolder"], "path": "storage/Documents" }
```

Deletes each **basename** under `path`. Empty `names` with non-empty `path` may delete the folder itself (if allowed). Cannot delete volume root or virtual storage root.

### `POST /api/files/rename`

```json
{ "path": "storage/Documents/old.txt", "name": "new.txt", "overwrite": false }
```

- `path` — current item logical path  
- `name` — new basename only (no `/`)  
- `overwrite` — optional `true` / `1`

### `POST /api/files/move`

```json
{
  "paths": ["storage/a.txt", "storage/b.txt"],
  "pathEnd": "storage/Archive",
  "overwrite": false,
  "destNames": ["a_renamed.txt", null]
}
```

- `paths` — full logical paths of items to move  
- `pathEnd` — destination **folder**  
- `destNames` — optional; same length as `paths` (per-item destination basename)

### `POST /api/files/copy`

Same shape as **move** (`paths`, `pathEnd`, `overwrite`, optional `destNames`).

### `POST /api/files/save`

```json
{ "path": "storage/notes.txt", "content": "file text…" }
```

Overwrite a text file. Hidden paths → `Error#66`.

### `POST /api/files/create`

```json
{ "path": "storage/Documents", "name": "NewFolder/" }
```

- `name` ending with **`/`** → directory  
- otherwise → empty file  

Parent folder in `path`; new name in `name`.

### `POST /api/files/upload`

Raw body = file bytes. **Not JSON.**

| Query | Meaning |
|-------|---------|
| `path` | Parent folder logical path |
| `name` | File name |
| `overwrite` | `true` / `1` / `yes` to replace |

**Resume:** send `Content-Range: bytes START-END/TOTAL` (or open-ended total). Server writes via `.downloaded` sidecar then renames on completion.

**Probe offset:** `GET /api/files/file?path=…/file.partial&info=1` → `X-File-Size`.

Rejects upload to multi-volume virtual root (`INVALID_PATH`).

---

## Account / prefs

### `POST /api/files/password`

Logged-in user changes own password.

```json
{ "current": "old", "password": "new" }
```

### `GET /api/files/prefs`

Returns web-client prefs blob for the session user. **404** if no file or guest.

```json
{ "v": 1, "updatedAt": 1234567890, "prefs": { … } }
```

### `PUT /api/files/prefs`

Same JSON body. **409 Conflict** with server blob if `updatedAt` is stale.

### `DELETE /api/files/prefs`

Reset prefs; triggers `prefs.reset` on `/api/files/events`.

---

## WebSockets

Connect to `ws://HOST:PORT/api/files/…` or `wss://…` with the same session cookie when password is on.

### `WS /api/files/search`

Requires `canRead`.

**Client → server (text JSON):**

```json
{ "type": "search", "query": "*.jpg", "path": "storage/DCIM", "scope": "current" }
```

| Field | Meaning |
|-------|---------|
| `query` | Plain prefix, glob (`*.jpg`), exact with dot, or `/regex/flags` |
| `path` | Start folder (logical) |
| `scope` | `current` or `all` |

```json
{ "type": "stop" }
```

Cancel in-flight search.

**Server → client (text JSON):**

| type | Fields | Meaning |
|------|--------|---------|
| `progress` | `scanned`, `hits`, `queue`, `folder` | ~every 250 ms / per batch |
| `hit` | `item` | Listing-shaped file entry |
| `done` | `scanned`, `hits`, `truncated` | Finished |
| `error` | `error` or `message` | Failed |

Limits: **300** hits, **8000** folders scanned → `truncated: true`.

### `WS /api/files/size-ws`

Requires `canRead`.

**Client → server:**

```json
{ "type": "size", "path": "storage/Download" }
```

```json
{ "type": "stop" }
```

**Server → client:**

| type | Fields |
|------|--------|
| `progress` | `path`, `size`, `size_max`, `files`, `folders` |
| `done` | same + `sizeComputed: true` |
| `error` | `message` |

### `WS /api/files/events`

Requires `canRead`. Push-only (client does not send commands).

| type | Payload | Meaning |
|------|---------|---------|
| `list.changed` | `path` | Folder listing changed — refresh |
| `upload.blocked` | `error`, `message` | Upload failed (e.g. no space) |
| `notice` | `id`, `level`, `on`, optional `percent` | Device notice |
| `battery` | `percent`, `charging` | Phone battery |
| `prefs.reset` | `user` | User prefs cleared on server |
| `auth.revoked` | `user` | Session invalidated |

Details: [websocket-events.md](websocket-events.md).

### `WS /api/files/status`

Optional keepalive. Sends text `online` periodically; `offline` when server stops. Gated by Client → **Check connection socket** (default off).

### `WS /api/files/download-zip`

Requires `canRead`. Protocol: [websocket-zip.md](websocket-zip.md).

### `WS /api/files/transfer`

Binary chunked upload/download (v1). Requires write for `put`, read for `get`.

**Client → server (text JSON):**

```json
{
  "v": 1,
  "op": "put",
  "path": "storage/Download",
  "name": "big.bin",
  "size": 1048576,
  "offset": 0,
  "end": 1048575,
  "overwrite": true
}
```

| Field | Meaning |
|-------|---------|
| `op` | `put` or `get` |
| `path` | Parent folder (`put`) or file path (`get`) |
| `offset` / `end` | Byte range (inclusive end) |

Server replies `{ "ready": true }`, then:

- **put:** client sends **one binary frame** with exact byte count → `{ "status": "partial"|"complete" }`
- **get:** server sends binary frame → `{ "status": "ok", "size": N }`

Useful when many parallel transfers are needed (browser connection limits).

---

## Related HTTP (not under `/api/files`)

| URL | Notes |
|-----|-------|
| `/files/…` | HTML SPA + direct file URLs; `?type=thumb`, `?type=download` |
| `/webdav/…` | WebDAV — [webdav-methods.md](webdav-methods.md); Basic auth |
| `/rss/…` | Folder RSS — [../features/rss.md](../features/rss.md) |
| `/info/` | Device dashboard JSON — `Accept: application/json` or `?format=json` |
| `/login`, `/logout` | Browser session |

---

## Implementation note

Routes live under `app/.../server/routes/api/`. If this doc drifts, verify against source or a live server on your build.
