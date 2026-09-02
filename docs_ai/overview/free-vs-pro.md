# Free vs Pro

Same app codebase (`:app`), two product flavors and two companion modules.

## Identifiers

| | Free | Pro |
|---|------|-----|
| Flavor | `free` | `pro` |
| `applicationId` | `tiar.ua.slf` | `tiar.ua.slf.pro` |
| `BuildConfig.IS_PRO` | `false` | `true` |
| Default listen port | **8080** | **8081** |
| Companion module | `:free` | `:pro` |

Both can be installed together. Notifications, prefs, and ports are isolated by package UID.

## Capability matrix

| Capability | Free | Pro |
|------------|------|-----|
| Ads (AdMob) | Yes | No |
| HTTP / WebDAV / API / RSS / Info / FTP | Yes | Yes |
| SSL certificate library | Yes | Yes |
| Multiple users | Limited (effectively one) | Unlimited |
| Redirects | No (Pro-gated) | Yes (experimental) |
| Persist hidden files | No | Yes |
| Custom server / welcome text | Limited | Yes |
| Home-screen server widgets | No | Yes |
| App shortcuts (start server) | No | Yes |
| Custom web favicon | Default | Optional custom icon |
| Blocked IPs | Capped | Unlimited |

Runtime checks go through the Pro façade (different implementations in `:free` vs `:pro`).

## UI / marketing

- Free shows ads and Pro upgrade entry points.
- Pro hides ad surfaces and enables widgets in the Pro manifest overlay.

## Implementation notes

- Do **not** fork business logic into flavor source sets unless necessary; prefer the Pro façade.
- Flavor manifests merge with `main`.
- Shared FGS / permissions stay in `main`; Pro adds widgets and shortcuts.

See also [../modules/free-pro.md](../modules/free-pro.md).
