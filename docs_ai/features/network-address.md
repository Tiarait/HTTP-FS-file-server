# Network address (bind vs advertise)

How HttpFS chooses the listen address and the URL shown to clients.

## Two different ideas

| Concept | Meaning |
|---------|---------|
| **Bind / listen** | Where the HTTP engine accepts connections |
| **Advertise** | Host shown in Home, notification, QR, share, and most URL summaries |

**Settings → Server → Address** controls both.

## Modes

| Selection | Bind | Advertise |
|-----------|------|-----------|
| **All interfaces** (default) | All local IPv4 (`0.0.0.0`) | Best **shareable LAN** address (see below) |
| **Only localhost** | `127.0.0.1` | Localhost only |
| **Specific IP** | That address | That address (sticky to the chosen interface when possible) |

## Advertise preference (All interfaces)

When advertising, the app prefers:

1. Wi‑Fi STA (`wlan*`)
2. Ethernet (`eth*`)
3. Hotspot (`ap*` / hotspot `wlan*`)

It **skips** VPN-style interfaces (`tun*`, `ppp*`, …) and normally skips cellular backhaul unless you are in hotspot mode. So a VPN may be active while Home still shows the Wi‑Fi IP that LAN clients should open.

## Address picker filtering

The list always includes All interfaces and localhost, then live interface addresses:

- **On a Wi‑Fi router:** mainly `wlan*` / `eth*` (hide VPN, hotspot `ap0`, cellular).
- **Hotspot mode:** hotspot / related `wlan*`, and cellular may appear.
- **VPN** entries are never offered as picks.
- A **previously saved** specific address stays visible even if temporarily filtered.

Open the same picker from **Home → Address** (phone or TV).

## Start / stop and network loss

- Starting without a shareable LAN (and not localhost-only) shows a **warning**; the user can open Wi‑Fi settings or continue.
- With **All interfaces**, if the usable LAN disappears while the server is running, the app **stops** the server (clients would otherwise keep an unreachable advertised URL).
- With a **specific** address, behavior follows **On IP change**: Auto-rebind, Keep running, or Stop server.
- **Localhost only** ignores LAN up/down for rebind/stop of that sort.

## Related

- User wording: [../user/settings-guide.md](../user/settings-guide.md)
- FAQ: [../user/safety-and-faq.md](../user/safety-and-faq.md)
- Lifecycle: [../operations/lifecycle.md](../operations/lifecycle.md)
