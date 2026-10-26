---
name: skeptical-gtm-form-lead-tracking
description: "Fix GTM form tags firing on pagination clicks; confirmation-event listener + enhanced conversions wiring. Use when a Google Tag Manager Form Submission trigger fires on every Next or Previous click of a multi-page form, lead counts in GA4 or Ads are inflated, or you are wiring enhanced conversions for a form."
when-to-use: "A GTM Form Submission trigger fires on every Next/Previous click of a multi-page form (Gravity Forms dispatches a real browser submit event on every page transition), lead counts in GA4 or Ads are inflated, or you are wiring enhanced conversions (User Provided Data) for a form."
---

# GTM form lead tracking (no double-counting)

## What this does (plain English)

If your Google Tag Manager form tag counts a lead every time someone clicks Next on a multi-page form, your lead numbers are wrong. Gravity Forms fires a real browser submit event on every page transition, not only on the final Submit, and GTM's built-in Form Submission trigger counts all of them. This skill gives your AI assistant the permanent fix: stop listening for the submit, and fire on the form's confirmation event instead, so exactly one event goes out per successful final submit. It also covers how to diagnose which GTM variable is clean, variants for non-AJAX forms, redirect (thank-you page) confirmations and other form plugins, how to wire enhanced conversions (User Provided Data) for Google Ads, GA4 and Microsoft UET without leaking PII, how to audit a published GTM container with just `curl`, and a verification checklist.

## Responsible use

Enhanced conversions use personal data (email and phone). Make sure your privacy policy, consent setup and the ad platforms' policies allow it, and never put raw email or phone values in GA4 event parameters. Only change tags and containers you own or are authorized to edit, and test in GTM Preview before publishing.

## Install what you need

Nothing to install. You work in Google Tag Manager (Preview mode and a Custom HTML tag). The container audit uses `curl` and `grep` (see the audit commands below).

The commands in this skill are written for a Unix-style shell: macOS Terminal, Linux, or on Windows Git Bash or WSL. Windows PowerShell and Command Prompt do not have tools such as `grep`, `tail` and `sort`, and in PowerShell `curl` may be an alias for a different command.

## Worked example

Audit a published GTM container with just `curl`, using the exact commands from the recipe (replace `GTM-XXXX` with the container ID):

```
curl -s "https://www.googletagmanager.com/gtm.js?id=GTM-XXXX" -o /tmp/gtm.js
grep -c gform_confirmation_loaded /tmp/gtm.js
grep -oE 'G-[A-Z0-9]{8,12}' /tmp/gtm.js | sort -u
```

Then walk the fix:

1. Diagnose first: in GTM Preview, walk to the form, click Next, open the `gtm.formSubmit` event, Variables tab, and read Click Classes, Click Element, Click ID and Click Text. Click Element is usually clean.
2. Add the Custom HTML listener tag (the `<script>` in "The permanent fix" below) on the All Pages trigger. It binds with jQuery and polls until jQuery loads.
3. Create a Custom Event trigger with event name `form_submit_final` and point the GA4 event tag at it. Map `form_id` and `submission_status` to same-name Data Layer Variables, and delete the old Form Submission trigger.
4. Verify with the checklist at the end: Next fires nothing, Previous fires nothing, the final Submit fires exactly one `form_submit_final` and one GA4 tag fire, a second submit in the same session does not fire, and a second form fires on its own.

---

The full production recipe follows.


## When to Use
- A GTM "Form Submission" trigger fires on every Next/Previous click of a
  multi-page form (Gravity Forms dispatches a REAL browser submit event on
  every page transition, not just the final Submit)
- Lead counts in GA4/Ads are inflated
- Wiring enhanced conversions (User Provided Data) for a form

