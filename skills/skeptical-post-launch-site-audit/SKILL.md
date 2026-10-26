---
name: skeptical-post-launch-site-audit
description: "Terminal-based post-launch website audit: crawl, redirects, schema, tracking, staging leaks; severity report. Use when a new or rebuilt website just went live and you need findings to hand to SEO and web dev teams, or for any post-launch check of a live domain."
when-to-use: "A new or rebuilt website just went live and you need findings to hand to SEO and web dev teams, or any post-launch check or technical review of a live domain."
---

# Post-launch site audit (terminal only)

## What this does (plain English)

A new website can look fine in a browser and still have serious problems underneath: the bare domain redirecting to a dev server, the same pageview counted twice, staging links left in the HTML, or broken schema. This skill gives your AI assistant a ten-step audit it runs entirely from the terminal with `curl` and Python: enumerate every URL, parse each saved page, test every redirect variant, check robots and sitemaps, verify the tracking stack, hunt staging leaks, QA the schema, and spellcheck. It ends with a severity-bucketed report, with each finding assigned to the responsible team (web dev or SEO), exact evidence, and a verified-working list. Refined across 15+ production site launches.

## Responsible use

Only audit sites you own or are authorized to test. Crawl politely: throttle your requests and retry once if a CDN challenges rapid fetches, rather than hammering the site. Do not try to evade security challenges.

## Install what you need

You need `curl` and Python 3 (from python.org or your system package manager). The recipe also uses `grep` and `mkdir -p`.

The commands in this skill are written for a Unix-style shell: macOS Terminal, Linux, or on Windows Git Bash or WSL. Windows PowerShell and Command Prompt do not have tools such as `grep`, `tail` and `sort`, and in PowerShell `curl` may be an alias for a different command.

## Worked example

Start an audit of a site you are authorized to test, using the exact commands from the procedure below.

1. Make one scratch directory per audit BEFORE the crawl loop (a failed `curl -o` is silent and prints "200 0" rows that look like empty pages), then save each page from the sitemap index and the nav menu (replace the placeholders):

```
mkdir -p <dir>
curl -sL -A "<real Chrome UA>" -o <dir>/name.html <url>
```

2. Measure from the saved files with `os.path.getsize()`, not from terminal output (terminal capture truncates around 50KB).
3. Test every protocol and host variant of the domain separately, including http://www and the bare domain, WITHOUT `-L`, to see each hop:

```
curl -s -o /dev/null -w '%{http_code} %{redirect_url}'
```

   Append the URL to that command, then follow multi-hop chains to the FINAL landing before grading. Check the bare domain specifically for 302s to a shared dev or staging host.
4. Continue with robots.txt and every sitemap, tracking, staging leakage, schema QA and spellcheck (steps 6 to 10 below), then write the report in severity buckets with a "Verified working" list and a one-line priority order.

---

The full production recipe follows.


## When to Use
- A new or rebuilt website just went live and you need findings to hand to
  SEO and web dev teams
- Any "post-launch check" or technical review of a live domain

## Procedure

1. ENUMERATE URLs from the sitemap index AND the nav menu (they disagree; that
   is itself a finding). Save pages: `curl -sL -A "<real Chrome UA>" -o <dir>/name.html <url>`, one scratch dir per audit. `mkdir -p` the save dir
   BEFORE the crawl loop - a failed curl -o is silent and prints "200 0" rows
   that look like empty pages. When building the URL list from a file plus
   echoed extras, guarantee newline separation (a file written via
   '\n'.join has no trailing newline; the last URL glues to the first echo'd
   one and 404s as garbage). Strip empties.
2. MEASURE FROM FILES, not stdout: `os.path.getsize()` per saved page. Terminal
   capture truncates around 50KB, so byte counts read from command output make
   every page look identical.
