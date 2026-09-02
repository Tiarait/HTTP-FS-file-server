# REST API and WebSockets

## Base path

**`/api/files/…`** (full mount: `/api` + `files`).

The bundled web UI (`client_HttpFS`) uses this API for listing, uploads, renames, deletes, zip, search, prefs sync, etc.

## Documentation map

| Doc | For |
|-----|-----|
| [../reference/api-routes.md](../reference/api-routes.md) | Complete route & WebSocket reference |
| [../reference/api-automation.md](../reference/api-automation.md) | **curl, scripts, HA** — auth, paths, recipes |
| [../reference/storage-paths.md](../reference/storage-paths.md) | `path=` rules, multi-volume |
| [../reference/websocket-zip.md](../reference/websocket-zip.md) | ZIP download socket |
| [../reference/websocket-events.md](../reference/websocket-events.md) | Live events socket |

## Capabilities (overview)

- Directory **list** / **tree** / **stat** / **size**
- **Upload** (with Content-Range resume), **download** via `/api/files/file`
- **Create**, **save** (text), **delete**, **rename**, **move**, **copy**
- **Search** over WebSocket (glob, regex)
- **Folder size** over WebSocket (progress)
- **ZIP** build over WebSocket
- **Transfer** socket (chunked put/get)
- Per-user **prefs** (GET/PUT/DELETE)
- **Live events** (list changed, notices, battery)

There is **no** public admin REST API yet (users, redirects, server config remain Android Settings).

## Auth

| Mode | REST API |
|------|----------|
| Password off | Anonymous; global Read/Write |
| Password on | **Session cookie** after `POST /login` |

WebDAV uses **HTTP Basic** on `/webdav/` — not the same as REST auth. See [users-auth.md](users-auth.md) and [api-automation.md](../reference/api-automation.md).

## WebSockets

Same host/port as HTTP. Cookie required on upgrade when password is on.

## Errors

Normalized JSON: `error` + `message`. Volume codes: `VOLUME_*`. See [api-routes.md](../reference/api-routes.md).

## Related

- [users-auth.md](users-auth.md)
- [storage.md](storage.md)
- [../modules/client.md](../modules/client.md)
- [../user/web-client.md](../user/web-client.md)
