# Module `:app`

Main Android application. Namespace / package: `tiar.ua.slf`.

## Responsibilities

- Native UI (phone tabs, TV guided Home, dialogs, widgets)
- Embedded HTTP server and route modules (+ optional FTP)
- Storage resolution (File + SAF)
- Settings façade
- Bundled web assets
- Product flavors `free` / `pro`

## Areas (conceptual)

| Area | Purpose |
|------|---------|
| App / service / receivers | Process singleton, FGS, boot |
| Server | Engines, routes, SSL, FTP |
| Storage | Multi-volume, path resolve, transfers |
| Settings | Preference façade |
| UI | Activities, fragments, preference cards, TV overlays |
| Utils | Network address helpers, dialogs, Pro façade, logs |
| Widget | Pro home-screen widgets |

## Source sets

| Path | Role |
|------|------|
| `src/main/` | Shared code, resources, base manifest |
| `src/free/` | Free manifest overlays (ads, package) |
| `src/pro/` | Pro manifest overlays (widgets, shortcuts) |

## Entry surfaces

- Splash — launcher / leanback / shortcuts
- Main — phone Home / Settings / Info / Logs; TV hosts guided Home stack
- Settings activity — nested settings (users, certs, …)

## When to edit here

Almost all product behavior lives in `:app`. Use `:free` / `:pro` only for the thin Pro façade / ads split, and `:acme` for ACME protocol details.
