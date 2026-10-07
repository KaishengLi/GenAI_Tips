# Slide Outline and Design Guide

## Part A: Design system

**Style: assertion–evidence.** Every slide title is a **full-sentence claim** (≤ 2 lines). The body is *evidence*: a figure, a demo screenshot, a prompt card, or a small table. Bullets only when unavoidable, ≤ 3 per slide.

| Element | Spec |
|---|---|
| Aspect | 16:9 |
| Fonts | One sans family (e.g. Inter / Source Sans / Helvetica). Titles 32–40 pt, body ≥ 20 pt, code ≥ 18 pt in a monospace font (JetBrains Mono / Fira Code) |
| Colors | Neutral background; one accent per module using Okabe–Ito: M1 Research `#0072B2` blue · M2 Writing `#009E73` green · M3 Figures `#E69F00` orange · M4 Slides `#CC79A7` purple · M5 Agents `#D55E00` vermillion |
| Recurring components | **Prompt card** (monospace, light background, "copy from repo" tag) · **Before / After** split · **Loop badge** (Spec / Generate / Verify icon in the corner, with the current step highlighted) · **⚠️ Policy box** · **Takeaway slide** at the end of each module |
| Images | Your own demo screenshots and figures. No stock photos, no AI clip art |
| Footer | `github.com/<you>/GenAI_Tips` + "as of Oct 2026" |
| Density | One message per slide. If you need two, make two slides |

**Rule for AI-generated decks (practice what you teach):**
1. Write the outline below yourself.
2. Have AI generate slides into your template.
3. Render → screenshot → critique (prompt P6).
4. Check every number by hand.

## Part B: Slide-by-slide (≈ 40 slides for 90 min)

Legend: 🎬 live demo · 🖼 visual evidence · 📋 prompt card · ⚠️ caution/policy

### Opening (slides 1–5)

| # | Title (assertion) | Evidence / visual | Notes |
|---|---|---|---|
| 1 | **Effectively Using AI for Research** | Title, name, date, QR to repo | — |
| 2 | Accepted papers at top venues already contain AI-fabricated citations | 🖼 Headline: 100+ hallucinated citations in ~51 accepted NeurIPS 2025 papers; ICLR 2026 desk rejections | Hook: "These passed 3+ reviewers." |
| 3 | Can you spot the fake reference? | 🖼 4 references, 1 fabricated (real authors, well-formed DOI) | Show of hands → reveal with a doi.org lookup |
| 4 | Every good AI workflow follows the same loop: Spec → Generate → Verify | 🖼 Loop diagram + the 5-module table from `plan.md` | The spine of the talk; refer back to it in every module |
| 5 | Today: five places where AI changes research work | 🖼 5 module icons in their accent colors | Agenda |

### Module 1: Research (slides 6–12) · blue

| # | Title | Evidence / visual | Notes |
|---|---|---|---|
| 6 | Four kinds of AI research tools fail in four different ways | 🖼 Four-quadrant map: graph discovery / scholarly search / deep research / grounded reading | `guide/01-research.md` §1 |
| 7 | Chatbots invent 18–29% of the references they cite from memory | 🖼 Bar chart: GPT-3.5 55%, GPT-4 18%, GPT-4o 20% (28–29% on niche topics) | Sources: Walters & Wilder 2023; JMIR 2025 |
| 8 | Ask the AI for search strategies, not references | 🎬 Asta/Elicit search → DOI check → scite | ≤ 4 min; backup video |
| 9 | An evidence matrix turns 30 PDFs into one comparable table | 📋 Matrix prompt + 🖼 output screenshot | "Write 'not reported' rather than guessing" |
| 10 | Let the AI ask you questions before it gives you ideas | 📋 Socratic ideation prompt | Mention the Ideation–Execution Gap study |
| 11 | Steelman, then red-team: let AI attack your idea before reviewers do | 📋 Steelman/red-team prompt | — |
| 12 | **Takeaway: graphs and indexes find papers; LLMs read them; you verify every reference** | ⚠️ Never upload others' manuscripts or grants (NIH, NSF, ICML's 497 desk rejections) | — |

### Module 2: Writing (slides 13–18) · green

| # | Title | Evidence / visual | Notes |
|---|---|---|---|
| 13 | AI should edit your writing, not author it | 🖼 Loop: outline (you) → draft (you) → constrained edit (AI) → diff (you) | — |
| 14 | A constrained edit prompt keeps your meaning and your voice | 📋 Template T1 | The workhorse prompt |
| 15 | Demand a diff and a reason for every change | 🖼 Before/after split + change table | 🎬 optional quick demo |
| 16 | AI leaves fingerprints: "delves" is up 28× in PubMed abstracts | 🖼 Word-frequency bars (Kobak et al. 2025) + banned-word list | Detectors flag 61% of non-native essays, so write clearly instead of worrying about detectors |
| 17 | Reverse outline, Reviewer 2, and the fresh-reader test catch what polish can't | 📋 T2 + T5, side by side | — |
| 18 | **Takeaway: AI edits, you author. Constrain → diff → verify → disclose** | ⚠️ Disclosure table (NeurIPS / ICLR / ACL / Elsevier / Springer / IEEE) + one-line disclosure sentence | — |

