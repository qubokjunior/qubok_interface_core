# QUBOK_INTERFACE_CORE — CODEX DEVELOPMENT PROTOCOL

Version: 2026-06-11C
Status: canonical prompt and sprint protocol

## Purpose

Use this file to prepare Codex prompts or new development chat prompts. Each sprint should stay narrow, testable and aligned with the documentation canon.

## Required task header

```text
Project: qubok_interface_core
Current source of truth: interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md
Terminology: node_graph is canonical; state_graph is legacy.
Phase:
Stabilization bridge: S0/S1/S2/S3/S4/S5/S6/S7/S8 or none
Feature:
Current maturity:
Target maturity:
Working path: I:\Art\_AI\app_development\qubok_interface_core
Out-of-scope:
```

## Required task sections

| Section | Purpose |
|---|---|
| Goal | one dominant mechanism |
| Files to inspect | context before patching |
| Files allowed to change | patch boundary |
| Files not part of this task | scope guard |
| Data owner | DocumentModel / SessionState / ViewState / WorkspaceLayout / RuntimeCache / History |
| Model fields affected | source-of-truth impact |
| Commands affected | mutation path impact |
| UI views affected | visible workflow impact |
| Runtime hot path impact | pointermove/render/validation/autosave/log risk |
| Validation affected | correctness guard |
| Manual tests | acceptance workflow |
| Acceptance criteria | done definition |
| Rollback risk | safety note |

## Scope rule

Good sprint scope:

- Selection Operation Target Contract,
- InteractionSession for drag/resize preview and commit,
- Canvas Instance Guardrail,
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

Migrate legacy code only in a dedicated cleanup sprint after runtime stabilization.

## Source-of-truth flow

Persistent mutation flow:

```text
UI event -> normalized intent -> command -> core update -> dirty flags -> validation -> render / preview / export update
```

Interactive pointer flow:

```text
pointerdown -> InteractionSession target snapshot -> transient preview on pointermove -> one transaction command on pointerup/blur/enter -> validation/autosave debounce
```

Canvas, inspector and graph must not keep conflicting persistent object state.

## Data ownership separation

| Owner | Owns | Must not own |
|---|---|---|
| DocumentModel | objects, hierarchy, components, style/token refs, rule/relation assignments, schema version | hover, active tool, transient drag, rendered cache |
| SessionState | selected ids, active id, hover id, active tool, active drag/session | persistent object props |
| ViewState | canvas camera, zoom, grid, overlay, scroll | object transform |
| WorkspaceLayout | dock tree, pane contents, split ratios | canvas object layout |
| RuntimeCache | computed bbox, resolved token, preview cache, validation cache | authoritative saved data |
| History | command records, undo/redo, debug trace | source object definitions |

## Layout separation

| Domain | Owns |
|---|---|
| app_shell_docking | split, merge, resize and store application panels |
| canvas_object_layout | object move, resize, align, snap and layout on canvas |
| graph_viewport_layout | graph camera, graph nodes, ports and lanes |

Do not mix these domains in one implementation task.

## Current stabilization bridge

Before more advanced systems, use this chain:

```text
S0 Safety baseline -> S1 Selection Operation Target Contract -> S2 InteractionSession -> S3 Canvas Instance Guardrail -> S4 Document/Session/View split audit -> S5 Core browser boundary audit -> S6 Default UI visibility cleanup -> S7 Versioned persistence/migrations -> S8 node_graph naming cleanup
```

## Required local context check for Codex

The current local project root contains many root-level sprint note files, build logs, diagnostics, backups, `dist`, `node_modules` and generated artifacts. Codex should not infer current architecture from root-level sprint notes alone.

For implementation tasks, inspect in this order:

1. `README.md`
2. `interface_core_documentation_00/00_INDEX.md`
3. `interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md`
4. `interface_core_documentation_00/01_TERMINOLOGY_CANON_NODE_GRAPH.md`
5. `interface_core_documentation_00/02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
6. `interface_core_documentation_00/03_FEATURE_MATURITY_MATRIX.md`
7. `interface_core_documentation_00/04_CODEX_DEVELOPMENT_PROTOCOL.md`
8. `interface_core_documentation_00/05_CURRENT_IMPLEMENTATION_STABILIZATION_MAP.md`
9. `interface_core_documentation_00/06_LOCAL_REPO_STRUCTURE_AND_CODEX_ACCESS_MAP.md`
10. the relevant `src/` files
11. only then older `SPRINT_*` notes if explicitly relevant
12. logs only if the task is build/runtime diagnosis

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

For stabilization work:

```text
1. Multi-select at least three objects.
2. Drag one selected object and confirm all selected top-level targets move.
3. Press Delete and confirm all selected top-level targets are removed.
4. Confirm clicking a selected object does not collapse selection during drag start.
5. Confirm pointermove does not spam command log, validation or autosave.
6. Open/split multiple canvas panes and confirm only one primary interactive canvas exists per viewId.
```

For documentation-only work:

```text
1. Open README.md.
2. Confirm it points to current source of truth.
3. Open 00_INDEX.md.
4. Confirm node_graph terminology and roadmap precedence.
5. Confirm legacy state_graph wording is marked or replaced.
6. Confirm stabilization bridge S0-S8 is visible.
```

## Acceptance criteria

A task is accepted when:

- build passes if code changed,
- core/creator boundary is not broken,
- command path is respected,
- interaction hot path does not commit full model changes per pointermove,
- default UI remains readable,
- new feature has maturity level,
- data owner is explicit,
- docs mention phase and tests,
- no new `state_graph` terminology is introduced.
