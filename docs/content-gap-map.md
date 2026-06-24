# Content Gap Map

Snapshot: 2026-06-25

This map tracks the main content gaps after the initial URL verification pass. The goal is to improve source quality and coverage without padding the list with low-signal links.

## Current Weak Areas

1. Non-coding production agent harnesses
   - Current list is still heavily coding-agent-centered.
   - Needed: customer support, enterprise content, research, and workflow agents with concrete architecture details.
   - Added in this batch: Sierra Ghostwriter, Box AI on Deep Agents, Anthropic Research.

2. Permissions, safety, and sandboxing
   - Current section has good first principles but thin coverage of agent-specific threat models and execution isolation.
   - Needed: agentic threat taxonomy, blast-radius containment, and real sandbox infrastructure.
   - Added in this batch: OWASP Agentic Top 10, Anthropic containment, E2B, Modal.

3. Observability
   - Current section lists tools but needs more standards and runtime evidence loops.
   - Needed: trace/span conventions, built-in SDK tracing, and feedback-to-learning workflows.
   - Added in this batch: OpenTelemetry agent observability, OpenAI Agents SDK tracing, LangChain feedback loop.

4. Product-specific teardown imbalance
   - Claude Code and Codex are well covered.
   - Cursor and GitHub Copilot remain thin beyond the currently verified sources.
   - Deferred: no extra entries added until high-quality primary sources are found.

5. Chinese-language sources
   - The Chinese README should stay in sync, but sourcing should remain quality-first.
   - Deferred: no quota-based Chinese additions in this batch.

## Source Quality Bar

- Prefer first-party engineering posts, official docs, specifications, and papers.
- Use third-party articles only when they add architecture detail not available elsewhere.
- Every new URL in this batch was checked directly before editing.
