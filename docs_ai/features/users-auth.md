# Users and authentication

## Modes

1. **Password protection off** — anonymous access; global **Permissions** (Read / Write) and/or **Read-only** apply.
2. **Password protection on** — users must authenticate (form login for browser, HTTP Basic for WebDAV).

## User model

Stored in preferences:

- Name / password
- Rights: Read, Write, or Read+Write
- Optional custom home folder

## Guest mode (password on)

When enabled, anonymous/guest browser access is limited to **viewing files** (not full directory-manager browsing). Authenticated users still use their account rights.

## Read-only (password off)

Global switch that blocks writes for everyone. Hidden while Password is on (use Read-only users instead).

## Sessions

- Cookie / server sessions with configurable timeout (default 60 minutes)
- Logout / logout-all in settings
- Session limits may differ by Free/Pro

## Free vs Pro

- Free: effectively a **single** user
- Pro: multiple users

## Blocked IPs

Separate list under Server security settings.

- Free: capped count
- Pro: unlimited

## Redirects (Pro)

Experimental URL/file redirects. Gated behind Pro.

## Related

- User steps: [../user/recipes.md](../user/recipes.md) (password / read-only recipes)
- Settings: [../user/settings-guide.md](../user/settings-guide.md)
- Free vs Pro: [../overview/free-vs-pro.md](../overview/free-vs-pro.md)
