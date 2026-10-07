# Slide Outline and Design Guide

The deck is framed as **tip sharing**, not a lecture. There are 27 numbered tips, each a practical move people can try the next day. Speaker notes are written in the first person and include `[Your story: …]` placeholders for the presenter's own experience. Background studies and policy detail are left to the guides in `guide/`, not the slides.

## Part A: Design system

| Element | Spec |
|---|---|
| Format | 16:9. Every title is a short, imperative tip or a claim ("Ask for search keywords, never for references"). One idea per slide. |
| Eyebrow | `Tip NN · Area` in the module's accent color |
| Fonts | Source Serif 4 (titles), IBM Plex Sans (body), IBM Plex Mono (prompts). Titles 56 px; body ≥ 24 px |
| Colors | Paper `#F6F4EF`, ink `#1B2430`. One Okabe–Ito accent per module: Research `#0072B2` · Writing `#009E73` · Figures `#E69F00` · Slides `#CC79A7` · Agents `#D55E00` |
| Recurring components | **Prompt card**: dark, monospace, key line in orange. **Before / after** split. **Numbered step rows.** **Recap slide**: "My … setup / workflow" at the end of each module. |
| Placeholders | Dashed boxes mark where a live demo or screenshot goes; `[…]` marks the presenter's own content |
| Footer | Repo link, or a one-line extra tip |

## Part B: Slide-by-slide (44 slides, about 90 min)

### Intro
| # | Slide | Content |
|---|---|---|
| 1 | Cover | "27 practical tips from daily use" |
| 2 | Warm-up | Spot the made-up reference (3 real + 1 a chatbot made up); reveal with doi.org |
| 3 | The habit behind every tip | Spec → Generate → Check. "When the output is off, rewrite the prompt." |
| 4 | Today | 5 areas, with tip ranges |

### Research (blue)
| # | Tip | Content |
|---|---|---|
| 5 | Divider | |
| 6 | **01** Pick the tool by the job | Map / find / scan / read, with tools for each |
| 7 | **02** Ask for search keywords, never for references | 4-step flow + live demo (Asta → DOI → scite) |
| 8 | **03** Turn a pile of PDFs into one table first | Evidence-matrix prompt |
| 9 | **04** Let AI interview you first | Socratic prompt; "works best in voice mode on a walk" |
| 10 | **05** Make AI argue against your idea | Steelman + red-team prompt, 5 attack angles |
| 11 | My research stack | Recap: map / search / read / critique tools |

### Writing (green)
| # | Tip | Content |
|---|---|---|
| 12 | Divider | |
| 13 | **06** You write the messy draft; AI cleans it up one paragraph at a time | Outline → draft → constrained edit → review; style guide |
| 14 | **07** The editing prompt I reuse every day | Template T1 |
| 15 | **08** Always ask for the change table | Before/after + change table |
| 16 | **09** Ban the AI words | Banned-word chips; "delves" ×28 |
| 17 | **10** Four quick checks before submitting | Reverse outline, Reviewer 2, fresh reader, consistency |
| 18 | My writing setup | Recap + ready-to-paste disclosure line |

### Figures (orange)
| # | Tip | Content |
|---|---|---|
| 19 | Divider | |
| 20 | Four routes, looks vs. control | Route table + practical pipeline |
| 21 | **11** Don't trust a one-line prompt | Live failure; the room spots the errors |
| 22 | **12** Fill in nine fields before you generate | Spec checklist |
| 23 | **13** Leave all text out of the image | Structured prompt + result |
| 24 | **14** Write the spec as JSON | JSON prompt + why |
| 25 | **15** Plan, draw, critique: the PaperBanana loop by hand | Pipeline + 3 copy-paste prompts |
| 26 | **16** Lock the layout with a sketch, fix one region at a time | 3 panels: sketch → styled → region edit |
| 27 | **17** For anything you will revise, ask for SVG or draw.io | SVG/draw.io prompt + formats |
| 28 | **18** Make a nice AI image editable | 5 steps + tool table |
| 29 | My figure workflow | Recap pipeline |

### Slides & posters (purple)
| # | Tip | Content |
|---|---|---|
| 30 | Divider | |
| 31 | **19** Make every title the claim, not the topic | Before/after mini-slides + prompt |
| 32 | **20** Write the story; AI builds it in your template | 5 steps + "when I need… I reach for" table |
| 33 | **21** Screenshot for critique, rehearse Q&A | Two prompt cards |
| 34 | **22** Posters: one big sentence in the middle | #betterposter mock + Paper2Poster + A4 test print |

### Agents (vermillion)
| # | Tip | Content |
|---|---|---|
| 35 | Divider | |
| 36 | Agents in 30 seconds | Model ↔ tools loop + what you configure |
| 37 | **23** Write a short CLAUDE.md | Example file + 3 rules |
| 38 | **24** Keep the context lean | /clear, subagents, progress file, restart after two failures |
| 39 | **25** Give the agent a way to check its work | Explore → … → Commit; checkable goals; autoresearch |
| 40 | **26** Pasted it three times? Make it a Skill | SKILL.md snippet + live demo of `skills/paper-figure` |
| 41 | **27** Git is your undo button | 5 safety habits |
| 42 | Repos worth starring | 9 repos |

### Close
| # | Slide | Content |
|---|---|---|
| 43 | If you only try five things | One line per area |
| 44 | What's your best AI trick? | QR + repo; open the floor |

## Part C: Before presenting

1. Fill in the `[Your story: …]` notes. Personal stories are what make tip sharing land.
2. Replace the dashed placeholders with screenshots from the demo paper.
3. Put a QR code on slide 44 and add your name and affiliation to slides 1 and 44.
4. Re-check tool and model names the week of the talk.
