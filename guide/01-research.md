# 1. AI for the Research Process: Discover, Read, Ideate, Critique

> **Principle:** *Discover* with tools built on scholarly indexes. *Reason* with LLMs over the actual PDFs. **Never use a chatbot's memory as a bibliography.**

---

## 1. Pick the right tool for the job

Most frustration comes from using the wrong class of tool. There are four classes:

| Class | Use it for | Tools | Hallucination risk |
|---|---|---|---|
| **A. Citation-graph discovery** | "What's around this seed paper?" | [ResearchRabbit](https://www.researchrabbit.ai/), [Connected Papers](https://www.connectedpapers.com/), [Litmaps](https://www.litmaps.com/) | None (no text generation) |
| **B. Scholarly search & synthesis** | "Find papers that…", screening, extraction tables | [Ai2 Asta](https://allenai.org/asta) (Paper Finder, free), [Undermind](https://www.undermind.ai/) (slow, exhaustive), [Elicit](https://elicit.com/) (systematic reviews, extraction), [Consensus](https://consensus.app/) ("what does the evidence say?"), [scite](https://scite.ai/) (supporting vs. contrasting citations), [Semantic Scholar](https://www.semanticscholar.org/) | Low: the papers are real, but the summaries can be wrong |
| **C. General Deep Research agents** | Broad landscape scans, cross-field background | ChatGPT Deep Research, Gemini Deep Research, Claude Research | Medium: sources can be misattributed |
| **D. Grounded reading over *your* PDFs** | Reading, Q&A, comparison across papers | [NotebookLM](https://notebooklm.google/), Claude/ChatGPT Projects, [paper-qa](https://github.com/Future-House/paper-qa), [alphaXiv](https://www.alphaxiv.org/) | Low: answers come only from your sources |

**Talk tip:** run the same question through two Deep Research tools and diff the reference lists. Where they disagree, the result needs a closer look.

### Reference managers + AI
- Zotero 8/9 has **no native AI**. Popular plugins and connectors: [zotero-gpt](https://github.com/MuiseDestiny/zotero-gpt), [zotero-mcp](https://github.com/54yyyu/zotero-mcp) (lets Claude/ChatGPT/Cursor query your library), [zotero-arxiv-daily](https://github.com/TideDra/zotero-arxiv-daily) (daily arXiv recommendations based on your library), and [zotero-pdf-translate](https://github.com/windingwind/zotero-pdf-translate).

### The frontier: "AI scientist" systems (show these, don't depend on them yet)
- **Google AI co-scientist** (multi-agent hypothesis generation; access via trusted testers), **Edison Scientific Kosmos** (vendor claims are not independently verified), and **Sakana AI Scientist** (a workshop paper passed review; reviewers still found citation errors).
- Evidence on AI ideas: LLM ideas were rated *more novel* than expert ideas ([Si et al. 2024](https://arxiv.org/abs/2409.04109)). But once executed, their scores dropped significantly more than human ideas' scores did ([Ideation–Execution Gap, ICLR 2026](https://arxiv.org/abs/2506.20803)).
  **Use AI to widen the funnel; use your judgment to narrow it.**

---

## 2. Workflows

### 2.1 The anti-hallucination citation workflow
1. Ask the LLM for **search strategies and keywords**, not references.
2. Get the actual papers from **Asta / Undermind / Elicit / Scopus / PubMed / Google Scholar**.
3. **Resolve every reference by DOI** (doi.org or the Crossref API). Open the paper and check that it actually says what you are citing it for. Fake references often have real author names and well-formed DOIs.
4. Use **scite** to check whether later work supports or contrasts each key claim.
5. Run a citation checker before submitting.

**Why bother?**
- GPT-4 fabricated **18%** of its citations in one study ([Walters & Wilder, *Sci Rep* 2023](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC10484980/)).
- GPT-4o fabricated **~20%**, and **45%** of its *real* references contained errors. Fabrication rose to **28–29% on niche topics** ([*JMIR Ment Health* 2025](https://mental.jmir.org/2025/1/e80371)).
- Accepted NeurIPS 2025 papers contained 100+ hallucinated citations ([Fortune](https://fortune.com/2026/01/21/neurips-ai-conferences-research-papers-hallucinations/)).

### 2.2 Literature-review matrix
```text
For each attached paper, fill a table:
citation | research question | design/sample (N) | key measures |
main finding with effect size | limitations stated by authors |
limitations you infer | page number for each extracted fact.
Write "not reported" rather than guessing.
```
Then ask: *"Cluster these papers by approach. Where do findings contradict each other? What question has nobody asked?"*

### 2.3 Socratic ideation (AI asks, you think)
```text
Don't give me ideas yet. Ask me one question at a time (max 8) to sharpen my
research question about [topic]: the mechanism, the population, what would
surprise me, and what result would change practice.
Then propose 3 testable hypotheses, each with the predicted direction and a
result that would falsify it.
```

### 2.4 Steelman → red-team an idea
```text
Here is my research idea and planned design: [...].
1. Steelman it: the strongest case for why it matters.
2. Then act as a skeptical expert in [field] and give the 5 most likely reasons
   it fails (confounds, measurement validity, prior work that already did it,
   feasibility, alternative explanations).
3. For each, propose a concrete check or design change. Do not soften.
```

### 2.5 Reading a paper with AI
```text
Explain this paper's core contribution in 3 sentences for a [field] PhD student.
List its assumptions. Reconstruct the argument of Figure 2.
What experiment is missing?
```
Follow up with **"Which page supports that?"** to force the answer back to the text.

### 2.6 Simulated Reviewer 2 (your own draft only)
```text
You are a demanding reviewer for [venue]. Review my attached draft:
summary, major concerns (novelty vs. specific prior work, statistical validity,
overclaiming), minor issues, and a score with justification.
Quote the exact sentences you object to.
```

### 2.7 Agentic literature review (if you use Claude Code / Codex)
- Use parallel subagents, each with **one** sub-question.
- Every claim must cite a source the agent actually opened.
- A final pass verifies DOIs/arXiv IDs.
- See our example skill: [`skills/lit-review/SKILL.md`](../skills/lit-review/SKILL.md).

---

## 3. GitHub resources (stars approx., Oct 2026)

| Repo | ★ | What it is |
|---|---|---|
| [K-Dense-AI/scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills) | ~48k | 170+ science skills plus 100+ database connectors for Claude Code / Codex / Cursor |
| [Imbad0202/academic-research-skills](https://github.com/Imbad0202/academic-research-skills) | ~51k | Claude Code skills: research → write → review → revise |
| [lfnovo/open-notebook](https://github.com/lfnovo/open-notebook) | ~40k | Self-hostable NotebookLM alternative (good for confidential material) |
| [stanford-oval/storm](https://github.com/stanford-oval/storm) | ~32k | Multi-perspective research → Wikipedia-style cited report |
| [assafelovic/gpt-researcher](https://github.com/assafelovic/gpt-researcher) | ~30k | Open deep-research agent, any LLM, MCP server |
| [kaixindelele/ChatPaper](https://github.com/kaixindelele/ChatPaper) | ~20k | arXiv summarization / translation / review / rebuttal |
| [SakanaAI/AI-Scientist](https://github.com/SakanaAI/AI-Scientist) | ~15k | End-to-end automated ML research (study it, don't trust it) |
| [Future-House/paper-qa](https://github.com/Future-House/paper-qa) | ~9k | High-accuracy RAG over your PDFs, with citations |
| [LearningCircuit/local-deep-research](https://github.com/LearningCircuit/local-deep-research) | ~9k | Fully local and private deep research |
| [54yyyu/zotero-mcp](https://github.com/54yyyu/zotero-mcp) | ~5k | Connects your Zotero library to Claude and other assistants |
| [blazickjp/arxiv-mcp-server](https://github.com/blazickjp/arxiv-mcp-server) | ~3k | arXiv search / reading / BibTeX for agents |
| [AkariAsai/OpenScholar](https://github.com/AkariAsai/OpenScholar) | ~2k | Open retrieval-augmented literature synthesis (UW/Ai2) |

---

## 4. Pitfalls and policies

- **Confidentiality.** Consumer chat tools may log or train on what you paste. Do not paste unpublished data, IRB-protected data, **someone else's manuscript or grant**, or proprietary material unless your institution has approved the tool. Local options: open-notebook, local-deep-research, paper-qa with local models.
- **Peer review is the strictest zone.**
  - **NIH** prohibits generative AI in peer review (NOT-OD-23-149).
  - **NSF** bars uploading proposals to non-approved tools ([NSF](https://www.nsf.gov/policies/ai/merit-review)).
  - **Springer Nature** forbids uploading manuscripts under review.
  - **ICML 2026** desk-rejected **497 papers** whose reciprocal reviewers broke its LLM policy.
- **Grant writing.** NIH (NOT-OD-25-132) treats applications "substantially developed by AI" as not the applicant's original work.
- **Coverage bias.** These tools skew toward open-access and STEM papers. Paywalled papers, books, and non-English work are underrepresented.
- **Over-smoothing.** AI synthesis tends to hide contradictory findings, so ask for disagreements explicitly.
- **Vendor accuracy claims.** Elicit claims ~97% screening sensitivity, but an independent test found much lower. Never use one tool as your only screener.

---

## Workshop takeaway (one slide)

> **"Graphs and indexes find papers. LLMs read papers. You verify every reference."**
