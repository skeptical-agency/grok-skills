# Grok skills

14 production-tested skills for the Grok skills folder, from the author's own content and marketing pipelines. Each one is a `SKILL.md` your Grok assistant reads, with a plain-English intro, install lines for macOS, Linux and Windows, a worked example, the exact commands and the known pitfalls.

## Skills

- [`skeptical-youtube-1080p-download`](skills/skeptical-youtube-1080p-download/SKILL.md): Download YouTube videos you have the rights to at full 1080p (up to 1080p60) from the terminal with yt-dlp and ffmpeg. The main recipe needs no account and no cookies: it uses the `web_embedded` player client.
- [`skeptical-vimeo-download-401-fix`](skills/skeptical-vimeo-download-401-fix/SKILL.md): A working path for downloading a Vimeo video you have the rights to when yt-dlp's Vimeo OAuth step returns a 401: player page, `config_refresh_url`, HLS playlist, ffmpeg.
- [`skeptical-burned-captions-ffmpeg`](skills/skeptical-burned-captions-ffmpeg/SKILL.md): TikTok and Shorts style word-timed captions burned into vertical video with whisper timings, your script text, PIL-rendered PNGs and ffmpeg overlay. No CapCut, no libass.
- [`skeptical-no-stutter-video-motion`](skills/skeptical-no-stutter-video-motion/SKILL.md): Why ffmpeg zooms stutter (whole-pixel rounding), the static-stills rule for text cards, and linting the build script instead of pixel-diffing.
- [`skeptical-shorts-thumbnail-safezone`](skills/skeptical-shorts-thumbnail-safezone/SKILL.md): Why text on 9:16 Shorts disappears in YouTube grids, the y = 400..1500 safe-zone rule on a 1080x1920 canvas, and a pre-publish crop check.
- [`skeptical-audio-loudness-ffmpeg`](skills/skeptical-audio-loudness-ffmpeg/SKILL.md): Hit -14 LUFS / -2.0 dBTP with ffmpeg loudnorm (two-pass), measure with ebur128, and duck music under voiceover with sidechaincompress.
- [`skeptical-ffmpeg-concat-traps`](skills/skeptical-ffmpeg-concat-traps/SKILL.md): The silent ffmpeg concat bugs: overlays that vanish after a segment boundary, SAR mismatches, dropped audio and one-frame stills, with fixes and checks.
- [`skeptical-gtm-form-lead-tracking`](skills/skeptical-gtm-form-lead-tracking/SKILL.md): Fix GTM form tags that fire on every Next click: fire on the confirmation event instead, wire enhanced conversions, and audit a published container with curl.
- [`skeptical-ga4-admin-api-automation`](skills/skeptical-ga4-admin-api-automation/SKILL.md): Script GA4 custom channel groupings, audiences, annotations and enhanced measurement with the Admin API, including the verified quirks (v1alpha, eachScope* fields, inListFilter UI bug, 2-grouping limit).
- [`skeptical-post-launch-site-audit`](skills/skeptical-post-launch-site-audit/SKILL.md): A ten-step terminal audit for a newly launched website: crawl, redirects, robots and sitemaps, tracking, staging leaks, schema, spellcheck, with a severity-bucketed report.
- [`skeptical-content-trend-triangulation`](skills/skeptical-content-trend-triangulation/SKILL.md): A daily topic-selection procedure: triangulate Google Trends RSS, Techmeme and trade press, never trust one AI search tool's denial, and log runner-ups.
- [`skeptical-source-footage-audit`](skills/skeptical-source-footage-audit/SKILL.md): A QA procedure for source footage: contact sheets, full-res fine pass, brightness check, privacy and policy audit, still images, and an exclusion log.
- [`skeptical-x-original-media-fetch`](skills/skeptical-x-original-media-fetch/SKILL.md): Fetch original-resolution photos and videos from X posts with no API: fxtwitter JSON for media URLs, then `?name=orig` on pbs.twimg.com.
- [`skeptical-osm-map-cards`](skills/skeptical-osm-map-cards/SKILL.md): Location map cards for videos and posts from OpenStreetMap tiles in pure Python: zoom picking, tile mosaic math, and the PIL black-strip bounds guard.

## Install

Clone this repo, then copy the skills you want into a skills directory.

User scope (all your projects), macOS or Linux, every skill:

    git clone https://github.com/skeptical-agency/grok-skills.git grok-skills
    mkdir -p ~/.grok/skills
    cp -r grok-skills/skills/* ~/.grok/skills/

One skill only:

    cp -r grok-skills/skills/skeptical-youtube-1080p-download ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    git clone https://github.com/skeptical-agency/grok-skills.git grok-skills
    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse grok-skills\skills\* "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r grok-skills/skills/skeptical-youtube-1080p-download .grok/skills/

## Responsible use

Several of these skills download or inspect third-party content or sites. Only download content you own or have the rights to use, respect each platform's terms and copyright law, and only audit sites and analytics properties you own or are authorized to test or administer. Each skill has its own responsible-use note where it applies.

## License

MIT, see LICENSE.
