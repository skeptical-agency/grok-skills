---
name: skeptical-shorts-thumbnail-safezone
description: "Keep Shorts/Reels text inside the platform thumbnail crop zones; simulate the crop before publishing. Use when burning hook or title text onto 9:16 video, or when a Short's thumbnail shows blank background while the in-video text looks fine."
when-to-use: "Burning hook/title text onto a 9:16 vertical video, a Short's thumbnail shows blank background while the in-video text looks fine, or pre-publish QA for Shorts, Reels and TikTok."
---

# Shorts Thumbnail Safe Zone QA

## What this does (plain English)

On YouTube, the thumbnail you see in grids, search results and shelves is not the whole vertical frame. It is a crop of the middle band. Text near the top or bottom of your 9:16 video plays fine while the video runs, but it is invisible in the place where people decide whether to click. This skill gives your AI assistant one hard rule (on a 1080x1920 canvas, keep all hook, title and thumbnail text within y = 400..1500), a way to enforce it in code instead of by eye, and a pre-publish check that simulates the crop.

## Install what you need

Nothing to install for the rule itself. It applies to whatever tool you use to render your video text.

## Worked example

Check a hook card on a 1080x1920 canvas, using only the numbers from the rule below:

1. Put the headline in the vertical middle of the frame. The cross-platform note suggests about y=640 for the hook, and everything thumbnail-relevant must sit inside y = 400..1500.
2. At render time, assert that the bounding box of every thumbnail-relevant text element falls inside y = 400..1500, with margin.
3. Keep burned word-captions in their own band, around y~1540-1690. Never put title text or card text there.
4. Before publishing, take the hook card frame, center-crop it to roughly 16:9 of the middle of the vertical frame, and look at it small: is the headline fully visible, centered and legible?
5. Check at crop scale that the picture matches the headline noun ("$2,599 LAPTOP" must show a laptop, not a box).
6. Render the hook card as a pure still, with no push-in or zoom, so every frame a platform might grab is composed correctly.

---

The full production recipe follows.


## When to Use
- Burning hook/title text onto a 9:16 vertical video
- A Short's thumbnail shows blank background while the in-video text looks fine
- Pre-publish QA for Shorts/Reels/TikTok

## The problem
YouTube grid/search thumbnails crop the 9:16 frame to a MIDDLE BAND. Text near
the top or bottom of the full frame plays fine in-video but is INVISIBLE in
every grid, search result, and shelf - which is exactly where the
click decision happens.

## The rule
On a 1080x1920 canvas, keep ALL hook/title/thumbnail-relevant text within
y = 400..1500. Outside that band, text exists only for the in-video experience.

(Captions are a separate band: burned word-captions live at y~1540-1690 by
convention and platforms expect to crop them; never put TITLE text down there,
and never put card text there either - collision.)

## Enforce in code, not by eyeball
1. At render time: assert every text element's bounding box falls inside the
   safe band (for thumbnails-relevant text) with margin.
2. Pre-publish simulation: extract a representative frame (the hook card is
   usually the thumbnail source), center-crop it to the platform's thumbnail
   aspect (YouTube grid thumbs render roughly 16:9 of the middle of the
   vertical frame), and LOOK at the crop: is the headline fully visible,
   centered, legible at small size?
3. Also verify at crop scale that the subject visual matches the headline noun
   ("$2,599 LAPTOP" must show a laptop, not a box).

## Why hooks should be static
If the hook card has any push-in/zoom motion, the platform's arbitrary
thumbnail-grab frame can land mid-drift, off-center. Render hook cards as pure
stills so EVERY possible grab frame is perfectly composed.

## Cross-platform note
Each platform crops differently (YouTube grid band, Instagram Reels cover
center-square, TikTok profile grid center). Designing inside y=400..1500 of
1920 with the headline in the vertical MIDDLE (~y=640 for the hook) survives
all of them.
