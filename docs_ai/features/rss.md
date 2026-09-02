# RSS feed

## Endpoint

**`/rss/{path…}`**, enabled by Server → Interfaces → RSS (**default on**).

## Behavior

- `GET` only.
- Directory → RSS XML listing of children (media-oriented feed for folder contents).
- File → serve/download that file (same storage stack as files/WebDAV).
- Multi-volume paths follow the same volume rules as `/files`.
- Respects hidden-file rules and volume errors like other mounts.
- Initial-file / hard-redirect helpers may apply in single-root mode.

## Clients

Any RSS reader pointed at `http(s)://host:port/rss/` or a subfolder path. Not a full podcast CMS — folder listing as feed items.

## Related

- [../user/connect-clients.md](../user/connect-clients.md)
- [../reference/url-map.md](../reference/url-map.md)
