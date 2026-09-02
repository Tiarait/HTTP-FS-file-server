# Architecture

## High-level diagram

```
┌──────────────────────────────────────────────────────────────┐
│  Android UI                                                   │
│  Phone: Home / Settings / Info / Logs                         │
│  TV: guided Home (QR + status + menu) · nested Settings       │
│  Dialogs: bottom sheets (phone) · leanback overlays (TV)      │
└────────────────────────────┬─────────────────────────────────┘
                             │ SharedPreferences façade
                             │ App singleton
┌────────────────────────────▼─────────────────────────────────┐
│  Embedded Ktor server                                         │
│  Engines: CIO or Netty  ·  SSL ⇒ Netty only                   │
│  Auth · blocked IPs · optional FTP listener                   │
│  Lifecycle: foreground service (specialUse) + wake lock       │
└───────┬──────────┬──────────┬──────────┬──────────┬──────────┘
        │          │          │          │          │
     /files     /webdav     /api      /rss       /info
   (+ HTML)                (+ WS)
        │
   Bundled web client assets (+ runtime config)
        │
┌───────▼──────────────────────────────────────────────────────┐
│  Storage layer                                                │
│  File API and/or SAF · multi-volume · path resolve/transfer   │
└──────────────────────────────────────────────────────────────┘
```

## Gradle modules

| Module | Role |
|--------|------|
| `:app` | Application: UI, server, storage, assets, flavors |
| `:free` | Free companion: AdMob + limited Pro façade |
| `:pro` | Pro companion: full façade, no ads |
| `:acme` | ACME / Let’s Encrypt DNS-01 helpers |
| `client/` | Web UI sources (not a Gradle module; npm → assets) |

## Runtime layers

### Application

- Holds the embedded server instance
- Starts/stops the foreground notification service
- Tracks advertised LAN IP, wake lock options, restart flags
- Reacts to network changes (advertise refresh, optional auto-stop / rebind) — see [../features/network-address.md](../features/network-address.md)

### Server

- Loads preferences
- Installs HTTP plugins (auth, sessions, websockets, compression, …)
- Mounts route modules for files, WebDAV, API, RSS, Info
- Resolves SSL keystores through the certificate library
- Optional FTP server on a separate port

### Storage

- Resolves logical URLs to filesystem or SAF documents
- Multi-volume “all storages” mode
- Listing, copy/move, zip, thumbnails helpers used by routes

### UI

- Settings screens are mostly **code-built preferences**, not XML preference graphs
- Phone Home host card: Refresh / Address / Copy / QR / Share
- TV guided Home + leanback dialogs (including QR)
- In-app Info tab (live health) is separate from optional HTTP `/info/`

## Data persistence

- **SharedPreferences** (ports, SSL flags, users, interface toggles, …)
- **App private files** for ACME stores, auto-cert caches, web resource copies
- **SAF persisted URI permissions** for removable volumes

## Process model

1. User starts server → foreground service notification.
2. Service starts the embedded HTTP engine if not already running.
3. Notification stays until Stop / server error / destroy.
4. Optional boot receiver restarts the server when “start on boot” is enabled.

## Related docs

- [product.md](product.md)
- [../features/http-server.md](../features/http-server.md)
- [../features/network-address.md](../features/network-address.md)
- [../modules/app.md](../modules/app.md)
