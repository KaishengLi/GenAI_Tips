# 4. AI for Slides and Posters

> **Principle:** **Story first, slides second.** AI is excellent at turning an approved outline into slides in *your* template. It is poor at deciding what your talk is about, and poor at judging its own visual quality.

---

## 1. The workflow

```
1. One-sentence message (you)
2. Story outline + assertion titles  ← AI drafts, YOU edit hard
3. Slides in YOUR template           ← AI generates
4. Real figures inserted             ← you (never let AI redraw data plots)
5. Visual QA                          ← render → screenshot → AI critique → you decide
6. Speaker notes + rehearsal + Q&A prediction ← AI
```

1. **Content first, no tools yet.** Write one sentence: *"What should the audience remember?"* Then outline the story: problem → gap → approach → key result → implication. One message per slide.
2. **Use assertion–evidence design** ([Michael Alley, Penn State](https://writing.engr.psu.edu/research.html)). Each title is a **full-sentence claim**, supported by **visual evidence**, not bullets. In controlled studies, audiences understood and remembered more, and presenters understood their own material better. This style is also easy to prompt for.
3. **AI outline → human edit.** Give the AI your paper and notes, the audience, and the time limit, and ask for the outline and titles only. Edit them hard yourself, because this step is the thinking.
4. **Generate into your template.** Point the tool at your lab or university template (Claude in PowerPoint, Copilot, the pptx skill), or generate Marp/Beamer code that uses your theme.
5. **Polish by hand.** Check every number against the paper, fix overflow, cut text, and make sure the back row can read the figures.
6. **Rehearse with AI.**
   - PowerPoint *Speaker Coach* reports pace and filler words.
   - Then paste your rehearsal transcript into an LLM for critique and to predict likely questions.

---

## 2. Tool landscape

| Tool | Strengths | Watch out for |
|---|---|---|
| **Claude in PowerPoint** add-in | Reads your slide master; produces native, editable objects; rewrites titles as "action titles"; adds speaker notes | Beta; plan availability varies |
| **Claude + pptx skill** ([anthropics/skills](https://github.com/anthropics/skills)) | Builds/edits .pptx; **built-in visual QA loop** (renders → checks overflow/overlap) | Less template-aware than the add-in |
| **Copilot in PowerPoint** | Edits in place, keeps formatting; uses your M365 files and brand kit | Grounding needs a paid license |
| **Gemini in Google Slides / Canvas** | Grounded in Drive files; can match another deck's style; "Beautify this slide" | Newer; English-first |
| **NotebookLM Slide Decks** | Every slide is grounded in your sources; "Scientific" style preset | Slides are mostly **images**, so text isn't editable |
| **Gamma / Beautiful.ai / Canva** | Fastest prompt → deck; auto-layout | Generic look; PPTX export can split or flatten text |
| **Napkin AI** | Paste text → editable diagrams (SVG/PPT) | Diagrams only, not whole decks |
| **Marp / Slidev / reveal.js / Beamer** | Plain text + version control; LLMs write Markdown/LaTeX reliably; math & code built in | The LLM can't *see* overflow unless you render the slides and show it |

**Evidence.**
- Coding assistants (Claude Code, Cursor) produced the most accurate and pedagogically sound slides. Students rated AI slides about as high as instructor slides and could not reliably tell them apart. **But slides they *suspected* were AI-made were rated lower** ([arXiv 2605.13532](https://arxiv.org/abs/2605.13532)).
- AI models are poor at spotting design flaws (F1 0.33–0.66; [SlideAudit, UIST'25](https://faculty.washington.edu/wobbrock/pubs/uist-25.01.pdf)), so the final visual check stays with you.

---

## 3. Prompts

### P1 — Outline from a paper
```text
Here is my paper [PDF]. I'm giving a 12-minute talk to [mixed-discipline grad
students]. Propose a 10–12 slide outline as a story (problem → gap → approach →
results → takeaway). For each slide give:
(a) a full-sentence assertion title, ≤12 words;
(b) the single piece of evidence that supports it (name the paper's figure/table number);
(c) 1–2 sentences of speaker notes.
Use only numbers that appear in the paper, and cite the section for each.
```

### P2 — Convert titles to assertions
```text
Here are my current slide titles: [list]. Rewrite each as a full-sentence claim
the audience should remember (Michael Alley's assertion–evidence style).
Flag any slide that has no single claim and should be split or cut.
```

### P3 — Marp / Beamer code
```text
Turn this approved outline into [Marp Markdown, theme: gaia, 16:9 |
LaTeX Beamer with \usetheme{metropolis}].
Rules: assertion titles; ≤2 short bullets per slide; put ![w:700](figs/figN.png)
placeholders where my figures go; speaker notes in <!-- --> [or \note{}];
don't invent data. Then list any slides likely to overflow.
```

### P4 — Predict audience questions
```text
Here is my talk transcript and slides. Act as three audience members: a skeptical
expert reviewer, a researcher from a neighboring field, and a first-year PhD student.
Give the 10 hardest questions most likely to be asked, ranked by likelihood.
For each: the weak point it targets, plus a 30-second answer outline.
```

### P5 — Poster layout spec
```text
Design a 48×36 in landscape #betterposter-style layout for [paper]. Output:
- main finding as one sentence (≤15 words, plain language);
- a grid with the size and position of each panel;
- 4 supporting panels (methods diagram, key figure, 3 results, limitations), ≤50 words each;
- minimum font sizes; QR code position; a colorblind-safe palette.
Then [write it as tikzposter code | describe it for me to build in PowerPoint].
```

### P6 — Critique rendered slides (multimodal)
```text
Here are screenshots of my slides. For each slide, check: Is the title a claim?
Is there one message? Any text under 18 pt? Overflow or misalignment?
Readable from the back of a 100-seat room? Answer slide by slide with concrete fixes.
```

---

## 4. Posters

- **#betterposter** ([Mike Morrison](https://osf.io/ef53g)): put the main finding in large plain-language type in the center, supporting detail in side panels, and a QR code to the paper.
- **First draft:** [Paper2Poster](https://github.com/Paper2Poster/Paper2Poster) (NeurIPS 2025) turns a paper PDF into an **editable .pptx poster**, which you then restyle. Alternatively, have an LLM write the layout spec (P5) and build the poster in PowerPoint or tikzposter.
- **Test print** the poster at A4 or Letter. If you can read it at arm's length, the full-size print will be readable.

---

## 5. GitHub resources (stars approx., Oct 2026)

| Repo | ★ | What it is |
|---|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) (`pptx`) | ~180k | Agent skill: create/edit PPTX with a visual QA loop |
| [hakimel/reveal.js](https://github.com/hakimel/reveal.js) | ~72k | HTML presentation framework |
| [slidevjs/slidev](https://github.com/slidevjs/slidev) | ~49k | Markdown + Vue slides for developers |
| [Anionex/banana-slides](https://github.com/Anionex/banana-slides) | ~16k | Image-model ("Nano Banana") deck generator; template images; exports editable PPTX |
| [marp-team/marp](https://github.com/marp-team/marp) | ~13k | Markdown → PDF/PPTX/HTML |
| [presenton/presenton](https://github.com/presenton/presenton) | ~11k | Open-source, self-hostable Gamma alternative |
| [icip-cas/PPTAgent](https://github.com/icip-cas/PPTAgent) | ~5k | Learns from reference decks, then generates new slides by editing them (EMNLP 2025) |
| [Paper2Poster/Paper2Poster](https://github.com/Paper2Poster/Paper2Poster) | ~4k | Paper → editable PPTX poster (NeurIPS 2025) |
| [HKUDS/Paper2Slides](https://github.com/HKUDS/Paper2Slides) | ~4k | Paper → slides/posters (image output, no PPTX) |
| [scanny/python-pptx](https://github.com/scanny/python-pptx) | ~4k | Programmatic PPTX |
| [OpenDCAI/Paper2Any](https://github.com/OpenDCAI/Paper2Any) | ~3k | Paper → editable figures, diagrams, PPTX |
| [showlab/Paper2Video](https://github.com/showlab/Paper2Video) | ~2k | Paper → presentation video |
| [takashiishida/paper2slides](https://github.com/takashiishida/paper2slides) | <1k | arXiv LaTeX → Beamer; self-checks its output, then compiles |

---

## 6. Pitfalls

- **Generic AI look.** Stock images, a repeated layout, gradient themes. Use your template and your own figures.
- **Text overload.** LLMs default to 4–6 bullets per slide. Set hard word limits and use assertion–evidence.
- **Invented or wrong numbers.** Cross-check every number against the paper.
- **Broken layouts.** Only use tools that render and inspect their own output, and inspect it yourself too.
- **Non-editable output.** Test the export before committing to a tool.
- **Losing your own understanding.** Building the outline yourself is how you learn the story.

---

## Workshop takeaway (one slide)

> **"Write the story, let AI build the slides, then you judge them."** Assertion titles + your template + render-and-check.
