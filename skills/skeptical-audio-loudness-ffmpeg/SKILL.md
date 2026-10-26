---
name: skeptical-audio-loudness-ffmpeg
description: "Hit streaming loudness targets (-14 LUFS) with ffmpeg loudnorm, ebur128 measurement, and music ducking. Use when a video or podcast sounds quiet or hot next to platform content, or when a music bed fights the voiceover."
when-to-use: "A video or podcast sounds quiet or hot next to platform content, you need YouTube/Spotify-standard loudness from the terminal, or a music bed fights the voiceover."
---

# Audio loudness for streaming (ffmpeg)

## What this does (plain English)

Platforms play your video at a standard loudness. If you upload louder than that, they turn you down; if you upload quieter, they do not turn you up, so you just lose. This skill gives your AI assistant the exact ffmpeg commands to measure your audio, normalize it to the common streaming target (-14 LUFS integrated, true peak at or below -2.0 dBTP) with a two-pass loudnorm, and sit a music bed under a voiceover with sidechain ducking. It also records the equivalent settings for a DAW chain and the pitfalls that silently ruin audio, like mixed sample rates dropping seconds of sound.

## Install what you need

You need `ffmpeg`.

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

The commands in this skill are written for a Unix-style shell: macOS Terminal, Linux, or on Windows Git Bash or WSL. Windows PowerShell and Command Prompt do not have tools such as `grep`, `tail` and `sort`, and in PowerShell `curl` may be an alias for a different command.

## Worked example

Normalize `in.wav` to the streaming target. The commands are exactly the ones from the recipe below; fill `<mi>`, `<mtp>`, `<mlra>`, `<mt>` and `<target_offset>` from pass 1's JSON output.

1. Measure first and read the I (integrated) and LRA values:

```
ffmpeg -i in.wav -af ebur128=peak=1 -f null - 2>&1 | tail -12
```

2. Pass 1 of loudnorm, printing the measured values as JSON:

```
ffmpeg -i in.wav -af loudnorm=I=-14:TP=-2:LRA=11:print_format=json -f null -
```

3. Pass 2, using the measured values:

```
ffmpeg -i in.wav -af loudnorm=I=-14:TP=-2:LRA=11:measured_I=<mi>:measured_TP=<mtp>:measured_LRA=<mlra>:measured_thresh=<mt>:offset=<target_offset>:linear=true -ar 48000 out.wav
```

4. Measure `out.wav` again with the step 1 command. Integrated loudness should land about -14 to -13.

---

The full production recipe follows.


## When to Use
- A video/podcast sounds quiet or hot next to platform content
- You need YouTube/Spotify-standard loudness from the terminal
- Music bed fights the voiceover

## Targets
- Integrated loudness: -14 LUFS (ITU-R BS.1770-4), true peak <= -2.0 dBTP.
  This is the YouTube/most-platform standard; louder uploads get turned DOWN,
  quieter ones don't get turned up - you just lose.
- Music-only beds under VO: very low, around -36 LUFS effective.
- Speech segments used as source audio inside another edit: -17 LUFS / -3 dBTP
  is a good normalization point.

## Measure first

```
ffmpeg -i in.wav -af ebur128=peak=1 -f null - 2>&1 | tail -12
```
Read the I (integrated) and LRA values. Or one-pass loudnorm with
print_format=json to see measured values for a second pass.

## Normalize (two-pass for accuracy)
Pass 1:

```
ffmpeg -i in.wav -af loudnorm=I=-14:TP=-2:LRA=11:print_format=json -f null -
```

Pass 2 (use measured_ values): 

```
ffmpeg -i in.wav -af loudnorm=I=-14:TP=-2:LRA=11:measured_I=<mi>:measured_TP=<mtp>:measured_LRA=<mlra>:measured_thresh=<mt>:offset=<target_offset>:linear=true -ar 48000 out.wav
```

One-pass `loudnorm=I=-14:TP=-2:LRA=11` is acceptable for speech when speed
matters; two-pass is measurably tighter.

## Music bed under voiceover
1. Drop the bed hard: loudnorm I=-18 on the track, THEN volume=-18dB. Result
   sits around -36 LUFS, present but never fighting the voice.
2. Duck it with the VO as sidechain key:

   ```
   [music][vo]sidechaincompress=threshold=0.06:ratio=6:attack=20:release=300[ducked]
   ```
3. Fade out 1.5s at the tail; never cut the music abruptly under the last word.
4. Mix with `amix ... :normalize=0` - amix's DEFAULT normalization quietly kills
   your carefully set voice level.
5. After the last spoken word, hold >= 1.0s of video with music fading over it
   (ending pad) so the final word is never truncated by platform trims.

## DAW chain equivalent (Fairlight/any DAW), dialed on a premixed stream track
- Gate: OFF (a premixed track has music/SFX under the voice; gating chops it)
- Compressor: Threshold -15, Ratio 2:1, Knee ~15, Attack ~35ms, Release ~200ms,
  Mix 100
- Limiter: Threshold -1.0 (not -21)
- Normalize: ITU-R BS.1770-4, Target -14 LKFS, -2.0 dBTP
- Integrated should land about -14 to -13.

## Pitfalls
- Normalize EVERY element to the same sample rate and channel layout (48k mono
  or 48k stereo) before concat/amix; mixed 44.1k stereo + 48k mono through the
  concat demuxer silently DROPPED 2.8s of audio in production.
- atempo for speed changes (preserves pitch); NEVER asetrate (chipmunk).
- Loudness changes only affect future renders; if a published video is off,
  re-render, don't chase it in platform settings.
