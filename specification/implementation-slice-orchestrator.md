# Implementation Slice Orchestrator

Use this workflow to create, update, or review Implementation Slices for an app specification
system.

Implementation Slices translate the full specification set into ordered, testable build plans for
coding agents. They are not tickets copied from user flows, not a technical backlog, and not a
place to invent product behavior. They consume product, domain, design, screen, technical, and
current-app evidence, then group the work into increments that can be implemented safely.

## Specification Profile

Read the first specification profile: `specs/SPECIFICATION_PROFILE.md`

Use the profile for product name, spec roots, discovery inputs, source-priority notes, prototype or
app server policy, role names, domain precision hints, screenshot locations, and project-specific
terminology. If no profile exists, use the defaults in [SKILL.md](SKILL.md).

## Resolve the Target Scope

From the user's request, identify whether the task is:

- create the Implementation Slices stage from scratch
- update the slice index or build order
- create or update one named slice
- split a slice that is too broad
- reconcile slices after an upstream spec change
- review slice coverage, ordering, dependencies, or implementation readiness
- turn a Technical Design implementation sequence into concrete slice artifacts

If the user asks for broad implementation planning, inspect the relevant upstream stage indexes
before drafting. Ask one concise clarification question only when the product boundary, release
target, or implementation constraints cannot be inferred and a wrong assumption would materially
change the slice set.

## Stage Authority

Implementation Slices consume upstream truth.

- User Flow Specs own workflow starts, actions, system responses, flow-local edge cases, and
  outcome intent.
- Domain and State Model artifacts own shared objects, states, transitions, permissions,
  invariants, source-of-truth rules, audit events, and product-level open decisions.
- Design System artifacts own visual language, interaction conventions, UI pattern behavior,
  density, state treatment, and component expectations.
- Screen and Route Specs own route surfaces, visible data, screen states, action availability,
  navigation exits, responsive behavior, and permission variants.
- Technical Design artifacts own implementation architecture, persistence strategy, boundaries,
  integrations, observability, testing strategy, and sequencing constraints.
- The current app is implementation evidence. It may reveal reusable code, scaffold gaps, hidden
  coupling, or retrofit work, but it does not automatically redefine product truth.

If a slice exposes conflicting upstream requirements, update the owning upstream artifact or add an
open decision there. Do not resolve product conflict inside the slice by quietly choosing one side.

## Grounding Order

1. Read the specification profile, if present.
2. Inspect `specs/implementation-slices/00_INDEX.md`, if it exists.
3. Inspect upstream stage indexes to learn the planned artifact set and reading order.
4. Read relevant Technical Design artifacts for architecture, sequencing, and testing constraints.
5. Read relevant Screen and Route Specs for surfaces, states, actions, and navigation.
6. Read relevant Design System artifacts for UI foundations and interaction obligations.
7. Read relevant Domain and State Model artifacts for shared product contracts.
8. Read relevant User Flow Specs for outcome and workflow intent.
9. Inspect current app code only where needed to identify likely files, existing primitives, gaps,
   and implementation risk.
10. Use discovery material only when the specification set is silent, contradictory, or the user
    asks for deeper source mining.

For broad slice planning, prefer breadth through indexes first, then focused reads. Do not load the
entire spec corpus into one context unless the repo is small enough for that to remain clear.

## Multi-Agent Distillation

Use subagents for read-only extraction and review when they make the work clearer, faster, or more
source-bound. Keep each subagent's context clean and source-bound.

Do not ask extraction agents to design the final slice set. Their job is to mine obligations from
one source authority and report in a normalized shape. The orchestrator owns deduplication,
tradeoffs, ordering, final slice boundaries, and artifact edits.

Useful read-only agents:

- User Flow obligations: outcomes, actors, steps, alternate paths, blockers, supporting
  capabilities, and workflow-local acceptance signals.
- Domain obligations: objects, states, transitions, permissions, invariants, audit events,
  source-of-truth rules, and open decisions.
- Design System obligations: UI foundations, component patterns, density, interaction conventions,
  reusable controls, feedback states, and visual constraints.
- Screen and Route obligations: routes, screen states, visible data, actions, blocked actions,
  navigation handoffs, permission variants, and responsive expectations.
