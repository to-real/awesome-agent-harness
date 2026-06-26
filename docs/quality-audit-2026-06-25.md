# Quality Audit

Date: 2026-06-25

Scope: README.md and README_zh.md. This audit checks link health, EN/ZH content parity, entry format consistency, section structure, and obvious duplicate pressure. It does not apply content fixes.

## Executive Summary

- EN/ZH parity is good at the URL-set level: both files contain the same 134 unique non-local URLs.
- The Chinese README is no longer link-sparse, but it still has more shorthand entries than the English README.
- The main cleanup work should be: finish remaining Chinese shorthand entries, then rerun link/format checks after any further source movement.
- Several failures from automated link checking are request-policy issues, not broken resources.

## Link Audit

### Counts

- README.md: 167 non-local link occurrences, 134 unique non-local URLs.
- README_zh.md: 166 non-local link occurrences, 134 unique non-local URLs.
- Unique URL diff between EN and ZH: 0.

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

### Request-Limited But Not Proven Broken

These failed under some automated request methods but were confirmed through another route or are likely bot/rate-limit issues.

- OpenAI main site and developer docs returned 403 to automated fetch/curl, but these URLs are known public pages and should not be marked broken solely from this check.
- `https://www.turingpost.com/p/nathanlambert` returned 403 to curl but was readable through web tooling.
- Google AI / Developers pages returned `000` to curl in one batch but returned 200 via `Invoke-WebRequest`.
- YouTube URLs returned request failures from Node/curl; this is common for automated requests.
- Some GitHub URLs timed out in one pass but many returned 200 in the second pass; treat remaining GitHub `000` as inconclusive unless reproduced with browser/web tooling.
- O'Reilly returned 403; likely request policy, not enough evidence to call broken.

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

1. Re-run link check after the duplicate/cross-reference cleanup.
2. Consider a final README skim for wording consistency around cross-reference markers.
3. Keep future source additions quality-first and preserve exact EN/ZH URL parity.
