# 15 — Architecture update v2.2: implementation diagnosis

status: active
version: v2.2
doc_type: architecture_diagnosis
last_updated: 2026-06-10

## Cel dokumentu

Ten dokument opisuje korekty wynikające z rozszerzonej diagnozy projektu oraz migracji nazewnictwa v2.2. Nie zmienia głównej idei `qubok_interface_core`. Doprecyzowuje implementację tak, aby projekt lepiej zniósł wzrost skali: więcej obiektów, więcej paneli, node graph workspace, docking, component library i export.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Starsze nazewnictwo `state_graph`, `stateGraph`, `StateGraph` i “state graph” jest zastąpione przez `node_graph`.

## Werdykt

Dotychczasowy kierunek jest nadal właściwy:

- React + TypeScript + Vite zostają sensowną bazą MVP.
- SVG/HTML zostaje właściwym rendererem dla primitive UI i export proof.
- `Project` jako source of truth zostaje najważniejszą decyzją.
- `command layer` zostaje obowiązkową ścieżką mutacji.
- `Region` i `Rectangle` muszą pozostać oddzielone semantycznie.
- `Node graph` i `Docking` nadal nie powinny dominować defaultowego MVP.

Projekt potrzebuje wcześnie: command history, selectorów, headless tests, render adapterów, external adapter policy i performance baseline.

## Najważniejsze zmiany v2.2

| Zmiana | Dlaczego | Kiedy wchodzi |
|---|---|---|
| Command history wcześniej | undo/redo, command log, graph outputs i debugging będą od tego zależne | Sprint 02 |
| Selectors jako core boundary | canvas, inspector, hierarchy i status nie mogą powielać lookup logic | Sprint 02 |
| Headless tests wcześniej | core ma być stabilny przed UI complexity | Sprint 02 |
| Render adapter types | MVP może renderować SVG, ale core nie może być zamknięty na SVG | Sprint 02/03 |
| Event/action registry poza nodeGraph | event layer jest wcześniejszy i prostszy niż visual node graph | Sprint 09 |
| External libraries as adapters | React Flow/FlexLayout mogą pomagać, ale nie mogą posiadać modelu | przed Sprint 08/10 |
| Performance baseline | pointer move, hit-test, panels i node graph szybko urosną | Sprint 03/04/07 |
| Naming migration | `state_graph` zastąpione przez `node_graph` | v2.2 |

## Core flow

```text
UI event
  -> creator handler
  -> command payload
  -> core/applyCommand
  -> Project next state
  -> selectors
  -> render model / inspector schema / hierarchy tree / validation
  -> React view update
```

## Command history

Command history powinien być wprowadzony jako warstwa core, nawet jeśli MVP undo/redo będzie początkowo ograniczone.

Minimalne typy:

```text
CommandHistoryState
- past: HistoryEntry[]
- future: HistoryEntry[]
- command_log: CommandLogEntry[]

HistoryEntry
- command_id
- command_type
- before_snapshot or inverse_command
- after_snapshot optional
- affected_object_ids
- history_domain
- timestamp
```

## Selectors

Selectors powinny żyć w `src/core/selectors`, a nie w komponentach React.

Minimalne selektory:

```text
getObjectById(project, id)
getRootObjects(project)
getChildrenOfObject(project, id)
getParentChain(project, id)
getSelectedObjectIds(project)
getSelectedObjects(project)
getActiveObject(project)
getSelectionBBox(project)
getObjectValidationStatus(project, id)
getObjectsForRender(project, viewport)
```

## Render adapter

MVP renderer może być SVG/HTML, ale warto wcześniej dodać neutralne typy renderowania:

```text
Project model
  -> selectors
  -> render adapter model
  -> SVG renderer / HTML overlay / future Canvas/WebGL renderer
```

## Inspector schema

Inspector powinien używać schema resolvera:

```text
resolveInspectorSchema(project, object_id)
  -> sections
  -> fields
  -> field path
  -> command mapping
  -> validation hints
  -> visibility conditions
```

Node graph może później odwoływać się do tych samych parametrów.

## Events poza nodeGraph

Event/action registry należy trzymać w osobnym module `core/events`. Node graph konsumuje tę warstwę później.

```text
core/events/
  eventTypes.ts
  eventRegistry.ts
  actionRegistry.ts
  targetResolver.ts
  eventAssignments.ts

core/nodeGraph/
  graphTypes.ts
  graphValidation.ts
  graphToCommands.ts
  graphExecution.ts
```

## Performance baseline

| Tier | Cel | Oczekiwanie |
|---|---:|---|
| Tiny | 10–100 obiektów | wszystko płynne bez optymalizacji |
| MVP | 100–1000 obiektów | selection, inspector, hierarchy bez zauważalnego lag |
| Stress | 1000–10000 obiektów | wymaga filtrów, spatial index, virtualization |

## External libraries

Biblioteki powinny być używane tylko jako adaptery:

| Obszar | Biblioteka możliwa | Rola |
|---|---|---|
| Node graph UI | React Flow | view adapter dla `Project.node_graphs` |
| Docking UI | FlexLayout lub react-resizable-panels | shell view adapter dla DockLayout |
| Immutable updates | Immer | wewnątrz applyCommand, jeśli potrzebne |
| State machine runtime | XState | opcjonalny backend później, nie source of truth |

## Najważniejszy efekt aktualizacji

Projekt zachowuje swój kierunek, ale staje się odporniejszy. Zamiast dodawać kolejne widoczne funkcje, najpierw wzmacniamy warstwy, które będą używane przez wszystkie funkcje: commands, history, selectors, validation, render adapters i tests.