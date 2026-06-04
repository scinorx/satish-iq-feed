# Satish IQ Feed

This folder is the local staging copy of the static feed consumed by the iPhone app.

Files:

- `feed.json`: manifest with latest generated date and download URLs.
- `satishos-current.json`: latest Satish IQ snapshot.
- `daily-briefing.mp3`: latest commute audio.

Publish command:

```bash
node scripts/export-ios-snapshot.mjs
node scripts/publish-feed.mjs
node scripts/publish-public-feed.mjs
```

The iOS app defaults to the public feed-only repository:

`https://raw.githubusercontent.com/scinorx/satish-iq-feed/main/feed.json`

Why separate repo: the main SatishOS repository can stay private, while the phone can still fetch a tiny public feed without storing GitHub tokens or secrets.
