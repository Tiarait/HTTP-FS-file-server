# Project layout

```text
HttpFS/
├── app/                 # Android application (:app)
│   ├── src/main/        # Shared code, res, assets, manifest
│   ├── src/free/        # Free overlays
│   ├── src/pro/         # Pro overlays
│   └── build.gradle     # Flavors, version, SDK
├── free/                # :free library
├── pro/                 # :pro library
├── acme/                # :acme ACME library
├── client_HttpFS/       # Web UI sources (Vue / npm)
├── docs_ai/             # Public documentation (this tree — self-contained)
├── build.gradle         # Root plugins / versions
├── settings.gradle      # Module includes
├── gradle.properties
└── gradle.properties.local   # Optional signing (local)
```

## Where to look first

| Task | Start here |
|------|------------|
| Version / SDK | `app/build.gradle` |
| Product behavior docs | `docs_ai/user/`, `docs_ai/features/` |
| Server / URL map | `docs_ai/features/http-server.md`, `docs_ai/reference/url-map.md` |
| Network address logic | `docs_ai/features/network-address.md` |
| SSL / ACME | `docs_ai/features/ssl-certificates.md`, `docs_ai/modules/acme.md` |
| Settings keys | `docs_ai/reference/settings-keys.md` |
| Free/Pro | `docs_ai/overview/free-vs-pro.md` |
| Web UI | `docs_ai/development/web-client.md` |
| FGS / Play text | `docs_ai/operations/foreground-service.md`, `play-console-fgs-special-use.md` |
| TV | `docs_ai/development/tv-focus.md` |

When publishing documentation, ship **only** `docs_ai/` — do not require files outside this folder.

## Generated / local (usually not for docs commits)

- `build/`, `.gradle/`, `client_HttpFS/dist/`
- IDE `.idea/`
- `local.properties`
