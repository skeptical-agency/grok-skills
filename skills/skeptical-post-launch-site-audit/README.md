# skeptical-post-launch-site-audit (Grok skill)

A ten-step terminal audit for a newly launched website: crawl, redirects, robots and sitemaps, tracking, staging leaks, schema, spellcheck, with a severity-bucketed report. Refined across 15+ production site launches.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-post-launch-site-audit ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-post-launch-site-audit "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-post-launch-site-audit .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-post-launch-site-audit` folder.

## Requirements

- `curl` and Python 3 (macOS and Linux have `curl`; Windows 10 and 11 include `curl.exe`)
- A Unix-style shell for the `grep` and `mkdir -p` steps (macOS, Linux, or Git Bash/WSL on Windows)

## Use

Ask Grok things like:

> run a post-launch audit on <domain> and give me a severity-bucketed report

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Only audit sites you own or are authorized to test. Crawl politely and do not try to evade security challenges.

## License

MIT, see LICENSE.
