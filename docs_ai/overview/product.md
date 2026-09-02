# Product overview

## What is HttpFS?

**HttpFS** (HTTP File Server / HTTP FS) is an Android application that turns a phone, tablet, or Android TV into a **local network file server**.

Other devices on the same Wi‑Fi or hotspot (PC, laptop, phone, media player) can:

- Browse and manage files in a **web file manager** in the browser
- Mount the device as a **WebDAV** share (Finder, Windows Map Network Drive, third-party clients)
- Call a **REST API** (and WebSockets) for automation and the web UI
- Optionally subscribe to an **RSS** feed of a folder
- Optionally open a **device/server info** page
- Optionally use **FTP** for legacy clients
- Use **HTTP** or **HTTPS** (self-signed, imported, or Let’s Encrypt DNS-01)

The Android app is the control plane (start/stop, settings, storage permissions). Remote UX is the bundled web client plus WebDAV/API/FTP.

## Who it is for

- Users who want a quick “NAS-like” share from a spare Android device
- People transferring photos/files without a USB cable
- Sharing files in a room via Wi‑Fi or phone hotspot (QR)
- Developers who need LAN file access / automation without a PC
- Android TV users (guided Home, Leanback settings, D-pad focus)

Longer scenarios: [../user/why-and-use-cases.md](../user/why-and-use-cases.md).

## Distribution

| Flavor | Package id | Default port | Store |
|--------|------------|--------------|-------|
| Free | `tiar.ua.slf` | 8080 | [Play](https://play.google.com/store/apps/details?id=tiar.ua.slf) |
| Pro | `tiar.ua.slf.pro` | 8081 | [Play](https://play.google.com/store/apps/details?id=tiar.ua.slf.pro) |

Both can be installed on the same device. See [free-vs-pro.md](free-vs-pro.md) and the user page [../user/free-vs-pro.md](../user/free-vs-pro.md).

| | |
|--|--|
| Website | [https://httpfs.pages.dev/](https://httpfs.pages.dev/) |
| Creator | [https://tiar.pages.dev/](https://tiar.pages.dev/) |
| Use cases | [../user/why-and-use-cases.md](../user/why-and-use-cases.md) |
| All links | [../user/links.md](../user/links.md) |

## Core user journey

1. Grant storage (and notification) permissions.
2. Optionally pick server root folder / multi-volume mode and which interfaces to expose (Files UI, WebDAV, RSS, Info, FTP).
3. Start on **Home** (or widget / shortcut on Pro).
4. Use the Home card (Refresh / Address / Copy / QR / Share) or the ongoing notification for `http(s)://{ip}:{port}`.
5. Open that URL from another device, or mount WebDAV / FTP as enabled.
6. Stop from the notification or the app when finished.

Phone tabs: **Home**, **Settings**, **Info**, and **Logs** when logging is enabled. TV uses guided Home menus instead of a bottom bar.

Long runs with the screen off use a foreground service (`specialUse`) and optional battery exemption — [../operations/foreground-service.md](../operations/foreground-service.md).

## What this product is not

- Not a cloud sync product
- Not a full NAS OS — FTP and Info are optional helpers alongside HTTP/WebDAV

## Branding / names

- Store / UI: **HTTP FS**, **HttpFS**
- Docs tree: `docs_ai/`
- Namespace: `tiar.ua.slf`
