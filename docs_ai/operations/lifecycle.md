# Server lifecycle (start / stop / boot)

## Start paths

| Trigger | Flow |
|---------|------|
| Home / TV Start | App start helpers → foreground service → embedded server bind |
| Splash / shortcut / widget | Same service path (Pro shortcuts/widgets) |
| Start with app | Splash may start service after permissions |
| Boot | Boot completed receiver if “start on boot” enabled |

Locked-boot handling waits for normal boot after unlock when auto-start is used.

If there is no shareable LAN and the mode is not localhost-only, the UI shows a Wi‑Fi warning; the user may open system Wi‑Fi settings or continue.

## Stop paths

| Trigger | Flow |
|---------|------|
| Notification Stop | Service action → stop server / service |
| Home / TV Stop | App stop helpers → clean engine shutdown |
| Restart | Stop then start again |
| Engine switch / SSL / listen-host change | Often needs restart |

## Network changes

| Mode | Behavior (high level) |
|------|------------------------|
| **All interfaces** | Advertise IP can change without rebinding `0.0.0.0`. If shareable LAN is lost, **auto-stop**. |
| **Specific address** | Follows **On IP change**: Auto-rebind / Keep running / Stop. |
| **Localhost only** | Ignores LAN up/down for that rebind/stop policy. |

See [../features/network-address.md](../features/network-address.md).

## Reliability pieces

- FGS `specialUse` + notification (see [foreground-service.md](foreground-service.md))
- Partial wake lock while the service runs
- Optional ignore-battery-optimizations
- Sticky-style service restart behavior while the notification is active

## Dual Free + Pro

Separate packages → separate services, ports (8080 / 8081), notifications, prefs. Both may run at once.
