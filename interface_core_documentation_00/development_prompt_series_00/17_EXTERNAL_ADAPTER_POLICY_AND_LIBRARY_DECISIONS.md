# 17 — External adapter policy and library decisions

status: active
version: v2.2
doc_type: external_adapter_policy
last_updated: 2026-06-10

## Cel

Ten dokument określa, jak korzystać z bibliotek zewnętrznych bez oddawania im kontroli nad modelem projektu.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Nie dodawać nowych referencji do starego nazewnictwa `state_graph`.

## Główna zasada

`Project / core model` pozostaje source of truth. Biblioteka zewnętrzna może być tylko adapterem widoku, interakcji albo pomocniczego runtime.

Poprawny przepływ:

1. Project model.
2. Adapter input.
3. External UI/runtime library.
4. Adapter output event.
5. Command payload.
6. applyCommand.
7. Project update.

Niepoprawny przepływ:

1. External library keeps the only state.
2. UI changes happen there.
3. Project is updated partially or not at all.

## Decyzje według obszaru

| Obszar | Narzędzie | Status | Rola |
|---|---|---|---|
| Node graph viewport | React Flow | later allowed | view adapter for `Project.node_graphs` |
| Docking shell | FlexLayout | later review | adapter for `DockLayout` if full docking is needed |
| Split panels | react-resizable-panels | later review | simple resize/split adapter |
| Immutable updates | Immer | allowed if useful | helper inside command reducer |
| State machine runtime | XState | experimental later | optional execution backend |
| Desktop shell | Tauri | post-MVP | file/desktop adapter |

## Node graph adapter rules

React Flow or any graph UI tool can be considered only in the Node Graph Workspace phase.

Allowed:
- node and edge rendering,
- node graph pan and zoom,
- selection UI,
- edge creation UI,
- custom node components.

Required:
- Project stores the node graph model.
- Adapter maps Project node graph to UI nodes and edges.
- UI changes become node graph commands.
- Node graph validation works without the UI library.
- Default MVP screen does not become node graph editor.

## Docking adapter rules

Docking shell is not canvas object layout.

Allowed:
- split application areas,
- resize application panels,
- move panel content between areas,
- save app layout separately.

Required:
- DockLayout is separate from Project object layout.
- Docking changes do not transform InterfaceObject positions.
- Docking data is not stored in Component JSON unless explicitly exported as app workspace preset.

Use custom docking model if only split/merge/resize is needed.
Consider FlexLayout only if tabsets and complex docking are required.
Consider react-resizable-panels only for lightweight splitter behavior.

## Immer rules

Immer may be used if it simplifies immutable updates in `applyCommand`.

Required:
- commands stay serializable,
- Project stays JSON-exportable,
- applyCommand remains headless-testable,
- no hidden side effects inside reducers.

## XState rules

XState is not needed for early MVP. It may be tested later as execution backend for selected node graph behavior.

Required:
- it must not replace Project model,
- it must not bypass command layer,
- it must not be required for basic object editing.

## Tauri rules

Tauri is post-MVP. It can wrap the web app later for desktop file access and packaging.

Required:
- core model must not depend on Tauri APIs,
- save/load must have a web-compatible adapter first,
- filesystem permissions and desktop shell are not core concerns.

## Checklist before adding any library

1. What problem does it solve?
2. Which layer owns that problem: core, creator, docking, node graph, export or desktop shell?
3. Where does the data live after reload?
4. Does every persistent change pass through command layer?
5. Can validation run without the library?
6. Can the feature be hidden if it is not L3/L4?
7. Does it keep app shell layout, canvas object layout and node graph viewport layout separate?

## Current recommendation

Do not add React Flow, FlexLayout, XState or Tauri during Sprint 02-07. First stabilize model, commands, validation, tests, canvas, inspector, hierarchy, regions and component proof. External UI libraries can be evaluated after the relevant adapter boundary exists.