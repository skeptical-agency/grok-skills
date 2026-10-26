---
name: skeptical-ffmpeg-concat-traps
description: "Fix silent ffmpeg concat failures: vanishing overlays, SAR mismatches, dropped audio, short stills. Use when captions or overlays vanish after a segment boundary, the concat filter fails with \"Failed to configure output pad\", or the final video is shorter than planned."
when-to-use: "Captions or overlays render on early segments then vanish for the rest, the concat filter fails with \"Failed to configure output pad\", the final video is mysteriously shorter than planned or audio drops chunks, or any pipeline concatenates mixed sources (stills, video, generated clips)."
---

# ffmpeg concat traps (the silent killers)

## What this does (plain English)

When you stitch video segments together with ffmpeg, some of the worst bugs make no error at all. Captions can disappear after the first segment boundary, audio can silently lose seconds, and a still image can turn into a single frame so your video comes out short. This skill gives your AI assistant six traps hit in production, each with its symptom, its fix and a way to verify, plus a general habit for checking every concat build. It covers color metadata mismatches, odd SAR values, mixed audio layouts, stills without `-loop 1`, input ordering, and zoompan on looped input.

## Install what you need

You need `ffmpeg` and `ffprobe`.

macOS:

```
brew install ffmpeg
```

Linux (Debian/Ubuntu shown; on Fedora use `sudo dnf install ffmpeg`, which may need the RPM Fusion repo enabled):

```
sudo apt install ffmpeg
```

Windows (PowerShell or Command Prompt; open a NEW terminal window after installing so the tools show up on PATH):

```
winget install Gyan.FFmpeg
```

Check it worked: `ffmpeg -version` and `ffprobe -version` should both print a version.

## Worked example

Your final video comes out many seconds short and nothing errored. This is Trap 4, using only commands from the recipe below.

1. Make sure every still is read with a loop, not as a single frame: `-loop 1 -i card.png -t <dur>`.
2. After the final mux, assert the VIDEO STREAM duration (not the format duration, which can still read the audio length and hide the problem). It should equal the planned total, plus or minus 1s:

```
ffprobe -v error -select_streams v -show_entries stream=duration -of csv=p=0 out.mp4
```

3. To stop cumulative drift, render stills with `-frames:v round(dur*FPS)` and plan cuts as integer frames.
4. If overlays also vanish after a segment boundary, do not `-c copy` the concat. Re-encode with the full normalization from Trap 1:

```
-vf "fps=30,setsar=1,setparams=range=tv:color_primaries=bt709:color_trc=bt709:colorspace=bt709,format=yuv420p"
```

---

The full production recipe follows.


## When to Use
- Captions/overlays render on early segments then VANISH for the rest
- Concat filter fails with "Failed to configure output pad"
- Final video is mysteriously shorter than planned, or audio drops chunks
- Any pipeline concatenating mixed sources (stills + video + generated clips)

## Trap 1: overlays silently vanish after a concat boundary (-c copy)
concat DEMUXER with -c copy glues segments carrying DIFFERENT color metadata
(PIL/image segments = unspecified; camera/trailer segments inherit
bt709/tv-range from source). The stream header takes the FIRST segment's
properties; overlay filters can't reconfigure when properties change
mid-stream and silently skip blending for EVERY frame after the first change.
Symptom: pixel-diff between clean and overlaid renders is clean early, pure
noise-free (overlay never applied) after the first foreign segment.
FIX: never -c copy a concat that will receive overlays. Re-encode with full
normalization:

```
-vf "fps=30,setsar=1,setparams=range=tv:color_primaries=bt709:color_trc=bt709:colorspace=bt709,format=yuv420p"
```
VERIFY: diff pre-overlay vs post-overlay in the overlay band at 10+ timestamps
spanning EVERY segment type; max pixel diff > ~60 = rendered. Vision alone
misses this (dark frames look plausible either way).

## Trap 2: SAR mismatch breaks the concat FILTER
Scaling with scale=W:-2 can emit absurd SAR values (e.g. 10240:10239) that
poison downstream concat: "Failed to configure output pad". FIX: normalize
EVERY input inside the filter graph before concat:

```
[i:v]fps=30,setsar=1,format=yuv420p[vi]; [i:a]aresample=async=1[ai]
```
then concat, label outputs, `-map "[v]" -map "[a]"`. Also add setsar=1
immediately after any scale=W:-2 so bad SAR never escapes.

## Trap 3: mixed audio layouts drop chunks
Concat demuxer with mixed 44.1k stereo + 48k mono silently dropped 2.8s of
audio in production. FIX: aresample=async=1 per input, or pre-normalize every
element to identical rate/channels (e.g. 48k mono) before concat.

## Trap 4: stills emit ONE frame (silent short video)
A still image input WITHOUT -loop 1 emits exactly one frame regardless of -t;
the concat comes out many seconds short and NOTHING errors. Worse: ffprobe
FORMAT duration still reads the audio length, hiding it. FIX: `-loop 1 -i card.png -t <dur>`, and after final mux assert:

```
ffprobe -v error -select_streams v -show_entries stream=duration -of csv=p=0 out.mp4
```
equals planned total (+/-1s). Check the VIDEO STREAM duration, never format.
ALSO: -loop 1 -t <dur> rounds UP ~1 frame per segment and the drift CUMULATES
(15 segments -> 0.15s late cut points). Render stills with
`-frames:v round(dur*FPS)` instead and plan cuts as integer frames.

## Trap 5: input ordering
ALL -i flags must come BEFORE -filter_complex/-map. Appending -i after map
options errors with "cannot be applied to input url".

## Trap 6: zoompan on looped input
zoompan applied to a -loop image input multiplies frames. Feed a SINGLE still
(no -loop) and let zoompan d= generate the frame count (output length = d/fps).

## General verification habit
After any concat build: (1) video-stream duration assert, (2) audio-stream
duration assert, (3) overlay-band pixel diff at timestamps spanning every
segment type, (4) ffprobe the codec/color metadata of the FINAL file - it
should read uniformly bt709/tv.
