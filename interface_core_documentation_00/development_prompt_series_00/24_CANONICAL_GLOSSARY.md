# 24 — Canonical glossary

status: active
version: v2.2
doc_type: glossary
last_updated: 2026-06-24

## Cel

Słownik pojęć dla `qubok_interface_core`. Używać przy pisaniu promptów, tasków, feedbacku i dokumentacji, aby nie mieszać podobnych pojęć: rectangle, region, panel, group, component, docking, layout, node graph viewport, feature_core, design tokens i runtime rules.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare graph naming jest superseded.

## Główne pojęcia

| Termin | Definicja | Nie mylić z |
|---|---|---|
| Project | główny model danych całego dokumentu/interfejsu | React state, filesystem file, workspace shell |
| Project JSON | serializowana forma Project | Component JSON, app workspace preset |
| InterfaceObject | pojedynczy obiekt UI w Project | React component |
| Primitive | podstawowy typ obiektu: rectangle, text, line, region, empty | component, panel, widget |
| Rectangle | primitive renderujący shape | interaction region |
| Text rectangle | rectangle z text_data / tekstowym renderem | zwykły label bez modelu |
| Line | primitive linii/krzywej | node graph edge |
| Empty | obiekt logiczny/grupujący bez bezpośredniego renderu | hidden object |
| Region | obiekt albo dane odpowiedzialne za interakcję | visual rectangle |
| visual_bbox | obszar wizualny shape | layout_bbox, interaction_region |
| layout_bbox | obszar używany do layoutu/snap/arrange | visual_bbox |
| interaction_region | obszar hit-testu/interakcji | visual_bbox |
| computed_bbox | bbox wyliczony z dzieci albo transformacji | ręcznie ustawiony bbox |
| Transform | x/y/scale/rotation/size albo podobny opis pozycji | viewport pan/zoom |
| Style | fill/stroke/radius/opacity/text style | app theme tokens |
| Selection | stan zaznaczenia w Project | hover, active dock area |
| Active object | główny obiekt w selection | focused input, active panel |
| Group | InterfaceObject z children_ids / parent relation | component asset |
| Panel | strukturalna grupa obiektów UI: frame/header/content/regions | app dock panel |
| Component | reusable serialized group/panel asset | React component |
| Component instance | kopia component asset w Project | linked instance, unless later supported |
| Library asset | zapisany component z metadata | file on disk only |
| Command | serializable opis mutacji Project/DockLayout/NodeGraph | direct function call from UI |
| applyCommand | core reducer/funkcja wykonująca command | React setState local patch |
| Command history | historia edycji do undo/redo/log | command registry |
| Selector | funkcja odczytująca Project bez mutacji | React hook owning data |
| Validation | sprawdzanie spójności Project/nodeGraph/docking/export | TypeScript type check only |
| Render adapter | mapowanie Project na neutralny model renderowania | SVG renderer itself |
| SVG renderer | renderer MVP dla primitive UI | Project model |
| Debug overlay | warstwa diagnostyczna widoku | default UI |
| Normal mode | czysty widok edycji bez nadmiaru debug labeli | all-debug mode |
| Inspector | panel edycji danych selected/active object | source of truth |
| Hierarchy | panel drzewa Project | filesystem tree |
| Status bar | mały pasek stanu selection/validation/zoom | debug console |
| Bottom shelf | dolna strefa preview/export/components/log | docking model |
| App shell docking | layout paneli aplikacji | canvas object layout |
| DockLayout | model split/merge/resize dla app shell | Project.objects_by_id |
| Canvas object layout | układ obiektów UI w Project | app shell docking |
| Node graph viewport layout | pozycje i zoom nodes w node graph | canvas viewport, app docking |
| Event registry | lista typów eventów i definicji event layer | visual node graph |
| Action registry | lista command-backed actions | arbitrary JavaScript execution |
| Target resolver | mechanizm wyboru obiektu docelowego dla event/action | CSS selector |
| Event assignment | przypisanie event -> target -> action -> parameters | node graph edge |
| Node graph | wizualny system komponowania eventów/conditions/actions/states | event registry itself |
| NodeGraph | model danych node graphu | React Flow internal state |
| Node | element node graph workspace | InterfaceObject primitive |
| Edge | połączenie w node graph workspace | line primitive |
| Workspace | tryb pracy aplikacji: interface creator, logic, docking, etc. | Project file |
| Maturity level | status gotowości funkcji L0-L5 | priority |
| L0 SPEC_ONLY | opis/specyfikacja bez implementacji | hidden working feature |
| L1 HEADLESS_CORE | działa bez React/UI | user workflow |
| L2 DEBUG_VIEW | widoczne w debug/workbench | default tool |
| L3 USER_WORKFLOW_MVP | działa w głównym workflow | fully polished |
| L4 POLISHED_TOOL | stabilne i gotowe defaultowo | experimental |
| L5 ADVANCED_COMPOSABLE | asset/graph/plugin/reusable subsystem | MVP requirement |

## 2026-06-24 additions — feature_core, tokens, runtime rules

