---
name: skeptical-gbp-vendor-audit-review
description: "Review a vendor's Google Business Profile (GBP) audit before approving changes: spot fake phone mismatches, UTM drift, unstaffed chat and bad descriptions, audit the auditor, and answer everything in one copy-paste message. Use when a vendor or agency sends per-location GBP audit sheets, walkthrough videos or optimization proposals and asks for approvals."
when-to-use: "A vendor or agency sends per-location GBP audit sheets, walkthrough videos or optimization proposals and asks for approvals and answers; you manage Google Business Profiles for a multi-location brand and need to accept, reject or hold vendor changes; or you want to audit the auditor before signing off."
---

# GBP vendor audit review

## What this does (plain English)

A Google Business Profile (GBP) is the free listing that shows a business's name, address, phone number, hours, photos, posts and reviews on Google Search and Maps. If you manage these for a business with several locations, an SEO vendor or agency will often send you audit sheets and videos that say what to change and ask you to approve it. Some of that advice is generic boilerplate, and some of it is wrong, such as a "phone mismatch" that is really just call tracking. This skill gives your AI assistant a way to review a vendor's audit before you approve anything: what to collect first, nine standing traps that vendors routinely get wrong, a verification pass that audits the auditor, and one plain-text message format that answers every vendor question at once, with a list of what is still pending on your side.

It is a workflow skill, so there is nothing to install and it works the same on macOS, Linux and Windows. You paste in the vendor's sheet contents and video transcripts, and your assistant applies the rules below and drafts the reply. A note on terms: NAP means name, address, phone, and a "citation" is a listing of your business on a third-party site such as Yelp or a directory.

## Responsible use

Never change a tracked phone number or a UTM source or medium without confirming the tracking setup first. Check with whoever owns call tracking and your GA4 channel groupings before approving or making any such change, because changing them can break call attribution and historical reporting. Only review audits for profiles you manage or are authorized to manage, and treat the verdicts as recommendations that you and the business owner approve.

## Install what you need

Nothing to install. This is a workflow skill that works on macOS, Linux and Windows: paste in the vendor's sheet contents or transcripts and ask your assistant to review them.

## Worked example

ILLUSTRATIVE ONLY. This example is built from the rules below. It contains no real business, number or data, and the placeholders (`<Location A>`, `<brand>`) stand in for yours.

**The vendor finding:** In the sheet for `<Location A>`, the vendor writes that the phone number on the GBP does not match the phone number on the website, and recommends "clear the cache" and then changing the GBP number to match the website.

1. **Intake first.** Check that the sheet and its transcript are for `<brand>` and `<Location A>` (a sheet ID must differ per brand), and acknowledge each item with one short line. Do not start analyzing until everything is in.
2. **Check the standing trap.** This is Standing trap 1. Ask whether the website uses call tracking with dynamic number insertion. If it does, the number visible on the page is a pool tracking number, not a NAP violation, and "clear the cache" is a wrong diagnosis. GBP numbers may themselves be tracked lines.
3. **Decide.** Do NOT change the GBP number to match the website. Verdict: skip the change.
4. **Find the real cleanup.** The only real cleanup is third-party citations (Yelp, directories) carrying a wrong number. Those need the canonical main line, which only the business owner can supply. Put it on the "still pending on our side" list.
5. **Write it in the output format.** The verdict goes into the CROSS-CUTTING section (phone/tracking), citing which profiles it applies to, and into the per-location bullets:

```
CROSS-CUTTING
1. Phone/tracking (<Location A>): skip. The website number is a call tracking number, so this is not a mismatch. We are not changing the GBP number.

<Location A>
- Phone change: skip. Intentional tracking number on the site, GBP number stays.

STILL PENDING ON OUR SIDE
- Canonical main phone number for <brand>, needed to clean up third-party citations.
```

6. **Keep it open.** Re-list the canonical phone number in every response until the owner answers. Never assume silence means resolved.

---

The full production recipe follows.


## When to Use

- A vendor or agency sends per-location GBP audit sheets, walkthrough videos, or optimization proposals and asks for approvals and answers
- You manage Google Business Profiles for a multi-location brand and need to accept, reject, or hold vendor changes without getting played by generic audit boilerplate
- You want to audit the auditor: catch what the vendor missed before signing off

Deliverable: ONE copy-paste message (Slack/email) with cross-cutting answers first, then per-location verdicts, then a "pending on our side" list.

## Intake

