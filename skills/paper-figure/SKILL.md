---
name: paper-figure
description: Creates publication-quality, editable scientific figures - data plots as reproducible matplotlib scripts and schematic/method-overview diagrams as SVG or draw.io - following a spec-first workflow, the lab style, and journal size rules. Use when the user asks for a paper figure, method overview, pipeline diagram, schematic, camera-ready plot, or to restyle/regenerate figures, even if they just say "plot this for the paper" or "draw my method".
---

# Paper figures (spec-first, editable)

## Step 0: decide which kind of figure this is

- **Data plot**: anything with numbers on an axis. → *Data plot workflow*. Never use an image-generation model for these.
- **Schematic**: method overview, pipeline, or concept diagram. → *Schematic workflow*.

## Lab style (apply to every figure)

- Widths: single column 3.5 in / 89 mm; double column 7.2 in / 183 mm. Fonts: Helvetica/Arial, ≥ 7 pt at final size.
- Palette (Okabe–Ito): `#0072B2 #E69F00 #009E73 #D55E00 #CC79A7 #56B4E9 #F0E442 #000000`. Never rely on color alone; also vary markers or line styles.
- Uniform stroke width (1–1.5 pt). No gradients, shadows, or 3D. White background.
- Text must stay live text: never outline it or rasterize it.

## Data plot workflow

- [ ] 1. Inspect the data: print the columns, dtypes, NaN counts, and value ranges.
- [ ] 2. Write `figures/src/fig_<name>.py`. It must read only from the results files; never type numbers in by hand.
- [ ] 3. Label every axis with its units. Show uncertainty (CI or SE) and say which one it is.
- [ ] 4. Export `figures/fig_<name>.pdf` (vector) and `.png` (300 dpi).
- [ ] 5. **Look at the PNG.** List any overlaps, unreadable ticks, or clipped labels. Fix them and re-run.
- [ ] 6. Report the script path, the data source, any excluded data and why, and a draft caption.

## Schematic workflow

- [ ] 1. **Spec first.** From the user's method text, write `figures/spec_<name>.md`:
  - components (id, label, short description)
  - edges (from → to, with a label if any)
  - a 12-column grid position for each component
  - the exact label strings
  - panel letters

  List anything ambiguous as questions. **Stop and get the user's approval of the spec.**
- [ ] 2. Build `figures/fig_<name>.svg`:
  - one `<g id>` group per component
  - real `<text>` elements
  - arrows that share one `<marker>`
  - a fixed viewBox at the target width (e.g. 1830 × H for 183 mm at 10 px/mm)

  If a draw.io MCP or CLI is available, also write `fig_<name>.drawio`.
- [ ] 3. Render it to PNG (e.g. `rsvg-convert` or `inkscape --export-type=png`) and **look at it**.
- [ ] 4. Check the render against the spec: every component is present, **every arrow matches an edge in the spec** (no extras, none missing), there are no overlaps, and spacing is consistent. Fix and repeat. Stop after 3 rounds and report what is left.
- [ ] 5. Optional styling pass: if the user wants a richer look, write an image-model prompt with **no text**, the same layout, and the palette. Note that the result must be vectorized and relabeled. Remind the user that Nature/Science/Elsevier restrict generative-AI images.

## Rules

- Never smooth data, drop outliers, or change axis limits to hide data without saying so.
- Never add components or arrows that are not in the approved spec.
- Keep the script/spec and the output side by side so the figure can be regenerated.
