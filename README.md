# Awesome Agent Harness [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated, bilingual (EN/中文) library of the best resources for building reliable AI agent harnesses — architecture patterns, context engineering, tool design, skills, memory, orchestration, and evaluation.

**[中文版 →](README_zh.md)**

---

The model is not the hard part anymore. Building the system around it is.

In February 2026, Mitchell Hashimoto gave a name to what practitioners had been building informally for years: **harness engineering** — the discipline of designing the full environment that wraps an AI agent and determines whether it succeeds or fails. Within weeks, OpenAI and Anthropic published engineering reports expanding on the idea. The term stuck because it names a real gap: prompt engineering optimizes a single turn; context engineering manages what the model sees; harness engineering governs the entire execution environment across every session.

This list focuses on the **harness**, not the model. Every entry is annotated with *what it is and why it belongs*. Markers: 🆕 = released/updated 2025–2026 · ⚠️ = caveat. Contributions welcome — see [CONTRIBUTING](CONTRIBUTING.md).

---

## Contents

- [⭐ Must-Read Starter Set](#-must-read-starter-set)
- [1 · What Is a Harness? (Definitions & Boundaries)](#1--what-is-a-harness-definitions--boundaries)
- [2 · Context Engineering](#2--context-engineering)
- [3 · Tool Design & MCP](#3--tool-design--mcp)
- [4 · Agent Loop & Verification](#4--agent-loop--verification)
- [5 · Architecture Patterns](#5--architecture-patterns)
- [6 · Skills & Progressive Disclosure](#6--skills--progressive-disclosure)
- [7 · Memory & State Management](#7--memory--state-management)
- [8 · Permissions, Safety & Sandboxing](#8--permissions-safety--sandboxing)
- [9 · Harness ↔ Eval Interaction](#9--harness--eval-interaction)
- [10 · Coding Agent Harness Teardowns](#10--coding-agent-harness-teardowns)
- [11 · Multi-Agent Orchestration](#11--multi-agent-orchestration)
- [12 · Frameworks & Tools](#12--frameworks--tools)
- [13 · Talks, Podcasts & Slides](#13--talks-podcasts--slides)
- [14 · Academic Papers](#14--academic-papers)
- [Contributing](#contributing)
- [License](#license)

---

## ⭐ Must-Read Starter Set

*Read these first. They form a complete conceptual map of why harness engineering matters and how it works.*

1. **[My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)** — Mitchell Hashimoto — *blog, Feb 2026* — The origin post. "Anytime you find an agent makes a mistake, you take the time to engineer a solution so that the agent never makes that mistake again." Coined "harness engineering" and gave the field its founding formula: **Agent = Model + Harness**.

2. **[Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/)** — OpenAI (Ryan Lopopolo) — *blog, Feb 2026* — The flagship field report. A 3-person team built a million-line product with zero human-written code over 5 months. Core lessons: repository as system of record, progressive disclosure over monolithic AGENTS.md, mechanical enforcement over documentation, and continuous entropy management. 🆕

3. **[Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)** — Anthropic — *blog, Dec 2024* — The foundational guide on when to use workflows vs. agents and how to compose primitives (prompt chaining, routing, orchestrator-workers, evaluator-optimizer). Start simple, add complexity only when needed.

4. **[Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)** — Anthropic — *blog, Sep 2025* — The primary statement that the harness's job is engineering context: editing, compaction, memory, programmatic tool-calling. "What configuration of context is most likely to generate the desired behavior?"

5. **[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)** — Anthropic (Prithvi Rajasekaran) — *blog, Mar 2026* — Three-agent harness (Planner → Generator → Evaluator) for sustained multi-session development. Key insight: every harness component encodes an assumption about what the model can't do; those assumptions expire as models improve. 🆕

6. **[Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)** — Martin Fowler / Birgitta Böckeler — *blog, 2026* — The clearest conceptual map: three interlocking systems — context engineering, architectural constraints, and entropy management. "Humans on the loop" — harness engineers who design environments rather than inspect individual outputs. 🆕

7. **[The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)** — LangChain — *blog, Mar 2026* — The five primitives: filesystem (durable state), code execution, sandbox (isolation + verification), memory (cross-session persistence), and context management (compaction against "context rot"). 🆕

8. **[Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)** — LangChain (Vivek Trivedy) — *blog, Feb 2026* — The proof by experiment: Top 30 → Top 5 on Terminal Bench 2.0 by only changing the harness (self-verification, tracing, middleware hooks), not the model. 13.7-point improvement with GPT-5.2-Codex. 🆕

9. **[Writing Effective Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents)** — Anthropic (Ken Aizawa) — *blog* — Tool design is agent UX. "Agents are only as effective as the tools we give them." Tool naming, schemas, error surfaces — the load-bearing part of any harness.

10. **[Agents Are Models Using Tools in a Loop](https://simonwillison.net/2025/May/22/tools-in-a-loop/)** — Simon Willison — *blog, May 2025* — The canonical definition: "the skill is in the design of both the tools and the loop." The cleanest statement of why harness, not model, dominates behavior.

---

## 1 · What Is a Harness? (Definitions & Boundaries)

*The model / harness / skill decomposition. Where prompt engineering ends and harness engineering begins.*

- **[My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)** — Mitchell Hashimoto — *blog, Feb 2026* — Origin of "harness engineering." Covers the full journey from chatbot skeptic → Claude Code user → harness engineer. The Ghostty AGENTS.md as a canonical example of incremental rule accumulation.

- **[Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/)** — OpenAI — *blog, Feb 2026* — Frontier-lab definition: the harness is "the scaffolding, tools, feedback loops, and constraints that enable agents to do reliable work at scale." The million-line experiment as proof. Also listed in Must-Read Starter Set.

- **[Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)** — Martin Fowler — *blog, 2026* — Three systems: context engineering (curating what the agent knows), architectural constraints (deterministic linters/tests), entropy management (repair agents). "Humans on the loop."

- **[Hidden Technical Debt: Agent Harness](https://leehanchung.github.io/blogs/2026/05/08/hidden-technical-debt-agent-harness/)** — Han-Chung Lee — *blog, May 2026* — "The harness is the agent; what teams call 'the model' is mostly harness + product." The model/harness/skill decomposition made explicit. 🆕

- **[The Model Is the Product](https://leehanchung.github.io/talks/2025/04/23/the-model-is-the-product/)** — Han-Chung Lee — *talk, Apr 2025* — The counterpoint: for foundation model providers, the model IS the product. The foundational text of the harness/model debate.

- **[The Model Is Not the Product](https://www.youtube.com/watch?v=EEw2PpL-_NM)** — Hamel Husain — *talk, Apr 2025* — The opposing side: great products are mostly harness + product + evals, not the model. Data Council 2025.

- **[Agents Are Models Using Tools in a Loop](https://simonwillison.net/2025/May/22/tools-in-a-loop/)** — Simon Willison — *blog, May 2025* — The canonical, widely-adopted agent definition. If this is what an agent IS, then everything outside the model — the tools and the loop — IS the harness.

- **[Trustworthy Agents in Practice](https://www.anthropic.com/research/trustworthy-agents)** — Anthropic — *research, 2026* — Defines agents as models that direct their own process and tool use, then frames why trustworthy harnesses need evaluation, monitoring, and controls. 🆕

- **[Harness Engineering for Language Agents: The Harness Layer as Control, Agency, and Runtime](https://www.preprints.org/manuscript/202603.1756)** — Anonymous authors — *preprint, Apr 2026* — The first academic paper treating harness engineering as a research object. Proposes the CAR decomposition (Control, Agency, Runtime) and HarnessCard as a reporting artifact. Audits 63 harness-relevant works. 🆕

- **[Natural-Language Agent Harnesses](https://arxiv.org/html/2603.25723v1)** — Pan et al. — *paper, Mar 2026* — Treats the harness design-pattern layer as a natural-language representation object. Connects AGENTS.md, AgentSkills, and skill bundles as portable operational knowledge. 🆕

- **[The Harness Model — AI Engineering Maturity Matrix, Q1 2026](https://handsonarchitects.com/blog/2026/the-harness-model-ai-engineering-maturity-matrix/)** — Laskowski & Michalak (HandsOn Architects) — *blog, Apr 2026* — Diagnostic tool: 10-dimension maturity grid mapping from "occasional AI" to "agent-first delivery." Practical self-assessment for teams. 🆕

- **[Same Model, Different Results: Why Coding Agents Aren't Interchangeable](https://blog.thepete.net/blog/2025/12/10/same-model-different-results-why-coding-agents-arent-interchangeable/)** — Pete Hodgson — *blog, Dec 2025* — Concrete teardown showing identical models yield different results: Claude Code's harness (system reminders, sub-agents, planning, IDE feedback) vs. other agents. 🆕

- **[Agent Harness Engineering](https://www.oreilly.com/radar/agent-harness-engineering/)** — Addy Osmani (O'Reilly Radar) — *blog* — "A decent model with a great harness beats a great model with a bad harness." Names the converging harness primitives across coding agents. 🆕

**Must-reads:** Hashimoto · OpenAI (Codex) · Fowler · Lee (harness)

---

## 2 · Context Engineering

*The art and science of filling the context window: what goes in, what gets pruned, and how it's structured.*

- **[Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)** — Anthropic — *blog, Sep 2025* — The primary source. Context = "the set of tokens included when sampling." Covers system prompts, tools, examples, message history, and just-in-time strategies. Also listed in Must-Read Starter Set. **(MUST)**

- **[Context Engineering for AI Agents: Lessons from Building Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)** — Manus — *blog, 2025* — Production lessons on tool explosion, action masking, filesystem-as-memory, KV-cache pressure, and stable agent state.

- **[How Long Contexts Fail and How to Fix Them](https://www.dbreunig.com/2025/06/22/how-contexts-fail-and-how-to-fix-them.html)** — Drew Breunig — *blog, Jun 2025* — Failure modes of long-context usage and practical fixes.

- **[LOCA-bench: Benchmarking Language Agents Under Controllable and Extreme Context Growth](https://arxiv.org/pdf/2602.07962)** — Zeng et al. — *paper, 2026* — Evaluates context engineering strategies (context editing, context awareness, memory tools, programmatic tool calling) under controlled conditions. 🆕

- **[Context Management for Deep Agents](https://www.langchain.com/blog/context-management-for-deepagents)** — LangChain (Curme & Daugherty) — *blog, Jan 2026* — Practical patterns for managing context in long-running agent sessions. 🆕

- **[From Context Engineering to Context Infrastructure](https://tacnode.io/post/from-context-engineering-to-context-infrastructure)** — Tacnode — *blog, Mar 2026* — Argues techniques are well-understood but infrastructure (real-time data pipelines, consistent snapshots) is missing. 🆕

- **[Agentic Context Engineering: Evolving Contexts for Self-Improving Language Models](https://openreview.net/forum?id=qQ5MZ5Mx7p)** — Zhang et al. (ICLR 2026) — *paper* — Models that evolve their own context management strategies. 🆕

- **[Dive into Claude Code: The Design Space of Today's and Future AI Agent Systems](https://arxiv.org/abs/2604.14228)** — Liu et al. — *paper, 2026* — Reverse-engineering of Claude Code's harness: permission modes, compaction, MCP/plugins/skills/hooks, subagents, and session storage. 🆕

- **[COMPASS: Enhancing Agent Long-Horizon Reasoning with Evolving Context](https://arxiv.org/abs/2510.08790)** — Wan et al. — *paper* — Evolving context strategies for long-horizon tasks.

- **[AgentSwing: Adaptive Parallel Context Management Routing for Long-Horizon Web Agents](https://arxiv.org/abs/2603.27490)** — Feng et al. — *paper, 2026* — Parallel context management for web agents tackling long-horizon tasks. 🆕

**Must-reads:** Anthropic (context engineering) · Manus · Breunig

---

## 3 · Tool Design & MCP

*Tools are the hands of the agent. MCP is the protocol that connects them. Both are load-bearing harness surfaces.*

- **[Writing Effective Tools for Agents — with Agents](https://www.anthropic.com/engineering/writing-tools-for-agents)** — Anthropic (Ken Aizawa) — *blog* — "Agents are only as effective as the tools we give them." Tool naming, schemas, error surfaces, and the principle that tool design is agent UX. **(MUST)**

- **[Model Context Protocol (MCP)](https://modelcontextprotocol.io/)** — Anthropic — *spec/docs* — The open standard for connecting AI models to external tools and data sources. The plumbing layer of modern harnesses.

- **[MCP: A New Standard for AI Tool Integration](https://www.anthropic.com/news/model-context-protocol)** — Anthropic — *blog, Nov 2024* — Announcement and design rationale for MCP.

- **[Code Execution with MCP: Building More Efficient Agents](https://www.anthropic.com/engineering/code-execution-with-mcp)** — Anthropic — *blog, Sep 2025* — Moves execution into an MCP server so agents can search, filter, and transform tool results before spending context-window tokens.

- **[A Practical Guide to Building AI Agents](https://openai.com/index/a-practical-guide-to-building-ai-agents/)** — OpenAI — *blog, Apr 2026* — Comprehensive guide covering tool design for many-to-many agent-tool relationships and layered guardrail patterns. Also highlighted in related section Must-reads. 🆕

- **[Gemini API Function Calling](https://ai.google.dev/gemini-api/docs/function-calling)** — Google — *docs* — Official schema and tool-calling interface for Gemini agents, including parallel function calls and compositional tool use.

- **[Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)** — Anthropic — *blog* — Skills as composable, progressively-disclosed capabilities built on tool foundations.

- **[OpenAI Agents SDK — Tools](https://openai.github.io/openai-agents-python/tools/)** — OpenAI — *docs* — How tool definitions, function signatures, and error handling work in the Agents SDK.

- **[Claude Code Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks)** — Anthropic — *docs* — Lifecycle hooks (PreToolUse, PostToolUse, etc.) that let harness engineers inject custom logic at every step of tool execution.

- **[Open Reward Standard (ORS)](https://docs.openreward.ai/)** — General Reasoning — *spec* — MCP-extending spec adding RL primitives (episodes, rewards, curriculum). 🆕 ⚠️(early stage)

**Must-reads:** Anthropic (tools) · MCP spec · OpenAI (practical guide)

---

## 4 · Agent Loop & Verification

*How agents iterate: the core loop, self-verification, doom-loop detection, and the "Ralph Wiggum" pattern.*

- **[Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/)** — OpenAI — *blog, Jan 2026* — Detailed breakdown of the Codex agent loop: Read Context → Plan → Execute → Validate → Commit (or Retry). The harness's role is to make Read and Validate as information-rich as possible. 🆕

- **[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)** — Anthropic — *blog, Mar 2026* — Sprint contracts between Generator and Evaluator agents. Separating "the agent doing the work from the agent judging it" as a strong lever. Also listed in Must-Read Starter Set. 🆕

- **[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)** — Anthropic — *blog, Nov 2025* — The pattern for maintaining progress across multiple context windows: initializer agent → coding agent handoff via feature lists, git commits, and test gates.

- **[Ralph Wiggum as a Software Engineer](https://ghuntley.com/ralph/)** — Geoffrey Huntley — *blog* — The minimalist `while :; do cat PROMPT.md | claude-code; done` harness pattern. Single-task loops, deterministic prompt stacking, bounded subagent parallelism.

- **[cwc-long-running-agents](https://github.com/anthropics/cwc-long-running-agents)** — Anthropic — *repo* — Example harness primitives from Code with Claude 2026: default-FAIL contracts, fresh-context evaluators, sprint-loop plugins. Cherry-pick what fits. 🆕

- **[Run Long-Horizon Tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/)** — OpenAI — *blog* — Milestone-based planning artifacts (Plan.md, Implement.md, Documentation.md) as harness-level state. 🆕

- **[Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)** — LangChain — *blog, Feb 2026* — Three optimization knobs: System Prompt, Tools, Middleware. Build-verify loops and doom-loop detection. Also listed in Must-Read Starter Set. 🆕

- **[Introducing Dynamic Workflows in Claude Code](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)** — Anthropic — *blog, May 2026* — Dynamic parallel subagent orchestration: Claude generates JavaScript orchestration scripts that fan out to parallel subagents with adversarial verification. The plan lives in executable code, not the context window. 🆕

- **[Better Harness: A Recipe for Harness Hill-Climbing with Evals](https://www.langchain.com/blog/better-harness-a-recipe-for-harness-hill-climbing-with-evals)** — LangChain (Vivek Trivedy) — *blog, Apr 2026* — "Evals are training data for agents." Six-step recipe: hand-curation, trace-mining, optimization/holdout splits, baselines, diagnose-experiment-validate loops, human review. 🆕

- **[How Middleware Lets You Customize Your Agent Harness](https://www.langchain.com/blog/how-middleware-lets-you-customize-your-agent-harness)** — LangChain (Sydney Runkle) — *blog, Mar 2026* — Hooks around model and tool calls as the primary customization surface. 🆕

**Must-reads:** OpenAI (Codex loop) · Anthropic (long-running) · LangChain (improving deep agents)

---

## 5 · Architecture Patterns

*How agents are structured: single-agent loops, plan-then-execute, multi-agent topologies, routing, and orchestration.*

- **[Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)** — Anthropic — *blog, Dec 2024* — The canonical pattern guide: prompt chaining, routing, parallelization, orchestrator-workers, evaluator-optimizer. "Start simple." **(MUST)**

- **[A Practical Guide to Building AI Agents](https://openai.com/index/a-practical-guide-to-building-ai-agents/)** — OpenAI — *blog, Apr 2026* — Single-agent vs. multi-agent orchestration (manager vs. decentralized handoffs). Also highlighted in related section Must-reads. 🆕

- **[Plan-and-Execute Agents](https://blog.langchain.com/plan-and-execute-agents/)** — LangChain — *blog* — Canonical write-up separating planning from execution: planner LLM generates step list; executor agent handles implementation.

- **[Agent Development Kit (ADK)](https://developers.googleblog.com/en/agent-development-kit-easy-to-build-multi-agent-applications/)** — Google — *blog, 2025* — Google's multi-agent topology, tool registration model, and eval pipeline.

- **[Agents as a Service](https://sierra.ai/blog/agents-as-a-service)** — Sierra — *blog, Mar 2026* — Customer-service agent harness as headless infrastructure: workspace access, sandboxed validation, and a build→test→ship improvement loop for Ghostwriter. 🆕

- **[Building Box AI: How an Enterprise Content Platform Went AI-Native with Deep Agents](https://www.langchain.com/blog/building-box-ai-how-an-enterprise-content-platform-went-ai-native-with-deep-agents)** — LangChain / Box — *case study, Jun 2026* — Enterprise content-agent architecture with a parent agent, dynamic child agents, middleware, citations, prompt caching, and context summarization. 🆕

- **[Scaling Managed Agents: Decoupling the Brain from the Hands](https://www.anthropic.com/engineering/managed-agents)** — Anthropic — *blog, 2026* — Architecture for managed agents where the planning ("brain") and execution ("hands") are separated and can scale independently. 🆕

- **[The "Think" Tool: Enabling Claude to Stop and Think](https://www.anthropic.com/engineering/claude-think-tool)** — Anthropic — *blog* — Giving agents explicit "thinking space" as a harness-level capability.

- **[Compound AI Systems](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/)** — Zaharia et al. (Berkeley) — *blog, Feb 2024* — The shift from single models to systems of models, retrievers, tools, and feedback loops. The conceptual ancestor of harness engineering.

- **[How Squad Runs Coordinated AI Agents Inside Your Repository](https://github.blog/ai-and-ml/github-copilot/how-squad-runs-coordinated-ai-agents-inside-your-repository/)** — GitHub — *blog, Mar 2026* — Repository-native multi-agent orchestration with a coordinator, specialist agents, versioned shared memory, and independent review loops. 🆕

- **[Confucius Code Agent: Scalable Agent Scaffolding for Real-World Codebases](https://arxiv.org/abs/2512.10398)** — Wong et al. — *paper, 2025* — Scalable scaffolding patterns for production codebases.

**Must-reads:** Anthropic (building effective agents) · OpenAI (practical guide) · LangChain (plan-and-execute)

---

## 6 · Skills & Progressive Disclosure

*Skills as modular, composable capabilities. Progressive disclosure as the antidote to context overload.*

- **[Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)** — Anthropic — *blog* — The primary source for the "skill" concept: composable, progressively-disclosed capabilities. Later made an open standard. **(MUST)**

- **[OpenAI Harness Engineering — Progressive Disclosure](https://openai.com/index/harness-engineering/)** — OpenAI — *blog, Feb 2026* — "Give Codex a map, not a 1,000-page instruction manual." The monolithic AGENTS.md anti-pattern and why progressive disclosure works. Also listed in Must-Read Starter Set.

- **[Integrating Agent Skills to Usher in a New Chapter for Agents](https://manus.im/blog/manus-skills)** — Manus — *blog, 2025* — Positions skills as workflow capsules on top of MCP connectors: MCP supplies data access, skills package procedural know-how.

- **[AGENTS.md](https://agents.md/)** — Multiple — *convention* — The foundational harness component: project-level instructions that grow incrementally. Ghostty's AGENTS.md is a public exemplar.

- **[Organizing, Orchestrating, and Benchmarking Agent Skills at Ecosystem Scale](https://arxiv.org/abs/2603.02176)** — Li et al. — *paper, 2026* — Skills as an ecosystem: organization, orchestration, and measurement at scale. 🆕

- **[SkillsBench](https://github.com/benchflow-ai/skillsbench)** — BenchFlow — *benchmark* — Evaluates how well skills work and how effectively agents use them. The measurement companion to skill design. 🆕

- **[Agent Skills Explained: How SKILL.md Files Work and Why They're Everywhere](https://www.firecrawl.dev/blog/agent-skills)** — Firecrawl — *blog* — Practitioner-oriented explainer of SKILL.md as a harness primitive.

**Must-reads:** Anthropic (agent skills) · OpenAI (progressive disclosure)

---

## 7 · Memory & State Management

*How agents remember across turns and sessions. Working memory, persistent memory, and the filesystem as external brain.*

- **[Your Harness, Your Memory](https://www.langchain.com/blog/your-harness-your-memory)** — LangChain (Harrison Chase) — *blog, Apr 2026* — Memory as a harness-level concern: what gets stored, how it's retrieved, and when it gets pruned. 🆕

- **[How Claude Remembers Your Project](https://docs.anthropic.com/en/docs/claude-code/memory)** — Anthropic — *docs* — Claude Code's project memory system: CLAUDE.md files, project-level context, and how durable state is managed across sessions.

- **[Effective Context Engineering for AI Agents — Memory Strategies](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)** — Anthropic — *blog, Sep 2025* — Memory tools that enable persistent storage and retrieval across conversations. Also listed in Must-Read Starter Set.

- **[Tree Ring Memory](https://github.com/TerminallyLazy/Tree-Ring-Memory)** — TerminallyLazy — *tool/repo, 2026* — Local-first memory lifecycle layer for agent harnesses; makes recall, forgetting, audit trails, and consolidation explicit instead of leaving retained state as ad hoc transcripts. 🆕

- **[MemCollab: Cross-Agent Memory Collaboration via Contrastive Trajectory Distillation](https://arxiv.org/abs/2603.23234)** — Chang et al. — *paper, 2026* — Memory collaboration across multiple agents via trajectory distillation. 🆕

- **[Filesystem as Agent Memory](https://openai.com/index/harness-engineering/)** — OpenAI — *blog, Feb 2026* — "From the agent's point of view, anything it cannot access in-context effectively does not exist." The repo as externalized memory. Also listed in Must-Read Starter Set.

**Must-reads:** LangChain (memory) · Anthropic (memory tools)

---

## 8 · Permissions, Safety & Sandboxing

*Guardrails, permission models, and isolation: how to let agents act without letting them act recklessly.*

- **[Beyond Permission Prompts: Making Claude Code More Secure and Autonomous](https://www.anthropic.com/engineering/claude-code-sandboxing)** — Anthropic — *blog, Oct 2025* — Sandboxing reduces approval fatigue by adding filesystem and network isolation around agent actions instead of relying only on prompts. **(MUST)** 🆕

- **[Claude Code Permission Model](https://docs.anthropic.com/en/docs/claude-code/security)** — Anthropic — *docs* — Default read-only stance until explicit approval. Every file edit reversible through automatic snapshots.

- **[A Practical Guide to Building AI Agents — Guardrails](https://openai.com/index/a-practical-guide-to-building-ai-agents/)** — OpenAI — *blog, Apr 2026* — Layered guardrail patterns: input validation, output filtering, tool-risk ratings, human-intervention triggers. Also highlighted in related section Must-reads. 🆕

- **[OWASP Top 10 for Agentic Applications 2026](https://genai.owasp.org/resource/owasp-top-10-for-agentic-applications-for-2026/)** — OWASP GenAI Security Project — *guide, Dec 2025* — Agent-specific threat model for systems that plan, act, use tools, and hold permissions across complex workflows. 🆕

- **[How We Contain Claude Across Products](https://www.anthropic.com/engineering/how-we-contain-claude)** — Anthropic — *blog, May 2026* — Blast-radius containment patterns across claude.ai, Claude Code, and Cowork: sandboxes, VMs, egress controls, and the limits of approval prompts. 🆕

- **[Implementing a Secure Sandbox for Local Agents](https://cursor.com/blog/agent-sandboxing)** — Cursor — *blog, May 2025* — Local coding-agent sandbox design: restrict filesystem writes and network access while keeping the agent useful inside the editor. 🆕

- **[Introducing My Computer: When Manus Meets Your Desktop](https://manus.im/blog/manus-my-computer-desktop)** — Manus — *blog, 2025* — Desktop-agent harness pattern: local CLI, file access, app control, and the permission boundary between cloud agent and user machine.

- **[Gemini API Code Execution](https://ai.google.dev/gemini-api/docs/code-execution)** — Google — *docs* — Model-side code execution tool for generated Python, with clear boundaries around executable computation inside the Gemini API.

- **[E2B Documentation](https://e2b.dev/docs)** — E2B — *docs* — Isolated Linux sandboxes for agents to execute code, process data, run tools, persist state, and export telemetry.

- **[Modal Sandboxes](https://modal.com/docs/guide/sandboxes)** — Modal — *docs* — Secure containers for untrusted user or agent code; useful reference for production-scale code execution isolation.

- **[Sandboxing and Isolation](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)** — LangChain — *blog, Mar 2026* — The sandbox as a harness primitive: isolation + verification. 🆕

- **[BenchJack / Meerkat-style Reward Hacking](https://github.com/benchflow-ai/benchflow)** — BenchFlow — *tool* — How agents game evaluation environments; hardened verifier defaults that block common reward-hacking patterns. 🆕

**Must-reads:** Anthropic (beyond permission prompts) · OWASP (Agentic Top 10) · Anthropic (containment)

---

## 9 · Harness ↔ Eval Interaction

*Same model, different harness, different score. How harness confounds evaluation, and how to measure harness quality.*

- **[Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)** — LangChain — *blog, Feb 2026* — The definitive proof: same model + different harness = dramatically different benchmark results (52.8% → 66.5%). Also listed in Must-Read Starter Set. 🆕

- **[Tuning Deep Agents to Work Well with Different Models](https://www.langchain.com/blog/tuning-deep-agents-different-models)** — LangChain — *blog, May 2026* — A single harness can't be optimal for every model. How to vary the harness per model for fair comparison. 🆕

- **[How We Build Evals for Deep Agents](https://www.langchain.com/blog/how-we-build-evals-for-deep-agents)** — LangChain — *blog, Mar 2026* — The eval-building process behind harness optimization: production traces, failure taxonomies, targeted experiments, and holdout discipline. 🆕

- **[Evaluating Deep Agents: Our Learnings](https://www.langchain.com/blog/evaluating-deep-agents-our-learnings)** — LangChain — *blog, Jun 2026* — Lessons from evaluating long-running agents: start from trajectories, isolate regressions, and avoid optimizing only final-answer scores. 🆕

- **[Agent Evaluation Readiness Checklist](https://www.langchain.com/blog/agent-evaluation-readiness-checklist)** — LangChain — *blog, Jun 2026* — Practical checklist for when an agent is ready for systematic evals: stable traces, representative tasks, clear pass/fail signals, and review loops. 🆕

- **[How We Compare Model Quality in Cursor](https://cursor.com/blog/cursorbench)** — Cursor — *blog, Apr 2025* — CursorBench shows how a coding-agent product evaluates models under realistic edit, retrieval, and tool-use workflows. 🆕

- **[Holistic Agent Leaderboard (HAL)](https://hal.cs.princeton.edu/)** — Princeton — *benchmark* — Standardized, cost-aware harness that runs the SAME agent harness across 9 benchmarks / 9 models (21,730 rollouts). The infrastructure answer to "harness confounds rankings." 🆕

- **[Quo Vadis, LLM Benchmarks? / Benches 2026](https://florianbrand.com/posts/benches-2026)** — Florian Brand (Prime Intellect) — *blog/talk* — The AlgoTune case: same model, different harness, **opposite ranking**. Every layer of the eval stack (prompt, sampling temp, grader, harness) swings the score. 🆕

- **[Hashline: Agent Edit Format as Harness Lever](https://blog.can.ac/2026/02/12/the-harness-problem/)** — Can Boluk — *blog, Feb 2026* — Changing only the edit format (adding line hashes) across 16 models: Grok Code Fast 1 jumped from 6.7% to 68.3%. No model weights changed. Output tokens dropped ~20%. 🆕

- **[AI Agents That Matter](https://arxiv.org/abs/2407.01502)** — Kapoor et al. — *paper, 2024* — Cost as a first-class metric; model-dev vs app-dev needs; missing holdouts breed overfitting. Critical for understanding harness-eval confounds.

- **[Meta-Harness: End-to-End Optimization of Model Harnesses](https://arxiv.org/abs/2603.28052)** — Lee et al. (Stanford) — *paper, Mar 2026* — Treating harness synthesis as an optimization target: automatically producing harnesses that improve agent behavior. 🆕

- **[A Synthetic Data Generation Harness: Hill-Climbing the Eval Set Itself](https://saulius.io/blog/synthetic-data-generation-harness-ai-agents)** — Saulius — *blog, Apr 2026* — The dark side: when harness optimization becomes eval-set overfitting. 🆕

**Must-reads:** LangChain (improving deep agents) · HAL · Brand (Benches 2026)

---

## 10 · Coding Agent Harness Teardowns

*How specific coding agents are actually built: what's in the harness, what's not, and what you can learn.*

### Claude Code

- **[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)** — Anthropic — *blog, Nov 2025* — Initializer → coding agent handoff; feature lists, git commits, test gates.
- **[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)** — Anthropic — *blog, Mar 2026* — Three-agent harness with sprint contracts and evaluator calibration. Also listed in Must-Read Starter Set. 🆕
- **[An Update on Recent Claude Code Quality Reports](https://www.anthropic.com/engineering/april-23-postmortem)** — Anthropic — *blog, Apr 2026* — Postmortem: quality degradation traced to three harness-level changes (reasoning-effort downgrade, caching bug, verbosity prompt). Minor harness adjustments compound into visible regressions. 🆕
- **[cwc-long-running-agents](https://github.com/anthropics/cwc-long-running-agents)** — Anthropic — *repo* — Take-home harness primitives from Code with Claude 2026. 🆕
- **[Claude Code Subagents](https://docs.anthropic.com/en/docs/claude-code/sub-agents)** — Anthropic — *docs* — How Claude Code delegates to subagents with rebuilt permission contexts.
- **[deepclaude](https://github.com/deepclaude)** — Community — *tool* — Ports Claude Code's full agent loop to DeepSeek V4 Pro. Practical evidence that loop architecture, not model identity, determines agent behavior. 🆕

### OpenAI Codex

- **[Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/)** — OpenAI — *blog, Feb 2026* — The million-line experiment. Progressive disclosure, repository as system of record, garbage collection agents. Also listed in Must-Read Starter Set. 🆕
- **[Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/)** — OpenAI — *blog, Jan 2026* — Step-by-step dissection of the Read → Plan → Execute → Validate → Commit loop. 🆕
- **[Unlocking the Codex Harness: How We Built the App Server](https://openai.com/index/unlocking-codex-harness/)** — OpenAI — *blog, Feb 2026* — Implementation details of the layered architecture enforcement. 🆕
- **[Custom Instructions with AGENTS.md](https://developers.openai.com/docs/codex/agents-md)** — OpenAI — *docs* — How AGENTS.md works in Codex: project-level + directory-level instructions with inheritance.
- **[Run Long-Horizon Tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/)** — OpenAI — *blog* — Plan.md, Implement.md as harness-level planning artifacts. 🆕

### Cursor

- **[Continually Improving Our Agent Harness](https://cursor.com/blog/continually-improving-agent-harness)** — Cursor — *blog, May 2025* — Cursor's account of harness iteration: product traces, targeted evals, tool design, and model-specific adjustments.

- **[Cursor Rules Files](https://docs.cursor.com/context/rules-for-ai)** — Cursor — *docs* — How .cursor/rules files configure agent behavior per-project, with built-in loop detection and model-specific prompt adaptation.

### GitHub Copilot

- **[The Coding Harness Behind GitHub Copilot in VS Code](https://code.visualstudio.com/blogs/2026/05/15/agent-harnesses-github-copilot-vscode)** — VS Code team — *blog, May 2026* — Three core loop responsibilities: context assembly, code execution, and verification. 🆕

### Cross-Agent Analysis

- **[Building Effective AI Coding Agents for the Terminal](https://arxiv.org/abs/2603.05344)** — Bui — *paper, 2026* — Scaffolding, harness, context engineering lessons learned from building terminal-based coding agents. 🆕
- **[Same Model, Different Results](https://blog.thepete.net/blog/2025/12/10/same-model-different-results-why-coding-agents-arent-interchangeable/)** — Pete Hodgson — *blog, Dec 2025* — Side-by-side teardown of Claude Code's harness showing why identical models diverge.
- **[I Improved 15 LLMs at Coding in One Afternoon. Only the Harness Changed.](https://blog.can.ac/2026/02/12/the-harness-problem/)** — Can Boluk (Hashline) — *blog, Feb 2026* — The most dramatic single-variable experiment: edit format alone moved scores by 10x. 🆕

**Must-reads:** Anthropic (long-running) · OpenAI (Codex loop) · Cursor (harness iteration)

---

## 11 · Multi-Agent Orchestration

*When one agent isn't enough: coordination, handoffs, delegation, and the distributed-systems patterns that apply.*

- **[Scaling Managed Agents: Decoupling the Brain from the Hands](https://www.anthropic.com/engineering/managed-agents)** — Anthropic — *blog, 2026* — Brain/Hands/Session separation for scalable multi-agent systems. 🆕

- **[Introducing Dynamic Workflows in Claude Code](https://claude.com/blog/introducing-dynamic-workflows-in-claude-code)** — Anthropic — *blog, May 2026* — JavaScript orchestration scripts fanning out to parallel subagents with adversarial verification. 🆕

- **[How Squad Runs Coordinated AI Agents Inside Your Repository](https://github.blog/ai-and-ml/github-copilot/how-squad-runs-coordinated-ai-agents-inside-your-repository/)** — GitHub — *blog, Mar 2026* — Repository-native orchestration with shared memory files, specialist agents, and independent review loops. 🆕

- **[How We Built Our Multi-Agent Research System](https://www.anthropic.com/engineering/multi-agent-research-system)** — Anthropic — *blog, Jun 2025* — Production research-agent architecture: lead researcher, parallel subagents, memory, citation agent, and eval practices for open-ended research. 🆕

- **[Agent Development Kit (ADK)](https://developers.googleblog.com/en/agent-development-kit-easy-to-build-multi-agent-applications/)** — Google — *blog* — Multi-agent topology: manager agents, decentralized handoffs, tool registration across agents.

- **[Why Do Multi-Agent LLM Systems Fail? (MAST)](https://arxiv.org/abs/2503.13657)** — Cemri, Pan et al. (UC Berkeley) — *paper* — 14-mode failure taxonomy across 7 MAS frameworks from 200+ annotated traces. The reference framework for diagnosing multi-agent failures. 🆕

- **[A2A Protocol (Agent-to-Agent)](https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/)** — Google — *spec, Apr 2025* — Inter-agent communication standard, complementing MCP (agent-to-tool).

**Must-reads:** Anthropic (managed agents) · GitHub (Squad) · MAST

---

## 12 · Frameworks & Tools

*Open-source frameworks, SDKs, and tools for building agent harnesses.*

### Agent SDKs & Frameworks

- **[Claude Agent SDK](https://docs.anthropic.com/en/docs/agents/agent-sdk)** — Anthropic — *docs/SDK* — Built-in permission model, hooks system, multi-session support.
- **[OpenAI Agents SDK](https://github.com/openai/openai-agents-python)** — OpenAI — *tool/repo* — Agents, tools, handoffs, guardrails, tracing.
- **[LangGraph](https://github.com/langchain-ai/langgraph)** — LangChain — *tool/repo* — State machines for agent workflows; persistence, streaming, human-in-the-loop.
- **[CrewAI](https://github.com/crewAIInc/crewAI)** — CrewAI — *tool/repo* — Multi-agent orchestration with role-based agents and Flows (event-driven pipelines). 🆕
- **[AutoGen](https://github.com/microsoft/autogen)** — Microsoft — *tool/repo* — Multi-agent conversation framework.
- **[Pydantic AI](https://github.com/pydantic/pydantic-ai)** — Pydantic — *tool/repo* — Type-safe agent framework with OTel tracing. 🆕
- **[Mastra](https://github.com/mastra-ai/mastra)** — Mastra — *tool/repo* — TypeScript-native agent framework with scorers, live evals, CI integration. 🆕
- **[Agno (formerly Phidata)](https://github.com/agno-agi/agno)** — Agno — *tool/repo* — Lightweight multi-modal agent framework. 🆕
- **[Google ADK](https://github.com/google/adk-python)** — Google — *tool/repo* — Google's Agent Development Kit. 🆕
- **[Smolagents](https://github.com/huggingface/smolagents)** — Hugging Face — *tool/repo* — Minimal agent library with code-execution agents. 🆕

### Harness Tools & Utilities

- **[Harbor](https://github.com/harbor-framework/harbor)** — Harbor Framework — *tool/repo* — Framework for running agent evals + creating RL environments. Powers Terminal-Bench 2.0. 🆕
- **[Citadel](https://github.com/SethGammon/Citadel)** — Seth Gammon — *tool/repo* — Harness for Claude Code and Codex with isolated worktrees, multi-agent coordination, and persisted memory. 🆕
- **[Harness Evolver](https://github.com/raphaelchristi/harness-evolver)** — Raphael Christi — *tool/repo* — Claude Code plugin that autonomously evolves harnesses using multi-agent proposers, LangSmith-backed evaluation, and git worktree isolation. Based on Meta-Harness. 🆕

### Observability

- **[AI Agent Observability: Evolving Standards and Best Practices](https://opentelemetry.io/blog/2025/ai-agent-observability/)** — OpenTelemetry — *blog, Jun 2025* — Frames observability as traces across prompts, tools, memory, retrieval, cost, and emerging semantic conventions. 🆕
- **[Tracing](https://openai.github.io/openai-agents-python/tracing/)** — OpenAI Agents SDK — *docs* — Built-in trace/span model covering agent runs, LLM generations, tool calls, handoffs, guardrails, and custom events.
- **[Agent Observability Needs Feedback to Power Learning](https://www.langchain.com/blog/agent-observability-needs-feedback-to-power-learning)** — LangChain (Harrison Chase) — *blog, May 2026* — Connects traces, explicit feedback, and production learning loops so agents improve from real usage. 🆕
- **[Debugging Deep Agents with LangSmith](https://www.langchain.com/blog/debugging-deep-agents-with-langsmith)** — LangChain — *blog, Jun 2026* — Shows why deep-agent traces need specialized debugging views: long trajectories, tool calls, subagent spans, and regression comparison. 🆕
- **[LangSmith Observability](https://docs.langchain.com/langsmith/observability)** — LangChain — *docs* — Official tracing and observability guide for capturing agent runs, inspecting spans, and connecting logs to datasets/evals.
- **[LangSmith](https://docs.langchain.com/langsmith/)** — LangChain — *docs/tool* — Trace storage, evaluation, datasets, prompt management.
- **[Arize Phoenix](https://github.com/Arize-ai/phoenix)** — Arize — *tool/repo* — OSS OTel tracing + response/retrieval evals.
- **[Langfuse](https://github.com/langfuse/langfuse)** — Langfuse — *tool/repo* — OSS tracing, evals, datasets, prompt management; self-hostable. 🆕
- **[Langfuse Observability Overview](https://langfuse.com/docs/observability/overview)** — Langfuse — *docs* — Product-agnostic tracing concepts for production LLM apps: traces, observations, scores, sessions, and datasets.
- **[Langfuse Agent Graphs](https://langfuse.com/docs/observability/features/agent-graphs)** — Langfuse — *docs* — Visualizes multi-step agent execution as a graph, making tool calls, branches, and nested spans inspectable.
- **[W&B Weave](https://github.com/wandb/weave)** — Weights & Biases — *tool/repo* — @weave.op trace trees + scorer-based eval harness. 🆕
- **[Braintrust](https://www.braintrust.dev/)** — Braintrust — *tool* — Eval + observability platform tying offline experiments to production logs.

**Must-reads:** OpenAI Agents SDK · LangGraph · OpenTelemetry · LangSmith

---

## 13 · Talks, Podcasts & Slides

- **[Multi-Turn RL for Multi-Hour Agents — Will Brown](https://www.latent.space/p/willccbb)** — Latent Space — *podcast* — The verifiers author on building multi-turn RL environments and reward design in practice. 🆕
- **[Harness Engineering (YouTube)](https://www.youtube.com/watch?v=kmTMc-fVSXw)** — Florian Brand — *talk* — 61-slide talk on why benchmarks break in the agent era.
- **[Why AI Evals Are the Hottest New Skill](https://www.lennysnewsletter.com/p/why-ai-evals-are-the-hottest-new-skill)** — Hamel Husain & Shreya Shankar — *talk/newsletter* — The "you can't vibe-check" story that mainstreamed evals to PMs. 🆕
- **[Context Engineering for AI Agents: Part 2](https://www.philschmid.de/context-engineering-part-2)** — Phil Schmid — *blog/talk* — Practical context engineering patterns. 🆕
- **[Nathan Lambert — "What technical people call the harness matters more than the model"](https://www.turingpost.com/p/nathanlambert)** — Turing Post — *interview*

**Must-reads:** Latent Space (multi-turn RL) · Brand (Harness Engineering) · Hamel/Shreya (evals)

---

## 14 · Academic Papers

*Research papers treating the harness layer as an explicit object of study.*

- **[Harness Engineering for Language Agents: The Harness Layer as Control, Agency, and Runtime](https://www.preprints.org/manuscript/202603.1756)** — Anonymous authors — *preprint, Apr 2026* — CAR decomposition; HarnessCard reporting artifact; audits 63 works. 🆕

- **[Natural-Language Agent Harnesses](https://arxiv.org/abs/2603.25723)** — Pan et al. — *paper, Mar 2026* — Harness design patterns as natural-language representations executed under a shared runtime. 🆕

- **[Meta-Harness: End-to-End Optimization of Model Harnesses](https://arxiv.org/abs/2603.28052)** — Lee et al. (Stanford) — *paper, Mar 2026* — Automated harness synthesis as an optimization target. 🆕

- **[Building Effective AI Coding Agents for the Terminal](https://arxiv.org/abs/2603.05344)** — Bui — *paper, 2026* — Scaffolding, harness, and context engineering lessons from terminal agents. 🆕

- **[AutoHarness](https://arxiv.org/abs/2603.03329)** — Lou et al. — *paper, 2026* — Automatically producing code harnesses that improve agent behavior. 🆕

- **[General Modular Harness](https://arxiv.org/abs/2507.11633)** — Zhang et al. — *paper, 2025* — Modular harness structure in multi-turn environments. 🆕

- **[AI Agents That Matter](https://arxiv.org/abs/2407.01502)** — Kapoor et al. — *paper, 2024* — Cost-controlled evaluation; harness confounds on benchmarks.

- **[Measuring AI Ability to Complete Long Tasks](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/)** — METR — *paper/blog, 2025* — Scaffolds change the measured horizon; success-vs-human-time as a primitive.

**Must-reads:** Harness Engineering for Language Agents · Natural-Language Agent Harnesses · AI Agents That Matter

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines. TL;DR:

- Every entry must say *what it is* and *why it belongs*.
- URLs must be verified and accessible.
- Use the standard entry format: linked title, author/org, type/date, and annotation.
- Mark `🆕` for 2025–2026 content, `⚠️` for caveats.

---

## License

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

To the extent possible under law, the contributors have waived all copyright and related rights to this work.
