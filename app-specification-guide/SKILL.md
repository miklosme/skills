---
name: app-specification-guide
description: Use when a user asks about an app specification system, implementation-readiness specs, user-flow hardening, domain modeling, UI language, screen and route specs, technical design, component design, feature design, implementation slices, or what specification stage should come before or after another stage.
---

# App Specification Guide

This skill explains a reusable specification system for turning noisy discovery material, prototypes, and user clarifications into implementation-ready product context.

It is a context skill, not an autonomous orchestrator. Follow the user's requested scope and use a stage-specific skill when one exists.

## Specification Profile

For repo-specific facts, read the first specification profile that exists:

1. `docs/SPECIFICATION_PROFILE.md`
2. `docs/spec-system/PROJECT_PROFILE.md`

The specification profile may define the product name, spec roots, discovery inputs, prototype URL, server policy, screenshot directory, domain-specific terminology, role names, and source-priority notes.

Keep reusable workflow rules in skills. Keep project-specific facts in the profile.

## Specification Goal

The specification exists to give coding agents enough product, UI, and technical context to implement an app deliberately.

The loop is:

1. Write or refine specification artifacts.
2. Ask coding agents to implement from those artifacts.
3. Test the app.
4. Improve the relevant specification artifact when implementation misses desired behavior.
5. Regenerate or revise the affected part of the app.

## Source Authority

- The app is evidence, not automatic product truth. It may be production, prototype, incomplete, or partially generated.
- Product discovery material is value input, not canonical specification truth.
- User Flow Specs own workflow-specific start conditions, actions, system responses, prototype observations, and flow-local edge cases.
- Domain and State Model artifacts own shared terminology, objects, states, transitions, permissions, source-of-truth rules, audit events, and product-level open decisions.
- UI Language artifacts own visual, interaction, density, hierarchy, and microcopy conventions.
- Screen and Route Specs own per-screen user-visible behavior, layout intent, states, and navigation.
- Technical Design artifacts own implementation architecture and engineering contracts.
- If artifacts conflict, call out the conflict and prefer updating the upstream product or UI artifact instead of inventing behavior inside downstream technical work.

## Artifact Conventions

Each specification stage usually has its own folder under `docs/`.

Each artifact filename is prefixed with a double-digit index:

```text
docs/<stage>/00_INDEX.md
docs/<stage>/01_FIRST_ARTIFACT.md
docs/<stage>/02_SECOND_ARTIFACT.md
```

Every stage folder needs a `00_`-prefixed index file, usually `00_INDEX.md`.

The stage index should:

- link to every planned artifact in the stage
- explain the role of each artifact
- give an overview of the application from that stage's perspective
- define the reading order for implementation agents
- be allowed to exist before every linked artifact exists

Index links are planning contracts:

- link exists and file is missing: the artifact is planned and ready to be written
- link exists and file exists: no creation work is needed unless the user asks for an update
- file exists but is not in the index: reconcile before relying on it as part of the stage

## Specification Stages

### 1. User Flow Specs

Default folder: `docs/user-flows/`

Created or materially updated by `user-flow-spec-orchestrator`.

User Flow Specs describe meaningful work from the user's perspective. They are flow-centered, outcome-grounded, and pair user actions with system responses.

Use this structure for new User Flow Specs and preserve it when materially updating existing specs:

```md
# User Flow: <Name>

## Outcome

## Actors

## Trigger

## Preconditions

## Main Flow

## Alternate Paths and Edge Cases

## Business Rules

## Supporting Capabilities

## Prototype Observations

- Clearly shown:
- Missing or unclear:
- Inconsistent or misleading:

## Implementation Notes for Product Behavior

## Open Questions
```

`Main Flow` should usually be numbered steps that pair user actions with system responses.

`Prototype Observations` describes what the app currently shows or fails to show. It is not a statement of what production must do.

Do not add default `Sources` or `Discovery Notes` sections. Discovery material is input, not the canonical artifact.

### 2. Domain and State Model

Default folder: `docs/domain-model/`

Created or materially updated by `domain-model-orchestrator`.

The Domain and State Model turns user flows into shared product contracts for implementation agents: glossary, product objects, state machines, actions and side effects, roles and permissions, invariants, source-of-truth rules, audit events, open decisions, and flow traceability.

Use it as the canonical source for cross-flow product behavior once it exists.

### 3. UI Language and Interaction System

Suggested folder: `docs/ui-language/`

This stage defines how the app should look, feel, and behave. It is not decoration; it is implementation context.

Expected coverage:

