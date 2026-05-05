---
name: app-spec-user-flow-orchestrator
description: Create or materially update User Flow Specs using indexed specification artifacts, discovery material, prototype evidence, bounded autonomous decision-making, screenshots, and final review. Use when the user asks to create, update, harden, or orchestrate a user flow spec.
---

# User Flow Spec Orchestrator

Use this skill to create or materially update User Flow Specs for an app specification system.

## Specification Profile

Read the first specification profile: `specs/SPECIFICATION_PROFILE.md`

Use the profile for product name, spec roots, discovery inputs, prototype URL, server policy, screenshot directory, domain-specific terminology, and role names. If no profile exists, use the defaults in `app-spec-guide`.

## Resolve the Target Flow

From the user's request, identify the flow number, flow name, and target file.

- If the user says something like "third user flow", resolve it through the user-flow index.
- If the user names a flow, match it to the planned flow in the index.
- If the target file does not exist and the flow is listed in the index, create it using the indexed filename.
- If the request cannot be confidently matched to an indexed flow, ask one concise clarification question before continuing.

## Grounding Order

1. Read the specification profile, if present.
2. Inspect the user-flow index.
3. Inspect neighboring or related User Flow Specs.
4. If a Domain and State Model exists, inspect relevant artifacts before making material cross-flow claims.
5. Read only relevant discovery material.
6. Inspect prototype code, mock data, and screenshots when they clarify visible behavior.
7. Use browser evidence when the profile, task, or change risk calls for it.

Discovery material is input, not canonical truth. The prototype is useful evidence, but may be incomplete, inconsistent, or misleading.

When the Domain and State Model exists, treat it as the canonical source for shared terminology, domain objects, lifecycle states, role permissions, source-of-truth rules, audit event vocabulary, and centralized product-level open decisions. User Flow Specs still own workflow-specific start conditions, user actions, system responses, prototype observations, and flow-local edge cases.

If a relevant User Flow Spec and Domain and State Model artifact conflict, do not silently choose one. Reconcile conservatively when the user's requested scope permits, or call out the conflict and add or update a product-level open decision.

## Precision Bias

Optimize for precision over brevity. User Flow Specs are consumed by implementation agents, so preserve behaviorally meaningful distinctions even when they make the spec denser.

Do not collapse separate states, gates, side effects, metadata records, source-of-truth rules, validation blockers, or user/system responsibilities into simpler prose unless the distinction is truly non-product behavior.

Editorial polish is welcome only after the product contract is preserved. If a concise rewrite removes a rule an implementation agent would need, keep the rule and tighten the wording instead.

## Length Guidance

There is no target line count, word count, or expected similarity to neighboring spec length. Neighboring specs are structural references, not size templates.

Make the spec the right length for the flow:

- long enough to preserve the product contract an implementation agent needs
- short enough to avoid duplicated discovery prose, generic PRD filler, and implementation details that do not affect user-visible behavior
- longer when the flow has many states, gates, actors, edge cases, request or approval records, source-of-truth boundaries, or downstream handoffs
- shorter when the flow is simple and has few meaningful alternate paths

## Prototype Evidence

Use the specification profile to determine:

- prototype URL
- whether the user or agent owns starting the dev server
- screenshot directory
- expected browser exploration workflow

For new or materially updated User Flow Specs, use browser exploration when a prototype is configured and reachable, unless the user requests a text-only edit or the change does not depend on UI behavior.

If browser evidence is required and the profile says the user owns the server, check reachability first. If unreachable, ask the user to start or fix the server and do not finalize browser-dependent observations.

When the user or current instructions explicitly permit subagent delegation, launch one `app_spec_prototype_browser_explorer` subagent with a narrow brief. Otherwise, perform the same work locally while keeping raw browser or code output out of the final spec.

The explorer brief should include:

- flow name and slug
- target spec file
- likely route or screen hints
- relevant source paths already identified
- screenshot expectations
- prototype URL and screenshot directory from the specification profile

The explorer must not write specs or make product decisions.

Require this report shape:

- Static prototype map
- Tested routes
- Actions taken
- Screens and states observed
- Product contract clues
- Screenshots captured
- Confirmed prototype behavior
- Missing or unclear behavior
- Inconsistent or misleading behavior
- Console or network errors
- Confidence

The report must separate "code suggests this exists" from "browser confirmed this behavior."

## Domain Model Awareness

Before drafting a new or materially updated User Flow Spec, check whether a Domain and State Model exists.

If it does not exist:

