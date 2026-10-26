# skeptical-gbp-vendor-audit-review (Grok skill)

Review a vendor's Google Business Profile audit before approving changes: nine standing traps, an audit-the-auditor verification pass, and one copy-paste reply that answers everything.

## Install

This is a drop-in skill folder. Copy it into a skills directory.

User scope (all your projects), macOS or Linux:

    mkdir -p ~/.grok/skills
    cp -r skeptical-gbp-vendor-audit-review ~/.grok/skills/

User scope, Windows PowerShell (assumes Grok reads skills from `$HOME\.grok\skills`):

    New-Item -ItemType Directory -Force "$HOME\.grok\skills"
    Copy-Item -Recurse skeptical-gbp-vendor-audit-review "$HOME\.grok\skills\"

Project scope (shared via git):

    mkdir -p .grok/skills
    cp -r skeptical-gbp-vendor-audit-review .grok/skills/

If you downloaded the zip, unzip it first so you have the `skeptical-gbp-vendor-audit-review` folder.

## Requirements

- Nothing to install. It is a workflow skill and works on macOS, Linux and Windows.

## Use

Ask Grok things like:

> review this vendor's GBP audit sheet before I approve anything: <paste sheet and transcript>
> audit the auditor: what did this GBP audit miss?

SKILL.md has a plain-English intro, a worked example, the exact commands and the known pitfalls.

## Responsible use

Never change a tracked phone number or a UTM source or medium without confirming the tracking setup first. Only review audits for profiles you manage or are authorized to manage.

## Suggestions (not part of the skill rules)

These are optional extras from the packager. The rules in SKILL.md are unchanged.

- Counting description characters: you can paste the vendor's suggested description into a quick Python check, for example `python3 -c "import sys; print(len(sys.stdin.read().rstrip('\n')))"` and then paste the text and press Ctrl-D (Ctrl-Z then Enter on Windows). The rule itself stays `<=750` characters.
- If your GA4 channel groupings filter on `utm_source` or `utm_medium`, see the related skill `skeptical-ga4-admin-api-automation` before approving any UTM change. For the website side of local search, see `skeptical-post-launch-site-audit`.

## License

MIT, see LICENSE.
