# Specification Skill Philosophy

This skill exists to make product, UI, technical, and domain intent explicit before implementation begins. It should create artifacts that are precise enough for coding agents to act on, but it should not become a project-management or implementation-planning system.

## Design Decisions

### Specification Stops Before Implementation

Specification artifacts describe what must be true and which technical contracts must be honored. They do not define vertical build slices, task tickets, sprint plans, coding runbooks, or step-by-step implementation sequences.

The handoff boundary is intentional:

- specifications preserve product and model truth
- implementation workflows decide how to sequence coding work
- test results can feed back into specs when the app misses desired behavior

### Stage Order Moves From Intent To Model

The stage order is:

1. Bootstrap
2. User Flow Specs
3. Design System
4. Screen and Route Specs
5. Technical Design
6. Domain Modeling

This order starts with user intent and visible behavior, then chooses technical boundaries, then finishes with the domain contracts that implementation agents need most. Domain Modeling is last because it can only fully describe schemas, state machines, event models, and model test plans after Technical Design has chosen the relevant substrate.

### Domain Modeling Is A Final Contract

Domain Modeling is more than a glossary and more specific than generic product analysis. It is the final specification pass that turns flows, screens, and technical choices into durable model contracts.

Examples:

- If Technical Design chooses SQL, Domain Modeling should include a schema design artifact with tables, relationships, constraints, indexes, source-of-truth boundaries, migration expectations, and seed-data notes.
- If Technical Design chooses FSMs, XState, or a similar workflow model, Domain Modeling should include detailed machine designs with states, events, guards, actions, invoked services, persistence boundaries, and a model-level test plan.
- If Technical Design chooses event sourcing or an outbox pattern, Domain Modeling should include event vocabulary, aggregate ownership, idempotency, replay behavior, projections, and test scenarios.

Domain Modeling should not choose the architecture on its own. If a model needs SQL, XState, event sourcing, or another substrate and Technical Design has not made that decision, update Technical Design or record an open decision first.

### Product Truth Has Owners

Each artifact family owns a different kind of truth:

- User Flow Specs own workflow intent, actions, system responses, and flow-local edge cases.
- Design System owns visual and interaction language.
- Screen and Route Specs own per-screen behavior, layout intent, states, and navigation.
- Technical Design owns architecture choices and engineering constraints.
- Domain Modeling owns final model contracts, including schema, state-machine, source-of-truth, permission, audit, invariant, and model-test obligations.

When artifacts conflict, fix the artifact that owns the truth instead of patching the conflict downstream.

### Discovery And The App Are Evidence

Discovery notes, generated prototypes, and current app behavior are evidence. They are not automatically canonical. The skill should preserve useful evidence while making clear which decisions are user-confirmed, inferred, provisional, or unresolved.

### Precision Beats Premature Brevity

Specs are allowed to be dense when the product needs precision. Do not simplify away lifecycle states, blockers, metadata, permissions, audit obligations, source-of-truth rules, schema constraints, state-machine guards, or model tests just to make artifacts shorter.

Clarity matters, but only after the product and model contract has survived the edit.

## Boundary Checklist

Before adding or changing a workflow rule, ask:

- Does this describe product, UI, technical, or model truth?
- Does this help implementation agents understand constraints without telling them how to schedule coding work?
- Is this owned by the right stage?
- If this is schema, FSM, event, or model-test detail, has Technical Design already chosen the substrate?
- If this is a task list, implementation sequence, or vertical slice, should it live outside the specification skill?