- draft the user flow normally
- keep unresolved product-level ambiguity in the flow's `## Open Questions`
- avoid inventing cross-flow states, permissions, or lifecycle rules beyond what the flow needs

If it exists:

- inspect the domain index
- inspect relevant domain artifacts
- reuse established domain terms, state labels, permission rules, audit event names, and source-of-truth boundaries
- update traceability when a flow's domain dependencies materially change and the user's scope permits
- add or update centralized open decisions for new product-level ambiguity instead of leaving it stranded in one flow

Product-level ambiguity includes questions about shared terminology, states, transitions, roles, permissions, source of truth, audit events, artifact ownership, notifications, deadlines, reminders, settings governance, or behavior that affects multiple flows.

Flow-local ambiguity may remain in the User Flow Spec only when it is narrow to the current flow and does not affect shared product behavior.

## Decision Loop

Run a bounded autonomous decision loop before drafting.

For material decision questions, consult a high-reasoning general subagent only when the user or current instructions explicitly permit subagent delegation. Otherwise, answer the same decision questions locally and record the reasoning internally.

Decision questions should:

- separate evidence from inference
- identify source conflicts
- recommend conservative interpretations
- identify must-preserve product details before recommending simplification
- avoid inventing unsupported product features, business rules, personas, UI behavior, or terminology
- label adjacent ideas as future follow-ups

Use this internal decision shape:

- Recommendation
- Confidence
- Evidence Used
- Inferences
- Must-Preserve Product Details
- Rationale
- Alternative Considered
- Scope Guardrails
- Insertable Prompt Wording, if useful

For a normal flow, use at most 3 decision passes and 5 internal decision questions.

If ambiguity blocks the flow boundary or business outcome, ask the user. If it does not block drafting, make the narrowest conservative assumption and surface uncertainty in `Open Questions` or centralized open decisions.

## Internal Pre-Write Ledger

Before drafting, create an internal synthesis ledger. Do not write it to the repo unless the user explicitly asks for a preserved reasoning artifact.

Include:

- flow boundary, outcome, start, end, and excluded scope
- source inventory
- relevant domain model artifacts consulted, if any
- prototype facts and screenshot paths
- product contract inventory:
  - readiness gates and blockers
  - state vocabulary and state semantics
  - user actions and system side effects
  - request/send prerequisites
  - metadata and audit records created
  - source-of-truth boundaries
  - concepts that must not be merged
- evidence vs inference
- conflicts and ambiguity
- conservative decisions
- required spec obligations
- screenshot plan
- domain-model updates or open-decision updates needed, if any

Use the ledger to draft the spec, but do not paste it into the spec.

## Drafting Rules

Use the User Flow Spec template from `app-spec-guide`.

Keep the spec flow-centered, outcome-grounded, and user-visible. Do not turn it into a technical design, ticket list, generic PRD, or copied discovery notes.

Update the user-flow index when adding a new flow.

If the Domain and State Model exists and the flow has no flow-local open questions, use this `## Open Questions` wording:

```md
Product-level open decisions are tracked in [09_OPEN_DECISIONS.md](../domain-model/09_OPEN_DECISIONS.md).
This flow has no remaining flow-local open questions.
```

If the Domain and State Model exists and the flow introduces or resolves shared product behavior, update the relevant domain artifact or flow-to-domain traceability in the same change when the user's scope permits.

Reference screenshots only when they clarify the flow.

## Final Review

After drafting, consult a high-reasoning reviewer subagent only when the user or current instructions explicitly permit subagent delegation. Otherwise, perform the same review locally.

Check:

- template compliance
- scope conflicts with neighboring specs
- consistency with relevant Domain and State Model artifacts, if present
- unsupported product claims
- missing edge cases or business rules
- lost precision compared with the ledger, explorer report, discovery evidence, or neighboring specs
- collapsed states, gates, metadata, source-of-truth rules, or validation blockers
- inappropriate length normalization against neighboring specs
- unnecessary padding, repeated discovery prose, or generic filler
- prototype observations mixed with intended behavior
- screenshot reference correctness
- index update correctness
- centralized open-decision handling when a Domain and State Model exists
- flow-to-domain traceability update correctness when applicable

Apply only concrete, grounded fixes. Prefer restoring product precision over shortening the spec. Make length serve the flow rather than an implicit target.

## Final Response

Summarize:

- spec files changed
- domain model files changed, if any
- whether browser exploration was used
- screenshots created or updated
- small prototype fixes, if any
- important open questions, centralized open decisions, or recommended follow-up flow boundaries
