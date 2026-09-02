# URL map (server mounts)

Logical mounts. Host/port omitted.

| Mount | Doc |
|-------|-----|
| `/` | Landing / redirect — [http-server.md](../features/http-server.md) |
| `/files/…` | Web UI HTML — Files UI toggle |
| `/web/template/…` | Bundled client assets |
| `/webdav/…` | [webdav-methods.md](webdav-methods.md) |
| `/api/files/…` | [api-routes.md](api-routes.md) · automation: [api-automation.md](api-automation.md) |
| `/rss/…` | [../features/rss.md](../features/rss.md) |
| `/info/…` | Device/server info (HTML or JSON) when Info toggle is on |
| `/login` | Form login — [../features/users-auth.md](../features/users-auth.md) |

FTP is **not** an HTTP mount: `ftp://host:ftpPort/` when FTP is enabled.

Pro **redirects** add extra path routes from settings (mapped toward `/files` paths).

Auth wraps most of the tree when password protection is on; WebDAV uses Basic, browser uses sessions after `/login`.
