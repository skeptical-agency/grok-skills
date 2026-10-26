# skeptical-osm-map-cards (Grok skill)

Location map cards for videos and posts from OpenStreetMap tiles in pure Python: zoom picking, tile mosaic math, and the PIL black-strip bounds guard.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-osm-map-cards ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-osm-map-cards "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-osm-map-cards .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-osm-map-cards` folder.

## Requirements

- Python 3 with `Pillow` (`python3 -m pip install Pillow`) and `curl` (included on macOS, current Windows 10 and 11, and most Linux systems)

## Use

Ask Grok things like:

> make a map card for <place> from OpenStreetMap tiles in Python, 5x5 mosaic, with the black-strip bounds assert

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Respect the Nominatim and OpenStreetMap tile usage policies (about 1 request per second, real user agent, cache tiles). Credit OpenStreetMap contributors on your card or post.

## License

MIT, see LICENSE.
