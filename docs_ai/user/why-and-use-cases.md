# Why HttpFS? Use cases

Short answer for “what is this app for?”:

**HTTP FS turns your Android phone, tablet, or TV into a temporary file server on your local network.** Other devices open a browser (or WebDAV/FTP) and work with files that live on the Android device — no USB cable, no cloud upload, no PC acting as the middleman.

Official site: [https://httpfs.pages.dev/](https://httpfs.pages.dev/) · Creator: [https://tiar.pages.dev/](https://tiar.pages.dev/) · Visual tour: [screenshots.md](screenshots.md)

---

## When it fits

| You want… | HttpFS helps by… |
|-----------|------------------|
| Move photos from phone → PC without cable | Start server → open `/files/` on the PC → download or drag-drop |
| Share a trip album with people in the room | Same Wi‑Fi (or phone hotspot) → QR / share URL → guests browse |
| Edit a document that only exists on the phone | Open it in the web file manager or mount WebDAV |
| Dump camera / drone / GoPro files onto a spare Android | Point Shared folder at the SD card; upload from laptop |
| Automate backups / Home Assistant | REST API or WebDAV — [../reference/api-automation.md](../reference/api-automation.md) (rclone + HA examples) |
| Use an old phone / TV box as a cheap LAN share | Leave it on Wi‑Fi with battery exemption + notification |

## When it does **not** fit

- Permanent internet-facing “cloud storage for anyone” (not designed for that; risky if port-forwarded)
- Full NAS with RAID, apps store, media center OS
- Real-time multi-user collaborative editing of one document
- Replacing Google Photos / Dropbox sync across continents

Prefer a dedicated NAS or cloud for always-on internet access. Prefer USB / Nearby Share for one-off transfers if both devices are in your hands and the network is hostile.

---

## Typical scenarios

### 1. Cable-free phone → PC file transfer

**Story:** You have hundreds of photos/videos on the phone and a laptop on the same Wi‑Fi.

1. Install Free or Pro → start the server on Home.
2. On the PC, open the URL from **Copy** / **QR** / notification.
3. In the browser file manager, open `DCIM` / `Download` → download or zip selection.

Optional: enable **WebDAV** and map a drive (macOS Finder / Windows client). Recipe: [recipes.md](recipes.md) A / C.

### 2. Share files with guests (hotspot, no home Wi‑Fi)

**Story:** Hotel / train / park — no shared Wi‑Fi, but people need a PDF or photos from your phone.

1. Turn on the phone **hotspot**; guests join it.
2. Start HttpFS → **Refresh** address → show **QR**.
3. Guests open the URL and download (set **Guest mode** or Read-only if you don’t want uploads).

Recipe: [recipes.md](recipes.md) B · Password: recipe D.

### 3. Family “photo dump” box

**Story:** One old Android phone sits on charging at home; family drops photos into a shared folder from their laptops.

1. Narrow **Shared folder** to e.g. `Download/FamilyInbox`.
2. Create users with Write (Pro: multiple users) or anonymous Write only on trusted LAN.
3. Start on boot + battery exemption so it survives overnight.

Safety: [safety-and-faq.md](safety-and-faq.md).

### 4. Android TV as the living-room file hub

**Story:** TV is always on the LAN; USB stick or internal storage holds movies.

1. Install on Android TV → wizard → **Start server** from guided Home.
2. Open `/files/` from a phone or PC on the same network; or WebDAV from a media player that supports it.

TV UI: [ui-map.md](ui-map.md).

### 5. Developer / reverse engineer on the go

**Story:** You need to pull APKs, logs, or project folders from a test phone without `adb pull` every time.

1. Shared folder → project directory or `Download`.
2. Browser or `curl` / scripts against `/api/files` with session cookie.
3. Optional HTTPS for untrusted café Wi‑Fi (self-signed or Let’s Encrypt).

Automation: [../reference/api-automation.md](../reference/api-automation.md).

### 6. Sync a PC folder with a phone folder

**Story:** Keep `~/Work/notes` on the laptop mirrored with a folder on the phone while you’re on the same network.

1. Serve the UI over **HTTPS** (or use localhost via a tunnel/proxy) so the browser allows folder picker.
2. Chrome/Edge → `/files/` → right-click folder → **Sync…**.

Details: [web-client.md](web-client.md) § Folder sync · Recipe N in [recipes.md](recipes.md).

### 7. RSS of a downloads / podcast folder

**Story:** New files appear in a folder; another device polls RSS.

1. Leave **RSS** ON (default).
2. Point a reader at `http://IP:PORT/rss/` or a subpath.

### 8. Monitor the phone while serving files

**Story:** You’re running a long upload and want battery / free space without unlocking the phone.

1. Enable Server → Interfaces → **Info**.
2. Open `/info/` (or use the in-app Info tab).
3. Or poll `GET /info/?format=json` from a script.

---

## How to answer “why should I install this?”

One paragraph you can reuse:

> HTTP FS is a local file server for Android. Start it on your phone or TV, open the link on another device on the same Wi‑Fi (or your hotspot), and browse, upload, or download files in a web UI — or mount WebDAV. It’s for cable-free transfers, sharing in the room, and light LAN automation — not a replacement for cloud storage on the public internet.

Setup: [getting-started.md](getting-started.md) · Store links: [free-vs-pro.md](free-vs-pro.md).
