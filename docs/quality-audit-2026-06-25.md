# Quality Audit

Date: 2026-06-25

Last refreshed: 2026-08-16

Scope: README.md and README_zh.md. This audit checks link health, EN/ZH content parity, entry format consistency, section structure, and obvious duplicate pressure. It does not apply content fixes.

## Executive Summary

- EN/ZH parity is good at the URL-set level: both files contain the same 133 unique non-local URLs.
- Targeted format cleanup is complete for the audited high-priority sections.
- The 2026-06-26 link refresh found no remaining true broken URL candidates.
- Remaining automated link-check failures are request-policy or network-limit issues, not confirmed broken resources.

## Link Audit

### Counts

- README.md: 166 non-local link occurrences, 133 unique non-local URLs.
- README_zh.md: 165 non-local link occurrences, 133 unique non-local URLs.
- Unique URL diff between EN and ZH: 0.

### 2026-06-26 Refresh

Method: extracted unique non-local URLs from README.md, checked with Node `fetch` using HEAD first and GET fallback.

- Checked: 133 unique non-local URLs.
- OK via HEAD: 119.
- OK via GET fallback: 1.
- Request-limited / network-limited: 13.
- Broken candidates after rerun: 0.
- Placeholder `](URL)` examples were removed from both README contribution sections so future link extraction does not count them as resource URLs.

### True Broken Or Stale URLs

These returned 404 or soft-redirected to the wrong content during the audit. They have since been fixed in both README files.

- `https://github.blog/engineering/multi-agent-workflows/`
  - Status: 404.
  - Fixed: replaced with `https://github.blog/ai-and-ml/github-copilot/how-squad-runs-coordinated-ai-agents-inside-your-repository/`
  - Also update title from "Multi-Agent Workflows Often Fail" to "How Squad runs coordinated AI agents inside your repository".

- `https://www.anthropic.com/engineering/beyond-permission-prompts`
  - Status: 404.
  - Fixed: replaced with `https://www.anthropic.com/engineering/claude-code-sandboxing`
  - Page title: "Beyond permission prompts: making Claude Code more secure and autonomous".

- `https://www.anthropic.com/engineering/scaling-managed-agents`
  - Status: 404.
  - Fixed: replaced with `https://www.anthropic.com/engineering/managed-agents`
  - Page title remains "Scaling Managed Agents: Decoupling the brain from the hands".

- `https://www.anthropic.com/engineering/introducing-dynamic-workflows`
  - Status: 404.
  - Fixed: replaced with `https://claude.com/blog/introducing-dynamic-workflows-in-claude-code`
  - Related docs: `https://code.claude.com/docs/en/workflows`.

- `https://www.anthropic.com/research/what-is-an-agent`
  - Status: 404.
  - No exact replacement found.
  - Fixed: replaced with `https://www.anthropic.com/research/trustworthy-agents` because it still defines agents and connects the definition to trust, evaluation, monitoring, and controls.

- `https://www.firecrawl.dev/blog/agent-skills-explained`
  - Status: 200 but final URL collapsed to the Firecrawl blog index in automated check.
  - Fixed: replaced with `https://www.firecrawl.dev/blog/agent-skills`
  - Treat this as a soft-broken link.

- `https://github.com/anthropics/agent-skills/blob/main/AGENTS.md`
  - Status: 404 in Node fetch and raw GitHub checks during the 2026-06-26 refresh.
  - Fixed: replaced with `https://agents.md/`.
  - Reason: this keeps the entry focused on the AGENTS.md convention rather than a stale repository path.

### Request-Limited But Not Proven Broken

These failed under some automated request methods but were confirmed through another route or are likely bot/rate-limit issues.

- OpenAI main site and developer docs returned 403 to automated fetch/curl, but these URLs are known public pages and should not be marked broken solely from this check.
- `https://www.turingpost.com/p/nathanlambert` returned 403 to curl but was readable through web tooling.
- Google AI / Developers pages returned `000` to curl in one batch but returned 200 via `Invoke-WebRequest`.
- YouTube URLs returned request failures from Node/curl; this is common for automated requests.
- Some GitHub URLs timed out in one pass but many returned 200 in the second pass; treat remaining GitHub `000` as inconclusive unless reproduced with browser/web tooling.
- O'Reilly returned 403; likely request policy, not enough evidence to call broken.

Latest request-limited set from the 2026-06-26 refresh:

