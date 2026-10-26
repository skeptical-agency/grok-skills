# skeptical-no-stutter-video-motion (Grok skill)

Why ffmpeg zooms stutter (whole-pixel rounding), the static-stills rule for text cards, and linting the build script instead of pixel-diffing.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-no-stutter-video-motion ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-no-stutter-video-motion "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-no-stutter-video-motion .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-no-stutter-video-motion` folder.

## Requirements

- `ffmpeg` and `ffprobe` on your PATH (macOS: `brew install ffmpeg`; Linux: `sudo apt install ffmpeg`; Windows: `winget install Gyan.FFmpeg`)

## Use

Ask Grok things like:

> my Ken Burns zoom on stills looks steppy in ffmpeg, fix it
> lint this ffmpeg build script for motion that will stutter: <script>

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## License

MIT, see LICENSE.
