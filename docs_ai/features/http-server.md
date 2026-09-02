# HTTP(S) server

## Stack

- **Ktor** embedded server
- Engines: **CIO** or **Netty**
- **HTTPS** forces **Netty**
- Optional **FTP** listener on a separate port
- Extra: native SSL helpers and BouncyCastle for certificates

## Lifecycle

1. User starts from Home / TV / widget / boot
2. Foreground service → `startForeground` (`specialUse` on API 34+)
3. Server binds host + port (and optional FTP)
4. Stop via notification, Home/TV UI, or clean shutdown helpers

Wake lock + sticky-style service behavior keep the process useful while the notification is active. Network-driven stop/rebind: [network-address.md](network-address.md).

## Main URL map

| Path | Purpose | Default |
|------|---------|---------|
| `/` | Redirect / landing | Always |
| `/files/...` | Web file manager | Files UI **ON** |
| `/web/template/...` | Static web assets | Always (when serving UI) |
| `/webdav/...` | WebDAV | **OFF** |
| `/api/...` | REST + WebSockets | Always (API surface) |
| `/rss/...` | RSS feed | **ON** |
| `/info/...` | Device/server info HTML/JSON | **OFF** |
| `/login` | Login when password auth enabled | When password on |

FTP uses `ftp://host:ftpPort/` (default FTP port **2121**), not an HTTP path.

## Configuration (high level)

From Server settings:

- Address / interface selection (+ On IP change)
- Port (+ FTP port when FTP on)
- Engine type
- Buffer size
- SSL on/off + selected certificate
- Auth, users, session timeout
- Interface toggles: Files UI, WebDAV, RSS, Info, FTP
- Root path / multi-volume
- Initial file + optional hard redirect
- Blocked IPs, redirects (Pro)

## Engines

| Engine | Notes |
|--------|-------|
| CIO | Lighter; OK for simple HTTP |
| Netty | Preferred for SSL / heavier load |

## Logging

Server and app logs surface in the Logs tab / TV Logs menu; useful for bind failures (port in use) and SSL errors.

## Related

- [network-address.md](network-address.md)
- [webdav.md](webdav.md)
- [api.md](api.md)
- [rss.md](rss.md)
- [server-engines.md](server-engines.md)
- [ssl-certificates.md](ssl-certificates.md)
- [../reference/url-map.md](../reference/url-map.md)
- [../operations/foreground-service.md](../operations/foreground-service.md)
- [../operations/lifecycle.md](../operations/lifecycle.md)
