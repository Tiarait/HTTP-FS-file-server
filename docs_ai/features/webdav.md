# WebDAV

## Endpoint

Base path: **`/webdav/`**.

Works over HTTP or HTTPS depending on server SSL settings.

## Supported operations (typical)

PROPFIND, GET, PUT, DELETE, MOVE, COPY, MKCOL, LOCK/UNLOCK (as implemented).

**Method table:** [../reference/webdav-methods.md](../reference/webdav-methods.md)

## Auth

When password protection is enabled, WebDAV uses **HTTP Basic** (and participates in the same user permission model: read / write).

When auth is off, anonymous access follows global read/write settings.

## Clients

Examples that users commonly try:

- macOS Finder → Connect to Server → `https://ip:port/webdav/`
- Windows “Map network drive” / third-party WebDAV tools
- Mobile file managers with WebDAV support

## Storage

Paths use the same storage resolution as the web UI (File API or SAF). Multi-volume mode exposes volume roots consistently with `/files`.

## Toggle

Server → Interfaces → WebDAV (**default off**). When disabled, WebDAV routes are not served.

User guide: [../user/connect-clients.md](../user/connect-clients.md).
