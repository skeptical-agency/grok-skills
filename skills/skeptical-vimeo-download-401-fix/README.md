# skeptical-vimeo-download-401-fix (Grok skill)

A working path for downloading a Vimeo video you have the rights to when yt-dlp's Vimeo OAuth step returns a 401: player page, `config_refresh_url`, HLS playlist, ffmpeg.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-vimeo-download-401-fix ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-vimeo-download-401-fix "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-vimeo-download-401-fix .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-vimeo-download-401-fix` folder.

## Requirements

- `curl`, `ffmpeg` and `ffprobe` on your PATH (optionally `yt-dlp` for the try-first step)
- macOS: `brew install yt-dlp ffmpeg`
- Linux: `pipx install yt-dlp` and `sudo apt install ffmpeg` (or `sudo dnf install ffmpeg`)
- Windows: `winget install yt-dlp.yt-dlp` and `winget install Gyan.FFmpeg`, then open a new terminal

## Use

Ask Grok things like:

> yt-dlp fails on this Vimeo video with an OAuth 401, can you pull it another way? <url>

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Only download content you own or have permission to download. Respect Vimeo's Terms of Service and copyright law. Not for getting around paywalls, DRM or access controls.

## License

MIT, see LICENSE.
