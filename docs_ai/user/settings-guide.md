# Settings guide

Settings are grouped into **App**, **Server**, and **Client**. Some items appear only when a parent switch is on, or only in Free / Pro.

Defaults below match the current app (**1.2.7**). Items that need the server **stopped** say so. Labels below match the English UI where possible.

On Android TV, **Quick settings** from Home opens a short subset (Port, Shared folder, LAN name, Password, HTTPS, Buffer) without walking the full Settings tree.

Goal recipes: [recipes.md](recipes.md) · Screen map: [ui-map.md](ui-map.md).

---

## 1. App

### Language

App UI language. Default: system. Changing it reloads settings UI.

### Start on boot

Auto‑start after reboot. Default: OFF. On Xiaomi / Huawei / Oppo / Vivo / Honor / etc. you may also need OEM auto‑start permission.

### Start on open

Start when you open the app. Default: OFF. Otherwise use **Start** on Home.

### Keep screen on

While the app is open. Default: OFF.

### Logs / All logs

Request logging for the Logs tab / TV Logs menu. Logs default ON; “All logs” is OFF and only shown when Logs is on. Logs screen may also offer date format, max size, clear, share, export.

### Haptics / Blur in dialogs

UI feedback. Blur defaults ON on Android 12+, OFF on older devices.

### Ads & privacy / Hide ads (Free)

Consent screen; rewarded ad hides banner/interstitial until the **app process ends**.

### Battery permissions

Request ignore‑battery‑optimizations so the server is less likely to be killed. Recommended for long runs.

### Folder access / Removable storage

Grant SAF access for SD cards and removable volumes. Use after **All storages** or when a volume needs re‑granting.

### Reset web client settings

Clears browser/web-UI preferences stored for the file manager (does not wipe server root or Android app prefs wholesale).

### Clear app data

Wipes HttpFS settings and app cache only — **not** your photos/documents on the device.

---

## 2. Server

### Interfaces (toggles)

Which **services** are exposed (can usually toggle while running):

| Setting | Default | Notes |
|---------|---------|-------|
| **Files UI** | ON | Browser file manager at `/files/` |
| **WebDAV** | OFF | `/webdav/` |
| **RSS** | ON | `/rss/` |
| **Info** | OFF | HTTP status page at `/info/` (not the in-app Info tab) |
| **FTP** | OFF | Separate listener; **FTP port** default **2121** (must ≠ HTTP port) |

### Name (Pro)

Server display name. Default: “HTTP FS”.

### Address

Bind / advertise mode:

- **All interfaces** (default) — listen `0.0.0.0`; advertise best LAN IP (not VPN `tun*`).
- **Only localhost** — `127.0.0.1` only.
- **Specific IP** — that interface; list is filtered (Wi‑Fi vs hotspot); sticky pick kept.

Also available from **Home → Address**. Details: [../features/network-address.md](../features/network-address.md).

### On IP change

Only for a **specific** address: **Auto-rebind** (default) / **Keep running** / **Stop server**.  
**All interfaces**: losing shareable LAN can **auto-stop** the server.

### Port

HTTP(S) listen port. Free **8080**, Pro **8081**. Change when stopped.

### Shared folder

Server root (UI title). Default: internal shared storage.

- **All storages (multi‑volume):** picker → go up to `/` → confirm. Paths like `/files/storage/…`. Grant removable volumes under App → Removable storage.
- Change when stopped.

### Landing file / Hard redirect

Optional file served as homepage. Hard redirect always sends `/` to that file. Change when stopped.

### LAN name

Optional friendly name on the **same Wi‑Fi** (mDNS / Bonjour style), default host **`httpfs`** → e.g. `http://httpfs.local:8080`.

- Works only on the same LAN as the phone (not mobile data / other networks / internet).
- Port is part of the URL unless 80/443.
- macOS/iOS usually resolve `.local`; Windows may need Bonjour.
- Confirm dialog explains the limitation when enabling.

Also in TV **Quick settings**.

### CORS

Allow browser requests from other sites. Default: OFF. For advanced / embedded web use.

### Cache

Faster Files and WebDAV listings. Default: ON.

### Max upload

Largest upload in MB; **0 = unlimited**. Very large values can fill the phone or fail on weak devices.

### Hide dotfiles

Hide names starting with `.` in listings. Default: OFF.

### Server type (engine)

**Auto** (default), **CIO**, or **Netty**. **HTTPS** forces Netty. Prefer Auto unless debugging.

### HTTP/2

Shown when **HTTPS** is on. Default: **OFF**. HTTPS ALPN (h2). Off is more stable for video/large files; enable only if needed.

### Buffer / Socket buffer

- **Buffer** — transfer/copy buffer (main knob).
- **Socket buffer** — TCP send/receive size (separate); prefer Recommended.

### Idle timeout (sec)

Seconds with no traffic (10–3600). Default **60**.

### Max connections

Parallel HTTP clients (8–1024). Default **128**.

### Blocked IPs

Deny listed client IPs. Free: capped; Pro: unlimited. Live apply.

### Redirects (Pro, experimental)

Map a path/URL to another. Nested Redirects screen.

### Password

Require login. Default: OFF. Change when stopped.

**When Password ON:** Users, Guest mode, Session, Logout (Read-only / Guest access hidden).  
**When Password OFF:** Read-only and/or Guest access (anonymous Read/Write).

### Users (password on)

Accounts with Read / Write / Read+Write and optional home folder. Free ≈ one user; Pro: multiple. WebDAV = HTTP Basic.

### Guest mode (password on)

Default: OFF. Anonymous guests can only **view files** (not full manager browsing). Logged-in users keep their roles.

### Read-only (password off)

Blocks writes for everyone. Default: OFF.

### Guest access (password off, not Read-only)

Anonymous **Read** and/or **Write**. Default: Read. UI title is **Guest access**.

### Session (min) / Logout (password on)

Session lifetime (default 60). Logout disconnects sessions immediately.

### HTTPS

Serve `https://`. Default: OFF. Forces Netty. See [https-certificates.md](https-certificates.md).

### Certificates (HTTPS on)

Certificate library: Auto (self‑signed), Import, Let’s Encrypt (DNS‑01). Select one profile; Export PEM when available.

---

## 3. Client

These toggles affect **what the server exposes to browsers**, not the in-browser ⚙ panel (theme, sort, etc.). Browser UI map: [web-client.md](web-client.md).

### Name

Client/device label used in places in the UI.

### Welcome dialog / Message (Pro for full text)

Optional message shown to browser visitors.

### Device notices

Battery and storage alerts in **connected browsers**. Default: **ON**.

### Installable web app (PWA)

Allow browsers to install the file manager as an app. Default: OFF. Needs a **trusted HTTPS** certificate (e.g. Let’s Encrypt). Self‑signed from the app usually **won’t** work for install; reload the page after enabling.

### Hide files (Pro)

Paths hidden from listings / WebDAV. Not a password substitute.

### Folder sizes

Recursive folder sizes in listings. Default: **OFF** (“battery expense”).

### Thumbnails

Image/video thumbs in the web UI. Default: **ON**. Clear cache when shown.

### Check connection socket

WebSocket online probe for the browser client. Default: **OFF**.

---

## Related

- Recipes: [recipes.md](recipes.md)
- UI map (Android app): [ui-map.md](ui-map.md)
- Web file manager: [web-client.md](web-client.md)
- [getting-started.md](getting-started.md)
- [https-certificates.md](https-certificates.md)
- [safety-and-faq.md](safety-and-faq.md)
- [connect-clients.md](connect-clients.md)
- Preference keys: [../reference/settings-keys.md](../reference/settings-keys.md)
