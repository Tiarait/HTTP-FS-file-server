# Free ads and Pro upgrade UI

High-level product behavior only. Ad unit IDs stay in the Free module / manifests, not in public docs.

## Where ads live

| Piece | Notes |
|-------|-------|
| AdMob interstitial / rewarded | Free companion module |
| UMP / privacy options | Settings → Ads privacy |
| Session “ads off after reward” | In-process until the app process ends |
| Upgrade prompts | About, Pro-locked preferences, Free limit hits |

Pro flavor has no ad SDK wiring.

## User flows

1. **Interstitial** — Free may show on certain navigation / resume paths when not in a rewarded-free session.
2. **Disable ads (rewarded)** — Settings → disable ads → watch rewarded ad → banner/interstitial gated off until process ends.
3. **Privacy / consent** — Settings → Ads privacy.
4. **Upgrade to Pro** — dialogs from About, locked prefs, and limit hits (users, blocked IPs, etc.).

Legacy stored expiry prefs may be cleared on upgrade paths; current model is process-lifetime session.

## Limits vs ads

Free feature caps (users, blocked IPs, …) are independent of whether ads are temporarily disabled.

## Related

- [../overview/free-vs-pro.md](../overview/free-vs-pro.md)
- [../modules/free-pro.md](../modules/free-pro.md)
