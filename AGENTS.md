# AGENTS.md — awesome-agent-harness

## Project Overview

This is an **awesome-list style GitHub repository** called `awesome-agent-harness` — a curated, bilingual (EN/中文) library of the best resources for building reliable AI agent harnesses.

Scope: everything that wraps around an AI agent model to make it work reliably — architecture patterns, context engineering, tool design, skills, memory, orchestration, evaluation interaction, and production practices.

Modeled after [benchflow-ai/awesome-evals](https://github.com/benchflow-ai/awesome-evals) in style: every entry is annotated with *what it is* and *why it belongs*, URLs are verified, and markers (🆕 for 2025–2026, ⚠️ for caveats) are used consistently.

## Repository Structure

```
awesome-agent-harness/
├── README.md          # English version (banner + badge row + curation statement; 14 sections + Related Lists)
├── README_zh.md       # Chinese version (mirrored structure, adapted annotations)
├── CONTRIBUTING.md    # Contribution guidelines
├── LICENSE            # CC0 1.0
├── .gitignore
├── .github/
│   ├── workflows/link-check.yml      # monthly + PR link check (fails on 404/410; warns on 403/000 bot walls)
│   └── PULL_REQUEST_TEMPLATE.md      # bilingual + format checklist for contributors
├── AGENTS.md          # This file
└── docs/
    ├── content-gap-map.md
    ├── quality-audit-2026-06-25.md   # audit log; appended 2026-08-16 refresh
    ├── social-preview.html           # source of the banner/social image
    └── social-preview.png            # 1280x640; also used as README banner
```

## Current State

### 2026-08-16 Refresh (latest)

- Full link re-check: 130 unique resource URLs; 4 true dead links fixed (OpenAI docs moved to learn.chatgpt.com, Agent SDK docs to code.claude.com, two openai.com slug changes). See the appended section in `docs/quality-audit-2026-06-25.md`.
- 13 new sources added, centered on the post-June self-improving-harness research wave (Self-Harness, Adaptive Auto-Harness, HarnessBank, SIA), the first harness-engineering surveys (RUC AI Box, Code as Agent Harness), LangChain deepagents + harness profiles, Claude Code Agent Teams, and Lance Martin's Manus teardown.
- New structure: header badges + "Last refreshed" line; §14 subsection "Self-Improving & Auto-Evolving Harnesses"; top-level "Related Lists" section that positions this list against `ai-boost/awesome-harness-engineering` (3.5k+ stars, EN) and `RUCAIBox/awesome-agent-harness` (paper taxonomy, EN). Differentiator: bilingual + annotated.
- EN/ZH parity maintained; one documented exception (Manus 中文版 link in README_zh §2).
- Backlog items partially addressed: cross-references (§11→§10 note), Chinese-audience adaptation (Manus zh link), observability/safety sections unchanged.

### What's Done (baseline, 2026-06)
- Full 14-section structure with 100+ annotated entries across both language versions
- 10-entry Must-Read Starter Set
- Sections: Definitions & Boundaries, Context Engineering, Tool Design & MCP, Agent Loop & Verification, Architecture Patterns, Skills & Progressive Disclosure, Memory & State, Permissions/Safety/Sandboxing, Harness↔Eval Interaction, Coding Agent Harness Teardowns (Claude Code / Codex / Cursor subsections), Multi-Agent Orchestration, Frameworks & Tools, Talks/Podcasts, Academic Papers
- CONTRIBUTING.md with entry format spec and quality criteria
- CC0 license

### What Needs To Be Done

#### Priority 1 — URL Verification (completed 2026-06-25)
The previously marked `⚠️(verify URL)` / `⚠️(verify arXiv ID)` entries were checked and README links were updated:
- `can.ac/blog/hashline` → `https://blog.can.ac/2026/02/12/the-harness-problem/`
- `code.visualstudio.com/blogs/2026/copilot-harness` → `https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode`
- `github.com/citadel-ai/citadel` → `https://github.com/SethGammon/Citadel`
- `github.com/harness-evolver/harness-evolver` → `https://github.com/raphaelchristi/harness-evolver`
- `arxiv.org/abs/2506.12345` → `https://arxiv.org/abs/2604.14228`
- `arxiv.org/abs/2606.xxxxx` → `https://arxiv.org/abs/2603.03329`
- `arxiv.org/abs/2605.xxxxx` → `https://arxiv.org/abs/2507.11633`
- `anthropic.com/engineering/claude-code-quality-update` → `https://www.anthropic.com/engineering/april-23-postmortem`
- `blog.langchain.com/context-management-for-deep-agents/` → `https://www.langchain.com/blog/context-management-for-deepagents`
- `blog.langchain.com/better-harness-recipe/` → `https://www.langchain.com/blog/better-harness-a-recipe-for-harness-hill-climbing-with-evals`
- `blog.langchain.com/how-middleware-lets-you-customize-your-agent-harness/` → `https://www.langchain.com/blog/how-middleware-lets-you-customize-your-agent-harness`
- `blog.langchain.com/your-harness-your-memory/` → `https://www.langchain.com/blog/your-harness-your-memory`

#### Priority 2 — Content Gaps to Fill
These areas need more entries (search for high-quality primary sources):

1. **Chinese-language primary sources** — Currently the list is heavily English-sourced. Search for:
   - Manus team's Chinese-language blog posts on context engineering
   - MetaGPT architecture documentation and blog posts
   - Chinese AI engineering community discussions (即刻, 知乎) on harness patterns
   - Moonshot/Kimi, Zhipu/GLM, ByteDance/Doubao agent engineering posts
   - 稀土掘金, InfoQ 中文 articles on agent harness

2. **Harness engineering with non-coding agents** — The list is coding-agent-heavy. Search for:
   - Customer service agent harnesses (Sierra, Intercom)
   - Research agent harnesses (Anthropic's multi-agent research system)
   - Data analysis agent harnesses
   - Enterprise workflow agent patterns

3. **Production war stories** — Real-world postmortems and case studies:
   - Stripe Minions harness design
   - Notion's agent integration
   - Vercel's agent-eval results
   - Any "what went wrong" posts about agent deployment

4. **Observability deep dives** — The Observability subsection under Frameworks is thin:
   - OpenTelemetry GenAI semantic conventions for agent spans
   - Comparison of tracing approaches across frameworks
   - Cost tracking and token accounting patterns

5. **Security & red-teaming** — Section 8 (Permissions, Safety) needs more:
   - Prompt injection defense patterns in agent harnesses
   - Agent authorization frameworks beyond Anthropic's
   - Sandboxing approaches (Docker, Daytona, E2B, Modal)

#### Priority 3 — Structural Improvements

1. **Add a `notes/` directory** — Like awesome-evals, create deep reading notes for key sources. Start with the Must-Read Starter Set. Format: `notes/hashimoto-ai-adoption-journey.md`, etc.

2. **Add a `docs/` directory** — Consider adding:
   - `docs/harness-landscape.md` — A market map of companies building harness tools
   - `docs/glossary.md` — Definitions of key terms (harness, scaffold, harness engineering, context engineering, etc.)

3. **Cross-references** — Many entries belong in multiple sections. Add `(also §N)` cross-reference markers like awesome-evals does.

4. **"Must-reads" per section** — Each section should end with a `**Must-reads:** ...` line (some already do, some don't).

5. **Keep EN and ZH in sync** — After any content change to README.md, mirror it to README_zh.md. The Chinese version should be adapted (natural Chinese annotations), not mechanically translated.

#### Priority 4 — Polish & Publish Prep

1. **Add badges** — GitHub stars, last commit, license badge at the top
2. **Add a banner image** — Like awesome-evals has its header
3. **Verify all entries are in correct format**: `**[Title](URL)** — Author/Org — *type, date* — Annotation.`
4. **Alphabetize within subsections** where order doesn't matter (e.g., Frameworks & Tools)
5. **Remove any entries where URL cannot be verified** — Better to have 80 verified entries than 100 with broken links

## Entry Format Spec

```markdown
- **[Title](URL)** — Author/Org — *type, date* — One-sentence annotation. 🆕
```

Types: `blog`, `paper`, `talk`, `docs`, `tool/repo`, `benchmark`, `spec`, `book`, `newsletter`, `podcast`

Markers:
- `🆕` = released/updated 2025–2026
- `⚠️` = caveat (unverified URL, discontinued, etc.)
- `**(MUST)**` = essential reading for the section

## Key Primary Sources (search from these first)

When looking for new entries, these authors/orgs consistently produce high-quality harness-relevant content:

- **Anthropic Engineering Blog**: anthropic.com/engineering — Context engineering, tool design, agent skills, harness design, managed agents, permission models
- **OpenAI Engineering Blog**: openai.com/index — Codex harness, AGENTS.md, agent loop, long-horizon tasks
- **LangChain Blog**: blog.langchain.com — Deep agents, harness engineering, middleware, memory, evals
- **Han-Chung Lee**: leehanchung.github.io/blogs — Hidden technical debt series, model/harness decomposition
- **Mitchell Hashimoto**: mitchellh.com/writing — Origin of harness engineering, AGENTS.md patterns
- **Martin Fowler / Thoughtworks**: martinfowler.com — Harness engineering synthesis
- **Simon Willison**: simonwillison.net — Agent definitions, tool-use patterns
- **Latent Space Podcast**: latent.space — Interviews with harness/agent practitioners
- **Hamel Husain**: hamel.dev/blog — Eval-adjacent harness concerns
- **arXiv cs.AI / cs.SE**: Recent papers on agent scaffolding, harness optimization, multi-agent systems

## Style Guide

- Write annotations in active voice, present tense
- Lead with the key insight, not a summary of the piece
- Avoid jargon in annotations — a senior engineer who hasn't read the piece should understand why it matters
- Chinese annotations should sound natural (口语化), not like machine translation
- Keep each annotation to 1-2 sentences max
- Don't editorialize — state what the resource contributes, not whether you agree with it

## Git Conventions

- Commit messages: `add: [section] entry title` / `fix: verify URL for X` / `refactor: reorganize section N`
- One logical change per commit
- PR description should explain what was added and why
