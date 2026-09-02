# Safety tips and FAQ

## Safety

- Prefer **password protection** on any network that isn’t fully trusted.
- Prefer **Read‑only** or Read-only users; give **Write** only to people you trust (write can delete files).
- **Guest mode** (with password on) limits anonymous browsing — still use a password on shared Wi‑Fi.
- Use **HTTPS** on untrusted Wi‑Fi; for LAN self‑signed, export and trust the certificate on clients.
- Choose a **narrow root folder** instead of sharing the whole device when possible.
- **Hidden files** (Pro) hide paths from listings — not a substitute for passwords.
- **Blocked IPs** stop specific clients quickly.
- Exposing the server to the **internet** (router port forward) increases risk. Use strong passwords, HTTPS with a real cert when possible, and understand what you are sharing. Not recommended for beginners.

## Diagnose in order

When a user says “it doesn’t work”, walk this list before deep settings:

1. **Is the server Running?** Home / TV status or ongoing notification.
2. **Same network?** Client on same Wi‑Fi **or** joined this device’s hotspot (not cellular-only browsing to the phone).
3. **URL scheme match?** `http://` if SSL off; `https://` if SSL on.
4. **IP still valid?** Home → **Refresh**. VPN should not force a `tun*` advertise address on All interfaces.
5. **Service enabled?** Files UI / WebDAV / Info / FTP toggles under Server → Interfaces.
6. **Auth?** Password on → correct user; password off → Permissions / Read-only.
7. **Logs** tab (if enabled) for bind / SSL errors.

Goal recipes: [recipes.md](recipes.md). Android UI: [ui-map.md](ui-map.md). Browser UI: [web-client.md](web-client.md).

## FAQ / troubleshooting

**What is this app for?**  
Local file server on Android: browse/upload/download from another device on the same Wi‑Fi or hotspot (browser, WebDAV, optional FTP/API). Not a public cloud. Scenarios: [why-and-use-cases.md](why-and-use-cases.md). Download: [links.md](links.md).

**Server won’t start**  
Port in use → change **PORT**. Check Logs for bind/SSL errors. If there is no usable LAN (no Wi‑Fi to a router, no hotspot, no Ethernet), the app warns you; you can still start (or choose **Only localhost**).

**Wrong IP on Home / QR (VPN / `tun0`)**  
With **All interfaces**, the app listens on all interfaces but **advertises** a LAN address (Wi‑Fi / Ethernet / hotspot), not the VPN. Tap **Refresh** or reopen **Address**. See [../features/network-address.md](../features/network-address.md).

**Other devices can’t open the URL**  
Same Wi‑Fi / hotspot? VPN or “AP isolation” / guest Wi‑Fi can block device‑to‑device traffic. Try `http://` vs `https://` to match SSL. Confirm the IP on Home still matches (**Refresh**). Corporate / guest networks often isolate clients — try hotspot recipe [recipes.md](recipes.md).

**Browser shows certificate warning**  
Expected for Auto self‑signed HTTPS. Export/trust the cert, or use HTTP on a trusted LAN, or Let’s Encrypt for a real domain.

**Server stopped after Wi‑Fi dropped**  
With **All interfaces**, losing a shareable LAN can auto‑stop the server. Reconnect Wi‑Fi/hotspot and start again. For a pinned interface, check **On IP change** (Auto-rebind / Keep running / Stop).

**WebDAV fails**  
Enable WebDAV under Server → Interfaces. Try HTTPS + trusted cert. Check username/password and Read/Write rights. Path must include `/webdav/`.

**FTP fails**  
Enable FTP; use the **FTP port** (default 2121), not the HTTP port. Confirm the port is free and differs from HTTP.

**`/info/` 404 or empty**  
Enable Server → Interfaces → **Info**. Do not confuse with the in-app Info tab.

**SD card / write fails**  
Grant **Storage permissions** / folder access for that volume. In multi‑volume mode, re‑run the permission flow after replugging the card.

**Files missing**  
They must be under the server root. Check **Hide files**. In multi‑volume mode, open the correct volume id in the path. If the browser UI is empty, confirm **Files UI** is on.

**`.local` / LAN name does not open**  
Must be enabled; client must be on the same Wi‑Fi; macOS/iOS usually OK; Windows may need Bonjour. Fall back to the numeric IP from Home.

**PWA install fails**  
Needs trusted HTTPS (Let’s Encrypt). App self‑signed cert is not enough for Chrome install.

**Can browse but cannot upload**  
Read-only ON (password off), or Guest access without Write, or user is Read-only, or Guest mode limiting anonymous use, or Max upload limit hit. Check Server security settings.

**Search stuck on “Loading…” or results jump around**  
Update to the latest build: live progress panel + **Found order** (no auto-sort). Use **Sort** manually if you want a different order. See [web-client.md](web-client.md) § Search.

**Folder sync unavailable in browser**  
Needs Chrome/Edge/Opera and HTTPS (or `localhost`). Plain `http://192.168.x.x` cannot pick a PC folder — use WebDAV or upload instead. See [web-client.md](web-client.md) § Folder sync.

**Slow or hot phone**  
Disable thumbnails and folder sizes; turn down “all logs”; keep buffer at recommended size; leave HTTP/2 off unless needed.

**Ads still show (Free)**  
Rewarded “disable ads” lasts until the app process ends. Upgrade to Pro for no ads.

**Certificate / Let’s Encrypt verify fails**  
TXT record wrong or not propagated; wait and retry. Avoid hammering production Verify (rate limits). See [https-certificates.md](https-certificates.md).

**Server dies overnight**  
Enable battery exemption; keep the notification; disable aggressive OEM battery savers for HttpFS; optional start‑on‑boot.

## More help

- [why-and-use-cases.md](why-and-use-cases.md) — what the app is for
- [links.md](links.md) — website & Play Store
- [recipes.md](recipes.md)
- [ui-map.md](ui-map.md)
- [settings-guide.md](settings-guide.md)
- [getting-started.md](getting-started.md)
- [connect-clients.md](connect-clients.md)
- Foreground service notes: [../operations/foreground-service.md](../operations/foreground-service.md)
