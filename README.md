# Satish IQ Feed

This folder is the local staging copy of the static feed consumed by the iPhone app.

Files:

- `feed.json`: manifest with latest generated date and download URLs.
- `satishos-current.json`: latest Satish IQ snapshot.
- `daily-briefing.mp3`: latest commute audio.

Publish command:

```bash
node scripts/generate-daily-audio.mjs --prepare-only
say -v Rishi -r 166 -o audio/daily-briefing.aiff -f audio/daily-briefing-script.md
node scripts/generate-daily-audio.mjs --master-existing-aiff
node scripts/export-ios-snapshot.mjs
node scripts/publish-podcast-episode.mjs
node scripts/publish-feed.mjs
node scripts/publish-public-feed.mjs
```

For a manually recorded Satish voice file, use:

```bash
node scripts/use-my-voice-briefing.mjs prepare
node scripts/use-my-voice-briefing.mjs import ~/Desktop/satish-briefing.m4a
node scripts/export-ios-snapshot.mjs
node scripts/publish-podcast-episode.mjs
node scripts/publish-feed.mjs
node scripts/publish-public-feed.mjs
```

Podcast packaging:

`node scripts/publish-podcast-episode.mjs`

This creates immutable episode audio, In Simple Terms branded episode artwork, a Spotify upload folder, and an optional RSS feed under `podcast/`.

The iOS app defaults to the public feed-only repository:

`https://raw.githubusercontent.com/scinorx/satish-iq-feed/main/feed.json`

Why separate repo: the main SatishOS repository can stay private, while the phone can still fetch a tiny public feed without storing GitHub tokens or secrets.
