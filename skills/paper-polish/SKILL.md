---
name: paper-polish
description: Polishes author-written academic text with minimal, explained edits that preserve meaning, terminology, citations, math, and the author's voice; also runs whole-manuscript consistency checks on LaTeX sources. Use when the user asks to polish, proofread, edit, improve the English of, tighten, or check consistency of a paper, abstract, section, or rebuttal - not when they ask you to write new content from scratch.
---

# Paper polish (minimal edits, author keeps control)

## Before editing

1. Look for a style guide in `STYLE.md`, `CLAUDE.md`, or the project instructions: terminology, notation, US/UK spelling, venue, and a voice sample. If there isn't one, ask for the venue and any must-keep terms.
2. Make sure the work is committed (or a copy saved) so a diff is possible.

## Paragraph or section polish

Apply these rules:

- Do **not** change technical content, claims, numbers, citations (`\cite{}`), math, labels, or LaTeX commands.
- Do **not** add new ideas, examples, or hedges. Do not make claims vaguer.
- **Use minimal edits.** If a sentence is already clear and correct, leave it unchanged. Do not edit for the sake of editing.
- Keep the author's sentence order and voice unless a sentence is genuinely unclear.
- Avoid AI-typical vocabulary: delve, intricate, meticulous, showcase, underscore, pivotal, notably, comprehensive, crucial, leverage (as a verb), realm, tapestry. Avoid em-dashes.

Output, in this order:

1. The revised text.
2. A change table with the columns `Original | Revised | Reason`, one row per change.
3. Any ambiguous sentences, as questions to the author.

## Whole-manuscript consistency check (report only, no edits)

Report each issue with its `file:line`:

- Terms or abbreviations that are defined twice, used before their definition, or never defined.
- Notation conflicts: the same symbol with two meanings.
- Numbers in the text that disagree with tables or figures.
- Contributions claimed in the abstract or intro that are not supported in the results.
- Inconsistent tense or person.
- Suspicious `\ref` or `\cite` keys.

## After editing

- Show `git diff --word-diff` (or suggest `latexdiff`) so the author reviews every change.
- If the venue requires disclosure, offer this sentence: *"We used [tool] to improve the grammar and clarity of author-written text; all content was reviewed by the authors."*
