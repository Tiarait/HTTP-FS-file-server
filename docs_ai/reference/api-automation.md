# API automation guide

How to call HttpFS from **curl**, scripts, Home Assistant, n8n, or other tools without the browser UI.

**Full route list:** [api-routes.md](api-routes.md)  
**Path rules:** [storage-paths.md](storage-paths.md)  
**WebDAV alternative:** [webdav-methods.md](webdav-methods.md) (Basic auth, good for simple file sync)

---

## Base URL

Replace placeholders:

```text
BASE=http://192.168.1.20:8080
API=$BASE/api/files
```

Free default port **8080**, Pro **8081**. Use `https://` when SSL is on.

Test reachability:

```bash
curl -sS "$API/list?path=" | head -c 200
```

---

## Authentication

### Password protection **off**

No login. Anonymous rights follow Server → Permissions (Read / Write) or Read-only.

```bash
curl -sS "$API/list?path=storage/Download"
```

### Password protection **on**

REST `/api/files/*` uses a **session cookie**, not Basic auth.

**1. Login (save cookie):**

```bash
curl -sS -c cookies.txt -b cookies.txt \
  -X POST "$BASE/login" \
  -d "username=alice&password=SECRET"
```

Optional long session: add `remember=1` to the form body.

**2. Call API with cookie:**

```bash
curl -sS -b cookies.txt "$API/list?path=storage/DCIM"
```

**3. Logout:**

```bash
curl -sS -b cookies.txt "$BASE/logout"
```

**WebDAV** on the same server accepts **HTTP Basic** with the same username/password — often easier for generic tools:

```bash
curl -u alice:SECRET "$BASE/webdav/storage/Download/"
```

WebSockets need the cookie on the upgrade request (e.g. `websocat -H "Cookie: …"`).

---

## Paths for automation

| Root mode | Example `path` | Meaning |
|-----------|----------------|---------|
| Single shared folder | `Photos/vacation` | Under the configured root |
| Multi-volume | `` (empty) | List volumes |
| Multi-volume | `storage/DCIM/Camera` | Primary internal storage |
| Multi-volume | `sd:XXXX-YYYY/Music` | SD card volume id |

Rules:

- Never prefix with `/files/` in API `path` — use logical path only.
- URL-encode: `curl -G --data-urlencode "path=storage/My Folder/file.txt" "$API/stat"`.
- User home folders: paths are still logical from server root; per-user roots are applied server-side.

See [storage-paths.md](storage-paths.md).

---

## Common recipes

### List a folder

```bash
curl -sS -b cookies.txt -G "$API/list" \
  --data-urlencode "path=storage/Download" \
  --data-urlencode "sort=name" \
  --data-urlencode "order=asc"
```

Parse JSON: `list[]` entries with `name`, `url`, `directory`, `size`, `writable`.

### Download a file

Direct (no `/api`):

```bash
curl -sS -b cookies.txt -o photo.jpg \
  "$BASE/files/storage/DCIM/photo.jpg"
```

Or API with Range:

```bash
curl -sS -b cookies.txt -o part.bin \
  -H "Range: bytes=0-1048575" \
  -G "$API/file" --data-urlencode "path=storage/big.bin"
```

### Upload a file

```bash
curl -sS -b cookies.txt -X POST \
  -G "$API/upload" \
  --data-urlencode "path=storage/Download" \
  --data-urlencode "name=report.pdf" \
  --data-urlencode "overwrite=true" \
  --data-binary @report.pdf
```

**Resume** after interruption:

```bash
OFFSET=$(stat -f%z partial.pdf 2>/dev/null || stat -c%s partial.pdf)
TOTAL=5000000
curl -sS -b cookies.txt -X POST \
  -G "$API/upload" \
  --data-urlencode "path=storage/Download" \
  --data-urlencode "name=big.zip" \
  -H "Content-Range: bytes ${OFFSET}-$((TOTAL-1))/${TOTAL}" \
  --data-binary @partial.pdf
```

Check partial size: `GET "$API/file?path=…/file.downloaded&info=1"` → header **`X-File-Size`**.

### Create folder

```bash
curl -sS -b cookies.txt -X POST "$API/create" \
  -H "Content-Type: application/json" \
  -d '{"path":"storage/Download","name":"NewFolder/"}'
```

