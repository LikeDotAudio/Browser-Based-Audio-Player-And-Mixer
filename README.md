# 🎚 Multichannel Mixer

A self‑contained, browser‑based audio mixer for multitrack stems. Open one HTML
file, point it at a folder of tracks, and mix a band in your browser — faders,
pan, echo and reverb sends, VU meters, solo/mute, and a stereo master. No
install, no server, no internet required.

> Best experienced with headphones.

---

## Quick start

1. **Extract** this folder somewhere on your computer.
2. Open **`mixer.html`** in a web browser (Chrome or Edge recommended).
3. Click **“📁 SELECT FOLDER OF TRACKS”** and choose the folder of audio files.
4. The tracks decode (you’ll see a progress bar), then **playback starts
   automatically**.

The folder you pick is remembered, so next time it can load on its own.

### Browser notes
- **Chrome / Edge** give the smoothest experience (one‑click folder picker that
  is remembered between sessions).
- **Firefox** also works but uses a standard folder‑upload dialog and won’t
  remember the folder.
- Because browsers can’t auto‑read files from disk for security reasons, the
  page always asks you to pick the folder once — there’s no way around that
  without a server, and none is needed here.

---

## Track files & naming

Drop any common audio files into a folder — `.wav`, `.flac`, `.mp3`, `.m4a`,
`.aac`, `.ogg`, `.aiff`, etc. **One file becomes one channel strip.**

The filename can encode the mix using this pattern:

```
NNN[L|R][pct] - Name [E## R## V##].ext
```

| Part            | Meaning                                                                 | Example              |
|-----------------|-------------------------------------------------------------------------|----------------------|
| `NNN` / `NNNN`  | **Group number** (3–4 digits). Same number → same colour **and** linked fader/ON/SOLO. | `009`                |
| `L` / `R`       | **Pan.** Bare `L`/`R` = 100% left / right.                              | `003L`, `003R`       |
| `L20` / `R10`   | **Pan amount.** `L20` = 20% left, `R10` = 10% right.                     | `003R10`             |
| `Name`          | The track label (shown under the strip; editable in the mixer).         | `Lead Vocal`         |
| `[E## R## V##]` | **Effects preset** (optional, any order):                               | `[E20 R10 V-6]`      |
| &nbsp;&nbsp;`E##` | Echo send %                                                           | `E20` → 20% echo     |
| &nbsp;&nbsp;`R##` | Reverb send %                                                        | `R10` → 10% reverb   |
| &nbsp;&nbsp;`V##` | Volume in **dB** (`V0` = 0 dB/unity, `V-20` = −20 dB, up to **+12 dB**) | `V-6` → −6 dB        |

**Examples**

```
000-Kick.wav                         → group 000, centre, defaults
003L - Congo.wav                     → group 003, hard left
003R10 - Bongo.wav                   → group 003, 10% right
009R-Rythm Guitar.wav                → group 009, hard right
030-Lead Vocal [E10 R10] .wav        → echo 10%, reverb 10%
003R10-Bongo [E20 V-6].wav           → 10% right, echo 20%, −6 dB
```

Files are sorted by number, then name. The `[...]` token and pan code are
stripped from the displayed label.

---

## The console

Each **channel strip** (top → bottom):

| Control        | What it does                                                              |
|----------------|---------------------------------------------------------------------------|
| **Waveform**   | Scrolling preview of the track; the playhead is at the bottom, by the fader. |
| **ECHO** knob  | Send level into the echo (delay) effect.                                  |
| **REVERB** knob| Send level into the reverb effect.                                        |
| **PAN** knob   | Left / right placement in the stereo image.                               |
| **Fader**      | Track level on a **dB scale** (silence → 0 dB ≈ ¾ up → **+12 dB** top), with a VU meter beside it. |
| **ON**         | Mute / active (green = playing, grey = muted).                            |
| **SOLO**       | Isolates this track; silences everything not soloed.                      |
| **Name**       | The track label — click to rename.                                        |

**Grouping:** tracks that share a leading number move together on **fader, ON,
and SOLO**, and share a colour. Pan and the two sends stay independent per track.

> **Tip — break a group temporarily:** hold **Alt** while dragging a grouped
> fader to adjust just that one. The next normal (non‑Alt) move re‑links the group.

### Effects returns & master

- **Echo Return** — stereo level + VU, plus **TAP** tempo (tap in time to set the
  echo speed) and note subdivisions **¼ · ⅛. · ⅛ · ⅛T · 1/16**.
- **Reverb Return** — stereo level + VU, plus **SHORT / MED / LONG** reverb size.
- **Master** — stereo (L/R) VU meters and the main output fader.
- **Rude SOLO light** (on the master) — blinks whenever any channel is soloed;
  click it to clear all solos at once.

### VU meters
Green up to about ¾, then yellow, then red near the top. If the **master meter
sits solid red**, pull the master (or boosted channels) down — that’s clipping.

### Transport
**▶ Play / ⏸ Pause · ⏮ Rewind · « 10s · [clock] · 10s »** — the 10‑second
buttons jump all tracks together. The clock shows position / total.

### Touch
Faders, knobs, and buttons all respond to touch on a touchscreen.

---

## Controls cheat‑sheet

| Action                         | How                                  |
|--------------------------------|--------------------------------------|
| Change a knob                  | Drag up/down, or scroll over it       |
| Reset a knob to default        | **Alt‑click** the knob                |
| Move a fader                   | Drag it                               |
| Unlink one grouped fader       | **Alt‑drag** the fader                |
| Clear all solos                | Click the **rude SOLO** light (master)|
| Skip ±10 seconds               | **« 10s** / **10s »**                 |

---

## Bonus tool: `wav_to_mp3.py`

A small Python helper (needs [FFmpeg](https://ffmpeg.org/) on your PATH) to make
lightweight MP3s of your stems:

```bash
# Convert a folder of WAVs (creates a sibling "TracksMP3" folder)
python wav_to_mp3.py "MyTracksWAV"

# Convert specific files into a destination folder
python wav_to_mp3.py "Audio 01.wav" "Bass.wav" -o "MyTracks"

# Options: -b 320 (CBR kbps) · -q 0..9 (VBR quality) · -r (recurse) · -y (overwrite)
```

---

*Created by Anthony Kuzub — for educational and experimental purposes.*
