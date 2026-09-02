# Settings keys reference

Persistence: default `SharedPreferences` via the app settings façade.

Defaults below are approximate — confirm in the settings layer when changing behavior.

## Server

| Pref key | Type | Default / notes |
|----------|------|-----------------|
| `port_value` | Int | Free 8080 / Pro 8081 |
| `name_client_value` | String | Client label |
| `name_serv_value` | String | `"HTTP FS"` |
| `path_value` | String | Internal storage path; multi-volume token possible |
| `server_root_mode` | String enum | `SINGLE_PATH` / `MULTI_VOLUME` |
| `volume_grants_v1` | String JSON | Volume grant list |
| `usefile_value` | Boolean | Use initial file |
| `file_redirect_value` | Boolean | Hard redirect |
| `file_value_path` / `file_value_name` | String | Initial file |
| `ssl_key` | Boolean | HTTPS on/off |
| `ssl_certificate_id` | String | Selected profile id |
| `cache_cert` | Boolean | Auto cert keep-after-restart |
| `secure_key` | Boolean | Password protection |
| `pass_key` | String | Legacy single-password when users list empty |
| `users_list` | String list | Preferred users storage |
| `users` | Set | Legacy; migrated into `users_list` |
| `session_key` | Int | Minutes, default `60` |
| `session_num_key` | Int | Session count limit |
| `permissions_key` | Set | Anonymous read/write flags |
| `servtype_key0` | String | `"Auto"` (engine selection) |
| `buffer_size` | Int | Transfer buffer |
| `blocked_ips` | Set | |
| `redirects` | Set | Pro |
| `webdav_key` | Boolean | default **false** |
| `rss_key` | Boolean | default **true** |
| `files_ui_key` | Boolean | default **true** |
| `info_key` | Boolean | HTTP `/info/` — default **false** |
| `ftp_key` | Boolean | default **false** |
| `ftp_port_key` | Int | default **2121** |
| `guest_mode_key` | Boolean | with password on; guests view-only browsing |
| `readonly_key` | Boolean | password off; blocks writes for everyone |
| `cors_key` | Boolean | default false |
| `http_cache_key` | Boolean | default true |
| `lan_name_key` | Boolean | mDNS advertise; default false |
| `lan_host_key` | String | default `httpfs` |
| `max_upload_mb_key` | Int | 0 = unlimited |
| `idle_timeout_key` | Int | seconds, default 60 |
| `max_connections_key` | Int | default 128 |
| `socket_buffer_key` | Int | TCP buffer (separate from copy buffer) |
| `http2_key` | Boolean | HTTPS only; default false |
| `hide_dotfiles_key` | Boolean | default false |
| `ip_host` | String | `"all"` / `"localhost"` / specific address |
| (IP change policy) | enum-like | Auto-rebind / Keep running / Stop (specific address) |

## App

| Pref key | Notes |
|----------|-------|
| `start_app_key` | Start with device (boot) |
| `start_server_key` | Start with app |
| `show_logs` | default true |
| `show_logs_all` | verbose logs |
| `app_lang` | empty = system |
| `server_lang` | server/web language |
| `use_vibro` | haptics |
| `use_blur` | dialog blur; default true on API 31+ |
| `battery_key` | battery exemption prefs; default true |
| `keep_screen_on_key` | while app open; default false |
| `permission_folder_key` | folder permission related |
| `version_value` | last seen version |
| `count_server_started` | starts counter |
| `show_first_splash` / `show_first_msgs` | onboarding flags |
| `main_started` | MainActivity started flag |
| `rewarded_ad_free_until` | legacy Free ads pause; session model preferred |

## Client

| Pref key | Notes |
|----------|-------|
| `client_con` | connection check socket → status WS |
| `battery_folder_size_key` | compute folder sizes |
| `battery_thumbnail_key` | thumbnails; default true |
| `welcome_dialog_key` | welcome HTML (Pro editable) |
| `welcome_dialog_show_key` | show welcome |
| `notices_key` | device notices in browsers; default true |
| `use_pwa` | installable PWA; default false |

## Storage helpers

| Pref key | Notes |
|----------|-------|
| `uri_sd_{name}` | Legacy/per-volume SAF URI helpers |
| `uri_cur` | Current picker URI |

## Certificate profiles

Certificate library metadata lives in app-private SSL/ACME store files. Prefs keep `ssl_key` + `ssl_certificate_id` (+ `cache_cert`). User guide: [../user/https-certificates.md](../user/https-certificates.md).

## Notes

- Prefer settings getters/setters over raw keys when changing code.
- Free/Pro limits are enforced in the Pro façade, not by omitting keys.
- Clearing app data wipes these prefs and local cert stores.
- End-user descriptions: [../user/settings-guide.md](../user/settings-guide.md).
- Address / LAN behavior: [../features/network-address.md](../features/network-address.md).
