# App Spec Bootstrapper

Use this workflow to set up the first durable specification structure for an app.

The bootstrapper is the front door. It creates project-specific context and indexes so the stage-specific workflows can work well later. It should not write full User Flow Specs, Design System specs, Screen and Route Specs, Technical Design, or Domain Modeling artifacts unless the user explicitly expands the scope.

## Bootstrap Goal

Create the minimum useful specification system:

- `specs/SPECIFICATION_PROFILE.md`
- `specs/user-flows/00_INDEX.md`
- a screenshot or evidence directory when a prototype is configured

Then recommend the next 1-3 specification steps.

## First Move

Inspect before interviewing when a repo exists.

Use `rg` and `rg --files` to look for:

- existing `specs/` specs, discovery material, PRDs, notes, tickets, or generated plans
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
- What outcome should the first specification pass support?
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

Create `specs/SPECIFICATION_PROFILE.md` with this structure, adapting section details to the repo:

```md
# Specification Profile: <Product Name>

## Product Summary

## Specification Roots

- User Flow Specs: `specs/user-flows/`
- Design System: `specs/design-system/`
- Screen and Route Specs: `specs/screens-and-routes/`
- Technical Design: `specs/technical-design/`
- Domain Modeling: `specs/domain-model/`

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

Create `specs/user-flows/00_INDEX.md` with this structure:

```md
# User Flow Specs

## Purpose

This folder captures meaningful work from the user's perspective. User Flow Specs define outcomes, actors, triggers, preconditions, user actions, system responses, edge cases, business rules, prototype observations, and flow-local open questions.

## Reading Order

1. Read this index.
2. Read the relevant User Flow Spec.
3. Read Domain Modeling artifacts when they exist and the flow needs established model terms.
4. Read prototype evidence or discovery inputs only when the spec points to them or when resolving ambiguity.

## Flow Index

| #   | Flow        | Status  | Artifact            | Notes                                      |
| --- | ----------- | ------- | ------------------- | ------------------------------------------ |
| 1   | <Flow Name> | Planned | `01_<FLOW_NAME>.md` | <Why this flow matters or source evidence> |

## Candidate Flows Not Yet Indexed

## Bootstrap Notes
```

Index only flows with enough evidence to be useful. Put weaker guesses under `Candidate Flows Not Yet Indexed`.

## Domain Modeling During Bootstrap

Do not create `specs/domain-model/` during bootstrap by default. Domain Modeling is the final stage, and it is most useful after User Flow Specs, Screen and Route Specs, and Technical Design have established the inputs it needs.

If the user explicitly asks to scaffold Domain Modeling during bootstrap, create only `specs/domain-model/00_INDEX.md` with planned links and a note that the final artifact set depends on later Technical Design choices. Do not create full domain artifacts during bootstrap unless the user explicitly asks.

## File and Folder Rules

- Preserve existing specs and indexes. Patch gaps instead of replacing user work.
- Keep reusable workflow rules out of the target repo. Put only project-specific facts in `specs/SPECIFICATION_PROFILE.md`.
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
- recommended next workflow, usually [user-flow-orchestrator.md](user-flow-orchestrator.md), [design-system-orchestrator.md](design-system-orchestrator.md), or [SKILL.md](SKILL.md)

If the next step is obvious, recommend the first concrete User Flow Spec to draft.
