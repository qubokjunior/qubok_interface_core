# 18 — Implementation changelog for existing prompts

## Cel

Ten dokument opisuje, co zmienić w myśleniu o istniejących promptach po aktualizacji v2.1. Nie trzeba kasować wcześniejszych sprintów. Trzeba je czytać z tym changelogiem jako nadrzędną korektą.

## Zmiana globalna

Każdy prompt od teraz powinien zakładać:

- command history albo jawny command history contract,
- selectors jako wspólną warstwę odczytu,
- headless tests dla core logic,
- render adapter dla przyszłych rendererów,
- external libraries only as adapters,
- event/action registry osobno od state graph,
- performance baseline dla pointer move, hit-test i panel rendering.

## Zmiany dla istniejących sprintów

| Sprint | Stara interpretacja | Nowa interpretacja v2.1 |
|---|---|---|
| Sprint 00 | audyt dokumentacji i repo | dodatkowo sprawdzić test setup, history strategy, external library usage |
| Sprint 01 | shell i tokens | dodać CSS containment plan i panel performance notes |
| Sprint 02 | model + commands + validation | rozszerzone o command history, selectors, tests, render adapter types |
| Sprint 03 | canvas + primitives | korzystać z selectors i render adapter, nie czytać Project ad hoc w wielu miejscach |
| Sprint 04 | inspector + hierarchy + region debug | schema-driven inspector, selectors obowiązkowo |
| Sprint 05 | component proof + export | sample validity tests i export through render model |
| Sprint 06 | UI cleanup + component library | deep copy instances only; linked components later |
| Sprint 07 | layout + panel builder | performance guardrails dla snap/hit-test/guide overlay |
| Sprint 08 | docking shell | library optional; DockLayout remains source of truth |
| Sprint 09 | event/action registry | przenieść mentalnie do `core/events`, nie wymaga state graph |
| Sprint 10 | state graph workspace | React Flow allowed only as adapter after graph model exists |
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

Nie oznacza to rezygnacji z SVG. Oznacza to, że SVG renderer nie jest modelem projektu.

### 4. `src/core/events`

Event/action registry lepiej umieścić jako osobny moduł:

- `eventTypes.ts`,
- `eventRegistry.ts`,
- `actionRegistry.ts`,
- `targetResolver.ts`,
- `eventAssignments.ts`.

`stateGraph` powinien konsumować tę warstwę później.

### 5. Testy

Wczesne testy powinny sprawdzać:

- create initial project,
- create/patch/delete object,
- select object,
- validate hierarchy,
- validate region links,
- command history basics,
- selectors consistency,
- Panel_Monitor sample validity.

Jeżeli test runnera nie ma, Sprint 02 powinien przynajmniej dodać strukturę i opisać komendę testową jako pending.

### 6. Performance notes

W każdym sprincie canvasowym trzeba pilnować:

- no all-debug labels by default,
- pointer move throttling or local drag session,
- command commit strategy,
- no full app rerender on every pointer move if avoidable,
- hierarchy virtualization later if object count grows.

## Nowy priorytet najbliższego developmentu

Najbliższy prompt powinien wykonać `Sprint 02 v2.1`, nie starszą wersję Sprint 02. To pozwoli uniknąć naprawiania architektury po zbudowaniu canvasu i inspectora.

## Reguła przy konfliktach

Jeżeli starszy dokument mówi mniej niż v2.1, preferuj v2.1.

Priorytet decyzyjny:

1. Project/core source of truth.
2. Command layer and command history.
3. Validation and tests.
4. Selectors.
5. BBox/region separation.
6. Render adapter.
7. UI/workspace implementation.
8. External adapters.
9. Advanced features.