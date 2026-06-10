# 16 — Roadmap v2.1: revised implementation order

## Cel dokumentu

Ten dokument aktualizuje kolejność developmentu po diagnozie v2.1. Nie kasuje poprzedniej roadmapy. Doprecyzowuje kolejność tak, aby techniczne fundamenty pojawiły się przed rozbudową UI, graphu i dockingu.

## Główna korekta

Poprzednia kolejność była dobra funkcjonalnie, ale zbyt mało eksponowała techniczne bramki. Nowa kolejność dodaje:

- command history przed rozbudowanym transform/edit,
- selectors przed canvas/inspector/hierarchy sync,
- headless tests przed ciężkim UI,
- render adapter przed zależnością od SVG,
- performance baseline przed dużym canvasem i graph workspace,
- external adapter policy przed docking/graph library decisions.

## Roadmapa v2.1

| Etap | Nazwa | Cel | Maturity |
|---|---|---|---|
| A | Foundation / decisions | słownik, granice, maturity matrix, scope gates | L0/L1 |
| B | Scaffold + concept shell | stały układ aplikacji i tokens | L2/L3 |
| C1 | Core model | Project, InterfaceObject, bbox, region, layout, selection | L1 |
| C2 | Command layer | serializable commands, applyCommand, dirty flags | L1 |
| C3 | Command history | undo/redo contract, command_log, history_domain | L1/L2 |
| C4 | Selectors | wspólne odczyty dla canvas/inspector/hierarchy/status | L1 |
| C5 | Validation + tests | validateProject, validateHierarchy, validateRegions, headless tests | L1/L2 |
| C6 | Render adapter types | neutralny model renderowania przed SVG renderer | L1 |
| D | Canvas renderer + viewport | SVG/HTML view, grid, pan, zoom, selected outline | L2/L3 |
| E | Primitive creation | rectangle, text, line, region, empty | L3 |
| F | Selection + transform | click, shift, box select, move, resize, delete | L3 |
| G | Inspector + hierarchy sync | schema-driven inspector, hierarchy tree, status | L3/L4 |
| H | BBox / region debug | visual/layout/interaction overlays, hover debug | L2/L3 |
| I | Performance baseline | pointer throttling, object count tiers, first hit-test strategy | L2/L3 |
| J | Component proof | button_group, Panel_Monitor sample as Project data | L3/L4 |
| K | Save/load/export | Project JSON, Component JSON, SVG, debug report | L3 |
| L | Default UI cleanup | concept-aligned interface creator, debug hidden | L4 |
| M | Component library MVP | save group/panel as asset, insert fresh IDs | L3 |
| N | Layout/snap/panel builder | align, distribute, snap, box arranger, structured panels | L3 |
| O | App shell docking | split/merge/resize application panels only | L3/L4 |
| P | Events/actions | headless event registry, action registry, target resolver | L1/L2/L3 |
| Q | State graph workspace | visual graph over events/actions, outputs commands | L2/L3 |
| R | Advanced | shape/path, reactions, procedural icons, external bridges | L0-L5 gated |

## Bramy przejścia

### Gate C — Core gate

Nie przechodzić dalej, jeśli:

- `src/core` importuje React lub `creator`.
- Command payloads nie są serializable.
- Brakuje minimalnego `validateProject`.
- Brakuje selectorów dla active/selected/root/children.
- Nie ma jawnego planu undo/redo albo command history.

### Gate D/F — Canvas edit gate

Nie przechodzić dalej, jeśli:

- Canvas utrzymuje własną listę obiektów poza Project.
- Pan/zoom zmienia transform obiektów.
- Drag zapisuje trwały stan poza command layer.
- Selection nie jest czytana z Project.selection.

### Gate G — Sync gate

Nie przechodzić dalej, jeśli:

- Canvas, hierarchy i inspector pokazują różne selected/active object.
- Inspector patchuje lokalny obiekt zamiast wysłać command.
- Hierarchy ma własną strukturę niezgodną z Project.children_ids.

### Gate H/I — Region/performance gate

Nie przechodzić dalej, jeśli:

- Regiony są ukryte wewnątrz rectangle bez jawnego `region_data` lub `region_rectangle`.
- All-debug labels są defaultowo włączone.
- Hit-test przy pointer move skanuje wszystko bez planu throttling/spatial index.

### Gate J/K — Component/export gate

Nie przechodzić dalej, jeśli:

- Panel_Monitor jest hardcoded JSX zamiast Project data.
- Save/load gubi parent/children links.
- SVG export zawiera debug/region internals jako visual output bez intencji.
- Component insert tworzy duplicate IDs.

### Gate O — Docking gate

Nie przechodzić dalej, jeśli:

- Docking zmienia InterfaceObject transforms.
- Docking state jest mieszany z canvas object layout.
- Docking library przejmuje source of truth.

### Gate P/Q — Logic/graph gate

Nie przechodzić dalej, jeśli:

- Event/action registry wymaga visual graph do działania.
- Graph patchuje DOM/canvas bez command layer.
- Graph viewport jest tym samym viewportem co canvas.
- Node state istnieje tylko w React Flow lub innym view adapterze.

## Zmieniona struktura sprintów

Istniejący `SPRINT 02` powinien objąć więcej headless fundamentów:

```text
Core model
+ command layer
+ command history
+ selectors
+ validation
+ tests
+ render adapter types
```

Dopiero później:

```text
Canvas renderer
+ primitive creation
+ selection/transform
```

## Minimalne testy, które powinny istnieć wcześnie

| Test | Cel |
|---|---|
| createInitialProject | Project ma poprawne pola startowe |
| object.create | dodaje object i aktualizuje root/parent links |
| object.patch | modyfikuje tylko wskazane pola |
| selection.set | active/selected IDs działają spójnie |
| validateHierarchy | łapie missing parent i circular refs |
| validateRegions | łapie broken linked_visual_id |
| commandHistory | object patch można cofnąć albo przynajmniej loguje historię |
| selectors | active/selected/root/children zwracają spójne dane |
| sample Panel_Monitor | sample validuje się bez broken links |

## Performance milestones

| Moment | Minimalny wymóg |
|---|---|
| Canvas MVP | 100 obiektów bez zauważalnego lag |
| Inspector/hierarchy sync | 1000 obiektów z akceptowalnym selection change |
| Region debug | overlay tylko dla selected/hover albo trybów debug |
| Graph workspace | pan/zoom bez re-render całej aplikacji |
| Component library | preview generowany kontrolowanie, nie na każdy render |

## Priorytet aktualny

Najbliższy rozwój powinien skoncentrować się na `Sprint 02 v2.1`. To jest najważniejszy etap wzmacniający dalszy development. Bez niego następne sprinty będą łatwe do napisania, ale trudne do utrzymania.