# Setup recipes (goal → clicks)

Short, goal-oriented paths for end users. Prefer these when someone asks “how do I…?” instead of dumping every setting.

If the question is **why install / what is it for**, start with [why-and-use-cases.md](why-and-use-cases.md). Download links: [links.md](links.md).

Defaults: Free HTTP port **8080**, Pro **8081**. Change under **Settings → Server → PORT** if needed.

---

## A. Share files on the same Wi‑Fi (browser)

1. Finish the first-run wizard (or skip and use defaults).
2. Leave **Files UI** ON (Server → Interfaces).
3. Leave **Address** on **All interfaces** unless you know you need a specific IP.
4. Home → **Start**.
5. Home → **Copy** / **QR** / **Share**, or use the notification URL.
6. On the other device (same Wi‑Fi), open `http://IP:PORT/` or `/files/`.

Optional: set **Path to folder** to a single album/folder instead of all storage.

---

## B. Phone as hotspot (no home Wi‑Fi)

1. Turn on the Android **hotspot**.
2. Connect the PC/tablet to that hotspot.
3. Start the server; **Refresh** the address on Home if the IP looks wrong.
4. Open the shown URL from the client. Address list may show `ap*` / hotspot interfaces.

If the server was already running before hotspot, tap **Refresh** or restart the server.

---

## C. Mount as a drive (WebDAV)

1. Server → Interfaces → **WebDAV** ON.
2. Start the server.
3. On the client use `http://IP:PORT/webdav/` (or `https://…` if SSL is on).
4. If **Use password** is on, enter username/password (HTTP Basic).

macOS: Finder → Go → Connect to Server.  
Windows: third-party WebDAV client or Map network drive (behavior varies).

Details: [connect-clients.md](connect-clients.md).

---

## D. Password-protect access

1. Stop the server if running (password change usually needs stop).
2. Server → **Use password** ON.
3. Open **Users** → create username/password and Read / Write / Read+Write.
4. Optional: **Guest mode** ON — guests can only view files (no full file-manager browsing for anonymous guests).
5. Start the server; browser login / WebDAV Basic apply.

Without password: anonymous access follows **Permissions** (Read / Write) or global **Read-only**.

---

## E. Read-only share (nobody can upload/delete)

- **Read-only** switch ON (Server), **or**
- Password off + Permissions = Read only, **or**
- Password on + users with Read only.

Do not enable anonymous Write on shared networks.

---

## F. HTTPS on the LAN (self-signed)

1. Stop server → Server → **SSL** ON → **Certificates** → add **Auto**.
2. Select the profile → start server → use `https://IP:PORT/…`.
3. Expect browser warnings until the cert is trusted; **Export** PEM and install on clients when possible.

Internet / public hostname: prefer Let’s Encrypt DNS‑01 — [https-certificates.md](https-certificates.md).

---

## G. Keep running overnight / screen off

1. Start the server (ongoing notification must stay).
2. App → **Battery permissions** / ignore battery optimizations.
3. Optional: App → **Start server with device**.
4. Do not swipe-kill the app on aggressive OEMs; stop from the notification when done.

---

## H. Android TV

1. Complete TV wizard (interfaces → permissions → quick setup).
2. Home menu → **Start server**.
3. Left side shows a large QR when running; or use action row **QR** / **Copy**.
4. **Quick settings** for port, folder, password, SSL, buffer without full Settings.

---

## I. Device status page in the browser

1. Server → Interfaces → **Info** ON.
2. Open `http://IP:PORT/info/` from another device.

This is **not** the same as the in-app **Info** tab (live health on the phone/TV itself).

---

## J. FTP for a legacy client

1. Server → Interfaces → **FTP** ON.
2. Set **FTP port** (default **2121**, must ≠ HTTP port).
3. Connect to `ftp://IP:FTP_PORT/`. Prefer WebDAV/browser when possible.

---

## K. Wrong IP after VPN

1. Home → **Refresh**, or **Address** → keep **All interfaces**.
2. The advertised URL should be Wi‑Fi/Ethernet/hotspot, not `tun*`.
3. If you previously pinned a VPN IP, pick All interfaces or the real LAN IP again.

See [../features/network-address.md](../features/network-address.md).

---

## L. Narrow what is shared

1. Stop server → **Shared folder** → pick one folder (Downloads, DCIM, …).
2. Or use multi-volume **All storages** and grant only needed volumes under App → Removable storage.
3. Pro: **Hide files** to omit paths from listings (not a password).

---

## M. Open by name on the LAN (`httpfs.local`)

1. Server (or TV Quick settings) → enable **LAN name**.
2. Optional: set **Host** (default `httpfs`).
3. On another device on the **same Wi‑Fi**, try `http://httpfs.local:PORT/` (or `https://…` if HTTPS is on).
4. Reminder: not reachable from mobile data or other networks. macOS/iOS usually work; Windows may need Bonjour.

---

## N. Sync a PC folder with a phone folder (browser)

1. Prefer **HTTPS** on the server (or test from `http://localhost` via dev proxy).
2. Use **Chrome, Edge, or Opera** on the PC.
3. Open `/files/`, log in if password is on.
4. Right-click a folder on the phone → **Sync…** → pick the local folder.
5. Optional: Account → enable **Sync settings with the server** so pair names follow your user.

Details and limits: [web-client.md](web-client.md) § Folder sync.

---

## When stuck

Use the decision FAQ: [safety-and-faq.md](safety-and-faq.md).  
Android controls: [ui-map.md](ui-map.md).  
Browser file manager: [web-client.md](web-client.md).  
Every setting: [settings-guide.md](settings-guide.md).
