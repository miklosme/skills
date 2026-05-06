# Domain Model Orchestrator

Use this workflow to create or materially update Domain and State Model artifacts for an app specification system.

The Domain and State Model is the shared product contract that implementation agents consume after User Flow Specs exist. It defines product nouns, states, transitions, permissions, side effects, invariants, source-of-truth boundaries, audit events, and centralized product-level open decisions.

It is not a database schema, API design, technical architecture, or UI language spec.

## Specification Profile

Read the first specification profile: `specs/SPECIFICATION_PROFILE.md`

Use the profile for product name, spec roots, discovery inputs, prototype URL, server policy, screenshot directory, domain-specific terminology, role names, and source-priority notes. If no profile exists, use the defaults in [SKILL.md](SKILL.md).

## Resolve the Target Scope

From the user's request, identify whether the task is:

- create the full Domain and State Model artifact set
- update one named domain artifact
- resolve or add product-level open decisions
- reconcile a domain-model conflict caused by a user-flow update
- update flow-to-domain traceability
- migrate User Flow Spec `Open Questions` into the domain model

If the request names a specific artifact, focus on that artifact and update the minimum affected set. If the request asks for implementation readiness, state modeling, shared terminology, permissions, source of truth, audit behavior, or product-level open questions without naming an artifact, inspect the full domain model set and perform the smallest useful domain-model update.

If the target cannot be confidently matched to a domain artifact or cross-flow concern, ask one concise clarification question before continuing.

## Expected Artifact Set

Default folder: `specs/domain-model/`

- `00_INDEX.md`
- `01_GLOSSARY.md`
- `02_DOMAIN_OBJECTS.md`
- `03_STATE_MACHINES.md`
- `04_ACTIONS_AND_SIDE_EFFECTS.md`
- `05_ROLES_AND_PERMISSIONS.md`
- `06_BUSINESS_RULES_AND_INVARIANTS.md`
- `07_SOURCE_OF_TRUTH_AND_DERIVED_DATA.md`
- `08_AUDIT_EVENTS.md`
- `09_OPEN_DECISIONS.md`
- `10_FLOW_TO_DOMAIN_TRACEABILITY.md`

Use this title format:

```md
# Domain Model: <Name>
```

When creating the full set, create all listed files. When updating one artifact, avoid broad churn in unrelated artifacts unless the product contract would become inconsistent.

## Grounding Order

1. Read the specification profile, if present.
2. Inspect the domain model folder if it exists.
3. Inspect the user-flow index.
4. Read current User Flow Specs.
5. Read relevant discovery material.
6. Inspect prototype code, mock data, screenshots, or browser behavior only when they clarify product language, states, routes, labels, or conflicts.

User Flow Specs are the strongest source for current workflow understanding. Discovery documents are important input, but may be verbose, stale, technical, or contradictory. Prototype behavior is useful evidence, but it is not automatically authoritative.

## Precision Bias

Optimize for implementation-useful precision.

Use the specification profile's domain precision hints when present. Do not simplify away states, blockers, metadata records, source-of-truth boundaries, validation gates, permissions, or audit requirements that implementation agents would need.

## Artifact Guidance

### 00_INDEX.md

Explain the purpose of the domain model folder, reading order, and relationship to User Flow Specs. Link each artifact.

### 01_GLOSSARY.md

Define canonical terms. Include explicit "Do not confuse with" notes when terms are close but not interchangeable.

Good glossary entries include:

- term
- definition
- related terms
- do-not-confuse notes
- where the term matters, if useful

### 02_DOMAIN_OBJECTS.md

Define product-level objects, not database tables.

For each object, include:

- purpose
- source of truth or owner
- important user-visible fields or metadata
- relationships to other objects
- lifecycle ownership when useful
- key rules or non-goals

### 03_STATE_MACHINES.md

Define valid states and transitions for relevant domain objects.

For each state machine, include:

- state list with meanings
- allowed transitions
- actor or system trigger
- preconditions and blockers
- side effects that belong in `04_ACTIONS_AND_SIDE_EFFECTS.md`
- terminal states and reopen or loopback rules
- known open decisions

### 04_ACTIONS_AND_SIDE_EFFECTS.md

Define product actions and what changes when they occur.

For each action, include:

- actor
- preconditions
- state changes
- objects created or updated
- notifications or tasks created
- artifacts or references created
- audit events created
- blocked or error states

### 05_ROLES_AND_PERMISSIONS.md

Define product-level permission rules for the roles in the specification profile and any external participants relevant to the product.

Prefer a compact permission matrix plus notes. Distinguish:

- route or screen visibility
- data visibility
- action availability
- execution-time authorization
- assignment scope
- sensitive actions requiring confirmation or audit

Do not invent a custom permission-builder product unless the user explicitly scopes it.

### 06_BUSINESS_RULES_AND_INVARIANTS.md

Collect rules that must always hold across implementation.

Focus on cross-flow product obligations, not code architecture or implementation mechanics.

