# 3. AI for Academic Figures and Illustrations

> **Principle:** **Structure first, pixels last.** Turn your idea into an explicit spec (components, connections, layout, labels, style) *before* any model draws anything. Then pick the rendering route by what matters most: looks, accuracy, or editability.

---

## 0. The map: four routes to a figure

| Route | Looks | Structural accuracy | Editable? | Best for |
|---|---|---|---|---|
| **A. Prompt → image** (Nano Banana, GPT-Image, …) | ★★★★★ | ★★ (arrows and labels drift) | ✗ pixels | Concept art, icons, graphical-abstract drafts |
| **B. Idea → structured spec → image** (PaperBanana-style) | ★★★★ | ★★★ | partly | Method-overview figures |
| **C. LLM writes code** (SVG, draw.io XML, TikZ, matplotlib, python-pptx) | ★★★ | ★★★★★ | ✓ fully | Pipelines, plots, anything you will revise |
| **D. Image → editable** (layering, segmentation, OCR, vectorizing) | ★★★★ | ★★★ | ✓ after cleanup | Making a good-looking AI draft editable |

**The workflow that works in practice:**

```
Idea ─► Structured spec (LLM drafts, YOU fix) ─► Layout lock (sketch / draw.io / code)
     ─► Style render (image model, NO text) ─► Vectorize / decompose ─► Live text + polish by hand
```

