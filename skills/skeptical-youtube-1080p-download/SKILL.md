---
name: skeptical-youtube-1080p-download
description: "Download YouTube videos at full 1080p from the terminal with yt-dlp. Use when pulling a video, VOD, or trailer locally for editing or archiving; covers cookie and anonymous web_embedded paths and known failure modes."
when-to-use: "Downloading a YouTube video, VOD or trailer locally at full quality, YouTube Studio only offers 720p and you need 1080p, or any workflow that needs source footage locally (clipping, editing, b-roll)."
---

# YouTube 1080p Terminal Download

## What this does (plain English)

This skill shows your AI assistant how to save a YouTube video to your computer at full HD quality, up to 1080p at 60 frames per second, using two free command-line tools: **yt-dlp** (the downloader) and **ffmpeg** (the tool that stitches video and audio together and checks the result).

Why it exists: in the author's experience, the download button inside YouTube Studio tops out at 720p. This recipe pulls up to 1080p60 from the terminal instead, with no account and no cookies needed in the main path. It works by asking YouTube for the video through the `web_embedded` player client, which in testing served full 1920x1080 where some other clients (mweb, android, tv_simply) were limited to 360p when not signed in.

## Responsible use

Only download content you own or have the rights to use, such as your own videos and VODs, content you have a license for, or uses that count as fair use where you live. Respect YouTube's Terms of Service and copyright law. This skill does not get past age restrictions, members-only content, or login walls, and it is not meant to.

Last re-verified 2026-10-26 on yt-dlp 2026.07.04: a bare anonymous `-F` listing on the web_embedded client returned 1920x1080p60 formats (h264 id 299, vp9 303, av1 399).

## When to Use

- "download this YouTube video", pulling a VOD or trailer at full quality
- YouTube Studio only offers 720p and the user needs 1080p
- Any workflow that needs source footage locally (clipping, editing, b-roll)

## Prereqs

You need `yt-dlp` and `ffmpeg` on your PATH. `ffprobe` ships with ffmpeg and is used for verification.

macOS:

```
brew install yt-dlp ffmpeg
```

Linux (Debian/Ubuntu shown; on Fedora use `sudo dnf install ffmpeg`, which may need the RPM Fusion repo enabled):

```
pipx install yt-dlp
sudo apt install ffmpeg
```

Windows (PowerShell or Command Prompt; Scoop `scoop install yt-dlp ffmpeg` or Chocolatey `choco install yt-dlp ffmpeg` also work). Open a NEW terminal window after installing so the tools show up on PATH:

```
winget install yt-dlp.yt-dlp
winget install Gyan.FFmpeg
```

Check it worked: `yt-dlp --version` and `ffprobe -version` should both print a version.

## Worked example (anonymous 1080p, no account)

1. Pick a folder to save into, for example your Videos folder (on Windows: `cd %USERPROFILE%\Videos`):

```
cd ~/Videos
```

2. Download with Recipe 2 (replace the URL placeholder with a video you have the rights to):

```
yt-dlp --extractor-args "youtube:player_client=web_embedded" -f "bestvideo[height<=1080]+bestaudio/best" --merge-output-format mp4 -o "my-video.mp4" "https://www.youtube.com/watch?v=VIDEO_ID"
```

3. Verify the file:

```
ffprobe -v error -select_streams v:0 -show_entries stream=width,height,codec_name,r_frame_rate -of csv=p=0 "my-video.mp4"
```

Example output (illustrative, not a recording of a specific download; codec and frame rate depend on the video):

```
h264,1920,1080,60/1
```

That reads as codec, width, height, frame rate. 1920,1080 means full HD.

## Recipe 1: authenticated pull (best quality, your own machine)

Uses your signed-in browser's cookies. Replace `<output dir>` (for example your `~/Videos` folder) and `<video url>`.

```
yt-dlp --cookies-from-browser chrome \
  -f "bestvideo[height<=1080][ext=mp4]+bestaudio[ext=m4a]/bestvideo[height<=1080]+bestaudio/best" \
  --merge-output-format mp4 \
  -o "<output dir>/%(title)s.%(ext)s" \
  "<video url>"
```

Simplest variant (worked for long stream VODs at 1080p60):

```
yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best" --merge-output-format mp4 \
  -o "<name>.mp4" "<vod url>" --cookies-from-browser chrome
```

Bare yt-dlp with no cookies increasingly gets a 403 on the default client. If that happens, use Recipe 2.

## Recipe 2: anonymous 1080p via the web_embedded client

The 360p limit when not signed in applies to the mweb, android and tv_simply clients. The web_embedded client served full 1920x1080 (often 1080p60) with no cookies:

```
yt-dlp --extractor-args "youtube:player_client=web_embedded" \
  -f "bestvideo[height<=1080]+bestaudio/best" \
  --merge-output-format mp4 \
  -o "<output>" "<video url>"
```

Caveats:

- The best web_embedded video format is often AV1 (av01) or VP9 in webm. Muxing to mp4 works, but for editing, re-encode to h264 (fps and scale to taste; keep setsar=1):

```
ffmpeg -i in.webm -vf "fps=30,scale=1920:1080,setsar=1,format=yuv420p" -c:v libx264 -crf 18 -c:a aac out.mp4
```

- Do NOT combine cookies with web_embedded. That combo has yielded only 360p where the same video pulled anonymously came in at 1080p h264. If a cookie pull fails with "The page needs to be reloaded" or "This video is unavailable", retry anonymously (drop `--cookies-from-browser`) with web_embedded.

## Verify every download

```
ffprobe -v error -select_streams v:0 -show_entries stream=width,height,codec_name,r_frame_rate -of csv=p=0 "<file>"
```

Confirm 1920x1080 (and the expected frame rate) before using the file. Sanity check: 1080p60 h264 runs roughly 3-6 Mbps.

## Known failure modes

1. **403 on the default client**: use Recipe 2, or add Chrome cookies (Recipe 1).
2. **macOS blocks Chrome cookies in cron or headless runs** ("Operation not permitted" on `~/Library/Application Support/Google/Chrome`): the permission can expire and needs a GUI prompt that a scheduled job can't answer. Recipe 2 needs no cookies, so it suits unattended runs.
3. **360p result**: you are likely on the mweb, android or tv_simply client, or you mixed cookies with web_embedded. Switch to anonymous web_embedded.
4. **`ytsearchdateN:` search prefix** can error "Unsupported url scheme" on some versions; use plain `ytsearchN:` and sort results yourself.
5. **Age-restricted or members-only videos** still need cookies; web_embedded does not get past login walls.
6. **YouTube changed something**: update yt-dlp (`brew upgrade yt-dlp`, `pipx upgrade yt-dlp`, or `winget upgrade yt-dlp.yt-dlp`).

## Optional: list formats first

```
yt-dlp --extractor-args "youtube:player_client=web_embedded" -F "<url>"
```

Look for ids in the 299 (h264 1080p60) / 303 (vp9) / 399 (av1) family.
