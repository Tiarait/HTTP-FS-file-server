# Permissions

Declared in the main app manifest (flavor overlays add ads / widgets).

## Network

| Permission | Use |
|------------|-----|
| `INTERNET` | Serve and client HTTP |
| `ACCESS_NETWORK_STATE` | Connectivity |
| `ACCESS_WIFI_STATE` | LAN / Wi‑Fi info |

## Storage

| Permission | Use |
|------------|-----|
| `MANAGE_EXTERNAL_STORAGE` | Broad file access (with UI flows) |
| `READ_EXTERNAL_STORAGE` / `WRITE_EXTERNAL_STORAGE` | Legacy (maxSdk 32) |

## Background / reliability

| Permission | Use |
|------------|-----|
| `FOREGROUND_SERVICE` | Base FGS |
| `FOREGROUND_SERVICE_SPECIAL_USE` | Long-running server FGS |
| `POST_NOTIFICATIONS` | Ongoing server notification |
| `RECEIVE_BOOT_COMPLETED` | Autostart |
| `QUICKBOOT_POWERON` | Some OEM boot intents |
| `REQUEST_IGNORE_BATTERY_OPTIMIZATIONS` | OEM survival |
| `WAKE_LOCK` | Partial wake while serving |

## Free-only

| Permission | Use |
|------------|-----|
| `AD_ID` | AdMob |

## Features (optional hardware)

Touchscreen / leanback / mic / bluetooth / wifi marked `required=false` for TV and varied devices.

## Notes

- Changing FGS type requires matching Play Console declaration and the subtype property string.
- Do not reintroduce `FOREGROUND_SERVICE_DATA_SYNC` for the server notification service.
