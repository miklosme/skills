---
name: app-spec-consistency-reviewer
description: Review an app specification set for conflicts, drift, missing traceability, misplaced decisions, stage-boundary violations, or implementation-readiness gaps. Use when the user asks to audit, reconcile, harden, validate, or review specification artifacts without necessarily creating a new artifact.
---

# Spec Consistency Reviewer

Use this skill to review specification artifacts as a coherent system.

This is a reviewer skill, not an authoring orchestrator. Lead with findings and concrete fixes. If the user asks you to apply fixes, update the minimum affected artifacts.

## Specification Profile

Read the first specification profile: `specs/SPECIFICATION_PROFILE.md`

Use the profile for product name, spec roots, discovery inputs, prototype URL, domain-specific terminology, role names, and source-priority notes.

## Review Targets

From the user's request, identify whether the review targets:

- one artifact
- one stage folder
- a flow plus related domain model entries
- all User Flow Specs
- the full Domain and State Model
- implementation-readiness across stages
- open decisions and traceability

If the target is ambiguous and a broad review would be expensive or noisy, ask one concise clarification question.

## Grounding Order

1. Read the specification profile, if present.
2. Read `app-spec-guide` for stage authority and artifact boundaries when needed.
3. Inspect relevant stage indexes.
4. Inspect targeted artifacts.
5. Inspect upstream artifacts that should govern the target.
6. Inspect downstream artifacts only when checking readiness or drift.
7. Use discovery material or prototype evidence only when the specification set is silent, contradictory, or the user asks for deeper evidence.

## What to Check

Look for:

- artifact missing from an index or indexed but absent without clear planned status
- artifact title or folder inconsistent with stage conventions
- user-flow behavior that conflicts with the Domain and State Model
- domain model claims not supported by any flow, discovery evidence, prototype evidence, or user decision
- product-level open questions stranded inside one flow
- flow-local questions incorrectly centralized as product-level decisions
- open decision IDs duplicated, missing, vague, or not traceable to affected artifacts
- flow-to-domain traceability that omits important objects, states, actions, permissions, audit events, invariants, source-of-truth rules, or open decisions
- prototype observations written as intended product behavior
- technical implementation details leaking into product-level artifacts
- UI language or screen behavior silently redefining domain rules
- implementation slices missing key source specs, acceptance criteria, roles, states, exclusions, or testing expectations
- language that collapses important product distinctions named in the specification profile

## Review Posture

Prioritize bugs, contradictions, missing gates, unresolved decisions, and implementation risks over style.

When sources disagree:

- separate evidence from inference
- name the conflicting artifacts or sources
- recommend the smallest conservative reconciliation
- keep the product distinction visible
- suggest which artifact should own the final truth

Do not rewrite for tone unless vague wording creates implementation risk.

## Output Shape

If reviewing only, lead with findings ordered by severity. Include file paths and line references where practical.

Use:

- `P1`: contradiction or missing rule likely to cause wrong implementation
- `P2`: ambiguity, missing traceability, or misplaced decision likely to slow or fragment implementation
- `P3`: cleanup, wording, organization, or minor consistency issue

After findings, include open questions or assumptions, then a brief summary.

If applying fixes, update the minimum affected artifacts and summarize:

- files changed
- conflicts resolved
- open decisions added, moved, or clarified
- traceability updates
- remaining risks or follow-up recommendations
