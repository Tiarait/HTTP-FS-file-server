# Android TV / D-pad focus

TV UI is detected at runtime (Leanback / television features). Phone bottom navigation is not used; TV uses a guided shell after the wizard.

## TV Home

Two-column layout:

| Side | Stopped | Running |
|------|---------|---------|
| **Left** | Logo, version, short product copy | Large QR + URL chip |
| **Right** | Status card + metrics + action row + menu | Same |

**Status card** shows Running/Stopped, advertised `host:port`, and compact metrics (Wi‑Fi, WebDAV, HTTPS, Files UI). Action buttons sit **inside** the card (Refresh, Address, Copy, QR, Share), full-width with dividers.

**Menu:** Start/Stop, Quick settings, Settings, Info, Logs (if enabled), Exit.

### D-pad (Home)

- Left branding / QR column does not take focus.
- Focus moves among action buttons and the menu list.
- Up from the first menu row returns to the action row; Left on the menu is clamped so focus does not escape into the left column.
- Center / Enter activates the focused control.

## Dialogs on TV

Alerts, single/multi choice, and text input use **leanback-style overlays** (bottom panel + action list), not phone bottom sheets.

The **QR** action opens a dedicated TV QR overlay (code + URL + OK), not a text-only alert.

## Settings lists

Preference-style screens use Leanback **vertical lists** on TV:

- Window alignment / focus scroll for D-pad navigation
- Preference click / long-press wiring stays on the preference adapter

## Certificate edit

Custom rows:

- Rows are focusable; padding adjusted for TV
- Non-interactive chrome marked non-focusable so focus stays on rows
- After panel rebuilds (mode / DNS state), restore focus to the first useful row

## End actions on TV

Where phone UI uses a trailing edit icon click, TV often uses **long‑press** (Certificates list → open editor).

## For contributors

When adding a settings list screen, copy an existing TV list screen pattern. For custom screens, mirror certificate edit: focusable rows + focus recovery after async UI changes. Home actions belong in the status card and must remain focusable descendants.