- Technical Design obligations: code boundaries, persistence needs, server/client/API boundaries,
  workflows, integrations, observability, testing, fixture, migration, and sequencing constraints.
- Current App gap mapping: existing implementation evidence, likely files, reusable primitives,
  scaffold gaps, risky coupling, test gaps, and retrofit needs.

Require extraction agents to separate evidence from inference and to avoid inventing missing
product behavior.

## Obligation Ledger

Before drafting slices, build an internal obligation ledger. Do not write it to the repo unless the
user explicitly asks for a preserved planning artifact.

Use a compact normalized shape:

```md
| ID | Source | Obligation | Type | Depends On | Blocks | Acceptance Signal | Likely Files | Open Questions |
```

Recommended obligation types:

- `Foundation`: auth, tenant boundaries, privacy primitives, audit model, source-of-truth
  primitives, canonical state helpers.
- `UI Foundation`: component system, app shell primitives, table/list/filter patterns,
  dialogs/sheets, forms, validation, state treatment, accessibility affordances.
- `Vertical Product`: a user-meaningful workflow increment such as intake triage, case creation,
  evidence gathering, review, delivery, closeout, settings, or reporting.
- `Integration`: background jobs, provider boundaries, file storage, notifications, imports,
  exports, webhooks, retries, and idempotency.
- `Hardening`: permission scoping, privacy leak prevention, audit truthfulness, logs, telemetry,
  query keys, failure modes, and regression tests.
- `Operational`: seed data, fixtures, observability dashboards, admin support, environment
  follow-up, and deployment constraints.

The ledger should make foundations visible. A slice can be implementation-critical even when it is
not directly user-facing, as long as it produces a reusable, testable capability that later slices
consume.

## Slice Synthesis

Derive candidate slices from the obligation ledger, not from one source stage alone.

Good slices:

- produce one meaningful implementation increment
- have explicit upstream source specs
- satisfy a coherent group of obligations with shared implementation boundaries
- preserve named domain invariants and technical constraints
- are small enough for focused implementation, testing, and review
- name dependencies and downstream slices they enable
- name excluded scope so agents do not silently absorb adjacent work
- define acceptance criteria in terms of observable behavior and preserved invariants

Avoid:

- broad feature buckets such as "build intake" or "implement settings"
- tiny mechanical tasks that do not produce a meaningful increment
- slices based only on route names when shared domain foundations are missing
- mixing unrelated foundations just because both must happen early
- hiding product ambiguity in implementation notes
- duplicating upstream specs instead of naming source artifacts to read

## Dependency Graph

Before authoring or reordering slices, produce an internal dependency graph.

Check:

- which slices require schema, seed, fixture, auth, privacy, audit, UI, or workflow foundations
- which vertical slices depend on foundation primitives
- which UI slices should precede repeated screen implementation
- which hardening slices must happen before sensitive user data or cross-role access ships
- whether any candidate slice has acceptance criteria that depend on an unresolved upstream
  decision
- whether any cycle means the slice boundaries are wrong

Prefer this broad ordering when the graph supports it:

1. Technical and product trust primitives.
2. UI/system primitives used by repeated surfaces.
3. Domain state and source-of-truth foundations.
4. First vertical workflows that exercise the foundations.
5. Later vertical workflows and integrations.
6. Cross-cutting reporting, settings, administration, and operational polish.

The graph wins over the generic order.

## Merge And Split Heuristics

Split a candidate slice when:

- it touches unrelated code boundaries
- it contains multiple independent risk areas
- it cannot be tested with a focused check
- it requires different specialist context, such as UI design plus cryptographic storage plus
  background jobs
- its likely implementation would exceed one careful coding pass
- review would need to reason about too many invariants at once

Merge candidates when:

- neither produces a meaningful increment alone
- they share the same implementation boundary and test setup
- separating them would create temporary behavior that violates upstream specs
- one is only a thin acceptance check for the other

Prefer sequential enabling slices over one silent foundation omnibus. For example, authorization,
local user status, protected sensitive-data handling, and audit events may each deserve separate
foundation slices if each has independent invariants, tests, and consumers.

## Artifact Shape

Default folder: `specs/implementation-slices/`

Use:

- `00_INDEX.md`
- one numbered slice artifact per implementation increment

The index should explain:

