# Grok skills

16 production-tested skills for the Grok skills folder, from the author's own content and marketing pipelines. Each one is a `SKILL.md` your Grok assistant reads, with a plain-English intro, install lines for macOS, Linux and Windows, a worked example, the exact commands and the known pitfalls.

## Skills

- [`skeptical-youtube-1080p-download`](https://github.com/skeptical-agency/skeptical-youtube-1080p-download): Download YouTube videos you have the rights to at full 1080p (up to 1080p60) from the terminal with yt-dlp and ffmpeg. The main recipe needs no account and no cookies: it uses the `web_embedded` player client.
- [`skeptical-vimeo-download-401-fix`](https://github.com/skeptical-agency/skeptical-vimeo-download-401-fix): A working path for downloading a Vimeo video you have the rights to when yt-dlp's Vimeo OAuth step returns a 401: player page, `config_refresh_url`, HLS playlist, ffmpeg.
- [`skeptical-burned-captions-ffmpeg`](https://github.com/skeptical-agency/skeptical-burned-captions-ffmpeg): TikTok and Shorts style word-timed captions burned into vertical video with whisper timings, your script text, PIL-rendered PNGs and ffmpeg overlay. No CapCut, no libass.
- [`skeptical-no-stutter-video-motion`](https://github.com/skeptical-agency/skeptical-no-stutter-video-motion): Why ffmpeg zooms stutter (whole-pixel rounding), the static-stills rule for text cards, and linting the build script instead of pixel-diffing.
- [`skeptical-shorts-thumbnail-safezone`](https://github.com/skeptical-agency/skeptical-shorts-thumbnail-safezone): Why text on 9:16 Shorts disappears in YouTube grids, the y = 400..1500 safe-zone rule on a 1080x1920 canvas, and a pre-publish crop check.
- [`skeptical-audio-loudness-ffmpeg`](https://github.com/skeptical-agency/skeptical-audio-loudness-ffmpeg): Hit -14 LUFS / -2.0 dBTP with ffmpeg loudnorm (two-pass), measure with ebur128, and duck music under voiceover with sidechaincompress.
- [`skeptical-ffmpeg-concat-traps`](https://github.com/skeptical-agency/skeptical-ffmpeg-concat-traps): The silent ffmpeg concat bugs: overlays that vanish after a segment boundary, SAR mismatches, dropped audio and one-frame stills, with fixes and checks.
- [`skeptical-gtm-form-lead-tracking`](https://github.com/skeptical-agency/skeptical-gtm-form-lead-tracking): Fix GTM form tags that fire on every Next click: fire on the confirmation event instead, wire enhanced conversions, and audit a published container with curl.
- [`skeptical-ga4-admin-api-automation`](https://github.com/skeptical-agency/skeptical-ga4-admin-api-automation): Script GA4 custom channel groupings, audiences, annotations and enhanced measurement with the Admin API, including the verified quirks (v1alpha, eachScope* fields, inListFilter UI bug, 2-grouping limit).
- [`skeptical-post-launch-site-audit`](https://github.com/skeptical-agency/skeptical-post-launch-site-audit): A ten-step terminal audit for a newly launched website: crawl, redirects, robots and sitemaps, tracking, staging leaks, schema, spellcheck, with a severity-bucketed report. Refined across 15+ production site launches.
- [`skeptical-content-trend-triangulation`](https://github.com/skeptical-agency/skeptical-content-trend-triangulation): A daily topic-selection procedure: triangulate Google Trends RSS, Techmeme and trade press, never trust one AI search tool's denial, and log runner-ups.
- [`skeptical-source-footage-audit`](https://github.com/skeptical-agency/skeptical-source-footage-audit): A QA procedure for source footage: contact sheets, full-res fine pass, brightness check, privacy and policy audit, still images, and an exclusion log.
- [`skeptical-x-original-media-fetch`](https://github.com/skeptical-agency/skeptical-x-original-media-fetch): Fetch original-resolution photos and videos from X posts with no API: fxtwitter JSON for media URLs, then `?name=orig` on pbs.twimg.com.
- [`skeptical-osm-map-cards`](https://github.com/skeptical-agency/skeptical-osm-map-cards): Location map cards for videos and posts from OpenStreetMap tiles in pure Python: zoom picking, tile mosaic math, and the PIL black-strip bounds guard.
- [`skeptical-gbp-vendor-audit-review`](https://github.com/skeptical-agency/skeptical-gbp-vendor-audit-review): Review a vendor's Google Business Profile audit before approving changes: nine standing traps, an audit-the-auditor verification pass, and one copy-paste reply that answers everything.
- [`skeptical-youtube-post-publish-optimizer`](https://github.com/skeptical-agency/skeptical-youtube-post-publish-optimizer): A nightly workflow that applies tags, category, recording date and standing defaults to just-published YouTube videos, verifies every write by read-back, runs alert-only QA gates, and enforces a 72 hour packaging-change protocol.

## Install

Every skill lives in its own repository (linked above) with its own README, LICENSE and `SKILL.md`. Clone the one you want straight into your skills folder.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    git clone https://github.com/skeptical-agency/skeptical-youtube-1080p-download.git ~/.grok/skills/skeptical-youtube-1080p-download

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    git clone https://github.com/skeptical-agency/skeptical-youtube-1080p-download.git "$HOME\.grok\skills\skeptical-youtube-1080p-download"

Project scope (shared via git):

    mkdir -p .grok/skills
    git clone https://github.com/skeptical-agency/skeptical-youtube-1080p-download.git .grok/skills/skeptical-youtube-1080p-download

Swap in any repo name from the list above. This repo also still holds a copy of all 16 skills under `skills/`, so you can clone it once and copy them all:

    git clone https://github.com/skeptical-agency/grok-skills.git grok-skills
    mkdir -p ~/.grok/skills
    cp -r grok-skills/skills/* ~/.grok/skills/

## Responsible use

Several of these skills download or inspect third-party content or sites. Only download content you own or have the rights to use, respect each platform's terms and copyright law, and only audit sites and analytics properties you own or are authorized to test or administer. Each skill has its own responsible-use note where it applies.

## License

MIT, see LICENSE.
