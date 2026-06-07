# Satish IQ Feed

This folder is the local staging copy of the static feed consumed by the iPhone app.

Files:

- `feed.json`: manifest with latest generated date and download URLs.
- `run-status.json`: latest automation-visible run marker and outcome summary.
- `satishos-current.json`: latest Satish IQ snapshot.
- `daily-briefing.mp3`: latest commute audio.

Repo-side run instrumentation:

- Start marker: `node scripts/daily-feed-run-status.mjs start --scheduled-for 2026-06-08T05:30:00-04:00 --stage kickoff`
- Stage update: `node scripts/daily-feed-run-status.mjs stage --name podcast --status running`
- Finish marker: `node scripts/daily-feed-run-status.mjs finish --status success --stage completed`
- Freshness check for a backup watchdog: `node scripts/check-daily-feed-freshness.mjs`

Publish command:

```bash
node scripts/generate-daily-audio.mjs --prepare-only
say -v Rishi -r 166 -o audio/daily-briefing.aiff -f audio/daily-briefing-script.md
node scripts/generate-daily-audio.mjs --master-existing-aiff
node scripts/export-ios-snapshot.mjs
zsh -lc 'source ~/.zshrc; node scripts/publish-in-simple-terms-podcast.mjs'
node scripts/publish-feed.mjs
node scripts/publish-public-feed.mjs
```

For a manually recorded Satish voice file, use:

```bash
node scripts/use-my-voice-briefing.mjs prepare
node scripts/use-my-voice-briefing.mjs import ~/Desktop/satish-briefing.m4a
node scripts/export-ios-snapshot.mjs
zsh -lc 'source ~/.zshrc; node scripts/publish-in-simple-terms-podcast.mjs'
node scripts/publish-feed.mjs
node scripts/publish-public-feed.mjs
```

Podcast packaging:

`node scripts/publish-in-simple-terms-podcast.mjs`

This reads `05-Outputs/Daily/IN_SIMPLE_TERMS_5MIN.md`, creates immutable ElevenLabs episode audio, In Simple Terms branded episode artwork, a manual fallback upload folder, and the automated RSS feed under `podcast/`.

The iOS app defaults to the public feed-only repository:

`https://raw.githubusercontent.com/scinorx/satish-iq-feed/main/feed.json`

Why separate repo: the main SatishOS repository can stay private, while the phone can still fetch a tiny public feed without storing GitHub tokens or secrets.