**Hard rule: data plots are always code, never pixels.** Image models produce "numerical hallucinations" on dense plots, and even the PaperBanana authors recommend code for plots ([arXiv 2601.23265](https://arxiv.org/abs/2601.23265)).

---

## A. Direct image generation: how to prompt well

### Current models (Oct 2026: check before presenting, this changes monthly)
- **Google "Nano Banana" family**: Nano Banana Pro (`gemini-3-pro-image`), Nano Banana 2 (`gemini-3.1-flash-image`), and newer versions listed in the [Gemini API docs](https://ai.google.dev/gemini-api/docs/image-generation). They accept up to 14 reference images, output 1K/2K/4K, and support multi-turn editing. Currently the strongest for diagrams with text.
- **OpenAI GPT-Image 2 / ChatGPT Images 2.0**: reasons before drawing; supports mask edits and transparent backgrounds. OpenAI's own docs warn that it "may have difficulty placing elements precisely in structured or layout-sensitive compositions" ([docs](https://developers.openai.com/api/docs/guides/image-generation)).
- **Others**:
  - Ideogram: text-heavy layouts.
  - FLUX.2: open weights, so you can fix the seed.
  - Seedream: layered edits.
  - Midjourney: best-looking output, worst text.
- **Native vector generators**: Recraft Vector (SVG output) and Adobe Illustrator *Text to Vector Graphic*.

### Eight rules for prompting academic figures
1. **Write a spec, not a vibe.** "The more specific you are, the more control you have" (Google's guide).
2. **Fill in the checklist, in this order:** canvas & aspect ratio → panel grid → components with positions → connections (from → to, arrow style) → exact label strings → style (flat vector, white background, uniform strokes, no gradients, shadows, or 3D) → palette as hex codes → font → **a "do NOT add" list**.
3. **Use JSON for complex figures.** Separate keys reduce attributes bleeding into each other, and a JSON prompt can be diffed and reproduced.
4. **Generate without text, add labels later.** Typography is still the weakest point, and accuracy drops past ~8–10 structural elements.
5. **Lock the layout with a reference image.** Upload a hand sketch, a PowerPoint box-and-arrow mockup, or a draw.io export, and say: *"Keep this exact layout and these connections; restyle only."* Add 1–3 figures from your target venue as **style** references.
6. **Edit region by region.** Ask for *"Change only the encoder block to blue; keep everything else identical."* Never regenerate the whole figure to fix one part.
7. **Generate-and-select.** Make 4–8 candidates and choose the best. Save the prompt, model ID, date, and every output, because hosted models are not deterministic.
8. **Make icons as separate assets.** Generate them on a transparent or white background, then arrange them yourself.

### Example 1: structured text prompt (method overview)
```text
Create a clean academic method-overview figure for a machine-learning paper.
Canvas: 16:9, white background, generous margins, flat vector style like a NeurIPS paper figure.
Layout: three columns left→right, separated by thin light-grey dashed vertical lines.
 Column 1 "Input": a stack of 3 overlapping document icons above a small table icon.
 Column 2 "Model": a rounded rectangle containing two stacked blocks — top block blue (#0072B2),
   bottom block orange (#E69F00); a small circular "loop" arrow on the right side of the rectangle.
 Column 3 "Output": a bar-chart icon above a checkmark-in-circle icon (green #009E73).
Connections: one thick dark-grey arrow from Column 1 to the top block; one arrow from the bottom
block to Column 3. No other arrows.
Style: 2px uniform strokes, no gradients, no shadows, no 3D, no people, no decorative background.
Text: DO NOT render any text, letters or numbers anywhere — leave clear empty space above each
column for labels I will add later.
Do not add any components not listed above.
```

### Example 2: JSON spec prompt
```json
{
  "figure_type": "pipeline schematic for a biology methods paper",
  "canvas": {"aspect_ratio": "3:1", "background": "#FFFFFF", "grid": "1 row x 4 panels, equal width"},
  "style": {
    "look": "flat vector, scientific journal",
    "stroke": "1.5px #333333", "corners": "rounded 6px",
    "palette": ["#0072B2", "#E69F00", "#009E73", "#CC79A7"],
    "font": "Helvetica-like sans-serif",
    "forbid": ["gradients", "drop shadows", "3D", "photorealism", "extra icons", "watermarks"]
  },
  "panels": [
    {"id": "a", "label": "Sample collection",      "icon": "test tube rack",                  "color": "#0072B2"},
    {"id": "b", "label": "Single-cell sequencing", "icon": "microfluidic chip with droplets",  "color": "#E69F00"},
    {"id": "c", "label": "Clustering",             "icon": "2D scatter of 3 colored clusters", "color": "#009E73"},
    {"id": "d", "label": "Marker validation",      "icon": "small 4x4 heatmap",               "color": "#CC79A7"}
  ],
  "edges": [{"from": "a", "to": "b"}, {"from": "b", "to": "c"}, {"from": "c", "to": "d"}],
  "edge_style": "straight horizontal arrows, solid, vertically centered",
  "text_rules": {
    "render_only_these_strings": ["a", "b", "c", "d", "Sample collection", "Single-cell sequencing", "Clustering", "Marker validation"],
    "panel_letters": "bold lowercase, top-left of each panel"
  },
  "constraints": ["exactly 4 panels", "exactly 3 arrows", "no additional text"]
}
```

---

## B. Idea → structured spec → image (the PaperBanana method)

**PaperBanana** (Google Cloud AI Research + PKU, ICML 2026; [arXiv 2601.23265](https://arxiv.org/abs/2601.23265)) is a five-agent pipeline:

```
Retriever (finds reference figures) → Planner (method text → detailed figure description)
→ Stylist (applies aesthetic guidelines) → Visualizer (image model) → Critic (3 refinement rounds)
```

- Scored **+17%** over prompting Nano Banana Pro directly, and won **72.7%** of blind human comparisons.
- The most common remaining failure is **wrong or duplicate arrows**. That is why *you* must check the spec and the connections.
- Repos: [dwzhu-pku/PaperBanana](https://github.com/dwzhu-pku/PaperBanana) (~7k★, authors' version) and [google-research/papervizagent](https://github.com/google-research/papervizagent) (official).

### Do it by hand in any chat model (no installation needed)
1. **Spec:** paste your method section → *"Produce a figure spec: components, edges (from→to, label), grid positions, exact label strings, palette. Ask me about anything ambiguous. Don't draw yet."*
2. **Fix the spec yourself.** This is where scientific errors get caught.
3. **Render:** send the spec plus 2–3 reference figures from your venue to the image model, without text.
4. **Critique:** *"Compare this image to the spec. List every missing, extra, or wrong component or arrow."* Fix region by region.
5. Make it editable (route D) or rebuild it as code (route C).

### Other systems worth knowing
| System | What it does | Link |
|---|---|---|
| **AutoFigure** (ICLR 2026) | Reasons about and checks the layout first; outputs **SVG + draw.io XML** | [ResearAI/AutoFigure](https://github.com/ResearAI/AutoFigure) (~2k★) |
| **AutoFigure-Edit** | Method text → raster draft → SAM3 segmentation → **editable SVG** (~$0.50/figure) | [ResearAI/AutoFigure-Edit](https://github.com/ResearAI/AutoFigure-Edit) (~4k★) |
| **DeTikZify / TikZero** | Sketch or figure → TikZ; text → TikZ | [potamides/DeTikZify](https://github.com/potamides/DeTikZify) (~2k★) |
| **Paper2Any** | Paper → figures, draw.io diagrams, editable PPTX/SVG | [OpenDCAI/Paper2Any](https://github.com/OpenDCAI/Paper2Any) (~3k★) |
| **DiagramAgent** (CVPR 2025) | Plan → Code → Check agents for LaTeX/DOT diagrams | [paper](https://openaccess.thecvf.com/content/CVPR2025/html/Wei_From_Words_to_Structured_Visuals_A_Benchmark_and_Framework_for_CVPR_2025_paper.html) |
| **ScImage** (ICLR 2025) | Benchmark: code output (Python/TikZ) vs. pixel output for scientific figures | [arXiv 2412.02368](https://arxiv.org/abs/2412.02368) |

---

## C. Code-based, fully editable output

**Why code:** exact text, exact connections, version control, and the figure is vector from the start.

| Format | Use for | How |
|---|---|---|
| **SVG** (LLM writes it directly) | Schematics you'll refine in Inkscape/Illustrator/Figma | Ask for `<g id>` groups, real `<text>` elements, a fixed viewBox, and a palette (Example 3) |
| **draw.io XML** | Diagrams your co-authors can edit | Official [jgraph/drawio-mcp](https://github.com/jgraph/drawio-mcp) (~6k★); [DayuanJiang/next-ai-draw-io](https://github.com/DayuanJiang/next-ai-draw-io) (~36k★), which can also **recreate a diagram from an uploaded image** and has a screenshot tool so the agent checks its own output |
| **Excalidraw** | Hand-drawn look for talks and blogs | [excalidraw/excalidraw-mcp](https://github.com/excalidraw/excalidraw-mcp) (~6k★) |
| **TikZ / PGFPlots** | LaTeX-native fonts and math | Expect compile-and-fix loops; DeTikZify helps |
| **Mermaid / Graphviz** | Quick flowcharts and DAGs (automatic layout) | Drafts and supplementary figures |
| **matplotlib / seaborn / plotly** | **All data plots** | The LLM writes the script against your *real* data file and you run it. Use [SciencePlots](https://github.com/garrettj403/SciencePlots) (~9k★) for journal styles |
| **python-pptx** | Labs that polish figures in PowerPoint | The LLM writes a script that draws native shapes, connectors, and text boxes |

**Agent workflow (Claude Code / Codex + draw.io MCP or SVG):**
> spec → generate → **render & screenshot** → the agent critiques its own render (overlaps, alignment, missing arrows) → fix → you review.

The visual feedback loop matters. In MatPlotAgent and PlotGen, adding a "look at the render and fix it" step raised plotting benchmark scores from 48.9 → 61–66.

**Computer-use agents** can also drive Inkscape, draw.io, or PowerPoint directly. This works but is slow and fragile, so prefer having the agent write the **file** (SVG / .drawio / .pptx) rather than click through the app.

### Example 3: editable SVG / draw.io from an LLM
```text
You are a scientific illustrator. From the method description below, first output a short spec
(components, edges, 12-column grid positions) and WAIT for my approval. Then produce:
(1) a standalone SVG, viewBox="0 0 1830 700" (183 mm double-column at 10 px/mm), with:
  - every component in a <g id="..."> group; text as <text> elements,
    font-family="Helvetica, Arial, sans-serif", ~7 pt at final size; no outlined text, no raster images;
  - arrows as <path> with one shared <marker> arrowhead; stroke-width 2;
    Okabe-Ito colors only (#0072B2 #E69F00 #009E73 #D55E00 #CC79A7 #56B4E9 #000000);
  - elements aligned to the grid, consistent 24 px padding, no overlaps;
(2) the same diagram as uncompressed draw.io (mxGraph) XML, one mxCell per box/arrow/label.
Do not invent components that are not in the description; list ambiguities as questions.
Method: <paste method paragraph>
```

---

## D. Image → editable file

| Tool | What it does | Notes |
|---|---|---|
| [QwenLM/Qwen-Image-Layered](https://github.com/QwenLM/Qwen-Image-Layered) (CVPR 2026, ~2k★) | Splits one image into separate RGBA layers you can move or recolor | Apache-2.0 |
| [BIT-DataLab/Edit-Banana](https://github.com/BIT-DataLab/Edit-Banana) (~6k★) | Diagram image → **draw.io XML** (SAM 3 + OCR + formulas → LaTeX) | Arrows remain images; license unclear |
| [ResearAI/AutoFigure-Edit](https://github.com/ResearAI/AutoFigure-Edit) | Raster → editable SVG | See B |
| [joanrod/star-vector](https://github.com/joanrod/star-vector) (CVPR 2025, ~5k★) / [OmniSVG](https://github.com/OmniSVG/OmniSVG) (~3k★) | Learned image → SVG | Good for icons and diagrams |
| [visioncortex/vtracer](https://github.com/visioncortex/vtracer) (~7k★), potrace, [Vectorizer.ai](https://vectorizer.ai), Illustrator *Image Trace* | Classic tracing | **Text becomes outlines**, so retype it |
| [ningzimu/image-to-editable-ppt-skill](https://github.com/ningzimu/image-to-editable-ppt-skill) (~3k★) | Agent skill: image → native PPT shapes and text | Uses many tokens |

**Workflow:**
1. Generate at 2K–4K **without text**.
2. Decompose it (Qwen-Image-Layered or SAM) or trace it (vtracer, StarVector).
3. Open in Illustrator, Inkscape, or Figma.
4. Add all text as **live text** in one font.
5. Snap to a grid, unify stroke widths, and recolor to your palette.

---

## E. Good-figure fundamentals (AI doesn't change these)

- **[Ten Simple Rules for Better Figures](https://pmc.ncbi.nlm.nih.gov/articles/PMC4161295/)** (Rougier et al. 2014): know your audience · identify your message · adapt to the medium · captions are not optional · **don't trust the defaults** · use color effectively · don't mislead · avoid chartjunk · message trumps beauty · get the right tool.
- **Nature specs** ([guide](https://research-figure-guide.nature.com/figures/preparing-figures-our-specifications)):
  - 89 mm (single column) / 183 mm (double column) wide.
  - Arial or Helvetica at 5–7 pt.
  - **"Do not rasterize line art or text"**; supply editable vector files. This is another reason pixel output needs route D.
- **Colorblind-safe palette (Okabe–Ito):** `#000000 #E69F00 #56B4E9 #009E73 #F0E442 #0072B2 #D55E00 #CC79A7`. Use viridis or cividis for continuous scales.
- **Keep a "lab figure style" block** (font, stroke width, palette, corner radius) and paste it into every prompt, or encode it in a skill: [`skills/paper-figure`](../skills/paper-figure/SKILL.md).

---

## F. ⚠️ Journal policies on AI-generated images: read before you submit

| Publisher | Policy |
|---|---|
| **Springer Nature / Nature** | Generative-AI images **not permitted** in publications (research figures *and* illustrations); narrow exceptions ([policy](https://www.springer.com/us/editorial-policies/artificial-intelligence--ai-/25428500)) |
| **Science (AAAS)** | Not permitted without explicit editor permission |
| **Elsevier** | Schematics and flowcharts OK **with disclosure in the caption**; primary research images prohibited; **no generative-AI graphical abstracts** ([policy](https://www.elsevier.com/about/policies-and-standards/generative-ai-policies-for-journals)) |
| **Cell Press** | Banned for primary data; sources conflict on schematics, so ask the editor |
| **Taylor & Francis** | Prohibited for images of research outputs |
| **ML conferences** | Generally disclosure only; check the current CFP |

**Practical guidance:**
- Figures drawn **by code that an AI wrote** (matplotlib, TikZ, draw.io, SVG) are generally *not* treated as "generative-AI images." Still disclose the AI assistance.
- For pixel-generated schematics, assume they are **banned at Nature/Science** and for Elsevier graphical abstracts. Use them as drafts, then rebuild the figure as vectors (routes C and D).
- Keep your prompts, model versions, and dates for the disclosure statement.

---

## Workshop takeaway (one slide)

> **"Spec first. Code for structure, image models for style. Never pixels for data."**
