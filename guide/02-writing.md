# 2. Writing Better with AI (Without Ghostwriting)

> **Principle:** *You* own the ideas, claims, structure, and voice. The AI is your **editor, critic, and checker** — not your author.

This is also the line most publishers draw: language polishing is generally fine (often no disclosure needed); AI-*generated* content must be disclosed or is restricted. See [Policies](#5-disclosure-policies-check-your-venue).

---

## 1. The recommended loop

```
 Outline (you) ──► Rough draft (you) ──► Targeted AI edits ──► Reverse-outline check
       ▲                                                              │
       └──────── Fresh-reader test ◄── Verify every fact/citation ◄───┘
```

1. **Outline first, by hand.** Claims, contribution list, figure plan. Use AI only to stress-test it: *missing pieces? better argument order? likely reviewer objections?*
2. **You write the rough draft.** Bad English is fine. Non-native speakers can draft in their first language.
3. **Targeted revision, one paragraph/section at a time, with explicit constraints** (template T1 below).
4. **Reverse outline.** Ask the AI to summarize each paragraph in one sentence. If its summary ≠ what you meant, the *writing* is the problem.
5. **Demand a diff + reason for every edit.** Accept/reject yourself. In LaTeX use [`latexdiff`](https://www.overleaf.com/learn/latex/Articles/How_to_use_latexdiff_on_Overleaf); in git use `git diff --word-diff`.
6. **Keep a persistent style guide** (Claude/ChatGPT *Project* instructions or a `CLAUDE.md`): terminology, notation, banned words, venue, US/UK spelling, tense rules per section, and **one paragraph of your own writing** as a voice sample.
7. **Verify everything at the end** — every citation, number, and claim.
8. **Fresh-reader test.** Paste the draft into a *new* chat with no context: *"What does this paper claim? What is the evidence?"* (Anthropic's `doc-coauthoring` skill builds this "reader testing" step in.)

### Why step 7 is non-negotiable

- GPTZero found **100+ hallucinated citations across ~51 accepted NeurIPS 2025 papers** ([Fortune](https://fortune.com/2026/01/21/neurips-ai-conferences-research-papers-hallucinations/), [The Decoder](https://the-decoder.com/over-100-fake-citations-slip-through-peer-review-at-top-ai-conference/)).
- **ICLR 2026 desk-rejected papers with confirmed hallucinated references** ([ICLR blog](https://blog.iclr.cc/2026/03/31/a-retrospective-on-the)).

---

## 2. Reusable prompt templates

### T1 — Constrained paragraph edit (the workhorse)
```text
You are a copy editor for a [venue] paper in [field]. Edit the paragraph below
for clarity, concision, and grammar ONLY.

Rules:
1. Do NOT change technical content, claims, numbers, citations, math, or LaTeX commands.
2. Do NOT add new ideas or hedges.
3. Keep my terminology exactly: [list].
4. Minimal edits. If a sentence is already fine, leave it unchanged.
5. Avoid: delve, intricate, meticulous, showcase, underscore, pivotal, notably,
   comprehensive, crucial. No em-dashes.

Output:
(a) the revised paragraph;
(b) a table: Original → Revised → Reason, one row per change;
(c) any sentence whose meaning was ambiguous to you — as a question, not a guess.

[PARAGRAPH]
```

### T2 — Reverse outline & structure audit
```text
For each paragraph below give (1) a one-sentence summary of its main point and
(2) its role (motivation / gap / claim / evidence / transition).
Then flag: paragraphs with >1 main point, missing logical steps between
paragraphs, and contributions claimed in the intro that are not supported later.
Do NOT rewrite anything.
```

### T3 — Non-native English polish that teaches you
```text
I am a non-native English speaker. Polish this text into natural academic English
while preserving my meaning and sentence order. After the edited version, list the
5 most frequent error patterns in my writing, each with a short rule, so I can
avoid them next time.
```

### T4 — Whole-manuscript consistency checker (best in Claude Code / Cursor on the LaTeX source)
```text
Read the whole manuscript. Report — do not edit:
1. inconsistent terms/abbreviations (defined twice, used before definition, never defined);
2. notation conflicts (same symbol, two meanings);
3. numbers in text that disagree with tables/figures;
4. contributions in abstract/intro not backed by results;
5. tense/person inconsistencies;
6. suspicious \ref or \cite keys.
Give file:line locations.
```

### T5 — Hostile Reviewer 2
```text
Act as a skeptical Reviewer 2 at [venue]. Give the 3 weakest points of this paper
(novelty, baselines, claims vs. evidence, clarity), the questions you would ask,
and a score with justification. Cite section numbers. Do not praise.
```

### T6 — Response to reviewers (you supply the substance)
```text
Here are the reviews and my bullet notes on how I will address each (planned
experiments, disagreements, fixes).
1. Split reviews into atomic comments, numbered R1.1, R1.2, ...
2. For each, draft a polite, concise response based ONLY on my notes.
   Insert [TODO] wherever my notes don't cover a point. Never invent results.
3. Flag any response that may read as defensive.
```

### T7 — Title & abstract critique
```text
Score my abstract against: context → gap → approach → key quantitative result →
implication. Flag vague phrases ("significantly improves", "novel framework").
Then propose 5 titles in different styles (descriptive, claim-based, question).
Keep my numbers unchanged.
```

### T8 — Chores
```text
- Draft a 250-word cover letter to [journal] using ONLY these facts: [...].
- Fix this LaTeX compile error and explain the cause: [log + surrounding lines].
- Convert this CSV into a booktabs table; bold the best result per column.
```

> 💡 Borrowed from Chinese prompt collections: *"If the text is already natural, return it unchanged. Do not edit for the sake of editing."* — this single line kills most over-editing.

---

## 3. Tools

| Tool | Best for | Notes |
|---|---|---|
| **Claude / ChatGPT Projects** | Persistent style guide + venue guidelines + voice sample | Every chat inherits them |
| **Claude Code / Cursor / Codex on your LaTeX repo** | Whole-manuscript checks, template migration, BibTeX cleanup, build errors | Commit before each AI pass; review with `git diff` / `latexdiff`. Sync with Overleaf via Git bridge |
| **Overleaf AI Assist** (Writefull + Error Assist) | In-editor language suggestions, LaTeX error fixes, table/equation from image | Free tier has daily limits ([docs](https://docs.overleaf.com/integrations-and-add-ons/ai-features)) |
| **OpenAI Prism** (Jan 2026) | Free LaTeX-native workspace with GPT inside | [InfoQ](https://www.infoq.com/news/2026/01/openai-prism) |
| **Grammarly / Paperpal / Trinka** | Sentence-level grammar, Word/Overleaf add-ins | Grammarly "Authorship" can log typed vs. pasted vs. AI text |

**Privacy:** before pasting unpublished work, check whether the tool trains on your data and whether your institution has an approved enterprise account.

---

## 4. GitHub resources (stars approx., Oct 2026)

| Repo | ★ | What it is |
|---|---|---|
| [anthropics/skills](https://github.com/anthropics/skills) | ~180k | Official Agent Skills incl. `doc-coauthoring` (context → section-by-section → reader test) |
| [binary-husky/gpt_academic](https://github.com/binary-husky/gpt_academic) | ~71k | Web UI: one-click polishing, translation, full PDF/LaTeX paper translation |
| [blader/humanizer](https://github.com/blader/humanizer) | ~55k | Skill that removes "AI-writing tells" (Chinese port: [op7418/Humanizer-zh](https://github.com/op7418/Humanizer-zh)) |
| [K-Dense-AI/scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills) | ~48k | 170+ science skills incl. scientific writing |
| [Leey21/awesome-ai-research-writing](https://github.com/Leey21/awesome-ai-research-writing) | ~35k | High-quality prompt library from MSRA/ByteDance Seed/Shanghai AI Lab/PKU researchers: translation, polishing, condensing, "de-AI", logic check, reviewer sim, captions |
| [Orchestra-Research/AI-Research-SKILLs](https://github.com/Orchestra-Research/AI-Research-SKILLs) | ~13k | `ml-paper-writing` skill with venue templates + citation verification |
| [ahmetbersoz/chatgpt-prompts-for-academic-writing](https://github.com/ahmetbersoz/chatgpt-prompts-for-academic-writing) | ~5k | Classic section-by-section prompt list (generic; some prompts generate content — use selectively) |
| [zLanqing/codex-claude-academic-skills](https://github.com/zLanqing/codex-claude-academic-skills) | ~4.6k | Writing / polishing / rebuttal skills for Claude Code & Codex |
| [Adkid-Zephyr/anti-defensive-writing-Skill](https://github.com/Adkid-Zephyr/anti-defensive-writing-Skill) | ~2.3k | Skill against over-hedged "defensive" academic prose |

See also our own example skill: [`skills/paper-polish/SKILL.md`](../skills/paper-polish/SKILL.md).

---

## 5. Disclosure policies (check your venue's *current* CFP)

Everyone agrees on two things: **AI cannot be an author**, and **humans are responsible for all content.**

| Venue / publisher | Rule (summary) |
|---|---|
| **NeurIPS** | Describe LLM use in methods if important/non-standard. Grammar/editing need no documentation. Verify LLM-generated references ([policy](https://neurips.cc/Conferences/2025/LLM)) |
| **ICML 2026** | LLMs may assist writing/research; notable use encouraged to be explained. Hidden prompt injection aimed at LLM reviewers ⇒ desk reject ([CFP](https://icml.cc/Conferences/2026/CallForPapers)) |
| **ICLR 2026/2027** | Disclosure required; hallucinations = Code of Ethics violation. ICLR 2027 requires an AI-use section ([policy](https://iclr.cc/Conferences/2027/AIPolicyForAuthors)) |
| **ACL / ARR** | Responsible NLP checklist asks scope of AI writing assistance; polishing treated like Grammarly ([ARR](https://aclrollingreview.org/responsible-nlp-checklist-appendices)) |
| **Elsevier** | Separate AI declaration statement; basic grammar/spelling exempt ([policy](https://www.elsevier.com/about/policies-and-standards/generative-ai-policies-for-journals)) |
| **Springer Nature** | "AI-assisted copy editing" need not be declared; generative use must be documented in Methods ([guidance](https://group.springernature.com/gp/group/ai/ai-guidance-for-our-researchers-and-communities)) |
| **Science (AAAS)** | Disclose in cover letter + acknowledgments; details in Methods; AI images need editor permission |
| **IEEE** | AI-generated content disclosed in Acknowledgments; editing help "generally outside" but disclosure recommended ([guidelines](https://open.ieee.org/author-guidelines-for-artificial-intelligence-ai-generated-text/)) |

**Practical default:** when in doubt, add one line —
*"We used [tool, version] to improve the grammar and clarity of author-written text; all content was reviewed and verified by the authors."* Keep your prompts/drafts as a record.

---

## 6. AI detectors & "AI-ese"

- **Detectors are unreliable and biased.** Seven detectors misclassified **61% of non-native (TOEFL) essays** as AI-generated ([Liang et al., *Patterns* 2023](https://arxiv.org/abs/2304.02819)). Don't panic about detectors; write clearly in your own voice.
- **The vocabulary shift is measurable.** Across 15M+ PubMed abstracts, ≥13.5% of 2024 abstracts were LLM-processed; "delves" ×28, "underscores" ×13.8, "showcasing" ×10.7 ([Kobak et al., *Sci. Adv.* 2025](https://arxiv.org/html/2406.07016v5)). Also see [Liang et al. 2024](https://arxiv.org/abs/2404.01268) and [Juzek & Ward, COLING 2025](https://arxiv.org/abs/2412.11385).
- **Protect your voice:** voice sample in the style guide, banned-word list, minimal-edit instruction, read it aloud, reject edits that make claims vaguer or add hedges you didn't write.

---

## Workshop takeaway (one slide)

> **"AI edits, you author."** Constrain the edit → demand a diff → verify every fact → disclose.
