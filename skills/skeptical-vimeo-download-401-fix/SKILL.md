---
name: skeptical-vimeo-download-401-fix
description: "Download Vimeo videos via the config_refresh_url + HLS path when yt-dlp OAuth fails. Use when yt-dlp returns an OAuth 401 on a Vimeo video you have the rights to download."
when-to-use: "yt-dlp fails on a Vimeo video with an OAuth 401 (\"Failed to fetch macos OAuth token\"), the old player.vimeo.com config endpoint returns a \"Sorry\" page, or you need a Vimeo-hosted video you have rights to locally at full quality."
---

# Vimeo download: a working path when yt-dlp's Vimeo OAuth returns 401

## What this does (plain English)

Sometimes yt-dlp cannot download a Vimeo video because its Vimeo login step returns an OAuth 401 error, and the old player config address no longer works. This skill teaches your AI assistant a manual path that worked in production: load the video's player page the way a browser would, find the `config_refresh_url` in it, read the JSON that comes back, and hand the HLS playlist (an `.m3u8` file) to ffmpeg with the same headers. If the saved file has glitches, a clean re-encode fixes it. It is a working path for when yt-dlp's Vimeo OAuth step fails. Try yt-dlp first, since it still works for many public Vimeo videos.

## Responsible use

Only download content you own or have permission to download. Respect Vimeo's Terms of Service, the video owner's privacy and embed settings, and copyright law. The Referer used in this recipe is the address of the site that really embeds the video, as a browser would send. This skill is not for getting around paywalls, DRM or access controls, and it should not be used for that.

## Install what you need

The steps use `curl` (already included on macOS, current Windows 10 and 11, and most Linux systems), `ffmpeg` and `ffprobe`. `yt-dlp` is only needed for the "try yt-dlp first" step.

macOS:

```
brew install yt-dlp ffmpeg
```

Linux (Debian/Ubuntu shown; on Fedora use `sudo dnf install ffmpeg`, which may need the RPM Fusion repo enabled):

```
pipx install yt-dlp
sudo apt install ffmpeg
```

Windows (PowerShell or Command Prompt; open a NEW terminal window after installing so the tools show up on PATH):

```
winget install yt-dlp.yt-dlp
winget install Gyan.FFmpeg
```

Check it worked: `yt-dlp --version` and `ffprobe -version` should both print a version.

## Worked example

Say yt-dlp fails on a Vimeo video you own with `Failed to fetch macos OAuth token`. Replace the placeholders (`<id>`, `<browser UA>`, `<embedding site url>`, `<embedding site>`, `<m3u8 url>`) with your own values. Every command below is copied from the manual path further down.

1. Fetch the player page with a browser User-Agent and the Referer of the site embedding the video:

```
curl -s -A "<browser UA>" -e "<embedding site url>" "https://player.vimeo.com/video/<id>" -o player.html
```

2. Find `config_refresh_url` in `player.html`, then fetch that URL with the headers User-Agent (browser), `X-Requested-With: XMLHttpRequest` and Referer (embedding site). The response is JSON config.
3. In the JSON, `files.progressive` is usually empty. Use `files.hls.cdns[default_cdn].url` (an m3u8 playlist).
4. Pull it with ffmpeg, passing the same headers:

```
ffmpeg -headers "User-Agent: <browser UA>\r\nReferer: <embedding site>\r\n" -i "<m3u8 url>" -c copy out.mp4
```

5. If ffmpeg prints "Invalid NAL unit size" warnings, re-encode cleanly:

```
ffmpeg -i out.mp4 -c:v libx264 -crf 18 -c:a aac clean.mp4
```

6. Verify with ffprobe (width/height/duration) and look at the frames before using the file.

---

The full production recipe follows.


## When to Use
- yt-dlp Vimeo pulls fail with OAuth 401 ("Failed to fetch macos OAuth token")
- The old player.vimeo.com/video/<id>/config endpoint returns a "Sorry" page (it is dead)
- You need a Vimeo-hosted video locally at full quality

Only download content you have the rights to.

## Try yt-dlp first
yt-dlp still works for many public Vimeo videos. When it 401s on the macOS OAuth
token fetch, use the manual path below.

## The manual path (production-verified 2026-09-20)

1. GET the player page with a browser User-Agent and the Referer of the site
   embedding the video:

   ```
   curl -s -A "<browser UA>" -e "<embedding site url>" "https://player.vimeo.com/video/<id>" -o player.html
   ```

2. Regex out "config_refresh_url" from the page HTML.

3. Fetch that URL with headers: User-Agent (browser), X-Requested-With:
   XMLHttpRequest, Referer (embedding site). Response is JSON config.

4. In the JSON: files.progressive is usually EMPTY. Use
   files.hls.cdns[default_cdn].url (an m3u8 playlist).

5. Pull with ffmpeg, passing the same headers:

   ```
   ffmpeg -headers "User-Agent: <browser UA>\r\nReferer: <embedding site>\r\n" -i "<m3u8 url>" -c copy out.mp4
   ```

6. Copy mode emits "Invalid NAL unit size" warnings; the file may be broken.
   A clean re-encode fixes it:

   ```
   ffmpeg -i out.mp4 -c:v libx264 -crf 18 -c:a aac clean.mp4
   ```

7. Verify with ffprobe (width/height/duration) and eyeball frames.

## Pitfalls
- Third-party demo embeds can carry recorder watermarks; check frames before use.
- Watch for BLACK "awaiting video" placeholder zones in the stream; exclude those windows.
- Headers matter on every request (player page, config refresh, m3u8/segments);
  dropping the Referer mid-flow gets 403s.
