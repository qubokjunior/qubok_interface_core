# QUBOK_INTERFACE_CORE — CODEX DEVELOPMENT PROTOCOL

Version: 2026-06-11
Status: canonical prompt and sprint protocol

## Purpose

Use this file to prepare Codex prompts or new development chat prompts. Each sprint should stay narrow, testable and aligned with the documentation canon.

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

| Section | Purpose |
|---|---|
| Goal | one dominant mechanism |
| Files to inspect | context before patching |
| Files allowed to change | patch boundary |
| Files not part of this task | scope guard |
| Model fields affected | source-of-truth impact |
| Commands affected | mutation path impact |
| UI views affected | visible workflow impact |
| Validation affected | correctness guard |
| Manual tests | acceptance workflow |
| Acceptance criteria | done definition |
| Rollback risk | safety note |

## Scope rule

Good sprint scope:

- command layer for object patching,
- region debug overlay,
- component save/instantiate MVP,
- app shell split/merge only,
- headless event registry only,
- `node_graph` output contract validation only.

Avoid combining graph, docking, rules, array, visual redesign and model migration in one sprint.

## Naming rule

Use `node_graph` in all new docs and public naming.

| Prefer | Avoid for new public APIs |
|---|---|
| `nodeGraphTypes.ts` | `stateGraphTypes.ts` |
| `NodeGraphWorkspace.tsx` | `StateGraphWorkspace.tsx` |
| `node_graph_profile` | `state_graph_profile` |
| `node_graph_output_contract` | `state_graph_output_contract` |
| `node_adapter_registry` | `stateGraphExecution` |

Migrate legacy code only in a dedicated cleanup sprint.

## Source-of-truth flow

```text
UI event -> normalized intent -> command -> core update -> dirty flags -> validation -> render / preview / export update
```

Canvas, inspector and graph must not keep conflicting persistent object state.

## Layout separation

| Domain | Owns |
|---|---|
| app_shell_docking | split, merge, resize and store application panels |
| canvas_object_layout | object move, resize, align, snap and layout on canvas |
| graph_viewport_layout | graph camera, graph nodes, ports and lanes |

Do not mix these domains in one implementation task.

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

## Manual tests

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
5. Confirm legacy state_graph wording is marked or replaced.
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
