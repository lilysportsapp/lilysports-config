# lilysports-config

Public runtime config for the [LilySports Android TV app](https://github.com/lilysportsapp/android-tv-app). The app fetches `sources.json` from this repo's `main` branch on a cadence (currently every ~6h, falling back to its last-known-good cache, then to hardcoded defaults if this repo is ever unreachable).

**Why this exists:** the app's stream sources are gray-market aggregator sites whose mirror domains rotate periodically (ppv.to was revoked and replaced by ppv.st on 2026-07-01). Editing this file lets every installed copy of the app pick up a new mirror on its next refresh — no app rebuild, no release, no reinstall.

## Updating a rotated mirror

1. Confirm the new domain is actually live and serves the same shape of data before editing (don't guess-swap blind):
   - `streamedBaseUrl` — should serve `GET {base}api/matches/live` and `GET {base}api/matches/all` as JSON arrays of matches.
   - `ppvBaseUrl` / `ppvReferer` — `{ppvBaseUrl}api/streams` should return `{"streams": [{"streams": [...]}]}` (category → stream list).
   - `streamsports99BaseUrl` — a channel watch-page host; `{base}<channel slug>` should load a working player page.
   - `daddyliveBaseUrl` — the DaddyLive front-door host; `{base}/watch.php?id=<n>` should load a watch page.
2. Edit `sources.json` directly on GitHub (web UI works fine) and commit to `main`.
3. Done — installed apps pick it up within their normal refresh cadence.

## Fields

| Field | Used for |
|---|---|
| `streamedBaseUrl` | streamed.pk-equivalent match catalog |
| `ppvBaseUrl` | ppv-equivalent match catalog API |
| `ppvReferer` | Referer header sent with the ppv API request |
| `streamsports99BaseUrl` | channel stream picker (WebView-resolved) |
| `daddyliveBaseUrl` | channel stream picker (pure-HTTP DaddyLive resolver) |
