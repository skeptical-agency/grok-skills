---
name: skeptical-x-original-media-fetch
description: "Pull original-res images/videos from X posts via fxtwitter JSON + ?name=orig, no API needed. Use when you need the full-size photo or video from an X post, or pbs.twimg.com image URLs arrive thumbnailed."
when-to-use: "You need the full-size photo or video from an X post as source material, pbs.twimg.com URLs arrive thumbnailed (?name=small, 900x900 etc.), or you have no X API access."
---

# Fetch original-resolution media from X posts (no API)

## What this does (plain English)

Images pulled from X posts often come back as small thumbnails. This skill gives your AI assistant a way to get the full-size originals without any X API access: ask the fxtwitter mirror's JSON endpoint for a post, read the direct media URLs from the response, and add `?name=orig` to the pbs.twimg.com image URL to get the untouched original. For videos, take the highest-bitrate mp4 variant. It also covers checking the real pixel size before use, a short "also works for" tips note for Facebook, TikTok and Instagram, and a source pecking order for when official footage is thin.

## Responsible use

Only fetch and use media you have the rights to use, and respect the terms of the platforms involved and the copyright of the people who posted the media. This recipe relies on a third-party mirror (fxtwitter), which can change or stop working at any time. Instagram and other platforms may restrict the tips in the "also works for" note, and the Instagram method varies and may break.

## Install what you need

You need `curl` (included on macOS, current Windows 10 and 11, and most Linux systems), Python 3 (for `python3 -m json.tool` and PIL dimension checks; install from python.org or your system package manager), and `ffprobe` from ffmpeg. `yt-dlp` is only needed for the Facebook/TikTok tip.

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

Get the original-size photo from a post. Replace `<user>` and `<id>` with the post's user and status id.

1. Ask fxtwitter for the post's JSON and read the media URLs:

```
curl -s "https://api.fxtwitter.com/<user>/status/<id>" | python3 -m json.tool
```

2. For a photo, take its pbs.twimg.com URL and force the original by appending `?name=orig` (replacing any existing `name=` parameter):

```
https://pbs.twimg.com/media/<id>.jpg?name=orig
```

3. Download it with a plain GET (a normal user agent works), then check the actual pixel dimensions with Python PIL or ffprobe/sips. A 900px thumbnail upscaled is garbage in a 1080p+ video.
4. For a video, take the highest-bitrate mp4 variant listed in the JSON, download it with `curl -L`, and verify it with ffprobe.

---

The full production recipe follows.


## When to Use
- You need the full-size photo or video from an X post as source material
- pbs.twimg.com URLs arrive thumbnailed (?name=small / 900x900 etc.)
- You have no X API access (or don't want to burn quota)

## The recipe

1. Hit the fxtwitter JSON API with the post's user + status id:

   ```
   curl -s "https://api.fxtwitter.com/<user>/status/<id>" | python3 -m json.tool
   ```
   The response includes media entries with direct URLs (photos and videos,
   including the best-quality mp4 variant).

2. For photos, take the pbs.twimg.com URL and force the original:
   append `?name=orig` (replacing any existing name= parameter):

   ```
   https://pbs.twimg.com/media/<id>.jpg?name=orig
   ```
   pbs.twimg.com is curl-friendly (plain GET with a normal UA works).

3. Verify before use: check actual pixel dimensions (Python PIL or
   ffprobe/sips) - press photos must survive full-res scrutiny; a 900px
   thumbnail upscaled is garbage in a 1080p+ video.

4. Videos: the fxtwitter JSON lists variant bitrates; take the highest mp4.
   Download with `curl -L`, verify with ffprobe.

## Also works for (tips)
Only use media you have rights to or permission to use. These are short tips for other platforms; the Instagram method varies and may break.
- Facebook/TikTok SHARE LINKS pull anonymously with yt-dlp (no cookies needed
  in practice); useful when a brand only posted vertical video there.
- Instagram: public post media can often be pulled from the `?__a=1`-style JSON
  or via yt-dlp on the post URL (varies with their current lockdown state).

## Source pecking order (when official footage is thin)
real footage windows > official hi-res press photos (this skill) >
user-supplied footage > generated text cards. Never the reverse.
