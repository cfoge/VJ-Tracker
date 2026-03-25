# VJ Tracker — ALPHA RELEASE- PC

This is a free tracker but for lofi vjing/ The canvas is 128×128 Use **Ctrl+Up / Ctrl+Down** to hop between the **song bar** at the top, the **tracker**, **mutes**, and the **palette** section at the bottom.

DOWNLOAD THE ZIP AND RUN THE EXE!

Expect some bugs!
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
- **Load / Save / New** — Load or save a **`.vt`** file (JSON under the hood). **Save** stores patterns, **eight palettes**, the **256×256 image buffer**, and the **sprite sheet**. 
**New** resets everything (see above).

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

Columns: **I**, **Val**, **C**, **FX**, **G**.

| Column | Meaning |
|--------|--------|
| **I** | Instrument id — **two decimal digits** (`00`–`62`). Example: **41** = first tunnel. |
| **Val** | Four **hex** digits (`0000`–`FFFF`). Meaning depends on the instrument (see below). |
| **C** | One hex digit **0**–**7**: palette index for most instruments. On **Plasma**, **Plasma2**, **Fire**, **Waves**, and **Spectrum**, **C** instead controls **how many** palette colours are blended (smaller **C** = more colours; **7** ≈ narrow range). |
| **FX** | Per-track effect type + **two hex digits** of value. |
| **G** | Glide — **c** / **d** / **e** glides toward the **next instrument row** on this track (**c** = params, **d** = FX only, **e** = both). **−** / **.** off. Prefer this over legacy **Glide** in the FX column. |

Digits in **Val** are always hex. When docs say **`XXYY`**, **XX** is the left byte (horizontal / first field) and **YY** is the right byte (vertical / second field), each **00**–**7F** (0–127) for positions on the 128×128 canvas.

---

### Shapes — filled (1–19) and outlined (20–34)

Outlined versions use the **same Val layout** as their filled twin; only stroke vs fill changes.

| ID | Name | Val (4 hex digits) |
|----|------|---------------------|
| 1 | Circle | **`XXYY`** — centre |
| 2 | Square | **`XXYY`** — centre |
| 3 | Tri | **`XXYY`** — centre |
| 4 | TriDn | **`XXYY`** — centre |
| 5 | BarH | **`XXYY`** — **XX** = bar thickness (min 2), **YY** = vertical position (full width) |
| 6 | BarV | **`XXYY`** — **XX** = horizontal position, **YY** = bar thickness (full height) |
| 7 | Diamond | **`XXYY`** — centre |
| 8 | Star | **`XXYY`** — centre |
| 9 | Cross | **`XXYY`** — centre |
| 10 | Ellipse | **`XXYY`** — centre |
| 11 | RndRect | **`XXYY`** — centre |
| 12 | LineH | **`XXYY`** — **YY** = line Y; **XX** unused for geometry |
| 13 | LineV | **`XXYY`** — **XX** = line X; **YY** unused for geometry |
| 14 | Hexagon | **`XXYY`** — centre (regular hexagon; not the same as **Cells** or **TunHex**) |
| 15 | Frame | **`XXYY`** — centre |
| 16 | Heart | **`XXYY`** — centre |
| 17 | FSCross | **Val ignored** — full-screen 1-pixel cross; **C** sets colour |
| 18 | Grid | **`XXYY`** — **XX** = columns, **YY** = rows (each 2–16; values under 2 default to 4). Checkerboard |
| 19 | Noise | **`0000`–`FFFF`** — seed; same seed ⇒ same random dot pattern |

| ID | Name | Val |
|----|------|-----|
| 20–34 | CirO … FramO | Same as 1–15 above |

**BarHO / BarVO** — same **`XXYY`** rules as **BarH** / **BarV**; minimum thickness is higher for the hollow look.

---

### Special instruments (35–40)

| ID | Name | Val |
|----|------|-----|
| 35 | Fill | Ignored — solid **C** colour |
| 36 | Pixel | **`XXYY`** — single pixel |
| 37 | Gradient | **`D C0 ? C1`** — **1st digit** **D**: `0` = gradient across **columns**, `1` = across **rows**. **2nd digit** = start palette index (when **C** is 0). **4th digit** = end palette index. Middle digit is unused |
| 38 | Image | **`XXYY`** — top-left where the chunk is drawn. Region in the 256×256 buffer comes from **FX → ImgReg (E)** and **C** (see below) |
| 39 | Feedback | **`HHLL`** — **high byte `HH`**: `00` plain, `01` opacity, `02` scale, `03` vertical offset. **Low byte `LL`**: `00` every frame, `01` once per play, `03` timed ~2s capture |
| 40 | Char | **`GGCC`** — **GG** = one byte: high nibble **X** grid (0–15), low nibble **Y** grid (0–15); **CC** = character code (hex, 00–7F). Renders one character |

**Image (38) + ImgReg** — Per-track effect **E** value is **two hex digits**: high nibble = 16×16 **block column** in the buffer (0–15), low nibble = **block row** (0–15), so start `(sx, sy) = (nib×16, nib×16)`. **C** sets the **square size**: `(C + 1) × 16` pixels (16…128).

