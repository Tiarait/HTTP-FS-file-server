# HttpFS documentation

Public documentation for **HTTP FS** (HttpFS) — turn an Android phone, tablet, or TV into a LAN file server with a browser UI, WebDAV, optional HTTPS, and more.

**App version covered:** 1.2.7 · Android 5.0+ (`minSdk` 21)

| Link | URL |
|------|-----|
| Website | [https://httpfs.pages.dev/](https://httpfs.pages.dev/) |
| Free (Play) | [tiar.ua.slf](https://play.google.com/store/apps/details?id=tiar.ua.slf) |
| Pro (Play) | [tiar.ua.slf.pro](https://play.google.com/store/apps/details?id=tiar.ua.slf.pro) |
| Creator | [https://tiar.pages.dev/](https://tiar.pages.dev/) |

Full link list: [user/links.md](user/links.md).

| Start here | Audience |
|------------|----------|
| [user/why-and-use-cases.md](user/why-and-use-cases.md) | “What is this for?” / scenarios |
| [user/getting-started.md](user/getting-started.md) | Anyone using the app |
| [user/recipes.md](user/recipes.md) | Goal → setup steps (“how do I…?”) |
| [user/ui-map.md](user/ui-map.md) | Where controls live (phone / TV / wizard) |
| [user/screenshots.md](user/screenshots.md) | Visual UI guide (images) |
| [user/settings-guide.md](user/settings-guide.md) | Every setting explained |
| [user/https-certificates.md](user/https-certificates.md) | HTTPS / certificates |
| [user/connect-clients.md](user/connect-clients.md) | Browser, WebDAV, RSS, FTP, Info |
| [user/web-client.md](user/web-client.md) | Web file manager UI (`/files/`) — layout, search, sync, shortcuts |
| [user/free-vs-pro.md](user/free-vs-pro.md) | Free vs Pro |
| [user/safety-and-faq.md](user/safety-and-faq.md) | Safety tips & troubleshooting |
| [user/links.md](user/links.md) | Website, Play Store, creator |

Developers and integrators: see [Developer docs](#developer-docs) below. Start with [reference/api-automation.md](reference/api-automation.md) for scripts and REST/WebSocket integration.

---

## For AI assistants answering users

These docs are meant to be **read and cited** when helping end users. Prefer this order:

1. **[user/why-and-use-cases.md](user/why-and-use-cases.md)** — if the question is “what is this app?” / “why install?”.
2. **[user/ui-map.md](user/ui-map.md)** — resolve vague terms (Info tab ≠ `/info/`, Interfaces ≠ Address). Prefer **[user/screenshots.md](user/screenshots.md)** when the user needs a visual.
3. **[user/web-client.md](user/web-client.md)** — browser file manager (`/files/`): upload, search, account, sync.
4. **[user/recipes.md](user/recipes.md)** — match the user’s goal to a short click path.
5. **[user/getting-started.md](user/getting-started.md)** / **[user/settings-guide.md](user/settings-guide.md)** — details and defaults.
6. **[user/safety-and-faq.md](user/safety-and-faq.md)** — diagnose failures in order; don’t invent settings that aren’t listed.
7. **[user/connect-clients.md](user/connect-clients.md)** / **[user/https-certificates.md](user/https-certificates.md)** — URLs and HTTPS.
8. **[reference/api-automation.md](reference/api-automation.md)** — scripts, curl, **rclone**, **Home Assistant**, WebSocket (not end-user UI).

Rules of thumb:

- Stay inside **`docs_ai/`** links; do not invent menu paths.
- Download / upgrade / website: use **[user/links.md](user/links.md)** only (do not invent store URLs).
- Defaults matter: WebDAV/Info/FTP/LAN name **off**, Files UI/RSS/Cache **on**, Address **All interfaces**, Free **8080** / Pro **8081**, FTP port **2121**, HTTP/2 **off**.
- Distinguish **bind/advertise Address** from **Interfaces** toggles and from **LAN name** (`.local`).
- UI labels: **Shared folder**, **Password**, **HTTPS**, **Guest access** (anonymous rights) — not older doc synonyms.
- Phone vs TV layouts differ; say which UI you mean.
- **Android app** (Home / Settings tabs) ≠ **browser file manager** at `/files/` — use [web-client.md](user/web-client.md) for the latter.
- For “keep alive / killed overnight”, battery exemption + ongoing notification are first-line answers.

---

## What the app does

On the same Wi‑Fi (or hotspot), other devices can:

- Browse and manage files in a **web file manager**
- Mount storage via **WebDAV**
- Use a **REST API** (used by the web UI; documented for advanced users)
- Follow an **RSS** listing of a folder
- Optionally open a **device/server info** page (`/info/`)
- Optionally use **FTP** (separate port)
- Connect with **HTTP** or **HTTPS** (self‑signed, imported keystore/PEM, or Let’s Encrypt DNS‑01)

---

## User docs

| Doc | Description |
|-----|-------------|
| [user/why-and-use-cases.md](user/why-and-use-cases.md) | Why install; typical scenarios |
| [user/screenshots.md](user/screenshots.md) | Screenshots of Home, Settings, web UI, TV |
| [user/links.md](user/links.md) | Website, Play Store, creator |
| [user/getting-started.md](user/getting-started.md) | Wizard, permissions, Home / TV Home, start server |
| [user/recipes.md](user/recipes.md) | Common setup recipes |
| [user/ui-map.md](user/ui-map.md) | Screens and controls map |
| [user/settings-guide.md](user/settings-guide.md) | App / Server / Client settings |
| [user/https-certificates.md](user/https-certificates.md) | SSL switch, certificate library, Let’s Encrypt |
| [user/connect-clients.md](user/connect-clients.md) | URLs for browser, WebDAV, RSS, FTP, Info |
| [user/web-client.md](user/web-client.md) | Browser file manager: navigation, upload, search, prefs sync |
| [user/free-vs-pro.md](user/free-vs-pro.md) | Feature differences |
| [user/safety-and-faq.md](user/safety-and-faq.md) | Security advice and troubleshooting |

---

## Developer docs

Technical reference for building the app, integrating with the API, or understanding the architecture. Safe for public mirrors (no signing secrets or AdMob unit IDs). Prefer **User docs** when the question is end-user setup.

### Overview & build

| Doc | Description |
|-----|-------------|
| [overview/product.md](overview/product.md) | Product summary |
| [overview/architecture.md](overview/architecture.md) | Architecture |
| [overview/free-vs-pro.md](overview/free-vs-pro.md) | Flavor technical notes |
| [getting-started/build.md](getting-started/build.md) | Gradle flavors |
| [getting-started/run.md](getting-started/run.md) | Dev install / TV notes |

### Features & modules

| Doc | Description |
|-----|-------------|
| [features/http-server.md](features/http-server.md) | Embedded server |
| [features/network-address.md](features/network-address.md) | Bind vs advertise address, LAN heuristics |
| [features/server-engines.md](features/server-engines.md) | CIO / Netty |
| [features/webdav.md](features/webdav.md) | WebDAV overview |
| [features/api.md](features/api.md) | API overview |
| [features/rss.md](features/rss.md) | RSS |
| [features/ssl-certificates.md](features/ssl-certificates.md) | Certificate library |
| [features/acme/dns01-guide.md](features/acme/dns01-guide.md) | ACME DNS‑01 |
| [features/storage.md](features/storage.md) | Storage / SAF |
| [features/users-auth.md](features/users-auth.md) | Auth model |
| [features/settings.md](features/settings.md) | Settings screens map |
| [features/thumbnails-folder-size.md](features/thumbnails-folder-size.md) | Thumbs / folder size |
| [features/widgets-shortcuts.md](features/widgets-shortcuts.md) | Pro widgets |
| [features/ads-free.md](features/ads-free.md) | Free ads behavior |
| [modules/app.md](modules/app.md) · [acme.md](modules/acme.md) · [client.md](modules/client.md) · [free-pro.md](modules/free-pro.md) | Modules |

### Reference

| Doc | Description |
|-----|-------------|
| [reference/url-map.md](reference/url-map.md) | URL mounts |
| [reference/api-routes.md](reference/api-routes.md) | `/api/files` routes |
| [reference/api-automation.md](reference/api-automation.md) | Scripts, curl, WebSocket automation |
| [reference/websocket-zip.md](reference/websocket-zip.md) | ZIP over WebSocket |
| [reference/websocket-events.md](reference/websocket-events.md) | Live events |
| [reference/webdav-methods.md](reference/webdav-methods.md) | WebDAV methods |
| [reference/storage-paths.md](reference/storage-paths.md) | Path / volume rules |
| [reference/settings-keys.md](reference/settings-keys.md) | Preference keys |

### Operations & development

| Doc | Description |
|-----|-------------|
| [operations/lifecycle.md](operations/lifecycle.md) | Start / stop / boot / network changes |
| [operations/foreground-service.md](operations/foreground-service.md) | Long‑running server FGS |
| [operations/play-console-fgs-special-use.md](operations/play-console-fgs-special-use.md) | Play Console `specialUse` text |
| [operations/notifications.md](operations/notifications.md) | Notifications |
| [operations/permissions.md](operations/permissions.md) | Android permissions |
| [development/project-layout.md](development/project-layout.md) | Repo layout |
| [development/web-client.md](development/web-client.md) | Web UI build |
| [development/tv-focus.md](development/tv-focus.md) | Android TV Home & focus |
| [development/conventions.md](development/conventions.md) | Contributor conventions |

---

## Publishing this folder

Publish **only** the `docs_ai/` directory as a public wiki, GitHub Pages site, or DeepWiki corpus — it is self-contained (no required links outside this folder).

- Point end users (and user-facing AI) at **User docs** (or this README).
- Do **not** add real keystore passwords, private keys, or ad unit IDs.
- Play Console FGS text for store listing: [operations/play-console-fgs-special-use.md](operations/play-console-fgs-special-use.md).