### Rename

```bash
curl -sS -b cookies.txt -X POST "$API/rename" \
  -H "Content-Type: application/json" \
  -d '{"path":"storage/old.txt","name":"new.txt"}'
```

### Move / copy

```bash
curl -sS -b cookies.txt -X POST "$API/move" \
  -H "Content-Type: application/json" \
  -d '{
    "paths": ["storage/a.txt", "storage/b.txt"],
    "pathEnd": "storage/Archive",
    "overwrite": false
  }'
```

Use `/api/files/copy` with the same JSON shape.

### Delete

```bash
curl -sS -b cookies.txt -X POST "$API/delete" \
  -H "Content-Type: application/json" \
  -d '{"path":"storage/Download","names":["temp.txt","OldDir"]}'
```

### Folder size (small trees)

```bash
curl -sS -b cookies.txt -G "$API/size" \
  --data-urlencode "path=storage/Download"
```

Large trees: use **`WS /api/files/size-ws`** (progress + cancel) — [api-routes.md](api-routes.md).

### File metadata

```bash
curl -sS -b cookies.txt -G "$API/stat" \
  --data-urlencode "path=storage/DCIM/IMG_001.jpg"
```

### Device / server info (JSON)

Requires Server → Interfaces → **Info** ON.

```bash
curl -sS -b cookies.txt -H "Accept: application/json" "$BASE/info/"
# or
curl -sS -b cookies.txt "$BASE/info/?format=json"
```

Battery, storage, server state — useful for monitoring scripts.

### User web prefs (advanced)

Logged-in only:

```bash
curl -sS -b cookies.txt "$API/prefs"
curl -sS -b cookies.txt -X PUT "$API/prefs" \
  -H "Content-Type: application/json" \
  -d '{"v":1,"updatedAt":1,"prefs":{"lang":"en"}}'
```

---

## WebSocket automation

