# Server engines (CIO / Netty)

Selection comes from Server → Server type (often **Auto**).

## Decision

| Case | Engine |
|------|--------|
| SSL on | **Netty** (always) |
| Auto | **Netty** |
| Explicit Netty | Netty |
| Explicit CIO | CIO (HTTP only) |

HTTPS uses an SSL connector with the KeyStore from the selected certificate profile. Bind host follows **Address**: all interfaces (`0.0.0.0`), localhost, or a specific IP — see [network-address.md](network-address.md).

## Netty tuning (why it matters)

Large buffers with very short write timeouts broke slow Wi‑Fi video. Current knobs (order of magnitude):

- Longer response write timeout (~60s)
- Request queue limit
- HTTP codec chunk size tied to the **buffer size** setting

CIO uses shorter idle timeouts and does **not** honor buffer size the same way for uploads — another reason Auto → Netty.

## Plugins (both engines)

Typical install order: logging, forwarded headers, compression, partial content, websockets, status pages, sessions, authentication, then routes (redirects, `/`, files, login, API, optional WebDAV/RSS/Info).

## Related

- [http-server.md](http-server.md)
- [../operations/lifecycle.md](../operations/lifecycle.md)
- [ssl-certificates.md](ssl-certificates.md)
