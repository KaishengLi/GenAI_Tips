# GenAI Tips: Effectively Using AI for Research

Workshop materials and a practical guide for grad students and researchers. Every prompt is copy-paste ready.

> **One loop runs through everything: Spec → Generate → Verify.**
> You decide what's wanted; the AI produces; you check it against reality.

## Guide

| # | Module | Takeaway |
|---|---|---|
| 1 | [Research: discover, read, ideate, critique](guide/01-research.md) | Graphs and indexes find papers. LLMs read them. You verify every reference. |
| 2 | [Writing better (without ghostwriting)](guide/02-writing.md) | AI edits, you author: constrain → diff → verify → disclose. |
| 3 | [Academic figures and illustrations](guide/03-figures.md) ⭐ | Spec first. Code for structure, image models for style, never pixels for data. |
| 4 | [Slides and posters](guide/04-slides-posters.md) | Write the story; AI builds the slides; you judge them. |
| 5 | [Agents, harnesses, and skills](guide/05-agents-skills.md) | Design the harness, not just the prompt. |

## Workshop

- [`workshop/plan.md`](workshop/plan.md): the storyline, learning goals, 90/60-minute run-of-show, preparation timeline, demo design, and risks.
- [`workshop/slide-outline.md`](workshop/slide-outline.md): the design system, plus a slide-by-slide outline (assertion titles, evidence, and speaker notes).

## Example Agent Skills

These skills use the open [Agent Skills](https://agentskills.io) format and work in Claude Code, Codex, Gemini CLI, Cursor, and others. Copy a folder into `.claude/skills/` (or `.agents/skills/`):

- [`skills/lit-review`](skills/lit-review/SKILL.md): a verified literature search with an evidence matrix and DOI checks.
- [`skills/paper-figure`](skills/paper-figure/SKILL.md): spec-first figures, as reproducible plots and editable SVG/draw.io.
- [`skills/paper-polish`](skills/paper-polish/SKILL.md): minimal, explained edits and whole-manuscript consistency checks.

## Ground rules

1. **Never trust a reference you haven't resolved and opened.**
2. **Never paste unpublished data or someone else's manuscript or grant into a tool your institution hasn't approved.**
3. **Disclose AI use according to your venue's current policy.** You are responsible for every word, number, and figure.
4. **Check the journal's policy before using AI-generated images.** Nature and Science prohibit them.

*Tools, model names, star counts, and policies were checked in October 2026. They change quickly, so verify before relying on them.*
