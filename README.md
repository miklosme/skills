# Agent Skills

A collection of agent skills that extend capabilities across planning, development, and tooling.

## Skills

- `app-specification-guide`: shared guidance for turning prototypes, discovery material, and user decisions into implementation-ready app specification artifacts.
- `user-flow-spec-orchestrator`: creates or materially updates User Flow Specs from indexes, discovery material, prototype evidence, and product decisions.
- `domain-model-orchestrator`: creates or updates Domain and State Model artifacts, including glossary, states, permissions, audit events, and traceability.
- `spec-consistency-reviewer`: reviews specification sets for conflicts, drift, misplaced decisions, and implementation-readiness gaps.
- `playwright-cli`: browser automation and Playwright CLI workflow reference.
- `planmaxxing`: relentless one-question-at-a-time planning and design interview.

## Codex Agents

- `.codex/agents/prototype_browser_explorer.toml`: focused subagent for static prototype reconnaissance plus browser evidence reports.

Keep reusable workflow rules in these skills. Put app-specific facts in a target repo profile such as `docs/SPECIFICATION_PROFILE.md` or `docs/spec-system/PROJECT_PROFILE.md`.
