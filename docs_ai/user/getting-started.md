# Getting started

## Install

- **Free:** [Google Play — HTTP FS](https://play.google.com/store/apps/details?id=tiar.ua.slf) (`tiar.ua.slf`, default port **8080**)
- **Pro:** [Google Play — HTTP FS Pro](https://play.google.com/store/apps/details?id=tiar.ua.slf.pro) (`tiar.ua.slf.pro`, default port **8081**)
- Website: [https://httpfs.pages.dev/](https://httpfs.pages.dev/) · all links: [links.md](links.md)

Both can be installed together (different apps). Sideloaded builds work the same if you trust the APK.

**What is this for?** Short scenarios: [why-and-use-cases.md](why-and-use-cases.md).  
**What does it look like?** [screenshots.md](screenshots.md).

![Home running](../images/screenshots/phone-home-running.png)

## First-run wizard

On first launch the app offers a short wizard (Skip is allowed; everything can be changed later):

1. **Welcome**
2. **What do you need?** — enable Files UI / WebDAV / Info / RSS (FTP is configured later in Settings)
3. **Permissions** — storage, notifications, battery exemption (recommended for a stable server)
4. **Quick setup** — port, shared folder, optional homepage file, optional password + user

Then you land on **Home**. Goal-oriented next steps: [recipes.md](recipes.md). Where controls live: [ui-map.md](ui-map.md).

## First run (phone / tablet)

1. Allow **notifications** (Android 13+) so the ongoing server notification can stay visible.
2. Allow **storage / files** access as prompted. For SD cards use **Settings → App → Storage permissions** when asked.
3. Open **Home** and start the server (center power control).
4. Read the URL from the home card or the notification, for example `http://192.168.1.20:8080`.
5. On another device on the **same Wi‑Fi** (or connected to this phone’s hotspot), open that URL in a browser.

Browser UI tour (upload, search, account): [web-client.md](web-client.md).

### Home address helpers

On the Home status card, next to `host:port`:

| Action | What it does |
|--------|----------------|
| **Refresh** | Re-detect the LAN address immediately (useful after Wi‑Fi / VPN / hotspot changes). |
| **Address** | Opens the same address picker as **Settings → Server → Address**. |
| **Copy** | Copies the full server URL. |
| **QR** | Shows a QR code for the current URL. |
| **Share** | System share sheet for the URL. |

Stop the server from the app or the notification **Stop** action.

## Keep it running with the screen off

1. Leave the server started (notification must remain).
2. In **Settings → App**, enable **Battery permissions** / ignore battery optimizations when offered.
3. On aggressive OEM skins, also allow auto‑start if you use **Start server with device**.

## Optional next steps

- Recipes for common goals: [recipes.md](recipes.md)
- Narrow **Path to folder** — [settings guide](settings-guide.md)
- **Use password** / Guest mode / Read-only
- **SSL** — [HTTPS & certificates](https-certificates.md)
- **WebDAV**, **FTP**, or browser **Info** page — [Connect clients](connect-clients.md)

## Android TV

After the TV wizard, Home is two columns:

- **Left:** branding while stopped; large **QR + URL** while running
- **Right:** status card + address actions + menu (**Start/Stop**, **Quick settings**, Settings, Info, Logs, Exit)

D‑pad focus stays on actions and the menu. Certificate lists: **long‑press** opens Edit.

More: [ui-map.md](ui-map.md) · [../development/tv-focus.md](../development/tv-focus.md).