## Diagnose first (don't guess which variable is clean)
1. GTM Preview -> walk to the form -> click Next.
2. Open the gtm.formSubmit event -> Variables tab.
3. Read Click Classes, Click Element, Click ID, Click Text.
4. Click Classes is often polluted by form-wrapper plugins (shows the wrapper,
   not the button). Click Element is usually clean and ends with the real
   input (e.g. ...input.gform_next_button...#gform_next_button_1_27).
If you must use click exclusions, key on Click Element with "does not contain
gform_next_button" / "does not contain gform_previous_button". NEVER "does not
equal" - Click Element is the full DOM path, equality never matches.

## The permanent fix: fire on CONFIRMATION, not submit
AJAX forms (check page source for gform_ajax_frame_<id>) fire
gform_confirmation_loaded only after a successful FINAL submit.

1. Custom HTML tag, trigger All Pages. Bind with jQuery - GF dispatches the
   event through jQuery; a native addEventListener will NOT hear it. jQuery may
   load AFTER GTM fires (async race, nondeterministic per pageview), so POLL:

```html
<script>
(function(){
  if (window.__gfLeadSent) return;
  window.__gfLeadSent = true;
  var tries = 0;
  function bindGF(){
    var $ = window.jQuery;
    if (!$ || !$.fn) { if (++tries < 100) setTimeout(bindGF, 100); return; }
    $(document).on('gform_confirmation_loaded', function(e){
      var formId = (e && e.formId) || '';
      var key = 'gf_lead_' + formId;
      try {
        if (sessionStorage.getItem(key)) return;   // once per session per form
        sessionStorage.setItem(key, '1');
      } catch(err) {}
      window.dataLayer = window.dataLayer || [];
      window.dataLayer.push({
        event: 'form_submit_final',
        form_id: String(formId),
        submission_status: 'success'
      });
    });
  }
  bindGF();
})();
</script>
```

2. Custom Event trigger, event name = form_submit_final.
3. Point the GA4 event tag at that trigger; map form_id and submission_status
   to same-name Data Layer Variables. REPLACE the tag's old parameters: built-ins
   like {{Form ID}}/{{Form Name}} only populate on the native gtm.formSubmit
   auto-event and resolve BLANK under a custom-event trigger.
4. DELETE the old Form Submission trigger, and audit the GA4 tag's full trigger
   list - template containers often carry a SECOND pipeline (another listener
   pushing its own form_submit) that keeps double-firing.

sessionStorage key includes form_id on purpose: each form gets its own
once-per-session counter.

## Variants
- NON-AJAX forms (full page POST): window globals die on reload. Persist every
  captured value to sessionStorage at capture time; add a load-time DOM check
  pushing form_submit_final when [id^=gform_confirmation_message_] exists
  (same sessionStorage dedupe).
- REDIRECT confirmations (thank-you page): neither the jQuery event nor the DOM
  check fires. Add a small Custom HTML tag on a Page View trigger, Page URL
  contains /thank-you, that pushes form_submit_final with the persisted form id.
  WARNING: if /thank-you/ later 301s to the homepage, the PV trigger can never
  fire - GTM sees the FINAL url.
- Other form plugins: Formidable (success renders .frm-success /
  .frm_ajax_success; watch MutationObserver + capture-phase submit scan) and
  Elementor Forms (jQuery elementor_pro/forms/submit_success +
  .elementor-message-success observer). When a site relaunch is pending, ship a
  DUAL-framework listener - plugin flips between build and launch happen.

## Enhanced conversions (User Provided Data)
- Capture email/phone on typing (input events) AND on gform_pre_submission
  (autofill fires NO input events - the pre_submission DOM scan is mandatory).
- NEVER use DOM Element variables for the values: by confirmation time the AJAX
  form is gone and selectors resolve empty.
- Google Ads: a User-provided Data Event tag (awud) hashing automatically, fed
  by a GTM UPD variable.
- GA4 Event tags have NO UPD section: use a separate Custom HTML tag on the
  same trigger running `gtag('set', {user_data:...})` - NOT `gtag('config')`, which
  sends an extra page_view per submit. Firing priority: UPD set tag = 1, GA4
  event tag = 10 (same-trigger order is not guaranteed).
- Never put raw email/phone in GA4 event params (PII policy).
- Normalize in Custom JS variables: E.164 phone, lowercased email.

## Audit a published container without GTM UI access

```
curl -s "https://www.googletagmanager.com/gtm.js?id=GTM-XXXX" -o /tmp/gtm.js
grep -c gform_confirmation_loaded /tmp/gtm.js
grep -oE 'G-[A-Z0-9]{8,12}' /tmp/gtm.js | sort -u
```
Compiled function names reveal what exists: __html custom HTML, __baut UET,
__googtag Google tag, __gaawe GA4 event, __fsl Form Submission trigger.
NOTE: one compiled Google tag spawns ~8 internal __ccd_*/__ogt_* sub-tags all
carrying the same G-ID; raw ID counts look like duplication but are benign.

## Pitfalls
- Cloudflare Turnstile rate-limits synthetic test submits after ~2 leads: the
  confirmation stalls with no visible error. Treat as blocked, not broken.
- GA4 enhanced-measurement auto form events (en=form_submit) can MASK a dead
  custom pipeline - GA4 shows form data while your GTM pipeline is silent.
  Also double-counts (and fires on Next clicks): turn EM form interaction off
  when a GTM pipeline owns form_submit.
- Google strips DOTS from gmail.com addresses before hashing; local SHA256
  verification of a gmail test address must use the dots-stipped lowercase form.
- Always REPLACE the full listener tag content when editing; a partial edit
  dropping the confirmation handler kills every downstream tag silently.
- Hard-refresh after publishing; exclude internal traffic in GA4.

## Verification checklist
Click Next -> no fire. Click Previous -> no fire. Final Submit -> exactly one
form_submit_final + one GA4 tag fire. Second submit same session -> no fire.
Second form on site -> its own fire.
