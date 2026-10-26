# skeptical-content-trend-triangulation (Grok skill)

A daily topic-selection procedure: triangulate Google Trends RSS, Techmeme and trade press, never trust one AI search tool's denial, and log runner-ups.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-content-trend-triangulation ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-content-trend-triangulation "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-content-trend-triangulation .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-content-trend-triangulation` folder.

## Requirements

- `curl` (included on macOS, current Windows 10 and 11, and most Linux systems)

## Use

Ask Grok things like:

> pick today's top 2 topics for my tech shorts channel using the triangulation stack and log the runner-ups

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Use public feeds politely and follow each source's terms. Verify stories against the primary source before publishing.

## License

MIT, see LICENSE.