### 07_SOURCE_OF_TRUTH_AND_DERIVED_DATA.md

Define where important values come from and how downstream data consumes them.

Include:

- canonical source
- copied or snapshotted values
- derived display/reporting values
- controlled refresh rules
- historical preservation rules
- values that must not be silently recomputed

### 08_AUDIT_EVENTS.md

Define audit-sensitive events across flows.

For each event, include:

- event name
- trigger
- actor
- target object
- required metadata
- related state transition
- affected flow(s)

Use stable event names when possible. Do not overfit event naming to code identifiers unless the repo already established a pattern.

### 09_OPEN_DECISIONS.md

Centralize product-level unresolved questions.

Use stable decision IDs such as `DM-001`, `DM-002`, etc. For each decision, include:

- decision question
- status: `Open`, `Proposed`, or `Resolved`
- affected flows
- affected domain artifacts
- why it matters for implementation
- evidence and source conflict, if any
- conservative default or recommendation, if known
- owner or needed input, if known

Prefer one clear product decision per item. Do not create vague buckets when separate role, state, source-of-truth, audit, or lifecycle decisions affect different implementation gates.

### 10_FLOW_TO_DOMAIN_TRACEABILITY.md

Map each User Flow Spec to the domain model pieces it consumes.

For each flow, include:

- domain objects
- state machines or states
- major actions
- permissions
- audit events
- business invariants
- source-of-truth rules
- open decision IDs

This artifact helps implementation agents load the right context for a vertical build slice.

## Open Decisions Migration

When creating or materially updating the open decisions artifact, review all User Flow Specs.

Do not mine only literal `## Open Questions` sections. Also inspect:

- `Prototype Observations`
- `Missing or unclear`
- `Inconsistent or misleading`
- `Implementation Notes for Product Behavior`
- source conflicts between neighboring flows
- discovery-doc conflicts
- current prototype gaps that affect shared product behavior

Move product-level open questions into the open decisions artifact.

After moving product-level decisions, update each affected User Flow Spec's `## Open Questions` section. Use this wording when no flow-local questions remain:

```md
Product-level open decisions are tracked in [09_OPEN_DECISIONS.md](../domain-model/09_OPEN_DECISIONS.md).
This flow has no remaining flow-local open questions.
```

Keep a flow-local question only when it is genuinely narrow to that flow and does not affect shared terminology, states, permissions, source-of-truth rules, audit events, artifacts, deadlines, reminders, or cross-flow behavior.

## Decision Loop

Before drafting or materially changing domain artifacts, run a bounded decision loop.

For material decision questions, consult a high-reasoning general subagent only when the user or current instructions explicitly permit subagent delegation. Otherwise, do the decision pass locally and record the reasoning internally.

Decision questions should be evidence-bound. They should:

- separate evidence from inference
- identify source conflicts
- recommend conservative interpretations
- preserve product distinctions implementation agents need
- avoid inventing unsupported features, roles, states, or integrations
- label adjacent future ideas as follow-ups

Use this internal decision shape:

- Question
- Recommendation
- Confidence
- Evidence Used
- Inferences
- Must-Preserve Product Details
- Alternative Considered
- Impact if Wrong
- Open Decision Entry, if unresolved

Stop the loop when:

1. The artifact boundary is clear.
2. Shared terms, objects, states, actions, permissions, invariants, source-of-truth rules, audit events, and open decisions are distinguishable.
3. Major source conflicts have been addressed or captured in open decisions.
4. More investigation is likely to add detail rather than change the product contract.

If ambiguity blocks a useful product contract, ask the user. If it does not block drafting, make the narrowest conservative assumption and track the unresolved decision.

## Drafting Rules

Use the artifact boundaries above.

Keep writing implementation-facing but product-level. Avoid:

- database schema proposals
- ORM models
- API route design
- component architecture
- provider or vendor choices
- UI styling rules
- test implementation details

It is acceptable to mention a technical constraint only when it changes user-visible behavior or product obligations.

Preserve user-flow links where useful, but do not create a default `Sources` section in every artifact. Product discovery documents are input material, not canonical product truth.

## Final Review

After drafting, perform a review pass. Consult a high-reasoning reviewer subagent only when the user or current instructions explicitly permit subagent delegation. Otherwise, review locally.

Check:

- artifact boundary compliance
- consistency with User Flow Specs
- unsupported product claims
- lost precision around states, gates, metadata, permissions, source-of-truth rules, or audit requirements
- contradiction between domain artifacts
- duplicated discovery prose
- unresolved product questions captured in open decisions
- flow `Open Questions` sections updated when product-level decisions were centralized
- traceability correctness

Apply only concrete, grounded fixes.

## Final Response

Summarize:

- domain model files changed
- User Flow Specs consulted or updated
- product-level open decisions added, resolved, or moved
- whether browser exploration was used
- small prototype fixes, if any
- important source conflicts or conservative decisions
- recommended next artifact or implementation-readiness follow-up
