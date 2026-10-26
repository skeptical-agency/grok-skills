# skeptical-ga4-admin-api-automation (Grok skill)

Script GA4 custom channel groupings, audiences, annotations and enhanced measurement with the Admin API, including the verified quirks (v1alpha, eachScope* fields, inListFilter UI bug, 2-grouping limit).

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-ga4-admin-api-automation ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-ga4-admin-api-automation "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-ga4-admin-api-automation .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-ga4-admin-api-automation` folder.

## Requirements

- A Google Cloud service account with the GA4 Admin API enabled and Editor on each GA4 account
- Python 3 with `requests` (`python3 -m pip install requests`)

## Use

Ask Grok things like:

> roll out our standard custom channel grouping to this GA4 property via the Admin API
> why does my GA4 inListFilter channel grouping show blank in the UI?

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Only change GA4 accounts and properties you own or are authorized to administer. Test on one property first and warn report consumers before changing event volume.

## License

MIT, see LICENSE.
