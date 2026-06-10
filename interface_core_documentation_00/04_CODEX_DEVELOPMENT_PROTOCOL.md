# QUBOK_INTERFACE_CORE — CODEX DEVELOPMENT PROTOCOL

Version: 2026-06-11
Status: canonical prompt and sprint protocol

## Purpose

Use this document when preparing Codex prompts or new development chat prompts. The goal is to keep each sprint narrow, testable and aligned with the current documentation canon.

## Required task header

```text
Project: qubok_interface_core
Current source of truth: interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md
Terminology: node_graph is canonical; state_graph is legacy.
Phase:
Feature:
Current maturity:
Target maturity:
Working path:
Out-of-scope:
```

## Required task sections

Each implementation task should include:

1. Goal.
2. Files to inspect.
3. Files allowed to change.
4. Files not part of this task.
5. Model fields affected.
6. Commands affected.
7. UI views affected.
8. Validation affected.
9. Manual tests.
10. Acceptance criteria.
11. Rollback risk.

## Scope rule

One sprint should implement one dominant mechanism.

Good examples:

- command layer for object patching,
- region debug overlay,
- component save/instantiate MVP,
- app shell split/merge only,
- headless event registry only,
- node_graph output contract validation only.

Avoid combining graph, docking, rules, array, full visual redesign and model migration in one sprint.

## Naming rule

Use `node_graph` in all new docs and public naming.

Preferred names:

- `nodeGraphTypes.ts`,
- `NodeGraphWorkspace.tsx`,
- `node_graph_profile`,
- `node_graph_output_contract`,
- `node_adapter_registry`.

Do not introduce new public names based on `stateGraph`. If old code still has this name, migrate it only in a dedicated cleanup sprint.

## Source of truth rule

Persistent changes should follow:

```text
UI event -> normalized intent -> command -> core update -> dirty flags -> validation -> render / preview / export update
```

Canvas, inspector and graph should not keep conflicting persistent object state.

## Three layout rule

| Domain | Owns |
|---|---|
| app_shell_docking | split, merge, resize and store application panels |
| canvas_object_layout | object move, resize, align, snap and layout on canvas |
| graph_viewport_layout | graph camera, graph nodes, ports and lanes |

Do not mix these models in one implementation task.

## Preferred final response after a sprint

```text
Done:
- changed files
- created files
- important decisions

How to test:
- command/build
- manual UI checks
- expected result

Not touched:
- explicit out-of-scope items

Next:
- one logical next task
```

## Manual test template

For UI/model work:

```text
1. Start app.
2. Confirm default shell is readable.
3. Select object on canvas.
4. Confirm hierarchy row and inspector match selection.
5. Edit parameter through inspector.
6. Confirm command log and canvas update.
7. Run validation/export if relevant.
```

For documentation-only work:

```text
1. Open README.md.
2. Confirm it points to current source of truth.
3. Open 00_INDEX.md.
4. Confirm node_graph terminology and roadmap precedence.
5. Confirm older state_graph wording is marked as legacy or replaced in edited files.
```

## Acceptance criteria

A task is accepted when:

- build passes if code changed,
- core/creator boundary is not broken,
- command path is respected,
- default UI remains readable,
- new feature has maturity level,
- docs mention phase and tests,
- no new `state_graph` terminology is introduced.
