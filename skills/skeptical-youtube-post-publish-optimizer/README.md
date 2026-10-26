# skeptical-youtube-post-publish-optimizer (Grok skill)

A nightly workflow that applies tags, category, recording date and standing defaults to just-published YouTube videos, verifies every write by read-back, runs alert-only QA gates, and enforces a 72 hour packaging-change protocol.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-youtube-post-publish-optimizer ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-youtube-post-publish-optimizer "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-youtube-post-publish-optimizer .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-youtube-post-publish-optimizer` folder.

## Requirements

- Python 3
- A Google Cloud project with the YouTube Data API v3 enabled, and a YouTube Data API OAuth client for a channel you own
- Works on macOS, Linux and Windows. The spec names no install command or library, so none is given.

## Use

Ask Grok things like:

> run tonight's post-publish pass for my channel and report what was optimized
> set up the post-publish optimizer for a new channel

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

The automation must never auto-delete, auto-privatize or auto-swap packaging on a video. QA gates alert; humans decide. Only manage channels you own or are authorized to manage, and you are responsible for YouTube API terms and quota use. OpusClip, Repurpose.io and VidIQ are trademarks of their owners; this skill is not affiliated with or endorsed by them.

## Zero approval loop vs never auto-swap packaging

These two ideas fit together because they cover different things:

- **No approval loop (metadata):** tags, category, recording date and standing defaults (license, madeForKids, embeddable) are applied automatically each night, and every write is verified by read-back.
- **Always a human decision (packaging):** titles and thumbnails are never changed by the automation. After 72 hours it may propose a change, and a human approves it. Titles and descriptions are written at schedule time and the optimizer never edits them (outside the optional sponsor-block flow).
- **QA gates only alert:** audio verification and the day 7 retention and CTR review never delete, privatize or change anything.

## One-time setup note

Creating the Google Cloud project, enabling the API and approving the OAuth consent screen are done once by a person in a browser. The automation itself follows the spec's NO BROWSER rule: if it ever seems to need a browser, it reports that and does not click.

## License

MIT, see LICENSE.
