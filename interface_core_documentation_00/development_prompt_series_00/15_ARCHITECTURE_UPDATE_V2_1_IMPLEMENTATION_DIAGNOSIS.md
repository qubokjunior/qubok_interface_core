# 15 — Architecture update v2.1: implementation diagnosis

## Cel dokumentu

Ten dokument opisuje korekty wynikające z rozszerzonej diagnozy projektu. Nie zmienia głównej idei `qubok_interface_core`. Doprecyzowuje implementację tak, aby projekt lepiej zniósł wzrost skali: więcej obiektów, więcej paneli, graph workspace, docking, component library i export.

## Werdykt

Dotychczasowy kierunek jest nadal właściwy:

- React + TypeScript + Vite zostają sensowną bazą MVP.
- SVG/HTML zostaje właściwym rendererem dla primitive UI i export proof.
- `Project` jako source of truth zostaje najważniejszą decyzją.
- `command layer` zostaje obowiązkową ścieżką mutacji.
- `Region` i `Rectangle` muszą pozostać oddzielone semantycznie.
- `State graph` i `Docking` nadal nie powinny dominować defaultowego MVP.

Zmienia się jednak poziom rygoru. Projekt potrzebuje wcześniej: command history, selectorów, headless tests, render adapterów, external adapter policy i performance baseline.

## Najważniejsze zmiany v2.1

| Zmiana | Dlaczego | Kiedy wchodzi |
|---|---|---|
| Command history wcześniej | undo/redo, command log, graph outputs i debugging będą od tego zależne | Sprint 02 |
| Selectors jako core boundary | canvas, inspector, hierarchy i status nie mogą powielać lookup logic | Sprint 02 |
| Headless tests wcześniej | core ma być stabilny przed UI complexity | Sprint 02 |
| Render adapter types | MVP może renderować SVG, ale core nie może być zamknięty na SVG | Sprint 02/03 |
| Event/action registry poza stateGraph | event layer jest wcześniejszy i prostszy niż visual graph | Sprint 09 |
| External libraries as adapters | React Flow/FlexLayout mogą pomagać, ale nie mogą posiadać modelu | przed Sprint 08/10 |
| Performance baseline | pointer move, hit-test, panels i graph szybko urosną | Sprint 03/04/07 |

## Implementacyjny rdzeń po aktualizacji

Zalecana logika przepływu:

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

Zakazany przepływ:

```text
UI event
  -> React component local object mutation
  -> canvas visual update only
  -> inspector/hierarchy stale state
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

Zalecenia:

- `history_domain: project` dla object create/patch/delete/group/layout.
- `history_domain: viewport` dla pan/zoom, domyślnie poza undo projektowym.
- `history_domain: ui` dla tab/collapse/selection UI, jeśli kiedykolwiek będzie logowane.
- Na MVP można użyć snapshotów Project fragmentów zamiast skomplikowanych inverse commands.
- Później można przejść na patches, jeśli model będzie duży.

Acceptance:

- object.patch może zostać cofnięty albo ma jawny TODO z typem danych przygotowanym pod undo.
- pan/zoom nie trafia do normalnego undo obiektów.

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

Cel:

- Canvas, inspector, hierarchy, status i target resolver czytają ten sam stan.
- Zmiana struktury Project nie wymaga przepisywania wszystkich paneli.
- Testy mogą wykryć drift między widokami.

## Render adapter

MVP renderer może być SVG/HTML, ale warto wcześniej dodać neutralne typy renderowania:

```text
RenderObjectDescriptor
- object_id
- kind: rect/text/line/path/group/region_debug
- transform
- visual_bbox
- style
- text optional
- layer_order
- debug_flags
```

Warstwy:

```text
Project model
  -> selectors
  -> render adapter model
  -> SVG renderer / HTML overlay / future Canvas/WebGL renderer
```

Co to daje:

- SVG export i canvas render nie muszą mieć identycznego kodu.
- Debug overlays mogą być osobną warstwą renderowania.
- W przyszłości graph, mini-preview albo performance renderer mogą mieć własny adapter.

## Inspector schema

Inspector nie powinien być ręcznie sklejonym panelem dla każdego typu. Potrzebny jest schema resolver:

```text
resolveInspectorSchema(project, object_id)
  -> sections
  -> fields
  -> field path
  -> command mapping
  -> validation hints
  -> visibility conditions
```

Korzyści:

- Jeden mechanizm dla rectangle, text, region, group, panel.
- Mixed selection łatwiejszy.
- Component exposed parameters mogą używać tej samej infrastruktury.
- State graph może później odwoływać się do tych samych parametrów.

## Events poza stateGraph

Dotychczasowe dokumenty często trzymały event/action registry w `core/stateGraph`. Po aktualizacji v2.1 lepszy podział to:

```text
core/events/
  eventTypes.ts
  eventRegistry.ts
  actionRegistry.ts
  targetResolver.ts
  eventAssignments.ts

core/stateGraph/
  graphTypes.ts
  graphValidation.ts
  graphToCommands.ts
  graphExecution.ts
```

Dlaczego:

- Event assignment może działać bez graph editor.
- Action registry to command wrappers, nie node graph.
- State graph jest późniejszym narzędziem do komponowania events/actions.

## Performance baseline

Już w MVP warto mieć minimalny budżet:

| Tier | Cel | Oczekiwanie |
|---|---:|---|
| Tiny | 10–100 obiektów | wszystko płynne bez optymalizacji |
| MVP | 100–1000 obiektów | selection, inspector, hierarchy bez zauważalnego lag |
| Stress | 1000–10000 obiektów | wymaga filtrów, spatial index, virtualization |

Wczesne zalecenia:

- Nie renderować all-debug labels w normal mode.
- Throttle pointer move / drag preview.
- Commit command na drag end albo kontrolowany interwał, nie na każdy piksel bez strategii.
- Używać memo/selectors w React.
- Hierarchy przy dużej liczbie elementów później wymaga virtualization.
- Rozważyć `contain` CSS dla paneli i viewportów.

## External libraries

Biblioteki powinny być używane tylko jako adaptery:

| Obszar | Biblioteka możliwa | Rola |
|---|---|---|
| Graph UI | React Flow | view adapter dla Project.state_graphs |
| Docking UI | FlexLayout lub react-resizable-panels | shell view adapter dla DockLayout |
| Immutable updates | Immer | wewnątrz applyCommand, jeśli potrzebne |
| State machine runtime | XState | opcjonalny backend później, nie source of truth |

Zakaz:

- Biblioteka graph/docking nie może być jedynym miejscem danych.
- Nie przechowywać node graph wyłącznie w React Flow state.
- Nie przechowywać docking wyłącznie w layout manager state.

## Najważniejszy efekt aktualizacji

Projekt zachowuje swój kierunek, ale staje się odporniejszy. Zamiast dodawać kolejne widoczne funkcje, najpierw wzmacniamy warstwy, które będą używane przez wszystkie funkcje: commands, history, selectors, validation, render adapters i tests.