- `https://ai.google.dev/gemini-api/docs/code-execution`
- `https://ai.google.dev/gemini-api/docs/function-calling`
- `https://developers.googleblog.com/en/a2a-a-new-era-of-agent-interoperability/`
- `https://developers.googleblog.com/en/agent-development-kit-easy-to-build-multi-agent-applications/`
- `https://developers.openai.com/blog/run-long-horizon-tasks-with-codex/`
- `https://developers.openai.com/docs/codex/agents-md`
- `https://openai.com/index/a-practical-guide-to-building-ai-agents/`
- `https://openai.com/index/harness-engineering/`
- `https://openai.com/index/unlocking-codex-harness/`
- `https://openai.com/index/unrolling-the-codex-agent-loop/`
- `https://www.turingpost.com/p/nathanlambert`
- `https://www.youtube.com/watch?v=EEw2PpL-_NM`
- `https://www.youtube.com/watch?v=kmTMc-fVSXw`

## Format Audit

Target format:

```markdown
- **[Title](URL)** — Author/Org — *type, date* — Annotation.
```

### Counts

- Original audit baseline: README.md possible format issues: 27.
- Original audit baseline: README_zh.md possible format issues: 46.
- After cleanup batches, targeted scans found no missing-author paper entries, no `— Org, date` compressed entries, and no resource entries with fewer than two ` — ` separators.

### Main Patterns

- Academic-paper entries that omitted author/org, e.g. entries that started with `— *paper...*`, were normalized after this audit.
- Framework/tool entries that omitted `*type/date*` in sections 12 and 14 were normalized after this audit.
- README_zh compressed entries in sections 10, 11, and 13, such as `— Anthropic, 2026.03 🆕`, were normalized after this audit.
- Tool entries that started directly with an annotation after the org name, e.g. `— OpenAI — Agents, tools...`, were normalized in the targeted cleanup sections.

### Highest-Priority Format Sections

- README.md: section 12 `Frameworks & Tools` and section 14 `Academic Papers` were normalized after this audit.
- README_zh.md: section 12 `框架与工具` and section 14 `学术论文` were normalized after this audit.
- README_zh.md: compressed entries in sections 10, 11, and 13 were normalized after this audit.
- README.md and README_zh.md: paper-heavy entries in earlier sections 1, 2, 5, and 7 were normalized after this audit.
- Remaining format work: none found in the targeted format scans. Future cleanup should focus on duplicate pressure, cross-references, and link rechecks after any content movement.

## Must-Reads Audit

### English README

- Sections 1-14 now have `Must-reads` lines.
- Sections 10, 12, 13, and 14 were filled after this audit.
- Section 11 was updated to reflect the fixed GitHub Squad link/title instead of the stale multi-agent-failures wording.

### Chinese README

- Sections 1-14 now have `必读` lines.
- Missing `必读` lines were added for sections 6, 8, 10, 11, 12, 13, and 14.
- Existing `必读` lines were aligned with English:
  - Section 2 now includes Breunig.
  - Section 5 now includes LangChain plan-and-execute.

## Duplicate Pressure

The duplicate pattern is mostly expected because Must-Read items reappear in their topical sections. The highest-duplication sources have now been marked in their repeated body entries with lightweight cross-references:

- `https://openai.com/index/harness-engineering/` appears 5 times; repeated body entries now point back to the Must-Read Starter Set.
- `https://blog.langchain.com/improving-deep-agents-with-harness-engineering/` appears 3 times; repeated body entries now point back to the Must-Read Starter Set.
- `https://openai.com/index/a-practical-guide-to-building-ai-agents/` appears 3 times; repeated body entries now point to related section `Must-reads`.
- `https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents` appears 3 times; repeated body entries now point back to the Must-Read Starter Set.
- `https://www.anthropic.com/engineering/harness-design-long-running-apps` appears 3 times; repeated body entries now point back to the Must-Read Starter Set.

Recommendation: keep these intentional duplicates. Re-run link checks after any future source movement rather than pruning them for deduplication.

## Recommended Next Fix Batch

1. Keep future source additions quality-first and preserve exact EN/ZH URL parity.
2. Re-run link checks only after future source movement or URL edits.

---

# 2026-08-16 Refresh

Scope: full link re-check of both READMEs, four dead-link fixes, thirteen new sources, one new README section (Related Lists), one new subsection (§14 Self-Improving & Auto-Evolving Harnesses).

## Link Re-Check

Method: extracted unique non-local, non-badge URLs from README.md (curl, HEAD→GET with -L, browser UA), flagged everything that was neither 200 nor 405.

- Checked: 130 unique resource URLs.
- OK: 124.
- True 404 (fixed this pass): 4 — see below.
- Bot-walled 403 to automated checks (human-accessible, kept): 2 — `openreview.net` (bot-check interstitial) and `preprints.org` manuscript page.
- The 2026-06-26 "request-limited" OpenAI URLs have since become real 404s (docs/URL relocations), confirming they were relocations in progress, not bot walls.

### Dead Links Fixed

