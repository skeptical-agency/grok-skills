# skeptical-ffmpeg-concat-traps (Grok skill)

The silent ffmpeg concat bugs: overlays that vanish after a segment boundary, SAR mismatches, dropped audio and one-frame stills, with fixes and checks.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-ffmpeg-concat-traps ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-ffmpeg-concat-traps "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-ffmpeg-concat-traps .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-ffmpeg-concat-traps` folder.

## Requirements

- `ffmpeg` and `ffprobe` on your PATH (macOS: `brew install ffmpeg`; Linux: `sudo apt install ffmpeg`; Windows: `winget install Gyan.FFmpeg`)

## Use

Ask Grok things like:

> my overlays vanish after the first few segments of my ffmpeg concat, why?
> my concatenated video is shorter than planned and ffmpeg shows no error

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## License

MIT, see LICENSE.
