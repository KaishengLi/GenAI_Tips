---
name: lit-review
description: Runs a verified literature search and synthesis on a research question - plans sub-questions, searches scholarly sources in parallel, builds an evidence matrix, and checks every reference against DOI/arXiv before reporting. Use when the user asks for a literature review, related-work section, "what's known about X", a reading list, or to find papers on a topic, even if they just say "find papers on...".
---

# Literature review (verified)

The goal is a synthesis the user can trust. **A reference that was not opened and verified must not appear in the output.**

## Workflow (copy this checklist into your progress notes and check items off)

- [ ] 1. **Scope.** Restate the question. Ask the user for: field, time window, inclusion/exclusion criteria, and how many papers they want. If they don't answer, default to the last 10 years and 15–30 core papers.
- [ ] 2. **Plan.** Split the question into 3–5 sub-questions. For each, write 2–3 search queries (keywords, synonyms, key authors).
- [ ] 3. **Search in parallel.** Run one subagent per sub-question if available. Use scholarly sources (Semantic Scholar API, arXiv, PubMed, Crossref, or MCP servers such as arxiv/zotero/scite) rather than model memory. Each subagent returns at most 10 candidates: title, authors, year, venue, DOI/arXiv ID, and a one-line reason it is relevant.
- [ ] 4. **Snowball.** For the 3–5 most central papers, check their references and the papers that cite them.
- [ ] 5. **Verify.** For every candidate, resolve the DOI (`https://doi.org/<doi>`) or arXiv ID and confirm that the title, authors, and year match. Drop anything that fails and record it under "Rejected (could not verify)".
- [ ] 6. **Extract.** Fill in `evidence_matrix.md` with one row per paper:
  `citation | question | design/sample | key finding (with effect size) | limitations | page/section`.
  Write "not reported" rather than guessing.
- [ ] 7. **Synthesize.** Cluster the papers by approach. Report **agreements, contradictions, and gaps**. Every sentence that makes a claim must cite at least one verified paper from the matrix.
- [ ] 8. **Report.** Produce `lit_review.md` (the synthesis), `evidence_matrix.md`, and `references.bib`, and list the rejected and unverified items.

## Rules

- Never invent a citation, DOI, page number, or effect size.
- If a source is paywalled and you can only see the abstract, mark the row `[abstract only]`.
- Prefer primary studies over reviews for specific claims. Use reviews to orient.
- Say explicitly where coverage is weak: non-English, paywalled, very recent, or outside STEM.
- Do not paste unpublished or confidential user material into external web tools.

## Output example (one matrix row)

| Citation | Question | Design / sample | Key finding | Limitations | Loc. |
|---|---|---|---|---|---|
| Walters & Wilder 2023, *Sci Rep*, doi:10.1038/s41598-023-41032-5 | How often do ChatGPT citations exist? | 636 citations in 84 generated literature reviews (42 topics) | 55% (GPT-3.5) and 18% (GPT-4) fabricated | Two model versions only; 2023 | Results |
