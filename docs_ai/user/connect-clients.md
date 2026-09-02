# Connect from other devices

Replace `IP` and `PORT` with the values shown in the app / notification.

| Service | URL / endpoint | Default |
|---------|----------------|---------|
| Landing / redirect | `http://IP:PORT/` or `https://IP:PORT/` | Always |
| Web file manager | `http://IP:PORT/files/` | Files UI **ON** |
| WebDAV | `http://IP:PORT/webdav/` | WebDAV **OFF** — enable in Server → Interfaces |
| RSS | `http://IP:PORT/rss/` or a subfolder under `/rss/…` | RSS **ON** |
| Device / server info | `http://IP:PORT/info/` | Info **OFF** — enable in Server → Interfaces |
| REST API (advanced) | `http://IP:PORT/api/…` | [../reference/api-automation.md](../reference/api-automation.md) · [../reference/api-routes.md](../reference/api-routes.md) |
| FTP | `ftp://IP:FTP_PORT/` | FTP **OFF**; default FTP port **2121** |
| LAN name (optional) | `http://httpfs.local:PORT/` (host configurable) | LAN name **OFF** — same Wi‑Fi only |

With **password protection**, the browser asks you to log in; WebDAV clients use **HTTP Basic** (username/password). **Guest mode** (when password is on) limits anonymous browsing to viewing files.

Use **Home → Copy / QR / Share** (or the notification) so you do not mistype the address. After VPN or network changes, tap **Refresh** or reopen **Address**.

Setup recipes: [recipes.md](recipes.md). UI map: [ui-map.md](ui-map.md). Screenshots: [screenshots.md](screenshots.md).

## LAN name (`.local`)

When **LAN name** is enabled, clients on the **same Wi‑Fi** can try `http://httpfs.local:PORT/` (default host `httpfs`; port required unless 80/443). Not a substitute for Copy/QR on networks that don’t resolve mDNS. See [settings-guide.md](settings-guide.md) and recipe M in [recipes.md](recipes.md).

## Browser

1. Same Wi‑Fi as the Android device (or join this device’s hotspot).
2. Open the home / files URL (`/files/`).
3. Upload / download / rename as allowed by permissions.

**UI guide** (layout, search, sync, keyboard shortcuts, account prefs): [web-client.md](web-client.md).

## WebDAV examples

Enable **Settings → Server → WebDAV** first (default is off).

- **macOS Finder:** Go → Connect to Server → `https://IP:PORT/webdav/` (or `http://…`).
- **Windows:** Map network drive / third‑party WebDAV tools with the same URL.
- **Mobile file managers:** Add a WebDAV account with host, port, path `/webdav/`, and credentials if needed.

Some clients prefer HTTPS. If you use a self‑signed cert, trust/export it — [https-certificates.md](https-certificates.md).

## Info page

When **Info** is enabled, `/info/` shows a lightweight device/server dashboard in the browser (JSON available for tooling). This is separate from the in-app **Info** tab on phone / TV.

## FTP

When **FTP** is enabled, clients connect to the configured FTP port (not the HTTP port). Keep the FTP port different from the HTTP port. Prefer WebDAV or the web UI when possible; FTP is optional for legacy clients.

## RSS

Point an RSS reader at `/rss/` (or a folder path) to see items derived from that directory listing. Useful for media folders; not a full podcast CMS.

## Multi‑volume (“All storages”)

Paths include a volume id, e.g. `/files/storage/DCIM/…`. WebDAV and RSS follow the same idea. Grant SD access under App → Storage permissions if a volume shows as needing permission.

## Widgets (Pro)

Home‑screen widgets and a launcher shortcut can start/stop or show status — see [../features/widgets-shortcuts.md](../features/widgets-shortcuts.md).
