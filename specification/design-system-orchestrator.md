# Design System Orchestrator

Use this workflow to create or materially update Design System artifacts for an app specification system.

## Stage Goal

Capture enough user-directed design intent for implementation agents to build UI that feels deliberate, coherent, and specific to the app.

This stage is intentionally loose. Do not force a fixed design-system template when the user wants a moodboard, terse rules, annotated screenshots, component examples, product personality notes, or a hybrid. The artifact shape should serve the user's design preferences and the app's implementation needs.

## First Move

Read the first specification profile, if present: `specs/SPECIFICATION_PROFILE.md`

Use the profile for product name, spec roots, discovery inputs, prototype URL, screenshot directory, source-priority notes, and product terminology.

Inspect before interviewing when a repo exists. Use `rg`, `rg --files`, and existing screenshots or app surfaces to look for:

- existing Design System
- component libraries, style tokens, theme files, Tailwind config, CSS variables, icon libraries, and form/table abstractions
- route, page, screen, layout, and shared component files
- screenshot directories, recordings, prototype captures, or browser evidence
- current product nouns, roles, statuses, and action labels that affect UI copy

If the user provided screenshots, treat them as optional grounding evidence, not automatic product truth. If no screenshots exist, continue with the user interview and record any desired screenshot follow-up.

## Planmaxxing Interview

The primary input for this stage is the user explaining their design preferences.

Before drafting the artifact, run a planmaxxing-style design interview:

- Ask one question at a time.
- For each question, include your recommended answer.
- Resolve dependent design choices in order instead of asking a large generic questionnaire.
- If a question can be answered by exploring the repo, current app, prototype, or screenshots, inspect that evidence instead of asking.
- Stop interviewing once the remaining uncertainty would not materially change implementation guidance.

High-value interview branches:

- overall product personality and emotional temperature
- density, spacing, information hierarchy, and scanability
- preferred references, disliked references, and visual patterns to avoid
- navigation model, page structure, and common layout rhythm
- table, list, filter, sort, bulk action, and drill-down behavior
- form structure, validation tone, blocked states, and destructive confirmations
- modal, drawer, tab, panel, toast, and timeline conventions
- status badges, color semantics, priority language, progress indicators, and confidence language
- button, icon, control, and microcopy preferences
- responsive behavior, accessibility expectations, and keyboard-heavy workflows

## Screenshot Grounding

Screenshots are optional but valuable when they clarify concrete UI patterns.

Use screenshots to ground examples such as:

- how a table should look and behave
- how a form should group fields and validation
- how a page should balance navigation, title area, actions, and body content
- how cards, panels, filters, search, tabs, drawers, modals, and empty states should appear
- which current patterns should be preserved, refined, or avoided

When citing screenshot evidence:

- name the screenshot path or capture label
- describe the specific pattern it grounds
- separate observed evidence from intended design guidance
- do not generalize one screenshot into a universal rule unless the user confirms it

## Artifact Shape

Prefer one `specs/design-system/00_INDEX.md` plus one or more focused artifacts. Adapt filenames to the user's preferred shape.

Useful artifact options:

- `01_DESIGN_DIRECTION.md`
- `02_UI_PATTERNS.md`
- `03_COMPONENT_AND_STATE_GUIDANCE.md`
- `04_SCREEN_EXAMPLES.md`
- `05_VISUAL_PATTERNS_TO_AVOID.md`

For small apps, one compact `01_DESIGN_SYSTEM.md` may be better than several thin files.

Do not create placeholder artifacts just to fill the stage. Use planned links in the index when future guidance is useful but not yet known.

## Suggested Index Structure

Use or adapt this structure:

```md
# Design System

## Purpose

## Reading Order

## User Design Preferences

## Screenshot Grounding

## Artifact Index

| Artifact                 | Status | Purpose                                                        |
| ------------------------ | ------ | -------------------------------------------------------------- |
| `01_DESIGN_DIRECTION.md` | Draft  | Product personality, density, hierarchy, and visual direction. |

## Open Design Questions
```

## Suggested Artifact Sections

Use only the sections that fit the user's preferred output.

```md
# Design System: <Scope>

## User Design Preferences

## Product Personality

## Layout and Density

## Navigation and Page Structure

## Core UI Patterns

## Components and Controls

## States and Feedback

## Responsive and Accessibility Expectations

## Screenshot-Grounded Examples

## Patterns to Avoid

## Open Design Questions
```

## Boundaries

- Do not invent product behavior that belongs in User Flow Specs or Domain Modeling artifacts.
- Do not silently override screen-specific requirements; update Screen and Route Specs when design guidance changes screen behavior.
- Do not turn screenshots into canonical design requirements without user preference or explicit confirmation.
- Keep technical implementation details out unless they materially constrain design, such as an existing component library.
- Preserve the user's taste even when it differs from generic UI best practices, while calling out accessibility or implementation risks.

## Handoff

End with:

- files created or updated
- design preferences captured
- screenshots or app evidence used
- assumptions made
- open design questions
- recommended next workflow, usually Screen and Route Specs or Consistency Review
