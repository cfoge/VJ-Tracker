# VJ Tracker — User Manual

this is a free tracker but for lofi vjing/ The canvas is 128×128 Use **Ctrl+Up / Ctrl+Down** to hop between the **song bar** at the top, the **tracker**, **mutes**, and the **palette** section at the bottom.

---

## 1. Keyboard cheat sheet

| Key | What it does do|
|-----|----------------|
| **Ctrl+Up / Ctrl+Down** | Move focus: Song panel ↔ Tracker ↔ Mutes/Loop ect... ↔ Palette |
| **Space** | Play / stop (when you’re *not* using Space to confirm something in the song or palette panel) |
| **Up / Down** | Previous / next row |
| **Shift+Up** | Jump to row 0 |
| **Shift+Down** | Jump to last row |
| **Tab / Shift+Tab** | Next / previous track (BG → L1 → L2 → SP → FX) |
| **Left / Right** | Move between digit boxes (each digit is its own box, except where i screwed that up) |
| **+ / −** | +/- values (tracker fields, song fields, mutes numbers, palette index, etc.) |
| **0–9 / A–F** | Type one hex digit into the current box |
| **Backspace** | Clear the current digit (sets it to 0) |
| **Delete** | Clear the whole row for that track |
| **Ctrl+C** | Copy this row on the current track |
| **Ctrl+Shift+C** | Copy the entire current track |
| **Ctrl+V** | Paste |
| **Page Up / Page Down** | Previous / next patten, this is pretty borken atm |
| **F1** | Toggle the little help overlay |
| **F12** | Spawn the external **video-out** window (good for a second monitor; **F11** there to go fullscreen) , but close that window and video is gone till u restart...booo|
| **Escape** | Quit, or close help  |

**When the song panel is focused**

| Key | What it does |
|-----|----------------|
| **L** | Load a `.vt` project |
| **S** | Save the project |
| **N** | new project |
| **Enter / Space** | activates whichever button is highlighted |

---

## 2. Song panel (top bar)

This is where the “transport” and file stuff lives. **Left / Right** moves between fields; on **Rate** you also move between the three digit boxes first.

- **Rate** — Three digits, like **120**. The *actual* playback speed uses **half** this number as BPM (so you get finer control when dialing things in). **Tap tempo** (in the mutes panel) can override this when its multiplier is on.
- **Extra st** — Extra **ghost ticks** per visible row (0–31). When it’s above 0, playback advances extra substeps between rows so **Glide** and **Feedback** can move more smoothly. **0** = off.
- **Steps** — How many rows the **current pattern** has (up to 64).
- **Order** — How long the **sequence** is (how many pattern slots in the song chain). The display is like `3/8` = you’re on slot 3 of 8.
- **Patterns** — How many patterns exist in the project. You can have several and pick which one plays in each order slot.
- **File** — Just the current filename (or “Untitled”). Shown for reference; you don’t type the path here.
- **Load / Save / New** — Load or save a **`.vt`** file (JSON under the hood). **Save** stores patterns, **eight palettes**, the **256×256 image buffer**, and the **sprite sheet**. **New** resets everything (see above).

The **palette swatches** on the right show the colours for the **current row** (after global palette effects), so you can see what you’re drawing with at a glance.

---

## 3. Mutes panel

**Ctrl+Down** from the tracker lands here. **Left / Right / Up / Down** move the highlight.

- **M / S / H** (per track, plus FX) — **Mute** hides a layer; **Solo** plays only the tracks you’ve soloed; **Hush** freezes a track at its current look and ignores later rows until you turn it off. **Space** or **Enter** toggles.
- **Top** — Jump playback to **order 0**, **row 0** (handy reset).
- **Loop** + **Start** / **End** — Row loop inside the **current pattern**. Turn **Loop** on, set start/end, and playback bounces inside that range.
- **Tap** — Tap in time; the app figures a BPM and can drive **Rate**. The **×** next to it picks a multiplier (or **0** for “don’t use tap”).
- **C1–C4** — Per-pattern **cue** step numbers and **Go** buttons to jump the playhead to that step.
- **Fwd / Bk** — Nudge the playhead one row forward or back (like a single step in sync).

---

## 4. Palette panel

**Ctrl+Down** again from mutes. **Left / Right** move between: load image into buffer, palette index, colour slots, save.

- **Enter / Space** on **Load** (image or sprite) opens a file dialog. On **Save** (palette), it writes the edited colours into the selected palette bank slot.
- **+ / −** adjust the palette index or channel values depending on what’s selected.

The **image buffer** and **sprite sheet** are part of the project when you **Save** from the song panel (one buffer, one sheet per project).