3. PER-PAGE PARSE (regex over saved HTML): status, title, meta description,
   canonical, robots meta, H1 count, og:image, schema @types, mixed content
   (http:// assets), duplicate element IDs, img alt coverage. Triage alts
   three ways before flagging: alt="" on icons is acceptable decorative markup;
   a bare <img> with no attributes is a lazy-load placeholder (one per page is
   normal); only content images truly missing alt get flagged.
4. REDIRECT CHAINS: test every protocol/host variant SEPARATELY, including
   http://www - one bad rule can hide behind a healthy https://www chain. Use
   `curl -s -o /dev/null -w '%{http_code} %{redirect_url}'` WITHOUT `-L` to see
   each hop, and follow multi-hop chains to the FINAL landing before grading -
   a 301 to /wp-admin/ that continues to a 404 page for anonymous visitors is
   a 404 finding, not a redirect finding. Check the BARE domain specifically
   for 302s to a shared dev/staging host while the www chain is healthy -
   visitors typing the domain without www land on the dev server (caught this
   CRITICAL on an actual launch day).
5. NAV HREFS: fetch each nav target; flag hub URLs that 301 to the homepage
   (broken nav promise). A parent with href="#" is an intentional non-link.
6. ROBOTS.TXT + EVERY SITEMAP: validate sitemap URLs return 200. If they 404,
   re-fetch with a cache-buster before declaring breakage - CDN edge caches
   transiently serve stale sitemaps that self-heal; the finding then becomes
   "verify purge-on-publish". Extract only <loc> lines (exclude image:loc or
   you'll crawl hundreds of assets); sitemap lines are tab-indented, so strip
   whitespace before ^https anchored greps.
7. TRACKING: pull the live container (`googletagmanager.com/gtm.js?id=...`) and
   grep it. Check GA4 measurement IDs, event names, cross-domain linker
   domains for leftover staging hosts. DOUBLE-FIRE CHECK: an inline
   `gtag("config","G-...")` from a WordPress plugin (Site Kit etc.) PLUS a GTM
   googtag config for the same ID = double-counted pageviews. The inline config
   may use a GT- wrapper: resolve it by fetching
   `googletagmanager.com/gtag/js?id=GT-XXXX` and grepping for the underlying G-
   ID before concluding. On Delay-JS sites, grep ~250 chars around each
   container-ID occurrence for delay markers: a DELAYED GTM snippet loses
   pageviews for bounce visitors; a delayed gtag still double-fires.
8. STAGING LEAKAGE: grep ALL saved pages for staging/dev domains in hrefs,
   srcs, AND data-* attributes - block editors leak staging URLs into
   production HTML (e.g. footer links keeping the staging URL in a data-id
   while the href is correct). Grade by visitor-facing vs inert.
9. SCHEMA QA: duplicate @id with conflicting names; missing LocalBusiness for
   local-service brands; placeholder images in primaryImageOfPage; internal
   author handles in twitter:data; entity name mismatch between templates
   (SEO plugin vs theme output commonly disagree on one @id, splitting the
   brand graph); home declaring Organization while inner pages declare
   ["Person","Organization"] on the same @id; two nodes sharing one @id with
   different @types on the SAME page (collision - merge); placeholder author
   names ("Web") in BlogPosting. For any brand name containing "&", grep the
   raw JSON-LD for &amp; INSIDE string values - entities are not decoded
   inside script blocks, so Google literally reads "A&amp;B".
10. SPELLCHECK: extract visible text (strip script/style then tags), tokenize,
    diff against a dictionary with a brand/city/industry whitelist. Expect
    HUNDREDS of false positives (inflections, curly-apostrophe splits) - never
    dump the raw diff; eyeball odd tokens and context-grep candidates. True
    finds cluster in migrated legacy copy. Entity hits from tag-stripped text
    are FALSE POSITIVES until confirmed in raw HTML: grep for the
    DOUBLE-escaped forms (&amp;hellip;) - only those render broken.

## Report format
- Severity buckets tagged with responsible team: CRITICAL - web dev
  (visitor-facing breakage, tracking corruption), HIGH - SEO (schema, brand
  consistency, sitemap hygiene), MEDIUM - web dev (performance, polish).
- Numbered findings, each with exact URL/evidence and recommended fix.
- End with a "Verified working" list, then a one-line priority order.
- For forwarding: one plain-text block, ALL-CAPS header + date, sections
  CRITICAL / SEO / POLISH, numbered 1-3 line items, VERIFIED GOOD list, DO
  FIRST line. No markdown inside (survives paste into email/Slack).

## Pitfalls
- Cloudflare may challenge rapid successive fetches; retry once before treating
  as blocked.
- Compressed HTML ~170KB/page is normal for modern WP builds; flag raw weight
  (~1.2MB+ from inline utility CSS) as a performance finding, not a fetch error.
- Never finalize from one fetch of a cacheable resource; cache-buster first.
- Verify <title> from RAW HTML (curl+grep), never browser document.title -
  runtime JS/extensions can inject characters not in the served page.
- NEVER audit from a logged-in user's pasted HTML: it contains admin-bar CSS,
  ga-disable opt-out snippets, admin JS. Always re-fetch anonymously.
- Tool output may redact phone-like digit strings (a tel: link with some digits replaced by asterisks); verify
  tel hrefs programmatically (parse digits, assert length in code, print
  booleans only).
- Hidden form-confirmation templates render with empty variables ("we will call
  you back at .") until a real submission; do an actual test fill first.
- Re-test PRIOR-generation URLs at every launch (old ad landing pages,
  singular/plural path variants): a relaunch silently orphans them; a 404'd
  paid-search LP is a CRITICAL-adjacent "verify ad destinations" finding.