Tools: [websocat](https://github.com/vi/websocat), Python `websocket-client`, Node `ws`.

**Search** (pass cookie if needed):

```bash
websocat -b cookies.txt "$API/search" <<< \
  '{"type":"search","query":"*.mp3","path":"","scope":"all"}'
```

Read text frames: `progress`, `hit`, `done`. Send `{"type":"stop"}` to cancel.

**Live folder changes:**

Connect to `WS $API/events`; on `{"type":"list.changed","path":"storage/Download"}` refresh your local cache or re-fetch list.

**ZIP download:** [websocket-zip.md](websocket-zip.md).

---

## When to use WebDAV instead

| Use REST `/api/files` | Use WebDAV `/webdav` |
|----------------------|----------------------|
| JSON listing tree/stat | Mount as network drive |
| Custom scripts matching web UI | Standard DAV clients (rclone, Windows) |
| Search / events WebSockets | Basic auth everywhere |
| Upload with Content-Range | PROPFIND / PUT native clients |

Many HA integrations prefer WebDAV or plain HTTP GET on `/files/…`.

---

## rclone (WebDAV)

Enable **Settings → Server → Interfaces → WebDAV**. Prefer **Password** + a user with Read/Write. Note the URL from Home (e.g. `http://192.168.1.20:8080`).

### One-shot remote

```bash
# Interactive
rclone config
# n) New remote
# name> httpfs
# Storage> webdav
# url> http://192.168.1.20:8080/webdav/
# vendor> other
# user> alice
# pass> SECRET
```

Or non-interactive:

```bash
rclone config create httpfs webdav \
  url="http://192.168.1.20:8080/webdav/" \
  vendor=other \
  user=alice \
  pass="$(rclone obscure 'SECRET')"
```

HTTPS / self-signed: add `extra_options` / use `https://…` and trust the cert, or for testing only:

```bash
# Not for production — skips TLS verify
rclone config create httpfs webdav \
  url="https://192.168.1.20:8080/webdav/" \
  vendor=other \
  user=alice \
  pass="$(rclone obscure 'SECRET')" \
  insecure_skip_verify=true
```

### Common commands

```bash
rclone lsd httpfs:
rclone ls httpfs:storage/Download
rclone copy ~/Photos httpfs:storage/DCIM/Camera --progress
rclone sync httpfs:storage/Download ~/Backup/phone-download --progress
rclone mount httpfs: ~/mnt/httpfs --vfs-cache-mode writes
```

Multi-volume roots use volume ids in the path (`storage/…`, `sd:…`). Single shared-folder mode paths are relative to that root (no `storage/` prefix). See [storage-paths.md](storage-paths.md).

**Tips:** leave HTTP/2 off unless you need it; large uploads are more reliable with default buffer. Do not expose WebDAV to the internet without HTTPS + strong password.

---

## Home Assistant

HttpFS is a **LAN file server**, not an official HA add-on. Typical uses: monitor the phone, pull a file, or mount storage via WebDAV / REST.

Replace `192.168.1.20:8080` with your device. Enable **Info** under Server → Interfaces if you poll `/info/`.

### Sensor: device info (REST)

Password **off** (or guest-readable Info):

```yaml
# configuration.yaml
sensor:
  - platform: rest
    name: HttpFS battery
    resource: http://192.168.1.20:8080/info/?format=json
    value_template: "{{ value_json.power.batteryPercent | default(value_json.battery.percent) }}"
    json_attributes_path: "$"
    scan_interval: 60
```

Exact JSON keys vary by build — open `/info/?format=json` once and map fields (battery, storage, server running). Prefer a dedicated read-only user if password is on; REST needs a **session cookie**, so for HA prefer either:

1. **Password off** + narrow Shared folder + Read-only, or  
2. **WebDAV** with Basic (HA / `command_line` / AppDaemon), or  
3. A small script that logs in and stores the cookie (advanced).

### Command line: list folder via WebDAV (Basic)

```yaml
command_line:
  - sensor:
      name: HttpFS download listing
      command: >
        curl -sS -u alice:SECRET
        "http://192.168.1.20:8080/webdav/storage/Download/"
      scan_interval: 300
```

(PROPFIND XML is verbose — often better to use `rclone lsjson` or REST `list` from a shell script.)

### REST list with cookie (shell)

```bash
#!/bin/sh
# /config/scripts/httpfs_list.sh
BASE=http://192.168.1.20:8080
COOKIE=/tmp/httpfs.cookie
curl -sS -c "$COOKIE" -b "$COOKIE" -X POST "$BASE/login" \
  -d "username=alice&password=SECRET" >/dev/null
curl -sS -b "$COOKIE" -G "$BASE/api/files/list" \
  --data-urlencode "path=storage/Download"
```

Call from HA `shell_command` / `command_line`.

### Notify when free space is low

Poll `/info/?format=json` or use in-app / browser notices; for HA, compare a free-bytes field from Info JSON or from `GET /api/files/list` (`freeBytes` on the current volume) and fire an automation when below a threshold.

### rclone as a bridge

Many people run **rclone** on the HA host (or a companion machine) with the WebDAV remote above, then use `shell_command` to `rclone copy` / `sync`. That avoids cookie handling in HA.

---

## Error handling

| HTTP | Typical cause |
|------|----------------|
| 401 / Unauthorized | Missing or expired session — re-login |
| 405 + Error#9 | Read-only user or global read-only |
| 404 | Bad path or empty stub URL |
| 409 on PUT prefs | Stale `updatedAt` — GET and merge |
| 507 | Not enough space (WebDAV copy/move) |

Always parse JSON `error` + `message` when present.

---

## Limits to plan for

| Feature | Limit |
|---------|-------|
| Search | 300 hits, 8000 folders scanned |
| Max upload | Server → Max upload (MB); `0` = unlimited |
| Max connections | Server → Max connections (default 128) |
| Session TTL | Server → Session (minutes); sliding while active |
| Blocked IPs | Denied before auth |

---

## Security notes

- Prefer **HTTPS** on untrusted LANs; self-signed needs `-k` in curl or trusted cert.
- Do not expose the server to the internet without strong passwords and narrow root folder.
- Scripts should store credentials outside the repo; use env vars (`HTTPFS_USER`, `HTTPFS_PASS`).
- Guest mode limits anonymous API access — automation should use a named user.

---

## Related

- User connect URLs: [../user/connect-clients.md](../user/connect-clients.md)
- Auth model: [../features/users-auth.md](../features/users-auth.md)
- Web client (what the API powers): [../user/web-client.md](../user/web-client.md)
- Screenshots (Address / WebDAV context): [../user/screenshots.md](../user/screenshots.md)
