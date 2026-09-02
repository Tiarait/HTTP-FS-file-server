# WebDAV methods reference

Mount: **`/webdav/{resource…}`** (`App.fmWebDavName` = `webdav`).

Disabled when Server → WebDAV is off (`webdavEnabled` / `webdav_key` pref). Paths use the same storage resolver as `/files` (see [storage-paths.md](storage-paths.md)).

Auth: HTTP **Basic** when password protection is on; otherwise anonymous read/write flags apply. Write methods require `canWrite`.

## Methods

| Method | Role |
|--------|------|
| `OPTIONS` | Advertise Allow / Public |
| `PROPFIND` | Listing / props (XML multistatus); Depth header as used by clients |
| `PROPPATCH` | Stub-friendly multistatus (Win32 props); not a full property store |
| `GET` / `HEAD` | Download / metadata |
| `PUT` | Upload file body |
| `DELETE` | Remove resource |
| `MKCOL` | Create collection (folder); may set `Location` |
| `COPY` | Copy (`Destination` header) |
| `MOVE` | Move/rename (`Destination` header) |
| `LOCK` / `UNLOCK` | In-memory lock manager (`lockManager`); Lock-Token header |

Allow header (as served):  
`COPY, DELETE, GET, HEAD, LOCK, MKCOL, MOVE, OPTIONS, PROPFIND, PROPPATCH, PUT, UNLOCK`

## Multi-volume

PROPFIND / mutations resolve via `resolveWebDavAccess()` / logical WebDAV path helpers. Virtual volume root cannot be treated as a normal file upload target.

## Client quirks (product behavior)

- Windows / Finder often hit `/webdav` without a trailing slash — collection href helpers normalize this.
- Hidden paths (Pro hidden list) return Method Not Allowed style errors (`Error#66` where JSON is used).

## Implementation

WebDAV methods and listing XML are implemented in the server WebDAV route module. For protocol edge cases, trust the running app / source over this table.

## REST API alternative

For JSON listing, search, and scripted uploads, use **`/api/files/*`** — [api-automation.md](api-automation.md). WebDAV uses **Basic** auth; REST uses a **session cookie**.