---

### Tunnels (41–45) and wireframe 3D (46–48)

| ID | Name | Val |
|----|------|-----|
| 41–45 | TunSq … TunStar | **`AABB`** — **AA** = horizontal centre (0–127), **BB** = rotation (0–255 → one full turn). Vertical centre is always mid-screen |
| 46–48 | WireCube, WireTetra, WireOcta | **`AABB`** — **AA** and **BB** each map 0–255 → 0–360° for the two rotation axes (orthographic view, centred) |

---

### Fullscreen / procedural (49–58)

**Val** is usually **`AABB`** (two bytes 0–255). **C** on **Plasma**, **Plasma2**, **Fire**, **Waves**, **Spectrum** = palette span (see column table above). Other instruments here use **C** as a normal colour index.

| ID | Name | **AA** | **BB** |
|----|------|--------|--------|
| 49 | Plasma | phase | scale |
| 50 | Ripples | ring spacing | phase offset |
| 51 | Spiral | start angle (0–255 → 0–360°) | turn count (maps to 1–8 turns) |
| 52 | Moire | grid angle | line spacing |
| 53 | **Cells** | horizontal **pattern offset** | vertical **pattern offset** (hexagonal tiling; **not** `XXYY` position) |
| 54 | Plasma2 | phase | scale (radial plasma) |
| 55 | Fire | phase | intensity |
| 56 | Waves | phase | scale |
| 57 | Spectrum | phase | band width |
| 58 | Metaball | **`XXYY`** — first blob centre; second blob mirrors at `(127−X, Y)` |

| ID | Name | Val |
|----|------|-----|
| 59 | Voronoi | **`0000`–`FFFF`** — seed for random sites |
| 60 | Lissajous | **`abPP`** — **a** = 1st hex digit (freq A), **b** = 2nd digit (freq B), **PP** = last two hex digits = phase byte (0–255 → 0–2π). Glide can animate phase |
| 61 | Rose | **`KKRR`** — **KK** = petal factor (clamped 2–50), **RR** = rotation (0–255 → 0–2π) |
| 62 | Text | **`xysi`** — nibbles: **x**, **y** grid (0–15), **s** = scale step, **i** = index into comma-separated **Txt Load** strings |

---

### Hexagon vs “hex” in the tracker

- **Hexagon / HexO (14 / 33)** — Normal shape: **`XXYY`** = pixel centre.
- **TunHex (42)** — Tunnel: **`AABB`** = horizontal centre + rotation; not the same encoding as shape Hexagon.
- **Cells (53)** — Fullscreen hex **tiling**: **`AABB`** = scroll the pattern in X and Y (offsets), not a single shape position.

---

## 6. SP track (Sprites)

Sprites use a **different** instrument set and **extra columns** — there is **no C (colour)** column.

| Column | Role |
|--------|------|
| **I** | **Decimal** `00`–`03`: **01** ImgCrop, **02** ImgScl, **03** SprSheet |
| **V1** | First param — **four hex digits** — canvas position **`XXYY`** (top-left where content is placed; 0–127) |
| **V2** | Second param — **four hex digits** — depends on instrument (below) |
| **Sz** | **Two hex digits** — size: maps **00**–**FF** linearly to draw size **16**–**128** px (sprite scale / crop size) |
| **FX** | Same per-track effects as BG/L1/L2 (effect + 2 hex value digits) |
| **G** | Glide, same as other tracks |

**256×256 image buffer** (palette indices) — load via palette **Load** image. **256×256 sprite sheet** (RGBA, 4×4 cells of 64×64) — load via palette **Load sprite**.

### ImgCrop (**I = 01**)

- **V1** = **`XXYY`** where to draw on the canvas.
- **V2** = **`XXYY`** = top-left corner inside the **image buffer** to copy from.
- **Sz** = square **window** side length (16–128): the crop is `min(size, buffer − sx, buffer − sy)` and drawn 1:1 at **V1**.

### ImgScl (**I = 02**)

- **V1** / **V2** same idea as ImgCrop (canvas position + buffer start).
- The region taken from the buffer is scaled with **nearest neighbour** so integer blocks fill up to 128×128 (chunky zoom).

### SprSheet (**I = 03**)

- **V1** = **`XXYY`** canvas position.
- **V2** = sprite index: use **`00`–`0F`** (one of 16 cells in a 4×4 sheet). Only the first two hex digits matter; pad as `00` … `0F`.
- **Sz** = on-screen size of the sprite (16–128); scales from the 64×64 cell.

**Project save** stores the image buffer and sprite sheet with the **`.vt`** file.

---

## 7. Effects (FX)

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
| E | ImgReg | **Image** instrument (38): value picks the 16×16 **block** in the buffer (high nibble = column, low nibble = row). **C** sets square size `(C+1)×16` — see §5 **Image** |

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


---

## 8. Random tips

- Params and colours **stick** until you put another command on that track — so you can animate by changing **Val** on later rows without restating the instrument.
- **Save** often; **New** is unforgiving and will destroy unsaved work.

Happy VJing.
Rob
