# Modules `:free` and `:pro`

Thin Android library modules with a **shared API surface** and different implementations.

## API surface (conceptually)

- Pro capability checks and Free limits (users, redirects, hidden files, …)
- Ad initialization hooks (real AdMob in free; no-ops / absent in pro)

`:app` depends on one of them via flavor (`freeImplementation` / `proImplementation`).

## Free (`:free`)

- Not Pro
- AdMob SDK + APPLICATION_ID in free manifest
- `AD_ID` permission
- Enforces free limits when called from the app façade

## Pro (`:pro`)

- Is Pro
- No ads
- Full multi-user / redirects / hidden files persistence behavior

## App-side façade

Prefer the in-app Pro façade instead of calling the companion API everywhere. That keeps call sites clean.

## Manifest split

Widget providers and app shortcuts are declared in the **Pro app source set** (and may also involve the `:pro` library) — check both when changing Pro-only entry points.

## Related

- [../overview/free-vs-pro.md](../overview/free-vs-pro.md)
- [../features/ads-free.md](../features/ads-free.md)
