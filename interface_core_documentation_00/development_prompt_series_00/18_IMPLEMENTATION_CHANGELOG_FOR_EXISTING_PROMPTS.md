# 18 — Implementation changelog for existing prompts

status: active
version: v2.2
doc_type: implementation_changelog
last_updated: 2026-06-10

## Cel

Ten dokument opisuje, co zmienić w myśleniu o istniejących promptach po aktualizacji v2.2. Nie trzeba kasować wcześniejszych sprintów. Trzeba je czytać z tym changelogiem jako nadrzędną korektą.

## Naming migration v2.2

| Stare | Nowe |
|---|---|
| `state_graph` | `node_graph` |
| `stateGraph` | `nodeGraph` |
| `StateGraph` | `NodeGraph` |
| `state graph` | `node graph` |
| `interface_core_00` / `qubok_interface_core_00` | `interface_core` / `qubok_interface_core` |

Local PC path:

```text
I:\Art\_AI\app_development\qubok_interface_core
```

## Zmiana globalna

Każdy prompt od teraz powinien zakładać:

- command history albo jawny command history contract,
- selectors jako wspólną warstwę odczytu,
- headless tests dla core logic,
- render adapter dla przyszłych rendererów,
- external libraries only as adapters,
- event/action registry osobno od node graph,
- performance baseline dla pointer move, hit-test i panel rendering,
- aktualne nazewnictwo v2.2.

## Zmiany dla istniejących sprintów

| Sprint | Stara interpretacja | Nowa interpretacja v2.2 |
|---|---|---|
| Sprint 00 | audyt dokumentacji i repo | dodatkowo sprawdzić test setup, history strategy, external library usage, naming v2.2 |
| Sprint 01 | shell i tokens | dodać CSS containment plan i panel performance notes |
| Sprint 02 | model + commands + validation | rozszerzone o command history, selectors, tests, render adapter types |
| Sprint 03 | canvas + primitives | korzystać z selectors i render adapter, nie czytać Project ad hoc w wielu miejscach |
| Sprint 04 | inspector + hierarchy + region debug | schema-driven inspector, selectors obowiązkowo |
| Sprint 05 | component proof + export | sample validity tests i export through render model |
| Sprint 06 | UI cleanup + component library | deep copy instances only; linked components later |
| Sprint 07 | layout + panel builder | performance guardrails dla snap/hit-test/guide overlay |
| Sprint 08 | docking shell | library optional; DockLayout remains source of truth |
| Sprint 09 | event/action registry | przenieść mentalnie do `core/events`, nie wymaga node graph |
| Sprint 10 | node graph workspace | React Flow allowed only as adapter after node graph model exists |
| Sprint 11 | advanced | every feature starts hidden/spec/headless unless L3/L4 |

## Konkretne update'y implementacyjne

### 1. `src/core/selectors`

Dodać wcześniej niż canvas/inspector/hierarchy:

- `getObjectById`
- `getRootObjects`
- `getChildrenOfObject`
- `getParentChain`
- `getSelectedObjectIds`
- `getSelectedObjects`
- `getActiveObject`
- `getSelectionBBox`
- `getObjectsForRender`
- `getObjectValidationStatus`

### 2. `src/core/commands/commandHistory.ts`

Dodać wcześniej niż transform polish:

- command log,
- undo/redo contract,
- `history_domain`,
- project edit history oddzielone od viewport history.

### 3. `src/core/render`

Dodać neutralną warstwę:

- `renderTypes.ts`,
- `mapProjectToRenderModel.ts`,
- później `svgRenderModel.ts`.

### 4. `src/core/events`

Event/action registry lepiej umieścić jako osobny moduł:

- `eventTypes.ts`,
- `eventRegistry.ts`,
- `actionRegistry.ts`,
- `targetResolver.ts`,
- `eventAssignments.ts`.

`nodeGraph` powinien konsumować tę warstwę później.

### 5. `src/core/nodeGraph`

Jeżeli starsze opisy wskazują `src/core/stateGraph`, użyj zamiast tego:

- `src/core/nodeGraph/**`,
- `Project.node_graphs`,
- `NodeGraph` model,
- `nodeGraph` selectors/commands/adapters.

### 6. Testy

Wczesne testy powinny sprawdzać:

- create initial project,
- create/patch/delete object,
- select object,
- validate hierarchy,
- validate region links,
- command history basics,
- selectors consistency,
- Panel_Monitor sample validity.

## Nowy priorytet najbliższego developmentu

Najbliższy prompt powinien wykonać `Sprint 02 v2.2`, nie starszą wersję Sprint 02. To pozwoli uniknąć naprawiania architektury po zbudowaniu canvasu i inspectora.

## Reguła przy konfliktach

Jeżeli starszy dokument mówi mniej niż v2.2, preferuj v2.2.

Priorytet decyzyjny:

1. Project/core source of truth.
2. Command layer and command history.
3. Validation and tests.
4. Selectors.
5. BBox/region separation.
6. Render adapter.
7. UI/workspace implementation.
8. External adapters.
9. Node graph / advanced features.