---

## 5. Instruments (BG / L1 / L2)

Columns: **I** (instrument id), **Val** (4 hex params), **C** (palette index 0–7 for most things), **FX** (per-track effects), **G** (glide).

**I** is two **decimal** digits (e.g. **41** = tunnel square). The tables below list the same id in **hex** so it lines up with how **Val** is split into nibbles.

**G (glide)** — Put **c**, **d**, or **e** in this column to glide from this row’s values toward the **next instrument row** on the same track: **c** = glide params, **d** = glide FX only, **e** = both. **−** or **.** turns glide off. **+ / −** cycles modes. (There’s also an older **Glide** in the FX column; **G** is the one you want so FX stays free.)

### Shape & special instruments (overview)

| Hex | Dec | Name | Param |
|-----|-----|------|--------|
| 00 | 0 | (empty) | — |
| 01 | 1 | Circle | centre `xxyy` |
| 02 | 2 | Square | centre `xxyy` |
| … | … | … | (filled shapes, outlines, Fill, Pixel, Gradient, Image, Feedback, Text — see in-app names) |
| 3A | 58 | Metaball | blob centre `xxyy` |
| 3B | 59 | Voronoi | seed `0000`–`FFFF` |
| 3C | 60 | Lissajous | **a**, **b** in first two nibbles; **phase** in last byte (0–255); glide **phase** for motion |
| 3D | 61 | Rose | first byte: **petals** (2–50); second byte: **rotation** (0–255 → full turn) |

The full list through **Spectrum** (57) matches the in-app instrument menu; **Metaball**, **Voronoi**, **Lissajous**, and **Rose** are the extra fullscreen-style ones at the end.

**Fullscreen generators (roughly 49–57)** — Things like **Plasma**, **Ripples**, **Spiral**, etc. fill the layer; **Val** is usually **`AABB`** with different meanings per instrument (phase, scale, spacing…). **C** on the multi-colour ones (**Plasma**, **Plasma2**, **Fire**, **Waves**, **Spectrum**) controls how many palette colours are used from index 0 upward; other instruments use **C** as a normal palette index.

**Tunnels (41–45)** — Param **`AABB`**: **AA** = horizontal centre, **BB** = rotation (0–255 = one turn). Vertical centre is mid-screen.

**Wireframe 3D (46–48)** — **`AABB`**: two rotation axes, each 0–255 → 0–360°.

### SP track (Sprites)

**I** is **00–03** only: **ImgCrop**, **ImgScl**, **SprSheet**. Columns **V1**, **V2**, **Sz**, **FX**, **G** — no separate colour column (sprites use the image/sheet).

---

## 6. Effects (FX)

### Per-track (BG / L1 / L2 / SP)

Value is **2 hex digits** unless noted. Names match the tracker’s effect column.

| ID | Name | Notes |
|----|------|--------|
| 0 | — | Off |
| 1 | Fade | Opacity 00–FF |
| 2 | Flash | Rate + count in value |
| 3 | FadeIn | Fade in/out over L rows |
| 4 | ScaleUp | Scale up/down over rows |
| 5 | Scale | Fixed scale |
| 6–7 | OfsX / OfsY | Pixel offset (**80** = centre) |
| 8 | Col±1 | Colour +1 / −1 |
| 9 | RndParam | Randomize param this row |
| A | Flip | Mirror / repeat modes |
| B | Tile | Grid tiling |
| C | Glide | Legacy; prefer **G** column |
| D | Palette | **FX track only** — pick global palette 0–3 |
| E | ImgReg | Image: region in the 256×256 buffer |

### Global FX (rightmost column)

Value is **4 hex digits**. Common ones:

| ID | Name | Idea |
|----|------|------|
| 1 | Fade | Whole-frame opacity |
| A | Flip | Global mirror / tile tricks |
| B | Tile | Repeat the frame in a grid |
| D | Palette | Which **palette bank** slot (0–3) colours come from |
| F | Pixelate | Chunky pixels (size from value) |
| 10 | Rotate | 0° / 90° / 180° / 270° |
| 11 | Kaleidoscope | Wedge count |
| 12 | Strobe | Flash on row changes (colour / speed / count in value) |
| 13 | Grayscale | Desaturate (+ optional invert) |

(Exact IDs match the **FX** column in the app — if something looks off, trust the in-app list.)

---

## 7. Random tips

- Params and colours **stick** until you put another command on that track — so you can animate by changing **Val** on later rows without restating the instrument.
- **Save** often; **New** is unforgiving and will distroyu all your hard work if u dont save.

Happy tracking.
