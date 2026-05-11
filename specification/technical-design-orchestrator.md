# Technical Design Orchestrator

Use this workflow to create or materially update Technical Design artifacts for an app
specification system.

Technical Design translates user flows, design, screens, and current implementation evidence
into technical architecture. It tells coding agents where behavior should live, how data
moves, what persistence boundaries exist, and which engineering contracts must be preserved.

It is not a product behavior spec, Design System spec, screen spec, migration plan, cloud
provisioning runbook, domain model, or task ticket list.

## Specification Profile

Read the first specification profile: `specs/SPECIFICATION_PROFILE.md`

Use the profile for product name, spec roots, discovery inputs, source-priority notes, prototype
or app server policy, role names, domain precision hints, and project-specific terminology. If no
profile exists, use the defaults in [SKILL.md](SKILL.md).

## Resolve the Target Scope

From the user's request, identify whether the task is:

- create the Technical Design stage from scratch
- update one technical-design section or artifact
- reconcile technical design with current implementation evidence
- move durable architecture or convention guidance out of an agent runbook
- review handoff readiness for a product area
- record a user-confirmed technical decision

If the request names a specific section, make the smallest affected update. If it asks for broad
technical design, inspect the full upstream spec set and current implementation before drafting.
Ask one concise clarification question only when the target architecture decision cannot be
inferred and a wrong assumption would materially constrain implementation.

## Grounding Order

1. Read the specification profile, if present.
2. Inspect existing `specs/technical-design/` artifacts and index.
3. Read relevant User Flow Specs, Design System artifacts, and Screen and Route Specs.
4. Read existing Domain Modeling artifacts only when updating an existing spec set or reconciling drift.
5. Inspect the current app implementation, especially routes, layouts, schema, actions, workflows,
   auth/session code, provider helpers, env config, tests, and package versions.
6. Read project agent runbooks such as `docs/CODING_INSTRUCTIONS.md` for operational guardrails and
   existing conventions.
7. Read framework or provider docs only when version-specific behavior affects the technical
   design.
8. Use discovery material only when upstream specs are silent or the user asks for deeper source
   mining.

The app is strong implementation evidence, not automatic product truth. If app behavior conflicts
with upstream specs, Technical Design should name the scaffold gap or implementation risk instead
of silently redefining the product.

## Evidence Ledger

Before drafting, build an internal ledger. Do not write it to the repo unless the user explicitly
asks for a preserved reasoning artifact.

Include:

- source specs read and important upstream constraints
- current stack, package versions, framework mode, build settings, and runtime assumptions
- route groups, layouts, server/client component boundaries, and navigation patterns
- data layer, server actions, route handlers, API boundaries, and cache/query patterns
- database schema shape, tenant isolation, authorization checks, migrations, and seed-data rules
- workflow/background job architecture, retries, idempotency, and failure visibility
- file/artifact ownership, storage boundaries, and signed URL rules
- external integrations, environment variables, secrets, and provider isolation
- observability, audit events, sensitive data handling, and recovery expectations
- testing and verification expectations
- known scaffold risks, implementation gaps, and user-confirmed locked choices

Use the ledger to separate evidence, inference, decisions, and open technical risks.

## Artifact Shape

Default folder: `specs/technical-design/`

Prefer:

- `00_INDEX.md`
- `01_TECHNICAL_DESIGN.md`

Use one main artifact for small or medium apps. Split into focused artifacts only when a single
file becomes hard for implementation agents to use, such as:

- `01_ARCHITECTURE_AND_BOUNDARIES.md`
- `02_DATA_AND_PERSISTENCE.md`
- `03_INTEGRATIONS_AND_WORKFLOWS.md`
- `04_TESTING_AND_OPERATIONS.md`

Do not create placeholder artifacts just to fill the stage. Use planned links in the index when
future technical guidance is useful but not yet known.

## Suggested Index Structure

Use or adapt this structure:

