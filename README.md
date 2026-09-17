# syncflow
Free automatic multicam sync for Premiere, Resolve and Final Cut. Point it at a folder of rushes or an editor XML — it aligns every camera and recorder by audio, groups them into takes, orders them by recording date, and returns a synced sequence. Sub-millisecond accuracy, runs entirely offline.


# Syncflow

**Free automatic multicam sync for Premiere, Resolve and Final Cut.**

Point it at a folder of rushes, or an XML from your editor. It listens to the audio in every file, works out how they line up, splits them into takes, orders those takes by when they were shot, and hands back a synced sequence you import straight into your NLE.

No subscription, no account, no upload. Everything happens on your machine.

---

## Install

**Double-click `Install.command`.**

That's it. It takes about five minutes, mostly downloads, and asks for your Mac password once if Homebrew isn't already installed.

If macOS says *"Install.command cannot be opened because it is from an unidentified developer"*: **right-click it and choose Open**, then click Open in the dialog. macOS flags anything downloaded from the internet. You only do this once.

When it finishes you'll have **Syncflow** in your Applications folder like any other app. Open it from Launchpad, Spotlight or Finder.

> The app opens its window in your browser. That's deliberate — it means there's no fragile native toolkit to break when macOS updates. It runs entirely on your own machine and nothing is sent anywhere.

To remove it later, double-click `Uninstall.command`.

---

## Use it

**1. Pick your footage.** *Choose Folder…* to point at a card or a folder of rushes, or *Choose XML…* if you'd rather round-trip through your editor. Either opens the normal macOS file chooser.

**2. Set the options** — or don't. The defaults are good for most shoots.

**3. Press Sync.** Watch the progress bar.

**4. Open the report** before you import anything. It's the fastest way to spot a problem: a coloured strip per camera, a table of takes, and a confidence score for every file. Green is solid, orange is worth a look, red didn't sync.

**5. Import the XML** into your editor.

### Getting it into Premiere Pro

**File → Import**, choose the XML. You get a new sequence with:

- **Each camera on its own permanent track** — V1 is always your A-cam, top to bottom, never swapping halfway down
- **Video and audio linked**, so clips move together
- **A marker on every take**, showing how many sources it holds and when it was recorded
- **Colour labels by sync quality** — green confident, orange uncertain, red unsynced

To cut multicam from there: select a take's clips, right-click → **Create Multi-Camera Source Sequence**, synchronise by **In Points**. Syncflow has already aligned everything, so Premiere doesn't need to re-analyse anything.

If you'd rather export from Premiere first than point at a folder: **File → Export → Final Cut Pro XML**. The warning about unsupported effects is expected — the XML only carries file paths and in/out points.

### DaVinci Resolve and Final Cut

Resolve reads the same XML: **File → Import → Timeline**. For Final Cut, switch **Editor** to *Final Cut Pro* before syncing, and tick **Create multicam clips** if you want native multicam clips rather than stacked lanes.

---

## Options

| Option | When to change it |
|---|---|
| **Take order** | *Chronological* packs takes back to back in recording order. *Real time* spaces them at the actual gaps of the shooting day, which makes a missing take obvious at a glance. |
| **Matching** | Start on *Normal*. Go *Strict* if you get a wrong match — repetitive music and long stretches of room tone are the usual culprits. Go *Loose* if things won't sync at all. |
| **Audio channels** | *Mix* normally. *Channel 1 only* if a recorder has two different mics on left and right, or a stereo pair cancels itself out when mixed. |
| **Gap between takes** | Seconds of space between takes on the output timeline. |
| **Drop camera audio** | Removes camera scratch audio wherever a proper recorder covers the same take. Halves your track count. |
| **Check for clock drift** | Slower. Worth it on takes over ten minutes — recorders drift, and this tells you by how much. |

---

## What it handles

- **Any mix of cameras and recorders**, including audio-only files and cameras with no usable audio
- **Multiple takes in one pass** — files are grouped by what actually shares sound, not by folder
- **Chronological ordering** from creation dates, including the awkward part: cameras disagree about whether `creation_time` means the start or the stop of a recording, and Syncflow works out which per device rather than assuming
- **Different mics, levels, EQ and room positions** — matching runs on onset envelopes, which discard exactly the things that differ between a camera mic across the room and a lav on a recorder
- **Files that shouldn't match** — an unrelated recording is left alone rather than forced into a take

Accuracy is sub-millisecond, verified against footage with known ground truth. The honest limit: Final Cut Pro XML stores whole frames, so sync lands within half a frame, about 20 ms at 25 fps. Every tool in this category has that constraint; the Final Cut export avoids it.

---

## If something goes wrong

Most issues and their fixes are in [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md). The three most common:

**Nothing syncs, every file is its own take.** The clips genuinely don't overlap, the audio is too quiet to match, or the recorder's channels cancel out — try *Channel 1 only*.

**A sync looks wrong.** Open the report and check the pairwise table at the bottom, sorted worst-first. Then raise **Matching** to *Strict*.

**Files show as MISSING.** The XML's paths don't match where the media now lives. Point Syncflow at the footage folder directly instead.

---

## More

- [docs/GETTING-STARTED.md](docs/GETTING-STARTED.md) — the slow walkthrough, if you'd like one
- [docs/TROUBLESHOOTING.md](docs/TROUBLESHOOTING.md) — errors and fixes
- [docs/HOW-IT-WORKS.md](docs/HOW-IT-WORKS.md) — the engineering, for the curious
- [docs/COMMAND-LINE.md](docs/COMMAND-LINE.md) — batch and scripted use
- [docs/BUILD-STANDALONE-APP.md](docs/BUILD-STANDALONE-APP.md) — building a self-contained `.app` and DMG

Requires macOS 11 or later. Linux and Windows work from the command line.

---

## Support this

Syncflow is free, and free to pass on to anyone who needs it.

It was built by **Dan Charlton**, a motion designer in London. If it saved you an afternoon of nudging waveforms, the best thing you can do is take a look at his work — or keep him in mind next time you need motion design.

### **[dancharlton.net](https://dancharlton.net)**

Sharing it with another editor who'd find it useful counts too.

---

MIT licensed — see [LICENSE](LICENSE). Uses [ffmpeg](https://ffmpeg.org), which carries its own licence.
