---
name: app-spec-bootstrapper
description: Initialize a new app specification effort. Use when the user asks to start, bootstrap, scaffold, or set up an app specification system for a repo, prototype, existing app, or planned app before detailed user-flow, domain-model, UI, screen, technical, or implementation-slice specs exist.
---

# App Spec Bootstrapper

Use this skill to set up the first durable specification structure for an app.

The bootstrapper is the front door. It creates project-specific context and indexes so the stage-specific skills can work well later. It should not write full User Flow Specs, Domain Model artifacts, UI Language specs, Screen and Route Specs, or Technical Design unless the user explicitly expands the scope.

## Bootstrap Goal

Create the minimum useful specification system:

- `docs/SPECIFICATION_PROFILE.md`
- `docs/user-flows/00_INDEX.md`
- a screenshot or evidence directory when a prototype is configured
- optionally `docs/domain-model/00_INDEX.md` when shared states, roles, permissions, or lifecycle terms are already visible

Then recommend the next 1-3 specification steps.

## First Move

Inspect before interviewing when a repo exists.

Use `rg` and `rg --files` to look for:

- existing `docs/` specs, discovery material, PRDs, notes, tickets, or generated plans
- route, page, screen, component, fixture, mock data, and seed-data files
- package scripts, dev-server commands, README setup notes, and test commands
- environment hints for local URLs or ports
- screenshots, recordings, or browser automation workflows
- existing product nouns, roles, status labels, lifecycle states, and action labels

If the user provided external discovery material or a prototype URL, include it in the evidence inventory.

## Questions

Ask only for decisions that materially change the scaffolding and cannot be inferred from the repo or user request.

Prefer one concise batch of at most five questions. Ask fewer when possible.

High-value bootstrap questions:

- What is the product or app name?
- What outcome should the first implementation pass support?
- Where is the prototype or running app, and who owns starting the dev server?
- Which discovery inputs should be treated as important source material?
- Which roles, domain terms, or product distinctions must not be simplified away?

If the user wants momentum and the missing answer is not blocking, make a conservative placeholder and record it in `Open Bootstrap Questions`.

## Source Priority

Set an explicit source-priority policy in the profile.

Default priority:

1. Direct user decisions in the current thread.
2. Existing specification artifacts in the repo.
3. Current app or prototype behavior as evidence.
4. Product discovery material as input.
5. Code names and generated fixtures as hints.

Make clear that app behavior and discovery material are evidence, not automatic product truth.

## Profile Template

Create `docs/SPECIFICATION_PROFILE.md` with this structure, adapting section details to the repo:

```md
# Specification Profile: <Product Name>

## Product Summary

## Specification Roots

- User Flow Specs: `docs/user-flows/`
- Domain and State Model: `docs/domain-model/`
- UI Language: `docs/ui-language/`
- Screen and Route Specs: `docs/screens-and-routes/`
- Feature Design: `docs/feature-design/`
- Technical Design: `docs/technical-design/`
- Implementation Slices: `docs/implementation-slices/`

## Discovery Inputs

## Prototype and App Evidence

- Prototype URL:
- Dev server policy:
- Browser workflow:
- Screenshot directory:

## Source Priority

## Product Language

- Product terms:
- Roles:
- Statuses and lifecycle terms:
- Distinctions to preserve:

## Initial Specification Plan

## Open Bootstrap Questions
```

Use `Not configured yet` instead of inventing URLs, scripts, or folders. Use `None found yet` when reconnaissance found no source for a section.

## User Flow Index Template

Create `docs/user-flows/00_INDEX.md` with this structure:

```md
# User Flow Specs

## Purpose

This folder captures meaningful work from the user's perspective. User Flow Specs define outcomes, actors, triggers, preconditions, user actions, system responses, edge cases, business rules, prototype observations, and flow-local open questions.

## Reading Order

1. Read this index.
2. Read the relevant User Flow Spec.
3. Read the Domain and State Model when it exists.
4. Read prototype evidence or discovery inputs only when the spec points to them or when resolving ambiguity.

## Flow Index

| # | Flow | Status | Artifact | Notes |
|---|------|--------|----------|-------|
| 1 | <Flow Name> | Planned | `01_<FLOW_NAME>.md` | <Why this flow matters or source evidence> |

## Candidate Flows Not Yet Indexed

## Bootstrap Notes
```

Index only flows with enough evidence to be useful. Put weaker guesses under `Candidate Flows Not Yet Indexed`.

## Optional Domain Index

Create `docs/domain-model/00_INDEX.md` only when the app clearly needs shared product contracts early, or when the user asks for implementation readiness beyond a single flow.

Use this structure:

```md
# Domain and State Model

## Purpose

This folder captures shared product contracts consumed by implementation agents: glossary, domain objects, state machines, actions and side effects, roles and permissions, business rules, source-of-truth boundaries, audit events, open decisions, and flow traceability.

## Reading Order

1. `01_GLOSSARY.md`
2. `02_DOMAIN_OBJECTS.md`
3. `03_STATE_MACHINES.md`
4. `04_ACTIONS_AND_SIDE_EFFECTS.md`
5. `05_ROLES_AND_PERMISSIONS.md`
6. `06_BUSINESS_RULES_AND_INVARIANTS.md`
7. `07_SOURCE_OF_TRUTH_AND_DERIVED_DATA.md`
8. `08_AUDIT_EVENTS.md`
9. `09_OPEN_DECISIONS.md`
10. `10_FLOW_TO_DOMAIN_TRACEABILITY.md`

## Artifact Index

| Artifact | Status | Purpose |
|----------|--------|---------|
| `01_GLOSSARY.md` | Planned | Shared terms and definitions. |
| `02_DOMAIN_OBJECTS.md` | Planned | Product objects, ownership, and relationships. |
| `03_STATE_MACHINES.md` | Planned | Lifecycle states and transitions. |
| `04_ACTIONS_AND_SIDE_EFFECTS.md` | Planned | User and system actions plus side effects. |
| `05_ROLES_AND_PERMISSIONS.md` | Planned | Actor capabilities and restrictions. |
| `06_BUSINESS_RULES_AND_INVARIANTS.md` | Planned | Cross-flow rules that must remain true. |
| `07_SOURCE_OF_TRUTH_AND_DERIVED_DATA.md` | Planned | Canonical data, derived values, and ownership. |
| `08_AUDIT_EVENTS.md` | Planned | Audit-worthy events and metadata. |
| `09_OPEN_DECISIONS.md` | Planned | Central product-level decisions. |
| `10_FLOW_TO_DOMAIN_TRACEABILITY.md` | Planned | Mapping from flows to domain contracts. |

## Bootstrap Notes
```

Do not create the planned domain artifacts during bootstrap unless the user explicitly asks.

## File and Folder Rules

- Preserve existing specs and indexes. Patch gaps instead of replacing user work.
- Keep reusable workflow rules out of the target repo. Put only project-specific facts in `docs/SPECIFICATION_PROFILE.md`.
- Prefer planned links over empty placeholder files.
- Use double-digit file prefixes for stage artifacts.
- Create directories only when the bootstrap output needs them.
- Record uncertainty as bootstrap questions instead of silently deciding product behavior.

## Handoff

End with:

- files created or updated
- evidence sources found
- assumptions made
- open bootstrap questions
- recommended next skill, usually `app-spec-user-flow-orchestrator`, `app-spec-domain-model-orchestrator`, or `app-spec-guide`

If the next step is obvious, recommend the first concrete User Flow Spec to draft.
