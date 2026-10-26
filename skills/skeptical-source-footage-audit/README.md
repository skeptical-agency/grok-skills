# skeptical-source-footage-audit (Grok skill)

A QA procedure for source footage: contact sheets, full-res fine pass, brightness check, privacy and policy audit, still images, and an exclusion log.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-source-footage-audit ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-source-footage-audit "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-source-footage-audit .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-source-footage-audit` folder.

## Requirements

- `ffmpeg` on your PATH (macOS: `brew install ffmpeg`; Linux: `sudo apt install ffmpeg`; Windows: `winget install Gyan.FFmpeg`)
- A Unix-style shell for the brightness command that uses `grep` (macOS, Linux, or Git Bash/WSL on Windows)

## Use

Ask Grok things like:

> audit this footage for watermarks, badges and privacy leaks before I use it: <file>

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

This audit checks what is visible in a clip. It does not clear your rights to use it. Only use footage you own or are entitled to use.

## License

MIT, see LICENSE.
