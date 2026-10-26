# skeptical-gtm-form-lead-tracking (Grok skill)

Fix GTM form tags that fire on every Next click: fire on the confirmation event instead, wire enhanced conversions, and audit a published container with curl.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-gtm-form-lead-tracking ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-gtm-form-lead-tracking "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-gtm-form-lead-tracking .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-gtm-form-lead-tracking` folder.

## Requirements

- Access to Google Tag Manager (Preview mode, Custom HTML tags)
- `curl` and `grep` for the container audit (macOS, Linux, or Git Bash/WSL on Windows)

## Use

Ask Grok things like:

> my GTM form tag fires on every Next click of my Gravity Form, fix the lead tracking
> audit this published GTM container with curl: GTM-XXXX

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Enhanced conversions use personal data. Check your privacy policy, consent setup and platform policies, and never put raw email or phone in GA4 event params. Only edit containers you own or are authorized to edit.

## License

MIT, see LICENSE.