- the purpose of the slice set
- reading order for coding agents
- planned slices with status and outcome
- build order and dependency rationale
- cross-slice rules or invariants
- current blocking decisions or conservative defaults

Use planned links for future slices only when their rough boundary and outcome are known.

## Suggested Slice Template

Use or adapt this shape:

```md
# Implementation Slice NN: <Name>

## Slice Outcome

## Source Specs To Read

## Dependencies

## Included Scope

## Excluded Scope

## Required Invariants

## Likely Files Or Modules

## Implementation Notes

## Acceptance Criteria

## Testing Expectations

## Seed Data, Fixtures, Migrations, And Env

## Downstream Slices Enabled

## Known Open Decisions And Assumptions
```

Keep slice docs compact enough for a coding agent to load alongside source specs and code. Do not
copy long upstream sections. Link to the owning artifacts and restate only the obligations needed
to keep the implementation safe.

## Acceptance Criteria

Acceptance criteria should prove behavior and invariants, not merely list edits.

Good criteria name:

- roles and permission variants
- state transitions and blockers
- sensitive-data handling and redaction
- source-of-truth preservation
- audit or timeline events
- user-visible screen states and validation
- workflow success, failure, retry, and idempotency behavior
- fixtures or seed cases needed to verify the increment
- focused tests and required project checks

When a slice is foundational, acceptance should prove the primitive is usable by later slices. For
example, "typed helpers reject unsupported roles and expose named product capabilities" is better
than "add authorization module."

## Coverage Review

After drafting or materially updating the slice set, run a coverage review before finalizing.

Use this matrix internally or write it if the user asks for a preserved review artifact:

```md
| Source Obligation | Covered By Slice | Status | Concern |
```

Check:

- every user-flow main step is covered by a slice or explicitly deferred
- every important alternate path and blocker has an owning slice
- every domain invariant is enforced by some slice
- every permission rule appears in acceptance criteria or is explicitly deferred
- every audit obligation has an owning slice
- every sensitive-data rule has an owning slice
- every screen state and action has an owning slice or planned exclusion
- every technical prerequisite appears before the slice that relies on it
- every design-system foundation needed by repeated screens is represented before broad UI work
- current-app gaps discovered during planning are assigned, deferred, or documented as risks
- open decisions are centralized in the owning upstream artifact, not buried in slice prose

If coverage fails, adjust slice boundaries, update the index, or recommend an upstream spec fix.

## Subagent Prompt Pattern

When delegating, use prompts with explicit scope and output schemas. Example:

```md
You are a read-only specification extraction agent.

Source authority: <User Flow Specs / Domain Model / Design System / Screens / Technical Design / Current App>.
Target planning question: derive obligations relevant to implementation slices.

Read only the files needed for this source authority. Do not write files. Do not propose the final
slice set. Separate evidence from inference and call out conflicts with upstream authority.

Return:

- source files read
- obligation table with ID, source, obligation, type, depends on, blocks, acceptance signal,
  likely files if known, and open questions
- conflicts or missing upstream decisions
- high-risk implementation notes
- confidence
```

For review agents:

```md
You are a read-only implementation-slice reviewer.

Review the proposed slice index and slice artifacts against the upstream specs named by the
orchestrator. Prioritize missing foundations, bad ordering, traceability gaps, overly broad slices,
unresolved decisions, and acceptance criteria that cannot prove behavior.

Return findings ordered by severity with file references, then a concise coverage summary.
```

## Final Review

Before finishing, check:

- the slice set consumes upstream specs without redefining product behavior
- source specs are listed for every slice
- dependencies and build order are explicit
- foundation slices are justified by downstream consumers
- vertical slices remain user-meaningful and testable
- UI foundation obligations are not lost inside product slices
- exclusions prevent accidental scope creep
- acceptance criteria are behavioral and invariant-based
- testing expectations are focused and realistic
- migration, seed, fixture, environment, or dev-server constraints are called out
- index links match files that exist or are intentionally planned
- open decisions remain in the correct upstream artifact

## Final Response

Summarize:

- implementation-slice files changed
- upstream specs changed, if any
- whether multi-agent extraction or review was used
- major slice boundaries or ordering decisions
- coverage gaps, open decisions, or recommended upstream fixes
- recommended next workflow, usually Consistency Review or implementation of the first ready slice
