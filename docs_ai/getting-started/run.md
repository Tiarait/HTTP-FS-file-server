# Running the app

## Install

1. Build a flavor (`assembleProDebug` / `assembleFreeDebug`) — see [build.md](build.md).
2. Install on a physical device or emulator with network access.
3. For LAN clients, prefer a **physical device on Wi‑Fi** (emulators often need port redirects).

## First launch

1. Splash / permission flow: notifications (API 33+), storage access.
2. Open **Home** → start the server.
3. Note the URL on the status card / notification (`http://{ip}:{port}`). Use Refresh / Address / Copy / QR / Share as needed.
4. From another device on the same Wi‑Fi (or this device’s hotspot), open that URL in a browser.

Defaults:

- Free port **8080**
- Pro port **8081**

Change under **Settings → Server → Port**.

## Android TV

- Leanback launcher entry is enabled.
- Guided **TV Home**: branding or large QR on the left; status card, address actions, and menu on the right.
- Settings lists and dialogs use leanback overlays; QR uses a dedicated TV QR panel.
- Certificate list: OK selects / opens selected; long-press opens Edit.
- Certificate edit: focusable rows, TV padding, focus recovery when panels change.

See [../development/tv-focus.md](../development/tv-focus.md).

## Keeping the server alive

For long runs (screen off):

1. Start the server (ongoing notification must stay).
2. Optionally allow **ignore battery optimizations** (Settings → App).
3. Do not swipe away the app from Recents in a way that kills FGS on aggressive OEMs — prefer Stop in the notification.

See [../operations/foreground-service.md](../operations/foreground-service.md).

## HTTPS

1. Settings → Server → enable SSL.
2. Open **Certificates**, add Auto / Import / Let’s Encrypt profile.
3. Select a ready certificate (radio).
4. Restart server if it was already running.

Details: [../features/ssl-certificates.md](../features/ssl-certificates.md).

## Side-by-side Free + Pro

Both packages can run at once. Use different ports (defaults already differ). Each has its own notification and preferences.
