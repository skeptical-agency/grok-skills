# youtube-1080p-download (Grok skill)

Download YouTube videos at full 1080p (up to 1080p60) from the terminal using yt-dlp + ffmpeg. The main recipe needs no account and no cookies: it uses the `web_embedded` player client, which works around the 360p limit some clients hit when not signed in.

Last re-verified 2026-10-26 on yt-dlp 2026.07.04.

## Install

This is a drop-in skill folder. Clone this repo, then copy the `skills/youtube-1080p-download` folder into a skills directory.

User scope (all your projects):

    mkdir -p ~/.grok/skills
    cp -r skills/youtube-1080p-download ~/.grok/skills/

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skills/youtube-1080p-download .grok/skills/

If you downloaded the zip, unzip it first so you have the `youtube-1080p-download` folder.

## Requirements

- `yt-dlp` and `ffmpeg` (includes `ffprobe`) on your PATH
- macOS: `brew install yt-dlp ffmpeg`
- Linux: `pipx install yt-dlp` and `sudo apt install ffmpeg` (or `sudo dnf install ffmpeg`)
- Windows: `winget install yt-dlp.yt-dlp` and `winget install Gyan.FFmpeg`, then open a new terminal (Scoop and Chocolatey also work)

## Use

Ask Grok things like:

> download this YouTube video in 1080p: <url>

The skill triggers on requests to download or pull a YouTube video, VOD or trailer locally. SKILL.md has a beginner walkthrough, the exact commands, a verify step and known failure modes.

## Responsible use

Only download content you own or have the rights to use (your own videos, licensed use, fair use where you live). Respect YouTube's Terms of Service and copyright law. This skill does not get past age restrictions, members-only content or login walls.

## License

MIT, see LICENSE.