- Vendors often paste truncated share links in email. Insist on full sheet URLs and video transcripts (or the sheet contents pasted directly). Acknowledge each item with one short line as it arrives; do not start analyzing until everything is in.
- WATCH FOR MIS-PASTES: verify each sheet's brand/location matches its transcript before accepting. A sheet ID must differ per brand; the same sheet URL appearing under two brands means someone pasted the wrong content. Send it back.
- Count LOCATIONS, not brands: one audit sheet may cover two or more locations of the same brand.

## Standing traps (what vendors routinely get wrong)

1. Phone "mismatch" between GBP and website is often INTENTIONAL. If the website uses call tracking with dynamic number insertion, the number visible on the page is a pool tracking number, not a NAP violation, and "clear the cache" is a wrong diagnosis. GBP numbers may themselves be tracked lines. Do NOT change the GBP number to match the website. The only real cleanup is third-party citations (Yelp, directories) carrying a wrong number; those need the canonical main line, which only the business owner can supply.
2. UTM consistency: define ONE scheme and enforce it, e.g. `utm_source=google&utm_medium=organic&utm_campaign=gbp-<location>`. Vendors drift between their own sheets (mixing `utm_source=GMB` and `utm_source=google` within one batch). Changing a utm_campaign value is low-risk; never casually change utm_source/medium if your GA4 channel groupings filter on them, or historical grouping breaks.
3. Booking/appointment links: for emergency-service businesses (restoration, plumbing, HVAC, towing), "appointment" = the contact page with UTMs. Approve; ask for the exact target URL they will use.
4. Chat/messaging: HOLD unless someone owns monitoring it. An unanswered chat is worse than no chat. Vendor enthusiasm for "enable chat" ignores who staffs it.
5. Descriptions: verify the suggested text is <=750 characters with a quick programmatic `len()` check; do not trust the vendor's "follows character limit" claim. No phone numbers or URLs allowed in GBP descriptions. Front-load services + city in the first ~250 characters, which is the "More" cutoff in search results.
6. Opening date: Google wants the date THAT LOCATION opened, not the brand founding date. Multi-location brands routinely have the founding year stamped on every profile; flag it and get the real per-location dates from the owner.
7. Category adds: approve only when the business genuinely performs the service AND the location landing page supports it. If the page lacks it, keep the category and fix the page (website work, not GBP work). Reject adjacent-trade categories that dilute relevance (e.g. "commercial cleaning" on a restoration profile; "building restoration service" already covers it).
8. Review responses, review frequency, GBP post cadence and content variety: core vendor scope, approve. Post rules worth enforcing: no repetitive posts, never incentivize reviews, no phone/email/social handles in post text, videos <=30 seconds, real unfiltered images (JPG/PNG, 10KB-5MB, at least 720x720), archive posts after ~6 months, links only to the business's own or related sites via the post button.
9. Dead or duplicate social links: replace or remove. Low-stakes approve. On platforms with near-zero business value, removal is fine.

## Audit the audit (verification pass)

- Re-check every "Good" status for what the vendor MISSED. Real catches from production rounds: a misspelled Facebook slug marked "Good", a stale website URL that only worked via redirect, a service-area city in the WRONG STATE (same city name, 150 miles away) sending relevance signals to the wrong market.
- Cross-check every vendor UTM suggestion against your standard; they drift between sheets in the same batch.
- Verify description character counts programmatically.
- Confirm suggested service-area additions are genuinely served: the business's own location page listing the city is sufficient evidence.
- Duplicate photos, cropped logos (horizontal wordmarks in the round avatar), identical posts published three times: all legitimate findings, approve the fixes and require one consistent square logo mark across all locations.

## Output format

One plain-text, chat-ready message in this order:

1. Intro line + overall grade on the audit round.
2. CROSS-CUTTING section: numbered answers that apply to multiple profiles, citing which ones (phone/tracking, UTMs, booking links, chat, descriptions, review and post policies).
3. PER LOCATION sections: short bullets, each one approved / skip / hold with a one-line reason.
4. STILL PENDING ON OUR SIDE: facts only the business owner can answer (canonical phone numbers for citations, true per-location opening dates, accessibility attribute verification, chat monitoring ownership).
5. Closing: what is approved to proceed + one genuine compliment. Keeps the vendor relationship warm and costs nothing.

Tone: direct and plain. Answer EVERY question the vendor wrote into the sheets; skipping one guarantees a follow-up round.

## Pending tracker

Items only the client/owner can answer recur until closed: canonical main phone per brand, true location opening dates, attribute verification, chat ownership. Re-list them in every response until answered; never assume silence means resolved.
