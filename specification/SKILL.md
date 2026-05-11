---
name: specification
description: Specification system for turning discovery material, prototypes, and product decisions into implementation-ready app specs while keeping implementation planning out of scope. Use when the user asks to start or bootstrap specs, create, update, or harden User Flow Specs, create or update Design System specs, review or reconcile specification consistency, plan screen and route specs, technical design, domain modeling, or decide which specification stage comes next.
---

# Specification

This skill explains a reusable specification system for turning noisy discovery material, prototypes, and user clarifications into durable app specifications.

The specification workflow moves through six stages: Bootstrap -> User Flow Specs -> Design System -> Screen and Route Specs -> Technical Design -> Domain Modeling. Earlier stages establish product truth, interaction intent, and surface behavior. Later stages translate that context into technical choices and final domain contracts that can be handed to implementation agents without becoming an implementation plan.

Implementation planning is out of scope for this skill. Do not create vertical build plans, task tickets, sprint slices, coding runbooks, or implementation sequence artifacts as part of the specification system. If the user asks for implementation planning, explain that the specification set can serve as source material for a separate implementation workflow.

It is both the shared guide and the router for specification work. Follow the user's requested scope, then load only the child workflow docs needed for the task.

For the design philosophy behind this skill, read [README.md](README.md).

## Child Workflow Docs

- **Bootstrap**: Read [bootstrapper.md](bootstrapper.md) when starting, bootstrapping, scaffolding, or setting up an app specification system.
- **User Flow Specs**: Read [user-flow-orchestrator.md](user-flow-orchestrator.md) when creating, updating, hardening, or orchestrating User Flow Specs.
- **Design System**: Read [design-system-orchestrator.md](design-system-orchestrator.md) when creating or materially updating Design System artifacts, design preferences, UI pattern guidance, visual language, interaction conventions, or screenshot-grounded UI examples.
- **Technical Design**: Read [technical-design-orchestrator.md](technical-design-orchestrator.md) when creating or materially updating Technical Design artifacts, architecture choices, code boundaries, persistence strategy, integration boundaries, testing strategy, or coding conventions.
- **Domain Modeling**: Read [domain-model-orchestrator.md](domain-model-orchestrator.md) when creating or updating final domain contracts, glossary, domain objects, schema design, state machines, permissions, audit events, model-level test plans, open decisions, or flow-to-domain traceability.
- **Consistency Review**: Read [consistency-reviewer.md](consistency-reviewer.md) when auditing, reconciling, validating, or reviewing specification artifacts for drift, conflicts, missing traceability, or handoff-readiness gaps.

Use this `SKILL.md` alone when the user asks about stage ordering, artifact boundaries, screen and route specs, or general specification guidance without asking for one of the child workflows.

When starting a specification effort from scratch, read [bootstrapper.md](bootstrapper.md) before stage-specific authoring workflows.

## Specification Profile

For repo-specific facts, read the first specification profile: `specs/SPECIFICATION_PROFILE.md`

The specification profile may define the product name, spec roots, discovery inputs, prototype URL, server policy, screenshot directory, domain-specific terminology, role names, and source-priority notes.

Keep reusable workflow rules in this skill and its child workflow docs. Keep project-specific facts in the profile.

## Specification Goal

The specification exists to give coding agents enough product, UI, domain, and technical context to implement an app deliberately.

The loop is:

1. Write or refine specification artifacts.
2. Ask coding agents to implement from those artifacts in a separate implementation workflow.
3. Test the app.
4. Improve the relevant specification artifact when implementation misses desired behavior.
5. Regenerate or revise the affected part of the app from the improved spec.

## Source Authority

- The app is evidence, not automatic product truth. It may be production, prototype, incomplete, or partially generated.
- Product discovery material is value input, not canonical specification truth.
- User Flow Specs own workflow-specific start conditions, actions, system responses, prototype observations, and flow-local edge cases.
- Design System artifacts own visual, interaction, density, hierarchy, UI pattern, and microcopy conventions.
- Screen and Route Specs own per-screen user-visible behavior, layout intent, states, and navigation.
- Technical Design artifacts own architecture choices, code boundaries, integration boundaries, persistence strategy, and engineering constraints.
- Domain Modeling artifacts own final model contracts: canonical terms, objects, lifecycle states, schema or persistence model details when applicable, state machines, permissions, source-of-truth rules, audit events, invariants, model-level test obligations, and product-level open decisions.
- If artifacts conflict, call out the conflict and prefer updating the upstream artifact that owns the relevant truth instead of inventing behavior inside downstream work.

## Artifact Conventions

Each specification stage usually has its own folder under `specs/`.

Each artifact filename is prefixed with a double-digit index:

```text
specs/<stage>/00_INDEX.md
specs/<stage>/01_FIRST_ARTIFACT.md
specs/<stage>/02_SECOND_ARTIFACT.md
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

## Stage Details

### 0. Bootstrap

Default files:

- `specs/SPECIFICATION_PROFILE.md`
- `specs/user-flows/00_INDEX.md`

Created or materially updated by the [bootstrapper workflow](bootstrapper.md).

Bootstrap establishes project-specific facts, source priority, prototype evidence settings, initial flow candidates, and open setup questions. It should not create full stage artifacts unless the user explicitly asks.

### 1. User Flow Specs

Default folder: `specs/user-flows/`

Created or materially updated by the [user-flow orchestrator workflow](user-flow-orchestrator.md).

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

### 2. Design System

Suggested folder: `specs/design-system/`

Created or materially updated by the [design-system orchestrator workflow](design-system-orchestrator.md).

This stage defines how the app should look, feel, and behave. It is not decoration; it is implementation context. It is intentionally loosely defined: the user's design preferences should control the artifact shape, depth, vocabulary, and visual direction.

Primary input comes from a planmaxxing-style interview where the user explains design preferences one decision at a time. Optional UI screenshots can ground concrete patterns such as tables, forms, pages, navigation, cards, filters, modals, empty states, and dashboards.

### 3. Screen and Route Specs

Suggested folder: `specs/screens-and-routes/`

Screen and Route Specs translate flows and Design System guidance into implementable app surfaces.

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

### 4. Technical Design

Suggested folder: `specs/technical-design/`

Created or materially updated by the [technical-design orchestrator workflow](technical-design-orchestrator.md).

Technical Design explains which architecture and engineering constraints should satisfy the product, Design System, and screen contracts.

Expected coverage:

- app architecture and major boundaries
- data persistence strategy and selected persistence technologies
- server/client/API boundaries
- authentication, authorization, and session handling
- background jobs, reminders, notifications, and retries
- file/artifact handling and external integration boundaries
- audit logging implementation obligations
- error handling and observability
- test strategy
- migration or seed-data strategy when relevant

Technical Design should consume earlier specs. It should not silently redefine product behavior, Design System guidance, or screen requirements.

Technical Design may choose SQL, an FSM library, event sourcing, or another modeling approach. It should not fully elaborate the resulting schema, state machine, or model test plan when those details belong in the final Domain Modeling stage.

### 5. Domain Modeling

Default folder: `specs/domain-model/`

Created or materially updated by the [domain-model orchestrator workflow](domain-model-orchestrator.md).

Domain Modeling is the final specification stage. It consolidates user flows, Design System implications, screen behavior, and Technical Design choices into precise model contracts that implementation agents can follow.

This stage includes the classic product-domain model: glossary, product objects, lifecycle states, actions and side effects, roles and permissions, invariants, source-of-truth rules, audit events, open decisions, and flow traceability.

It also expands technical modeling choices made in Technical Design:

- If Technical Design chooses SQL, Domain Modeling should include a schema design artifact with tables, columns, relationships, constraints, indexes, migrations or seed-data expectations, and source-of-truth notes.
- If Technical Design chooses FSMs, XState, or another state-machine approach, Domain Modeling should include elaborate state-machine artifacts with states, events, guards, actions, side effects, persistence boundaries, and a model-level test plan.
- If Technical Design chooses event sourcing, Domain Modeling should include event vocabulary, aggregate boundaries, replay rules, projection ownership, and test scenarios.

Domain Modeling should not choose the architecture on its own. If the needed modeling substrate is unknown, update Technical Design first or record an explicit open decision.

## Stage Status Language

Use simple status language in index files when useful:

- `Planned`: listed in the index but artifact may not exist yet
- `Draft`: artifact exists but has open questions or has not been reconciled with adjacent artifacts
- `Complete`: artifact has no unresolved local questions and product-level ambiguity is resolved or centralized in the correct upstream artifact
- `Needs Update`: artifact conflicts with a newer upstream spec or app decision

## Reading Order for Coding Agents

For downstream implementation work, prefer this context order:

1. Relevant Domain Modeling artifacts for final model contracts.
2. Relevant Technical Design artifacts for architecture and engineering constraints.
3. Relevant Screen and Route Specs for user-visible surfaces.
4. Design System artifacts for interaction and visual decisions.
5. Relevant User Flow Specs for workflow intent.
6. App code and current app behavior as implementation evidence.
7. Product discovery only when the specification set is silent or the user asks for deeper expectation mining.

## Open Questions and Open Decisions

During early flow or screen drafting, artifacts may contain local `## Open Questions`.

Once Domain Modeling exists, product-level and model-level questions should be centralized in the domain model's open decisions artifact. Product-level questions include ambiguity about shared terminology, domain objects, lifecycle states, state transitions, role permissions, sensitive gates, source-of-truth boundaries, audit requirements, artifact ownership, notification side effects, deadlines, reminders, settings governance, or behavior that affects multiple flows.

Model-level questions include unresolved schema ownership, persistence representation, state-machine event shape, guard semantics, derived-data refresh rules, audit metadata, and model test coverage.

After product-level questions have been centralized, affected User Flow Specs should point to the centralized artifact. When no flow-local questions remain, use:

```md
Product-level open decisions are tracked in [09_OPEN_DECISIONS.md](../domain-model/09_OPEN_DECISIONS.md).
This flow has no remaining flow-local open questions.
```

Flow-local questions may remain in a User Flow Spec only when they are narrow to that flow and do not affect shared product behavior.
