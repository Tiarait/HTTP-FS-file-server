# Notifications

## Server ongoing notification

Shown whenever the file server is running (foreground service).

| Item | Notes |
|------|-------|
| Purpose | Keep the server process alive; show LAN URL; **Stop** action |
| Content | Advertised `http(s)://…` URL (same idea as Home) |
| Stop | Stops the server and dismisses the notification |

Free and Pro can both be installed; each app has its own notification.

**User tip:** if the notification is gone, the server is not running as a foreground service — start again from Home. Do not swipe away the notification if you want overnight sharing.

## Other notifications

- Low free space / transfer helpers may post extra alerts.
- Network change alerts can appear when the server auto-stops or rebinds after Wi‑Fi/IP changes (see [../features/network-address.md](../features/network-address.md)).

These are UX messages, not the long-running FGS anchor.

## Related

- [foreground-service.md](foreground-service.md)
- [lifecycle.md](lifecycle.md)
- [../features/widgets-shortcuts.md](../features/widgets-shortcuts.md)
- User recipes: [../user/recipes.md](../user/recipes.md)
