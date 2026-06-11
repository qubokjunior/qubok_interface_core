# QUBOK_INTERFACE_CORE — CURRENT IMPLEMENTATION STABILIZATION MAP

Version: 2026-06-11
Status: canonical bridge from current local prototype state to the roadmap

## 1. Why this file exists

The current local project at:

```text
I:\Art\_AI\app_development\qubok_interface_core
```

shows a working prototype plus many sprint artifacts in the root folder: `_qubok_backups`, `_qubok_build_logs`, `_diagnostics`, `dist`, `node_modules`, `src`, root `SPRINT_*` notes, README backups and package files.

This is useful history, but noisy for Codex. Codex should not treat every root-level note as equal current architecture. This file tells Codex what to prioritize now.

## 2. Current stage classification

Current implementation stage:

```text
Phase E/F/G/H stabilization + Phase I preparation
```

Meaning:

| Phase | Current interpretation |
|---|---|
| E Primitive + selection | exists, but multi-edit target contract needs stabilization |
| F Inspector + hierarchy | exists, but selected/active/hit ownership must be audited |
| G BBox + regions | direction is valid, but debug exposure must be gated |
| H Component proof | sample/proof work is useful, but save/export/component behavior must stay contract-based |
| I Default MVP cleanup | next visual/product gate after runtime stabilization |

Not current priority:

| Not now | Why |
|---|---|
| Full Phase N `node_graph` workspace | graph viewport, ports, profiles and output contracts need stable foundations first |
| Advanced rules/relations/array/procedural systems | they require schema, target resolver, preview and validation contracts |
| Docking polish before canvas guardrail | multiple canvas panes can multiply render cost |
| Global naming migration | legacy `stateGraph` cleanup can break imports if mixed with runtime work |

## 3. Main diagnosis from local structure

The local root contains many root-level sprint notes such as `SPRINT_13_5_*`. This indicates rapid iterative work and many local decisions. The result is useful but must be consolidated:

| Observation | Meaning | Documentation response |
|---|---|---|
| Many `SPRINT_*` markdown files in root | old task history is mixed with current working context | use docs canon first, sprint notes only as historical evidence |
| `_qubok_build_logs` and `_diagnostics` exist | build/debug cycles already produced useful diagnostics | keep as local evidence, do not expose as main roadmap |
| `_qubok_backups` and README backups exist | rollback safety exists locally | do not delete automatically without explicit task |
| `dist` and `node_modules` visible | generated/dependency folders present locally | Codex should not inspect them unless build tooling requires it |
| `src` exists | implementation is real, not only documentation | current work should inspect source after reading current canon |

## 4. Stabilization bridge S0-S8

These bridge tasks are the current implementation map. They do not replace Phase A-O; they reconnect the current prototype to the canonical roadmap.

| Bridge | Name | Goal | Done when |
|---:|---|---|---|
| S0 | Safety baseline / repo cleanliness | establish build state, rollback path and root-context rules | build status known, noisy files documented, no destructive cleanup without approval |
| S1 | Selection Operation Target Contract | unify multi-select, active object, hit object and operation targets | multi-delete and multi-drag work on selected top-level targets |
| S2 | InteractionSession | move drag/resize hot path to transient preview + final commit | pointermove does not spam command/validation/autosave |
| S3 | Canvas Instance Guardrail | prevent multiple docked canvases from mounting full interactive renderers | only one primary interactive canvas exists per viewId |
| S4 | Document / Session / View / Workspace / Cache split audit | assign ownership for every state field | no persistent/session/view/cache state is ambiguously duplicated |
| S5 | Core browser boundary audit | keep core headless and testable | `src/core` has no React/DOM/browser/storage ownership |
| S6 | Default UI visibility cleanup | make default interface readable and maturity-gated | debug/docking/node_graph do not dominate default screen |
| S7 | Versioned persistence and migrations | prepare schema growth safely | saved data has schema version and migration path |
| S8 | `node_graph` naming cleanup | migrate legacy graph naming safely | new public names use `node_graph`; semantic `state` remains valid |

## 5. S1 Selection Operation Target Contract

Required concepts:

| Concept | Meaning | Rule |
|---|---|---|
| `selected_ids` | current selected set | batch operations start from this set |
| `active_id` | focus for inspector/primary selection | never the implicit batch target list |
| `hit_id` | object/region under pointer | does not collapse selection if already selected |
| `operation_target_ids` | stable target snapshot for current operation | computed once at operation start |
| `top_level_target_ids` | targets excluding children when parent is also selected | avoids double transform/delete |
| `hover_id` | current hover target | session/debug only |

Expected behavior:

```text
selected_ids = [A, B, C]
user starts drag on B
operation_target_ids = [A, B, C]
active_id may become B
selection must not collapse to [B]
```

Manual gate:

