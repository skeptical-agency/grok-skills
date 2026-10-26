# skeptical-audio-loudness-ffmpeg (Grok skill)

Hit -14 LUFS / -2.0 dBTP with ffmpeg loudnorm (two-pass), measure with ebur128, and duck music under voiceover with sidechaincompress.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-audio-loudness-ffmpeg ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-audio-loudness-ffmpeg "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-audio-loudness-ffmpeg .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-audio-loudness-ffmpeg` folder.

## Requirements

- `ffmpeg` on your PATH (macOS: `brew install ffmpeg`; Linux: `sudo apt install ffmpeg`; Windows: `winget install Gyan.FFmpeg`)
- A Unix-style shell for the commands that use `tail` (macOS, Linux, or Git Bash/WSL on Windows)

## Use

Ask Grok things like:

> normalize this audio to -14 LUFS and -2 dBTP with ffmpeg: <file>
> duck the music under my voiceover with ffmpeg

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## License

MIT, see LICENSE.