```md
# Technical Design: Index

## Purpose

## Reading Order

## Source Inputs Read

## Locked Technical Choices

## Scope Boundary
```

The index should make the design's authority clear: Technical Design owns technical
architecture and engineering contracts, but not product behavior, visual design, screen
requirements.

## Suggested Main Artifact Sections

Use only sections that fit the app and the user's request.

```md
# Technical Design

## Purpose

## Current Scaffold Evidence

## Architecture Overview

## Code Boundaries

## Tenant Isolation and Authorization

## Persistence Design

## Audit and Event Timeline

## Workflows and Background Jobs

## Routes and Navigation

## Data Fetching, Mutations, and Forms

## Files and Artifacts

## External Integrations

## Errors, Observability, and Recovery

## Testing and Verification

## Domain Modeling Handoff

## Coding Conventions

## Open Technical Risks
```

Adapt section names to the project's stack. Include diagrams only when they clarify actual
boundaries for implementation agents.

## What Technical Design Should Decide

Technical Design may define:

- major code boundaries and ownership by folder/module
- server/client/API boundaries and data loading patterns
- persistence strategy, schema direction, snapshots, history, idempotency, and seed rules
- auth, authorization, tenant isolation, and execution-time permission checks
- background job/workflow ownership, retries, outbox/intent records, and failure states
- file storage, artifact metadata, signed URL handling, and provider isolation
- audit logging obligations and sensitive-data redaction rules
- observability, error taxonomy, correlation IDs, and recovery behavior
- testing levels, required checks, and where deterministic business logic should live
- durable coding conventions that implementation agents should preserve

Technical Design should not invent product states, role capabilities, visual language, screen
layouts, or user-visible workflow behavior. It should also avoid fully elaborating schema,
state-machine, or event-model details that belong in Domain Modeling. Update the artifact that
owns the relevant truth instead.

## Runbook Boundary

If reconciling Technical Design with an agent runbook:

- keep durable architecture, code boundaries, data patterns, and coding conventions in Technical
  Design
- keep operational agent rules in the runbook, such as which commands to run, who starts servers,
  whether agents may run migrations, cloud provisioning restrictions, git rules, browser auth
  helpers, reviewer workflows, and wrap-up scripts
- make the runbook point to Technical Design instead of duplicating long convention lists

This prevents drift while keeping high-risk operational guardrails easy to find.

## Locked Choices and Risks

Only mark a technical choice as locked when it is:

- user-confirmed
- already established in the implementation and compatible with upstream specs
- required by deployed infrastructure or provider contracts
- necessary to preserve a known security, compliance, or data-integrity boundary

When a current scaffold choice is risky or temporary, name it as a scaffold risk instead of
canonizing it. Include the desired direction only when it follows from upstream specs, user
decisions, or implementation evidence.

## Open Questions

Use open questions sparingly. Prefer resolving implementation details from the current app and
upstream specs.

Keep product-level and model-level questions in the Domain Modeling open decisions artifact. Keep design
questions in Design System artifacts. Keep route or screen behavior questions in Screen and Route
Specs.

Technical open questions should be about implementation constraints, such as provider choice,
storage ownership, migration sequencing, deployment topology, runtime limits, or test strategy.

## Final Review

Before finishing, check:

- the technical design consumes upstream specs without redefining product behavior
- current implementation evidence is separated from intended architecture
- locked choices and scaffold risks are distinct
- code boundaries are specific enough for implementation agents
- data ownership, source of truth, audit, auth, error, and testing obligations are explicit
- agent runbook content is not duplicated except as a short pointer or boundary note
- index links match files that exist or are intentionally planned
- the Domain Modeling handoff is explicit when technical choices require schema, state-machine, event, or model-test artifacts

## Final Response

Summarize:

- technical-design files changed
- upstream specs or runbooks changed, if any
- current implementation evidence used
- locked choices, scaffold risks, or open technical questions added
- recommended next workflow, usually Domain Modeling or Consistency Review