1. Select three independent objects.
2. Drag one selected object.
3. All selected objects move together.
4. Press Delete.
5. All selected objects are deleted.
6. Select parent and child.
7. Transform/delete only top-level target to avoid double operation.

## 6. S2 InteractionSession

Pointer interaction must not commit full document updates on every pointermove.

Required flow:

```text
pointerdown
  set pointer capture
  resolve operation_target_ids
  snapshot start transforms
  create InteractionSession

pointermove
  update transient preview at most once per animation frame
  do not run full validation/autosave/export

pointerup / blur / enter commit
  create one transaction command
  apply command
  update dirty flags
  run validation/autosave debounce
```

Hot path must avoid:

| Avoid during pointermove | Use instead |
|---|---|
| full document commit | transient preview layer |
| full validation | debounce after commit |
| autosave/localStorage | idle/debounced save |
| command log spam | one session summary |
| hierarchy rebuild | commit-time update |
| export preview regeneration | dirty flag + lazy preview |

## 7. S3 Canvas Instance Guardrail

Docking can create multiple panes that show canvas-like content. This must not create multiple full interactive renderers for the same view.

Recommended canvas instance types:

| Type | Purpose | Pointer commands |
|---|---|---|
| `PrimaryInteractiveCanvas` | main editable canvas for a viewId | yes |
| `PassiveCanvasMirror` | live visual mirror | no |
| `SnapshotCanvasPreview` | cached preview thumbnail/pane | no |
| `IndependentCanvas` | intentionally separate full viewport | yes, explicit only |
| `CanvasPlaceholder` | protects layout when content unavailable | no |

Gate:

```text
For one viewId, default maximum primary interactive canvas count = 1.
Additional dock panes are mirror/snapshot/placeholder unless user explicitly creates independent view.
```

## 8. S4 State ownership split

Persistent document data must be separated from session/view/layout/cache data.

| Field kind | Owner |
|---|---|
| objects, hierarchy, component assets | DocumentModel |
| selected ids, active id, hover id, active tool | SessionState |
| camera, zoom, grid, overlay mode | ViewState |
| docking tree, pane content, split ratios | WorkspaceLayout |
| computed bbox, resolved tokens, preview cache | RuntimeCache |
| undo/redo, debug trace, command chain | History |

Audit question for every field:

```text
Is this saved project data, local session data, viewport data, app layout data, derived cache, or history/debug data?
```

If the answer is unclear, do not add the field yet.

## 9. S5 Core boundary

`src/core` must stay headless TypeScript.

Forbidden in `src/core`:

- React imports;
- JSX/TSX components;
- direct `window`, `document`, `localStorage`, browser event handling ownership;
- CSS imports;
- creator/panel imports.

Allowed in `src/core`:

- pure types;
- command application;
- validation;
- geometry and bbox utilities;
- hit-test math;
- region resolution;
- export serialization;
- docking data model;
- node_graph data model and validation.

## 10. S6 Default UI visibility cleanup

Default UI should show current workflow, not every internal system.

Default interface may show:

- L3/L4 user workflow;
- compact L2 status;
- collapsed debug panels;
- clear access to debug workspace.

Default interface should not be dominated by:

- raw action registry;
- full event stream;
- full node graph before Phase N;
- docking workbench controls everywhere;
- labels covering canvas content;
- all debug overlays enabled by default.

## 11. S7 Versioned persistence

Any saved project should include at least:

```text
schema_version
app_version or document_version
objects_by_id
root_children
components/assets if present
settings/tokens if present
```

Migration rules:

| Rule | Reason |
|---|---|
| never silently drop unknown data | protects future versions |
| run validation after migration | catches broken references |
| keep migration isolated from visual cleanup | reduces blast radius |
| document schema changes in the sprint note | helps future Codex tasks |

## 12. S8 node_graph naming cleanup

`node_graph` is canonical. `state_graph` is legacy.

Do not run naming migration together with S1/S2/S3.

Safe order:

1. Build passes.
2. Runtime contracts stable.
3. Identify legacy files/imports.
4. Rename internal modules in a dedicated sprint.
5. Preserve semantic `state` terms for hover/pressed/disabled/dirty/etc.
6. Update docs and build.

## 13. Codex usage

Every next Codex task should include:

```text
Stabilization bridge: S0/S1/S2/S3/S4/S5/S6/S7/S8
Phase:
Feature:
Current maturity:
Target maturity:
Data owner:
Files to inspect:
Files allowed to change:
Manual tests:
Out-of-scope:
```

## 14. Current best next step

The most useful next implementation step is S0 if the repository state is not clean and build status is unknown. If build status is already known and safe, go directly to S1.

Preferred sequence:

```text
S0 -> S1 -> S2 -> S3 -> S4 -> S5 -> S6 -> S7 -> S8
```
