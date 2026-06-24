# Awesome Agent Harness [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> 精选双语（EN/中文）资源库：构建可靠 AI Agent Harness 的最佳资源——架构模式、上下文工程、工具设计、技能、记忆、编排与评估。

**[English Version →](README.md)**

---

模型已不再是难点，围绕模型构建的系统才是。

2026 年 2 月，Mitchell Hashimoto 为从业者们一直在做的事情给出了名字：**Harness Engineering（驾具工程）**——设计包裹 AI Agent 的完整环境，决定其成败的工程学科。几周内，OpenAI 和 Anthropic 发布工程报告加以扩展。这个术语之所以成立，是因为它命名了一个真实的缺口：Prompt Engineering 优化单轮交互；Context Engineering 管理模型看到的内容；Harness Engineering 治理跨越每个会话的整个执行环境。

本列表聚焦 **Harness**，而非模型。每个条目都附有*是什么*和*为什么收录*的注释。标记：🆕 = 2025–2026 年发布/更新 · ⚠️ = 注意事项。欢迎贡献——见 [CONTRIBUTING](CONTRIBUTING.md)。

---

## 目录

- [⭐ 必读入门集](#-必读入门集)
- [1 · 什么是 Harness？（定义与边界）](#1--什么是-harness定义与边界)
- [2 · 上下文工程（Context Engineering）](#2--上下文工程context-engineering)
- [3 · 工具设计与 MCP](#3--工具设计与-mcp)
- [4 · Agent 循环与验证](#4--agent-循环与验证)
- [5 · 架构模式](#5--架构模式)
- [6 · 技能与渐进披露](#6--技能与渐进披露)
- [7 · 记忆与状态管理](#7--记忆与状态管理)
- [8 · 权限、安全与沙箱](#8--权限安全与沙箱)
- [9 · Harness ↔ 评估交互](#9--harness--评估交互)
- [10 · 编程 Agent Harness 拆解](#10--编程-agent-harness-拆解)
- [11 · 多 Agent 编排](#11--多-agent-编排)
- [12 · 框架与工具](#12--框架与工具)
- [13 · 演讲、播客与幻灯片](#13--演讲播客与幻灯片)
- [14 · 学术论文](#14--学术论文)
- [贡献指南](#贡献指南)
- [许可证](#许可证)

---

## ⭐ 必读入门集

*先读这些。它们共同构成了 Harness Engineering 的完整认知地图。*

1. **[My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)** — Mitchell Hashimoto — *博客, 2026.02* — 起源帖。"每次发现 Agent 犯错，就花时间工程化一个解决方案，让它再也不会犯同样的错。" 创造了 "Harness Engineering" 并给出了基础公式：**Agent = Model + Harness**。

2. **[Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/)** — OpenAI (Ryan Lopopolo) — *博客, 2026.02* — 旗舰实战报告。3 人团队在 5 个月内用零人工代码构建了百万行产品。核心经验：仓库即知识系统、渐进披露优于单一 AGENTS.md、机械化执行优于文档、持续熵管理。🆕

3. **[Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)** — Anthropic — *博客, 2024.12* — 基础架构指南：何时用 workflow vs. agent，如何组合原语（提示链、路由、编排器-工作者、评估器-优化器）。从简单开始。

4. **[Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)** — Anthropic — *博客, 2025.09* — Harness 的核心工作是工程化上下文：编辑、压缩、记忆、编程式工具调用。"什么样的上下文配置最可能产生期望行为？"

5. **[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)** — Anthropic (Prithvi Rajasekaran) — *博客, 2026.03* — 三 Agent Harness（规划器→生成器→评估器）用于持续多会话开发。关键洞察：Harness 的每个组件都编码了一个关于"模型做不到什么"的假设，而这些假设会随模型进步而过期。🆕

6. **[Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)** — Martin Fowler / Birgitta Böckeler — *博客, 2026* — 最清晰的概念图谱：三个互锁系统——上下文工程、架构约束、熵管理。"Humans on the loop"——设计环境而非检查单个输出。🆕

7. **[The Anatomy of an Agent Harness](https://blog.langchain.com/the-anatomy-of-an-agent-harness/)** — LangChain — *博客, 2026.03* — 五个原语：文件系统（持久状态）、代码执行、沙箱（隔离+验证）、记忆（跨会话持久化）、上下文管理（对抗"上下文腐烂"的压缩）。🆕

8. **[Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)** — LangChain (Vivek Trivedy) — *博客, 2026.02* — 实验证明：仅改变 Harness（自验证、追踪、中间件钩子）就让 Agent 从 Terminal Bench 2.0 的 Top 30 跃升至 Top 5。模型不变，提升 13.7 分。🆕

9. **[Writing Effective Tools for Agents — with Agents](https://www.anthropic.com/engineering/writing-tools-for-agents)** — Anthropic (Ken Aizawa) — *博客* — 工具设计就是 Agent UX。"Agent 的能力天花板取决于我们给它的工具。" 工具命名、Schema、错误表面。

10. **[Agents Are Models Using Tools in a Loop](https://simonwillison.net/2025/May/22/tools-in-a-loop/)** — Simon Willison — *博客, 2025.05* — 公认的 Agent 定义："技巧在于工具和循环的设计。" 最简洁地说明了为什么 Harness 而非模型决定了行为。

---

## 1 · 什么是 Harness？（定义与边界）

*模型 / Harness / 技能的分解。Prompt Engineering 在哪里结束，Harness Engineering 从哪里开始。*

- **[My AI Adoption Journey](https://mitchellh.com/writing/my-ai-adoption-journey)** — Mitchell Hashimoto — *博客, 2026.02* — "Harness Engineering"的起源。从聊天机器人怀疑者→Claude Code 用户→Harness 工程师的完整旅程。Ghostty 的 AGENTS.md 是增量规则积累的典型范例。

- **[Harness Engineering: Leveraging Codex in an Agent-First World](https://openai.com/index/harness-engineering/)** — OpenAI — *博客, 2026.02* — 前沿实验室定义：Harness 是"让 Agent 进行可靠工作的脚手架、工具、反馈循环和约束的全部环境"。

- **[Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai/harness-engineering.html)** — Martin Fowler — *博客, 2026* — 三个系统：上下文工程、架构约束（确定性 linter/测试）、熵管理（修复 Agent）。

- **[Hidden Technical Debt: Agent Harness](https://leehanchung.github.io/blogs/2026/05/08/hidden-technical-debt-agent-harness/)** — Han-Chung Lee — *博客, 2026.05* — "Harness 就是 Agent；团队所谓的'模型'主要是 Harness + 产品。" 🆕

- **[The Model Is the Product](https://leehanchung.github.io/talks/2025/04/23/the-model-is-the-product/)** vs. **[The Model Is Not the Product](https://www.youtube.com/watch?v=EEw2PpL-_NM)** — Han-Chung Lee vs. Hamel Husain — *演讲, 2025* — Harness/模型辩论的两面。

- **[Agents Are Models Using Tools in a Loop](https://simonwillison.net/2025/May/22/tools-in-a-loop/)** — Simon Willison — *博客, 2025.05* — 如果 Agent 的定义是"模型+工具+循环"，那么模型之外的一切——工具和循环——就是 Harness。

- **[What Is an AI Agent?](https://www.anthropic.com/research/what-is-an-agent)** — Anthropic — *博客* — Agent 的定义，将 Harness 设计决策锚定在对 Agent 本质的清晰理解上。

- **[Harness Engineering for Language Agents: CAR Decomposition](https://www.preprints.org/manuscript/202603.1756)** — *论文, 2026.04* — 首篇将 Harness Engineering 作为研究对象的学术论文。提出 CAR 分解（Control, Agency, Runtime）和 HarnessCard 报告工件。🆕

- **[The Harness Model — AI Engineering Maturity Matrix](https://handsonarchitects.com/blog/2026/the-harness-model-ai-engineering-maturity-matrix/)** — *博客, 2026.04* — 10 维度成熟度矩阵，从"偶尔使用 AI"到"Agent 优先交付"。团队自评工具。🆕

- **[Agent Harness Engineering](https://www.oreilly.com/radar/agent-harness-engineering/)** — Addy Osmani (O'Reilly Radar) — *博客* — "一个平庸模型+优秀 Harness 击败优秀模型+糟糕 Harness。" 🆕

**必读：** Hashimoto · OpenAI (Codex) · Fowler · Lee (harness)

---

## 2 · 上下文工程（Context Engineering）

*填充上下文窗口的艺术与科学：放什么进去、裁剪什么、如何结构化。*

- **[Effective Context Engineering for AI Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)** — Anthropic — *博客, 2025.09* — 主要信源。上下文 = "采样时包含的 token 集合"。涵盖系统提示、工具、示例、消息历史、即时策略。**(必读)**

- **[Context Engineering for AI Agents: Lessons from Building Manus](https://manus.im/blog/Context-Engineering-for-AI-Agents-Lessons-from-Building-Manus)** — Manus — *博客, 2025* — 构建通用 Agent 过程中的上下文窗口管理实战经验。中国团队，面向全球。

- **[Context Management for Deep Agents](https://blog.langchain.com/context-management-for-deep-agents/)** — LangChain — *博客, 2026.01* — 长时运行 Agent 会话中的上下文管理实践模式。🆕

- **[From Context Engineering to Context Infrastructure](https://tacnode.io/post/from-context-engineering-to-context-infrastructure)** — Tacnode — *博客, 2026.03* — 技术已经明确，但基础设施（实时数据管道、一致性快照）仍缺失。🆕

- **[LOCA-bench](https://arxiv.org/pdf/2602.07962)** — *论文* — 在受控条件下评估各种上下文工程策略（上下文编辑、上下文感知、记忆工具、编程式工具调用）。🆕

- **[AgentSwing: Adaptive Parallel Context Management](https://arxiv.org/abs/2603.27490)** — *论文, 2026* — 面向长时 Web Agent 的并行上下文管理路由。🆕

**必读：** Anthropic（上下文工程） · Manus

---

## 3 · 工具设计与 MCP

*工具是 Agent 的双手。MCP 是连接它们的协议。两者都是 Harness 的承重面。*

- **[Writing Effective Tools for Agents](https://www.anthropic.com/engineering/writing-tools-for-agents)** — Anthropic — *博客* — "Agent 的能力取决于工具。" 工具命名、Schema、错误表面。**(必读)**

- **[Model Context Protocol (MCP)](https://modelcontextprotocol.io/)** — Anthropic — *规范/文档* — 连接 AI 模型和外部工具/数据源的开放标准。现代 Harness 的管道层。

- **[A Practical Guide to Building AI Agents](https://openai.com/index/a-practical-guide-to-building-ai-agents/)** — OpenAI — *博客, 2026.04* — 工具设计的多对多 Agent-工具关系和分层护栏模式。🆕

- **[Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)** — Anthropic — *博客* — 技能作为基于工具基础的可组合、渐进披露的能力。

- **[Claude Code Hooks](https://docs.anthropic.com/en/docs/claude-code/hooks)** — Anthropic — *文档* — 生命周期钩子（PreToolUse, PostToolUse 等），让 Harness 工程师在工具执行的每个步骤注入自定义逻辑。

**必读：** Anthropic（工具设计） · MCP 规范 · OpenAI（实践指南）

---

## 4 · Agent 循环与验证

*Agent 如何迭代：核心循环、自验证、死循环检测、"Ralph Wiggum"模式。*

- **[Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/)** — OpenAI — *博客, 2026.01* — Codex Agent 循环详解：读取上下文→规划→执行→验证→提交（或重试）。🆕

- **[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)** — Anthropic — *博客, 2026.03* — 生成器和评估器 Agent 之间的"Sprint 合同"。"将做工作的 Agent 与评判工作的 Agent 分开"是强有力的杠杆。🆕

- **[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)** — Anthropic — *博客, 2025.11* — 跨多个上下文窗口保持进度的模式：初始化 Agent→编码 Agent 交接。

- **[Ralph Wiggum as a Software Engineer](https://ghuntley.com/ralph/)** — Geoffrey Huntley — *博客* — 极简 `while :; do cat PROMPT.md | claude-code; done` Harness 模式。

- **[cwc-long-running-agents](https://github.com/anthropics/cwc-long-running-agents)** — Anthropic — *代码库* — Code with Claude 2026 的示例 Harness 原语。🆕

- **[Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)** — LangChain — *博客, 2026.02* — 三个优化旋钮：系统提示、工具、中间件。构建-验证循环和死循环检测。🆕

- **[Introducing Dynamic Workflows in Claude Code](https://www.anthropic.com/engineering/introducing-dynamic-workflows)** — Anthropic — *博客, 2026.05* — 动态并行子 Agent 编排：Claude 生成 JavaScript 编排脚本，扇出到并行子 Agent 并进行对抗性验证。🆕

**必读：** OpenAI（Codex 循环） · Anthropic（长时运行） · LangChain（改进 Deep Agents）

---

## 5 · 架构模式

*Agent 的结构：单 Agent 循环、规划-执行、多 Agent 拓扑、路由、编排。*

- **[Building Effective Agents](https://www.anthropic.com/research/building-effective-agents)** — Anthropic — *博客, 2024.12* — 经典模式指南：提示链、路由、并行化、编排器-工作者、评估器-优化器。"从简单开始。" **(必读)**

- **[A Practical Guide to Building AI Agents](https://openai.com/index/a-practical-guide-to-building-ai-agents/)** — OpenAI — *博客, 2026.04* — 单 Agent vs. 多 Agent 编排。🆕

- **[Plan-and-Execute Agents](https://blog.langchain.com/plan-and-execute-agents/)** — LangChain — *博客* — 将规划与执行分离为不同 Harness 层的经典写法。

- **[Compound AI Systems](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/)** — Zaharia 等 (Berkeley) — *博客, 2024.02* — 从单一模型到模型+检索器+工具+反馈循环的系统转变。Harness Engineering 的概念先驱。

- **[Multi-Agent Workflows Often Fail](https://github.blog/engineering/multi-agent-workflows/)** — GitHub — *博客, 2026.02* — 多 Agent 系统 = 分布式系统。每次交接需要类型化 Schema 和显式边界验证。🆕

**必读：** Anthropic（构建有效 Agent） · OpenAI（实践指南）

---

## 6 · 技能与渐进披露

*技能作为模块化、可组合的能力。渐进披露作为上下文过载的解药。*

- **[Equipping Agents for the Real World with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)** — Anthropic — *博客* — "技能"概念的主要信源。**(必读)**

- **[OpenAI Harness Engineering — 渐进披露](https://openai.com/index/harness-engineering/)** — OpenAI — *博客, 2026.02* — "给 Codex 一张地图，而不是一本千页操作手册。" 单一 AGENTS.md 的反模式。

- **[SkillsBench](https://github.com/benchflow-ai/skillsbench)** — BenchFlow — *基准测试* — 评估技能的有效性以及 Agent 使用技能的能力。🆕

---

## 7 · 记忆与状态管理

*Agent 如何跨轮次和会话记住。工作记忆、持久记忆、文件系统作为外部大脑。*

- **[Your Harness, Your Memory](https://blog.langchain.com/your-harness-your-memory/)** — LangChain (Harrison Chase) — *博客, 2026.04* — 记忆作为 Harness 级关注点。🆕

- **[How Claude Remembers Your Project](https://docs.anthropic.com/en/docs/claude-code/memory)** — Anthropic — *文档* — Claude Code 的项目记忆系统。

- **[Filesystem as Agent Memory](https://openai.com/index/harness-engineering/)** — OpenAI — *博客, 2026.02* — "从 Agent 的角度看，任何不在上下文中的东西实际上都不存在。"

**必读：** LangChain（记忆） · Anthropic（记忆工具）

---

## 8 · 权限、安全与沙箱

*护栏、权限模型和隔离：如何让 Agent 行动而不让它鲁莽行事。*

- **[Beyond Permission Prompts](https://www.anthropic.com/engineering/beyond-permission-prompts)** — Anthropic — *博客* — 构建结构化权限和授权系统，而非依赖自然语言权限文本。**(必读)**

- **[A Practical Guide to Building AI Agents — 护栏](https://openai.com/index/a-practical-guide-to-building-ai-agents/)** — OpenAI — *博客, 2026.04* — 分层护栏模式：输入验证、输出过滤、工具风险评级、人工干预触发器。🆕

---

## 9 · Harness ↔ 评估交互

*同一模型、不同 Harness、不同分数。Harness 如何混淆评估，以及如何衡量 Harness 质量。*

- **[Improving Deep Agents with Harness Engineering](https://blog.langchain.com/improving-deep-agents-with-harness-engineering/)** — LangChain — *博客, 2026.02* — 决定性证明：同一模型 + 不同 Harness = 截然不同的基准结果（52.8% → 66.5%）。🆕

- **[Tuning Deep Agents to Work Well with Different Models](https://www.langchain.com/blog/tuning-deep-agents-different-models)** — LangChain — *博客, 2026.05* — 单一 Harness 无法对所有模型最优。如何按模型调整 Harness。🆕

- **[Holistic Agent Leaderboard (HAL)](https://hal.cs.princeton.edu/)** — Princeton — *基准测试* — 标准化、成本感知的 Harness，在 9 个基准/9 个模型上运行相同 Agent Harness（21,730 次 rollout）。🆕

- **[Benches 2026](https://florianbrand.com/posts/benches-2026)** — Florian Brand (Prime Intellect) — *博客/演讲* — AlgoTune 案例：同一模型、不同 Harness、**相反排名**。🆕

- **[Hashline](https://can.ac/blog/hashline)** — Can Boluk — *博客, 2026.02* — 仅改变编辑格式（添加行哈希），16 个模型中 Grok Code Fast 1 从 6.7% 跳到 68.3%。模型权重未变。🆕 ⚠️(验证 URL)

- **[Meta-Harness: End-to-End Optimization of Model Harnesses](https://arxiv.org/abs/2603.28052)** — Lee 等 (Stanford) — *论文, 2026.03* — 将 Harness 合成作为优化目标。🆕

**必读：** LangChain（改进 Deep Agents） · HAL · Brand（Benches 2026）

---

## 10 · 编程 Agent Harness 拆解

*具体编程 Agent 的实际构建方式：Harness 里有什么、没有什么、能学到什么。*

### Claude Code
- **[Effective Harnesses for Long-Running Agents](https://www.anthropic.com/engineering/effective-harnesses-for-long-running-agents)** — Anthropic, 2025.11
- **[Harness Design for Long-Running Application Development](https://www.anthropic.com/engineering/harness-design-long-running-apps)** — Anthropic, 2026.03 🆕
- **[Claude Code Quality Postmortem](https://www.anthropic.com/engineering/claude-code-quality-update)** — Anthropic, 2026.04 — 质量退化追溯到三个 Harness 级变更 🆕
- **[cwc-long-running-agents](https://github.com/anthropics/cwc-long-running-agents)** — Anthropic — 示例 Harness 原语 🆕

### OpenAI Codex
- **[Harness Engineering](https://openai.com/index/harness-engineering/)** — OpenAI, 2026.02 — 百万行实验 🆕
- **[Unrolling the Codex Agent Loop](https://openai.com/index/unrolling-the-codex-agent-loop/)** — OpenAI, 2026.01 🆕
- **[Run Long-Horizon Tasks with Codex](https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/)** — OpenAI 🆕

### 跨 Agent 分析
- **[Same Model, Different Results](https://blog.thepete.net/blog/2025/12/10/same-model-different-results-why-coding-agents-arent-interchangeable/)** — Pete Hodgson, 2025.12
- **[Hashline](https://can.ac/blog/hashline)** — Can Boluk, 2026.02 — 仅改编辑格式，分数提升 10 倍 🆕 ⚠️(验证 URL)

---

## 11 · 多 Agent 编排

- **[Scaling Managed Agents](https://www.anthropic.com/engineering/scaling-managed-agents)** — Anthropic, 2026 — 脑/手/会话分离 🆕
- **[Dynamic Workflows in Claude Code](https://www.anthropic.com/engineering/introducing-dynamic-workflows)** — Anthropic, 2026.05 🆕
- **[Multi-Agent Workflows Often Fail](https://github.blog/engineering/multi-agent-workflows/)** — GitHub, 2026.02 🆕
- **[Why Do Multi-Agent LLM Systems Fail? (MAST)](https://arxiv.org/abs/2503.13657)** — UC Berkeley — 14 种失败模式分类学 🆕

---

## 12 · 框架与工具

### Agent SDK 与框架
- **[Claude Agent SDK](https://docs.anthropic.com/en/docs/agents/agent-sdk)** — Anthropic — 内置权限模型、钩子系统
- **[OpenAI Agents SDK](https://github.com/openai/openai-agents-python)** — OpenAI
- **[LangGraph](https://github.com/langchain-ai/langgraph)** — LangChain — Agent 工作流的状态机
- **[CrewAI](https://github.com/crewAIInc/crewAI)** — 基于角色的多 Agent 编排 🆕
- **[AutoGen](https://github.com/microsoft/autogen)** — Microsoft
- **[Pydantic AI](https://github.com/pydantic/pydantic-ai)** — 类型安全 Agent 框架 🆕
- **[Mastra](https://github.com/mastra-ai/mastra)** — TypeScript 原生 🆕
- **[Google ADK](https://github.com/google/adk-python)** 🆕
- **[Smolagents](https://github.com/huggingface/smolagents)** — Hugging Face 🆕

### 可观测性
- **[LangSmith](https://docs.langchain.com/langsmith/)** — LangChain
- **[Arize Phoenix](https://github.com/Arize-ai/phoenix)** — OSS OTel 追踪
- **[Langfuse](https://github.com/langfuse/langfuse)** — OSS，可自托管 🆕
- **[W&B Weave](https://github.com/wandb/weave)** 🆕
- **[Braintrust](https://www.braintrust.dev/)**

---

## 13 · 演讲、播客与幻灯片

- **[Multi-Turn RL for Multi-Hour Agents — Will Brown](https://www.latent.space/p/willccbb)** — Latent Space — *播客* 🆕
- **[Harness Engineering (YouTube)](https://www.youtube.com/watch?v=kmTMc-fVSXw)** — Florian Brand — *演讲*
- **[Why AI Evals Are the Hottest New Skill](https://www.lennysnewsletter.com/p/why-ai-evals-are-the-hottest-new-skill)** — Hamel Husain & Shreya Shankar 🆕

---

## 14 · 学术论文

- **[Harness Engineering for Language Agents: CAR Decomposition](https://www.preprints.org/manuscript/202603.1756)** — *2026.04* 🆕
- **[Natural-Language Agent Harnesses](https://arxiv.org/abs/2603.25723)** — *2026.03* 🆕
- **[Meta-Harness: End-to-End Optimization](https://arxiv.org/abs/2603.28052)** — Stanford — *2026.03* 🆕
- **[Building Effective AI Coding Agents for the Terminal](https://arxiv.org/abs/2603.05344)** — *2026* 🆕
- **[AI Agents That Matter](https://arxiv.org/abs/2407.01502)** — Kapoor 等 — *2024*
- **[Measuring AI Ability to Complete Long Tasks](https://metr.org/blog/2025-03-19-measuring-ai-ability-to-complete-long-tasks/)** — METR — *2025*

---

## 贡献指南

见 [CONTRIBUTING.md](CONTRIBUTING.md)。简要说明：

- 每个条目必须说明*是什么*和*为什么收录*。
- URL 必须经过验证且可访问。
- 使用格式：`**[标题](URL)** — 作者/组织 — *类型, 日期* — 注释。`
- 标记 `🆕` 表示 2025–2026 年内容，`⚠️` 表示注意事项。

---

## 许可证

[![CC0](https://licensebuttons.net/p/zero/1.0/88x31.png)](https://creativecommons.org/publicdomain/zero/1.0/)

在法律允许的范围内，贡献者已放弃本作品的所有版权及相关权利。