### Module 3: Figures ⭐ (slides 19–29) · orange

| # | Title | Evidence / visual | Notes |
|---|---|---|---|
| 19 | There are four routes to an AI figure, trading looks against control | 🖼 The 4-route table (looks / accuracy / editability) | `guide/03-figures.md` §0 |
| 20 | A vague prompt gives a pretty but wrong figure | 🎬/🖼 One-line prompt result: wrong arrows, garbled text | Fail on purpose |
| 21 | Write a spec, not a vibe: the 9-part checklist | 🖼 Checklist: canvas → grid → components → edges → labels → style → palette → font → "do NOT add" | — |
| 22 | The same idea with a structured prompt and no text | 📋 Example 1 + 🖼 result | "Add labels later" |
| 23 | JSON specs make figure prompts reproducible and diff-able | 📋 Example 2 (JSON) + 🖼 result | — |
| 24 | PaperBanana: planning before drawing beats direct prompting by 17% | 🖼 5-agent pipeline (Retriever → Planner → Stylist → Visualizer → Critic) | The remaining failure is wrong arrows, so you check edges |
| 25 | Lock the layout with a sketch, then restyle region by region | 🖼 Hand sketch → styled output → one region edit | — |
| 26 | Code gives exact, editable figures: SVG, draw.io, TikZ | 📋 Example 3 + 🎬 Claude → SVG/draw.io opened in diagrams.net | Mention next-ai-draw-io and the drawio MCP |
| 27 | Pixels can become editable: layering, segmentation, vectorizing | 🖼 Raster → Qwen-Image-Layered / Edit-Banana → draw.io → live text | — |
| 28 | ⚠️ Nature and Science ban generative-AI images; Elsevier bans AI graphical abstracts | ⚠️ Policy table | Code-drawn figures are generally fine, but disclose |
| 29 | **Takeaway: spec first; code for structure, image models for style, never pixels for data** | 🖼 Recommended pipeline graphic | — |

### Module 4: Slides & posters (slides 30–34) · purple

| # | Title | Evidence / visual | Notes |
|---|---|---|---|
| 30 | Students can't tell AI slides from instructor slides, but rate suspected-AI slides lower | 🖼 Study result (arXiv 2605.13532) | Generic look costs you |
| 31 | Assertion–evidence slides beat topic + bullets | 🖼 Before (topic + 6 bullets) / after (claim + figure), using a slide from *this* deck | Michael Alley |
| 32 | Write the story yourself; let AI build the slides into your template | 🖼 6-step workflow + tool table (Claude pptx / PowerPoint add-in / Copilot / Gemini / NotebookLM / Marp) | Meta moment: "this deck was made this way" |
| 33 | AI can't see its own layout bugs, so render, screenshot, and critique | 📋 P6 critique prompt + P4 Q&A prediction | — |
| 34 | **Takeaway: write the story, AI builds, you judge.** Posters: #betterposter + Paper2Poster | 🖼 #betterposter layout | — |

### Module 5: Agents & skills (slides 35–41) · vermillion

| # | Title | Evidence / visual | Notes |
|---|---|---|---|
| 35 | An agent is just a model calling tools in a loop | 🖼 Loop diagram: model ↔ tools ↔ environment | — |
| 36 | The harness around the model matters as much as the model | 🖼 Anatomy diagram: memory file, skills, subagents, hooks, MCP, permissions | Which-mechanism table |
| 37 | Context is a budget: give the agent just the right information | 🖼 "Context rot" sketch + 4 tactics (just-in-time, compaction, progress file, subagents) | Karpathy quote |
| 38 | Give the agent a way to check its work | 🖼 Explore → Plan → Code → Verify → Commit; autoresearch example (one file, one metric, git) | — |
| 39 | A Skill is a folder that teaches an agent your workflow | 🖼 `skills/paper-figure/SKILL.md`, annotated: description = trigger; checklist; rules | 🎬 Claude Code runs the skill on the demo paper |
| 40 | Five rules for safe research agents | 🖼 Plan first · verifiable goals · separate reviewer · git + read-only raw data · avoid the lethal trifecta | — |
| 41 | **Takeaway: design the harness, not just the prompt** | Top repos: anthropics/skills, K-Dense scientific skills, superpowers, awesome-claude-code | — |

### Closing (slides 42–44)

| # | Title | Evidence / visual | Notes |
|---|---|---|---|
| 42 | Five takeaways on one slide | The five takeaway lines + the loop | Photo slide for attendees |
| 43 | Before you use AI: confidentiality, disclosure, responsibility | ⚠️ Combined policy cheat sheet | — |
| 44 | Everything is in the repo | Big QR code + repo link + "Questions?" | — |

## Part C: How to build this deck with AI (and demo the process)

1. Paste this outline into your slide tool. Prompt:
   > *"Build slides 1–44 in [my template]. Use each title exactly as written. Put the evidence item as the main visual and leave placeholders for screenshots. Speaker notes from the Notes column. ≤ 3 bullets per slide."*
2. Insert your real demo screenshots and figures.
3. Export every slide to PNG → run the critique prompt P6 → fix.
4. Rehearse → transcript → prompt P4 for Q&A prep.
