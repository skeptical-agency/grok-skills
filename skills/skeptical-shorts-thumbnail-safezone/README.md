# skeptical-shorts-thumbnail-safezone (Grok skill)

Why text on 9:16 Shorts disappears in YouTube grids, the y = 400..1500 safe-zone rule on a 1080x1920 canvas, and a pre-publish crop check.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-shorts-thumbnail-safezone ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-shorts-thumbnail-safezone "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-shorts-thumbnail-safezone .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-shorts-thumbnail-safezone` folder.

## Requirements

- Nothing to install. The rule applies to whatever tool you use to render your video text.

## Use

Ask Grok things like:

> check my Short's hook text against the thumbnail safe zone
> my Short's thumbnail is blank in the grid but the text looks fine in the video, why?

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## License

MIT, see LICENSE.