- `https://developers.openai.com/docs/codex/agents-md` → `https://learn.chatgpt.com/docs/agent-configuration/agents-md` (OpenAI developer docs moved to learn.chatgpt.com)
- `https://docs.anthropic.com/en/docs/agents/agent-sdk` → `https://code.claude.com/docs/en/agent-sdk/overview` (Agent SDK docs moved to code.claude.com)
- `https://openai.com/index/a-practical-guide-to-building-ai-agents/` → `https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/` (3 occurrences per README)
- `https://openai.com/index/unlocking-codex-harness/` → `https://openai.com/index/unlocking-the-codex-harness/` (slug changed)

## New Sources Added (13)

All new URLs verified 200 at time of addition.

1. Context Engineering in Manus — Lance Martin (§2, both languages)
2. How to Build a Custom Agent Harness — LangChain (§4)
3. Claude Code Agent Teams — Anthropic docs (§11)
4. deepagents repo — LangChain (§12)
5. Deep Agents v0.6: Harness Profiles — LangChain (§12)
6. Agent Systems with Harness Engineering — RUC AI Box (§14) ⚠️ OpenReview bot-checks automated access
7. Code as Agent Harness — Ning et al. survey (§14)
8. Self-Harness — Zhang et al. (§14 new subsection)
9. Adaptive Auto-Harness — Liu et al. (§14 new subsection)
10. HarnessBank — Luo et al. (§14 new subsection)
11. SIA: Harness & Weight Updates — Hebbar et al. (§14 new subsection)
12. awesome-harness-engineering — ai-boost (new Related Lists section)
13. awesome-agent-harness — RUCAIBox (new Related Lists section)

## EN/ZH Parity

- URL sets remain aligned except one deliberate exception: README_zh links the official Manus 中文版 in the §2 Manus annotation (`manus.im/zh-cn/blog/...`). This is an intentional bilingual adaptation, not drift.
- Both READMEs gained the same sections, subsections, and must-read updates; §14 must-reads updated in both.

## Structural Changes

- Header: added stars / refreshed / license / language badges; added "Last refreshed" line to both intros.
- New top-level section: Related Lists (differentiates this bilingual, annotated list from the two English-only lists).
- §14 gained a "Self-Improving & Auto-Evolving Harnesses" subsection covering the June–July 2026 research wave; §14 must-reads updated accordingly.
- §11 gained Claude Code Agent Teams with a §10 cross-reference note.

---

# 2026-08-16 Refresh — Batch 2 (audience-positioned expansion)

Trigger: competitive gap analysis vs ai-boost/awesome-harness-engineering (435 links) and RUCAIBox/awesome-agent-harness (607 links). Audience reaffirmed: **AI PMs and agent product engineers** — entries selected and annotated for decisions and patterns, not academic coverage.

## Changes

- +28 entries across §2–§12 and Related Lists; entry count now ~200 (EN) / ~190 (ZH, some entries merged per zh convention).
- Biggest additions:
  - Codex skills/hooks/SDK (§6, §10) — Skills become a cross-vendor standard (same SKILL.md open standard, agentskills.io).
  - MCP July 2026 release candidate + tool annotations (§3) — protocol direction: stateless, Extensions first-class, 12-month deprecation policy.
  - Don't Build Multi-Agents (Cognition, Walden Yan) + Choosing the Right Multi-Agent Architecture (§11) — the canonical counterpoint to fan-out.
  - New §12 subsection "Reference Harnesses & Open-Source Coding Agents": OpenHands, aider, SWE-agent, OpenHarness, Open SWE, deepagents (+ v0.6 profiles entry moved alongside).
  - §8 trust/authorization cluster: Two Types of Agent Authorization (on-behalf-of vs fixed credentials), IETF agent auth draft, UK AISI inspect_ai, NVIDIA sandboxing guidance.
  - §9 harness-tuning cluster: Nemotron 3 Ultra playbook (0.80→0.86, ~10× cost advantage), Human Judgment in the Agent Improvement Loop. §9 must-reads updated.
- Related Lists expanded 2 → 5 (+awesome-evals, +Awesome-Context-Engineering, +best-of-Agent-Harnesses).
- Intro/curation statement in both languages now names the audience explicitly.
- Repo description updated to name the audience.

## Link Verification

All 28 new URLs verified 200 (curl, browser UA, follow redirects) on 2026-08-16. Canonical final URLs used where redirects exist (cognition.ai → cognition.com; developers.openai.com/codex/skills → learn.chatgpt.com/docs/build-skills; blog.langchain.com → www.langchain.com).

## URL Redirects Normalized

Pre-existing entries still pointing at redirecting hosts were left as-is if they resolve 200; new entries use canonical targets.
