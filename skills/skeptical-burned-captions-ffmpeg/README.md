# skeptical-burned-captions-ffmpeg (Grok skill)

TikTok and Shorts style word-timed captions burned into vertical video with whisper timings, your script text, PIL-rendered PNGs and ffmpeg overlay. No CapCut, no libass.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-burned-captions-ffmpeg ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-burned-captions-ffmpeg "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-burned-captions-ffmpeg .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-burned-captions-ffmpeg` folder.

## Requirements

- `ffmpeg` and `ffprobe` on your PATH (macOS: `brew install ffmpeg`; Linux: `sudo apt install ffmpeg`; Windows: `winget install Gyan.FFmpeg`)
- Python 3 with `faster-whisper` and `Pillow` (`python3 -m pip install faster-whisper Pillow`)

## Use

Ask Grok things like:

> burn word-timed captions into this 9:16 video from the terminal, using my script for the text: <video> <script>

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## License

MIT, see LICENSE.
