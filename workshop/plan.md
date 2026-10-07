# Workshop Plan: *Effectively Using AI for Research*

## 1. The narrative spine

One mental model runs through all five modules, so the workshop feels like one talk rather than five tool tours:

> ### **Spec → Generate → Verify**
> *You* decide what is wanted (the spec). AI produces (generate). *You* check it against reality (verify).

| Module | Spec (you) | Generate (AI) | Verify (you + tools) |
|---|---|---|---|
| 1. Research | Research question, search strategy | Search, summaries, evidence matrix, critiques | Resolve every DOI, open the paper, check with scite |
| 2. Writing | Outline, claims, style guide | Constrained edits | Diff + reasons, reverse outline, fresh-reader test |
| 3. Figures | Figure spec (components, edges, layout) | Image / SVG / draw.io / code | Compare the render to the spec, vectorize, check journal policy |
| 4. Slides & posters | One-sentence message, assertion titles | Slides in your template, notes | Render → critique, check every number |
| 5. Agents & skills | `CLAUDE.md`, `SPEC.md`, skills | Multi-step work | Tests, separate reviewer, git |

Close with: **"AI scales whatever you give it. Give it a good spec and a way to check."**

## 2. Audience and goals

- **Audience:** grad students and researchers from mixed fields. Some code, some don't. Many are non-native English speakers.
- **By the end, every attendee should be able to:**
  1. Run a literature search that produces **zero fake citations**.
  2. Use 3 reusable prompt templates for editing their own writing.
  3. Turn a method paragraph into a figure spec, and choose the right rendering route.
  4. Generate an outline with assertion titles for their next talk.
  5. Explain what a harness, `CLAUDE.md`, and a Skill are, and know where to start.

## 3. Format and timing (90 min; 60-min version in brackets)

| Time | Segment | Format | 60-min version |
|---|---|---|---|
| 0:00–0:07 | **Hook + framework.** "Spot the fake citation" quiz; the Spec→Generate→Verify loop | Poll / show of hands | 5 min |
| 0:07–0:22 | **Module 1: Research.** Four tool classes, anti-hallucination workflow, Socratic ideation | Talk + **live demo** (Asta/Elicit + DOI check) | 10 min |
| 0:22–0:35 | **Module 2: Writing.** Constrained edit, diff, reverse outline, AI-ese, policies | Talk + **before/after demo** | 8 min |
| 0:35–0:58 | **Module 3: Figures** ⭐ (longest). Four routes; spec-first; three prompt styles; image → editable; journal policies | Talk + **live demo**: same method → (a) raw prompt, (b) spec → image, (c) SVG/draw.io | 15 min |
| 0:58–1:08 | **Module 4: Slides & posters.** Assertion–evidence; AI outline → template; QA; Q&A prediction | Talk + quick demo (this deck was made this way) | 7 min |
| 1:08–1:22 | **Module 5: Agents & skills.** Harness anatomy, context engineering, 14 tips, a Skill walkthrough | Talk + **live Claude Code demo** using `skills/paper-figure` | 10 min |
| 1:22–1:30 | **Wrap-up.** Policy cheat sheet, one-slide summary, repo QR, Q&A | | 5 min |

**Optional 30-min hands-on lab** (makes it a 2-hour session). Attendees bring one paragraph of their own method section and:
1. Run template T1 (constrained edit) on it.
2. Turn it into a figure spec and render it in one tool.
3. Write 5 assertion titles for a talk about it.

## 4. Preparation timeline

| When | Task |
|---|---|
| **T-3 weeks** | Confirm the room, duration, A/V, and Wi-Fi. Send a **pre-survey** (field, tools already used, biggest pain point, any institution-approved AI accounts). |
| **T-2 weeks** | Finalize the storyline and slide titles (use `slide-outline.md`). Choose **one open-access demo paper** that is used in every module so the demos connect. Check your university's approved-AI-tools list. |
| **T-10 days** | Build the demo assets: the figure spec, generated images, SVG/draw.io files, and the before/after writing example. **Re-check model names and policies** (they change monthly). |
| **T-1 week** | **Record backup videos of every live demo** (2–3 min each) in case Wi-Fi or a model fails. Dry run #1 with a colleague; time each module. |
| **T-3 days** | Dry run #2. Use AI to predict Q&A (prompt P4 in `guide/04-slides-posters.md`). Freeze the slides. Put the repo link and QR code on the first and last slides. |
| **Day-of** | Log into every tool in advance and open the tabs in demo order. Have the backup videos on the desktop. Use large fonts (≥ 24 pt in the browser, zoom the terminal). Have pre-made prompts ready to copy from a text file. |
| **After** | Share the slides + repo. Send a 3-question feedback form. Add the questions you got to `guide/`. |

## 5. Demo design rules

- **One paper through the whole workshop.** For example, an open-access methods paper from a field most attendees understand. Module 1 finds related work for it, Module 2 polishes its abstract, Module 3 draws its method figure, Module 4 turns it into 5 slides, and Module 5 has an agent do the figure end-to-end with a skill.
- **Show failures on purpose.**
  - A hallucinated citation, caught live with a DOI lookup.
  - An image model drawing a wrong arrow and garbled text.
  - An AI deck with 6 bullets per slide.

  Failures teach the "Verify" step better than successes.
- **Every live demo has:** a pre-typed prompt, an expected result, a time box (≤ 4 min), and a recorded backup.
- **Show prompts large.** Show the prompt, then the output, side by side.

## 6. Handouts

- QR code → this repo (`guide/` has every prompt as copy-paste text).
- Optional 1-page cheat sheet: the Spec→Generate→Verify table, the five takeaways, the policy table, and the top 10 links.

## 7. Risks and mitigations

| Risk | Mitigation |
|---|---|
| Wi-Fi or model outage | Recorded backup videos; screenshots in the slides |
| Mixed audience (coders vs. non-coders) | Module 5 opens with concepts first; Claude Code demo is short; chat-only alternatives are mentioned everywhere |
| "Is this allowed?" questions | Policy slides in Modules 2 and 3 + the wrap-up cheat sheet; point people to the venue's current CFP |
| Facts going out of date | Date the slides ("as of Oct 2026"); keep the repo updated |
| Privacy concerns | Repeat the rule: no unpublished / IRB / others' manuscripts in non-approved tools |