| Termin | Definicja | Nie mylić z |
|---|---|---|
| feature_core | funkcjonalna baza definicji engine: runtime, layout, events, actions, nodes, colors, debug | Project model |
| design tokens | wizualne wartości i role: color/type/layout/parts/node_graph/runtime_feedback | command layer, runtime behavior |
| Figma mirror | lustrzana reprezentacja wizualnych tokenów w Figma variables | repo source of truth |
| runtime rules | reguły działania UI runtime, np. panel behavior, value math, command emission | visual tokens |
| layout rules | reguły rozmiaru, flow, overflow, readability i collapse | canvas object transforms |
| panel_resize_rule | reguła reakcji panelu na zmianę rozmiaru | CSS-only resize hack |
| panel_readable_size | próg, od którego panel/sekcja pozostaje czytelna | minimalny techniczny rozmiar |
| compact_when_default | ukrywanie pól z defaultową wartością bez kasowania danych | usuwanie parametrów |
| collapse_when_small | zwijanie sekcji przy małym rozmiarze panelu | global hide |
| overflow_rule | zasada obsługi nadmiaru treści: scroll, wrap, truncate, hide, popover | przypadkowy clipping |
| event/action runtime | headless warstwa eventów, target resolvera i command-backed actions | visual node graph |
| path_graph | późniejszy subsystem grafu/path/curve, bardziej proceduralny niż core runtime | core event/action runtime |
| color_swatch | pojedyncza próbka koloru | pełna paleta |
| swatch_stack | zestaw próbek służący do wyprowadzenia palety | layers stack |
| palette_from_swatches | procedura budująca paletę z próbek | ręczna edycja komponentów |
| palette_roles | role kolorów, np. background, panel, border, accent, warning | konkretne heksy bez znaczenia |
| target_by_color_role | wybór elementów przez rolę koloru | wybór przez selekcję UI |
| shift_saturation | transform nasycenia dla wybranych ról/kolorów | manualny repaint |
| shift_luminance | transform jasności dla wybranych ról/kolorów | manualny repaint |
| shift_hue | transform hue dla wybranych ról/kolorów | manualny repaint |
| assign_color_by_role | przypisanie koloru do elementu przez rolę | jednorazowa wartość bez semantyki |
| debug/inspection | trace, dry run, preview targets, validation reports | default UI |

## Rozróżnienia krytyczne

### Rectangle vs Region

Rectangle:
- renderuje wizualny shape,
- ma style,
- może być eksportowany do SVG.

Region:
- obsługuje interakcję,
- może być niewidoczny w normal mode,
- pojawia się w Interaction Debug,
- nie musi być eksportowany jako SVG visual output.

### Group vs Component

Group:
- istnieje w aktualnym Project,
- ma children_ids,
- jest edytowalny na canvasie.

Component:
- jest zapisanym assetem,
- można go wstawić jako nową kopię,
- może mieć metadata, tags, version, preview.

### Panel object vs Dock panel

Panel object:
- jest strukturą w Project,
- może być eksportowany jako Component JSON/SVG,
- ma frame/header/content/regions.

Dock panel:
- jest obszarem aplikacji, np. Inspector/Hierarchy/Canvas,
- należy do DockLayout,
- nie jest InterfaceObject.

### Canvas viewport vs Node graph viewport

Canvas viewport:
- pokazuje InterfaceObjects,
- operuje na Project canvas space.

Node graph viewport:
- pokazuje node graph nodes/edges,
- operuje na `Project.node_graphs[graph_id].viewport`.

Nie dzielić jednego viewport state między nimi.

### feature_core vs Project model

Project model:
- zapisuje konkretny dokument/interfejs użytkownika,
- pozostaje persistent source of truth,
- jest mutowany przez command layer.

feature_core:
- opisuje dostępne funkcje, reguły, akcje i konfiguracje engine,
- może zasilać runtime, UI wyboru, walidację i późniejszy node_graph,
- nie przejmuje danych użytkownika.

### design tokens vs Figma mirror

Design tokens:
- powinny mieć repo source of truth w JSON/TypeScript/schema,
- mogą być eksportowane lub synchronizowane.

Figma mirror:
- pomaga wizualnie projektować i porównywać wartości,
- nie jest runtime ownerem i nie steruje Project modelem.

## Zalecane nazwy w promptach

Używaj:

- `Project model`, nie „global state” bez doprecyzowania.
- `InterfaceObject`, nie „element” jeśli chodzi o obiekt danych.
- `app shell docking`, gdy chodzi o split/merge paneli aplikacji.
- `canvas object layout`, gdy chodzi o układ obiektów w projekcie.
- `node graph viewport`, gdy chodzi o pan/zoom node graphu.
- `node_graph`, `nodeGraph`, `NodeGraph`, gdy chodzi o graph system.
- `feature_core`, gdy chodzi o funkcjonalną bazę definicji engine.
- `design tokens`, gdy chodzi o wizualne wartości i role.
- `Figma mirror`, gdy chodzi o odbicie tokenów w Figma variables.
- `runtime rules`, `layout rules`, `panel_resize_rule`, gdy chodzi o zachowanie runtime.
- `region_rectangle`, gdy region jest osobnym obiektem.
- `region_data`, gdy region jest właściwością obiektu.
- `command-backed action`, gdy akcja eventu wywołuje command.

Unikaj:

- „panel” bez doprecyzowania: panel object czy dock panel?
- „node” bez doprecyzowania: node graph node czy UI primitive?
- „layout” bez doprecyzowania: app shell, canvas object czy node graph viewport?
- „component” jako React component, jeśli chodzi o reusable UI asset.
- starego graph naming.
- traktowania `feature_core` jako Project model.
- traktowania Figma jako runtime source of truth.

## Krótka forma dla tasków

Przy zadaniu używaj wzoru:

```text
Feature: [nazwa]
Type: [primitive/group/component/dock/workspace/event/node_graph]
Data owner: [Project/InterfaceObject/DockLayout/NodeGraph/feature_core]
Command path: [...]
View: [...]
Validation: [...]
Maturity: [L0-L5]
Default UI: [yes/no/debug/workspace only]
```