- product personality, density, and hierarchy
- navigation and page layout rules
- tables, lists, filtering, sorting, and drill-down behavior
- forms, validation, blocked states, and destructive confirmations
- modals, drawers, tabs, panels, and activity timelines
- status badges, priority language, confidence indicators, and color semantics
- empty, loading, error, warning, success, and read-only states
- responsive behavior and accessibility expectations
- icon, button, control, and microcopy conventions
- visual patterns the app should avoid

### 4. Screen and Route Specs

Suggested folder: `docs/screens-and-routes/`

Screen and Route Specs translate flows, domain contracts, and UI language into implementable app surfaces.

Expected coverage:

- route or navigation location
- primary actors and role variants
- user goal and related user flows
- visible data and source-of-truth references
- layout regions and information hierarchy
- available actions and blocked actions
- state, empty, loading, error, read-only, and permission-denied behavior
- validation and confirmation behavior
- navigation exits and handoffs to other screens
- responsive expectations
- screenshots or app evidence when useful

### 5. Feature and Capability Design

Suggested folder: `docs/feature-design/`

Feature Design is useful for cross-cutting capabilities larger than one screen but narrower than the whole app.

Expected coverage:

- capability outcome
- source user flows and domain rules
- actors and permissions
- affected screens or routes
- user-visible states and edge cases
- validation, notification, audit, or reporting behavior
- exclusions and non-goals
- open decisions

Use this stage sparingly. Prefer User Flow Specs for workflow behavior, Domain and State Model artifacts for shared product contracts, and Screen and Route Specs for screen-level behavior.

### 6. Technical Design

Suggested folder: `docs/technical-design/`

Technical Design explains how implementation should satisfy product, domain, UI, and screen contracts.

Expected coverage:

- app architecture and major boundaries
- data persistence strategy
- server/client/API boundaries
- authentication, authorization, and session handling
- background jobs, reminders, notifications, and retries
- file/artifact handling and external integration boundaries
- audit logging implementation obligations
- error handling and observability
- test strategy
- migration or seed-data strategy when relevant

Technical Design should consume earlier specs. It should not silently redefine product behavior, UI language, or screen requirements.

### 7. Component Design

Suggested folder: `docs/component-design/`

Component Design is useful when repeated UI patterns need precise implementation guidance. It should come after UI Language and at least the relevant Screen and Route Specs.

Expected coverage:

- reusable component purpose
- where it appears
- product-level props or configuration
- visual states and interaction states
- accessibility expectations
- data and permission assumptions
- anti-patterns and non-goals

Avoid component design for one-off screens unless the user explicitly asks for it.

### 8. Implementation Slices

Suggested folder: `docs/implementation-slices/`

Implementation slices are vertical build plans for coding agents. They group the minimum product, UI, domain, and technical context needed to implement and test one meaningful increment.

Expected coverage:

- slice outcome
- source specs to read
- included routes, actions, states, and roles
- excluded scope
- acceptance criteria
- seed data or fixtures needed
- testing expectations
- known open decisions or blocked assumptions

Prefer vertical slices over broad feature buckets.

## Stage Status Language

Use simple status language in index files when useful:

- `Planned`: listed in the index but artifact may not exist yet
- `Draft`: artifact exists but has open questions or has not been reconciled with adjacent artifacts
- `Complete`: artifact has no unresolved local questions and product-level ambiguity is resolved or centralized in the correct upstream artifact
- `Needs Update`: artifact conflicts with a newer upstream spec or app decision

## Reading Order for Coding Agents

For implementation work, prefer this context order:

1. The relevant implementation slice, if present.
2. Relevant Screen and Route Specs.
3. UI Language artifacts for interaction and visual decisions.
4. Relevant Feature and Capability Design artifacts, if present.
5. Domain and State Model artifacts for shared product behavior.
6. Relevant User Flow Specs for workflow intent.
7. App code and current app behavior as implementation evidence.
8. Product discovery only when the specification set is silent or the user asks for deeper expectation mining.

## Open Questions and Open Decisions

During early flow drafting, User Flow Specs may contain `## Open Questions`.

Once a Domain and State Model exists, product-level questions should be centralized in the domain model's open decisions artifact. Product-level questions include ambiguity about shared terminology, domain objects, lifecycle states, state transitions, role permissions, sensitive gates, source-of-truth boundaries, audit requirements, artifact ownership, notification side effects, deadlines, reminders, settings governance, or behavior that affects multiple flows.

After product-level questions have been centralized, affected User Flow Specs should point to the centralized artifact. When no flow-local questions remain, use:

```md
Product-level open decisions are tracked in [09_OPEN_DECISIONS.md](../domain-model/09_OPEN_DECISIONS.md).
This flow has no remaining flow-local open questions.
```

Flow-local questions may remain in a User Flow Spec only when they are narrow to that flow and do not affect shared product behavior.
