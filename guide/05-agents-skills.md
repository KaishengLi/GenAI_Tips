# 5. Agents, Harnesses, and Skills

> **Principle:** The model is the engine; the **harness** is the car around it. Most of the quality you get from an agent comes from the context, tools, guardrails, and verification you design around it.

---

## 1. Core concepts in one page

**Agent = model + tools + loop.** The model asks for an action (read a file, run code, search). The harness runs it and feeds the result back. Repeat until done.
> "Agents are typically just LLMs using tools based on environmental feedback in a loop." ([Anthropic, *Building effective agents*](https://www.anthropic.com/engineering/building-effective-agents))

**Harness = everything around the model.**

| Component | What it is | Research example |
|---|---|---|
| **System prompt** | Base instructions | Provided by the tool |
| **Tools** | Read/edit files, bash, search, web | Run your analysis script |
| **Memory file**: `CLAUDE.md` / `AGENTS.md` / `GEMINI.md` | Always-loaded project rules | "Data lives in `data/raw` (read-only). Run `make test` before committing." |
| **Skills** (`SKILL.md` folders) | On-demand workflows plus scripts | `lit-review`, `paper-figure`, `paper-polish` |
| **Subagents** | Isolated context windows that return summaries | 5 parallel literature searches |
| **Hooks** | Deterministic scripts that run on events | Auto-format after each edit; block writes to `data/raw` |
| **MCP servers** | Standard plugs to external systems | Zotero, arXiv, PubMed, scite |
| **Permissions / sandbox** | What the agent may touch | No network plus read-only raw data |

**Which mechanism to use** (from the [Claude Code docs](https://code.claude.com/docs/en/features-overview)):

| Need | Use | Context cost |
|---|---|---|
| A rule that always applies | `CLAUDE.md` / `AGENTS.md` (keep under ~200 lines) | Every request |
| Knowledge or a workflow needed *sometimes* | **Skill** | Only the description loads until the skill is used |
| Heavy reading or a parallel search | **Subagent** | Separate window |
| Something that must happen *every* time | **Hook** | Zero |
| An external system | **MCP** / CLI tool | Small |

> *"'Never edit .env' in CLAUDE.md is a request, not a guarantee. A hook that blocks the edit is enforcement."*

**Rule of thumb for adding pieces:**
- The agent gets a convention wrong twice → add it to `CLAUDE.md`.
- You paste the same procedure a third time → make it a **Skill**.
- A rule must never be broken → make it a **hook**.

---

## 2. Context engineering: the key idea

> "The delicate art and science of filling the context window with just the right information for the next step." (Andrej Karpathy, 2025)

- **Context is a finite attention budget.** Recall degrades as the window fills ("context rot"). ([Anthropic, *Effective context engineering*](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents))
- A **few diverse, canonical examples** beat a long list of rules.
- **Just-in-time loading:** keep file paths and links, and load the content only when it's needed.
- **Long tasks** use compaction, a progress file, and subagents that each return a short summary.
- **The file system is memory.** The agent keeps a `todo.md` and rewrites it to restate its goals, and it leaves its errors in context so it learns from them ([Manus lessons](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)).

---

## 3. Lessons from the best guides

| Source | Key takeaways |
|---|---|
| [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) | Use the simplest thing that works. Workflow patterns: chaining, routing, parallelization, orchestrator–workers, evaluator–optimizer. Invest in the agent–computer interface and mistake-proof the tools. |
| [Claude Code best practices](https://code.claude.com/docs/en/best-practices) | **Give the agent a way to verify its work.** Follow explore → plan → implement → commit. Keep `CLAUDE.md` short. Course-correct early. `/clear` after two failed corrections. Let Claude interview you, then write a `SPEC.md`. |
| [Multi-agent research system](https://www.anthropic.com/engineering/multi-agent-research-system) | A lead agent plus parallel subagents scored **+90%** over a single agent on Anthropic's research eval, but used about 15× the tokens. Give each subagent an objective, an output format, and boundaries. Search broad first, then narrow. |
| [Effective harnesses for long-running agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents) | An initializer creates `init.sh`, a progress log, a JSON feature list (all "failing"), and a first commit. Each session reads them, does **one** item, tests it, commits, and leaves things clean. **Never edit tests to make them pass.** |
| [Harness design for long-running apps](https://www.anthropic.com/engineering/harness-design-long-running-apps) | Planner → generator → **separate skeptical evaluator** (agents praise their own work). Every harness piece encodes an assumption about what the model can't do yet, so remove pieces as models improve. |
| [Writing tools for agents](https://www.anthropic.com/engineering/writing-tools-for-agents) | Build a few high-impact tools. Return high-signal output, paginate long results, and make error messages actionable. |
| [The lethal trifecta](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/) (Simon Willison) | **Never** give one agent all three: private data + untrusted content + a way to send data out. Together they make prompt-injection data theft possible. |

---

## 4. Agent Skills

A **Skill** is a folder with a `SKILL.md` (YAML frontmatter plus instructions) and optional `scripts/`, `references/`, and `assets/` folders. It is an **open standard** ([agentskills.io](https://agentskills.io/specification)) supported by Claude Code, Codex, Gemini CLI, Cursor, VS Code, and others.

```
paper-figure/
├── SKILL.md          ← name + description (always visible), instructions (loaded on use)
├── scripts/          ← deterministic helpers; only their OUTPUT enters context
└── references/       ← detailed docs, loaded only if needed
```

**Progressive disclosure:**
1. Metadata (~100 tokens) is always loaded.
2. The `SKILL.md` body (< 500 lines) loads when the skill triggers.
3. Scripts and references load only when needed.

### How to write a good skill ([Anthropic best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices))
1. **The description is the trigger.** Say *what* the skill does **and when to use it**, with the words users actually type. Be a little pushy, because models tend to under-trigger skills.
2. **Be concise.** The model is already smart, so add only what it doesn't know: your lab's conventions, your pitfalls, your formats.
3. **Match freedom to fragility.** Use prose for judgment calls and exact scripts for fragile steps.
4. **Give the workflow as a checklist,** with validation loops (run checker → fix → repeat).
5. **Include one or two concrete input → output examples.**
6. **Test it.** Run 3+ real tasks with and without the skill. Check that it triggers when it should and stays quiet otherwise. Anthropic's `skill-creator` skill automates this.
7. **Write it with two Claudes.** Claude A writes the skill; a fresh Claude B uses it on real tasks; you watch where B stumbles.
8. **Security:** skills contain code. Install only from trusted sources and read the scripts first.

**Our example research skills:** [`skills/lit-review`](../skills/lit-review/SKILL.md), [`skills/paper-figure`](../skills/paper-figure/SKILL.md), [`skills/paper-polish`](../skills/paper-polish/SKILL.md).

---

## 5. Research-specific patterns

- **Autonomous experiment loop** ([karpathy/autoresearch](https://github.com/karpathy/autoresearch)): the agent edits **one file**, each run gets a fixed time budget, and it is judged on **one scalar metric**. If the metric improves, the change is committed; otherwise it's reverted. The result is about 100 experiments overnight.
  Lesson: **one editable file + one metric + git as the record = safe autonomy.**
- **Reproducibility:** have the agent build an end-to-end pipeline script (not notebook cells), pin the environment (`uv`/conda lock files), fix the seeds, and add a `Makefile`. Then have it re-run everything and check that the numbers in the paper match the code.
- **Cleaning up a paper's code release:** the agent writes the README, environment file, tests, and figure scripts. A fresh reviewer subagent then checks the result.
- **Literature review:** parallel subagents, one per sub-question. Every claim needs a source the agent opened, and a final pass verifies the DOIs.
- **Safety net:**
  - Use plan mode first.
  - Verify with tests, data-shape assertions, and plots you look at yourself.
  - Commit often and work on branches.
  - Mount raw data read-only.
  - **Passing checks ≠ scientifically valid.** Domain review is still your job.

---

## 6. Fourteen practical tips

1. Write a short `CLAUDE.md`/`AGENTS.md`: setup, how to run things, data locations, conventions, and "never touch data/raw". Start with `/init`, then prune.
2. **Explore → plan → code → commit.** Use plan mode for any change that touches several files.
3. **Give verifiable goals:** a test, an expected number, a reference figure. Ask for evidence, not "done."
4. Keep tasks small: name the files, the constraints, and an existing pattern to copy.
5. Course-correct early (Esc). After two failed fixes, `/clear` and re-prompt with what you learned.
6. Run `/clear` between unrelated tasks.
7. Use subagents for heavy reading and parallel search, and a **fresh-context reviewer** to check work.
8. Use hooks for must-always rules: formatting, linting, blocking dangerous writes.
9. For long tasks, keep a progress file, a JSON task list, and git commits; every session reads them first.
10. Let the agent **interview you**, write `SPEC.md`, then implement it in a fresh session.
11. Commit often and use branches or worktrees for parallel agents.
12. Sandbox the agent and limit its permissions. Avoid the lethal trifecta.
13. Turn repeated prompts into Skills, starting from a real session that worked.
14. Prefer CLI tools (`gh`, `uv`, `quarto`) over sprawling MCP setups; they use less context. Bonus: when a new model ships, delete scaffolding it no longer needs.

---

## 7. GitHub resources (stars approx., Oct 2026)

| Repo | ★ | What it is |
|---|---|---|
| [obra/superpowers](https://github.com/obra/superpowers) | ~296k | Skills methodology: brainstorm → plan → TDD → subagent per task → verify |
| [anthropics/skills](https://github.com/anthropics/skills) | ~180k | Official skills (docx/pdf/xlsx/pptx, `skill-creator`) |
| [anthropics/claude-code](https://github.com/anthropics/claude-code) | ~150k | Claude Code |
| [openai/codex](https://github.com/openai/codex) | ~128k | Codex CLI |
| [google-gemini/gemini-cli](https://github.com/google-gemini/gemini-cli) | ~107k | Gemini CLI |
| [karpathy/autoresearch](https://github.com/karpathy/autoresearch) | ~97k | Agent runs ML experiments overnight |
| [modelcontextprotocol/servers](https://github.com/modelcontextprotocol/servers) | ~91k | Reference MCP servers |
| [ComposioHQ/awesome-claude-skills](https://github.com/ComposioHQ/awesome-claude-skills) | ~77k | Curated skill list |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | ~55k | Curated Claude Code resources |
| [K-Dense-AI/scientific-agent-skills](https://github.com/K-Dense-AI/scientific-agent-skills) | ~48k | 170+ science skills |
| [openai/skills](https://github.com/openai/skills) | ~28k | Codex skills catalog |
| [agentskills/agentskills](https://github.com/agentskills/agentskills) | ~26k | The open Skills spec |
| [agentsmd/agents.md](https://github.com/agentsmd/agents.md) | ~25k | The AGENTS.md format |

---

## Workshop takeaway (one slide)

> **"Design the harness, not just the prompt."** Short memory file → verifiable goals → plan before acting → a separate verifier → git as a safety net → repeated workflows become Skills.
