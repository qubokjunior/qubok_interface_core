# 24 — Canonical glossary

status: active
version: v2.1
doc_type: glossary
last_updated: 2026-06-10

## Cel

Słownik pojęć dla `qubok_interface_core`. Używać przy pisaniu promptów, tasków, feedbacku i dokumentacji, aby nie mieszać podobnych pojęć: rectangle, region, panel, group, component, docking, layout, graph viewport.

## Główne pojęcia

| Termin | Definicja | Nie mylić z |
|---|---|---|
| Project | główny model danych całego dokumentu/interfejsu | React state, filesystem file, workspace shell |
| Project JSON | serializowana forma Project | Component JSON, app workspace preset |
| InterfaceObject | pojedynczy obiekt UI w Project | React component |
| Primitive | podstawowy typ obiektu: rectangle, text, line, region, empty | component, panel, widget |
| Rectangle | primitive renderujący shape | interaction region |
| Text rectangle | rectangle z text_data / tekstowym renderem | zwykły label bez modelu |
| Line | primitive linii/krzywej | edge state graph |
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
| Command | serializable opis mutacji Project/DockLayout/Graph | direct function call from UI |
| applyCommand | core reducer/funkcja wykonująca command | React setState local patch |
| Command history | historia edycji do undo/redo/log | command registry |
| Selector | funkcja odczytująca Project bez mutacji | React hook owning data |
| Validation | sprawdzanie spójności Project/graph/docking/export | TypeScript type check only |
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
| Graph viewport layout | pozycje i zoom nodes w state graph | canvas viewport, app docking |
| Event registry | lista typów eventów i definicji event layer | visual graph |
| Action registry | lista command-backed actions | arbitrary JavaScript execution |
| Target resolver | mechanizm wyboru obiektu docelowego dla event/action | CSS selector |
| Event assignment | przypisanie event -> target -> action -> parameters | graph edge |
| State graph | wizualny system komponowania eventów/conditions/actions/states | event registry itself |
| Node | element state graph workspace | InterfaceObject primitive |
| Edge | połączenie w graph workspace | line primitive |
| Workspace | tryb pracy aplikacji: interface creator, logic, docking, etc. | Project file |
| Maturity level | status gotowości funkcji L0-L5 | priority |
| L0 SPEC_ONLY | opis/specyfikacja bez implementacji | hidden working feature |
| L1 HEADLESS_CORE | działa bez React/UI | user workflow |
| L2 DEBUG_VIEW | widoczne w debug/workbench | default tool |
| L3 USER_WORKFLOW_MVP | działa w głównym workflow | fully polished |
| L4 POLISHED_TOOL | stabilne i gotowe defaultowo | experimental |
| L5 ADVANCED_COMPOSABLE | plugin/asset/graph/reusable subsystem | MVP requirement |

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

### Canvas viewport vs Graph viewport

Canvas viewport:
- pokazuje InterfaceObjects,
- operuje na Project canvas space.

Graph viewport:
- pokazuje state graph nodes/edges,
- operuje na Project.state_graphs[graph_id].viewport.

Nie dzielić jednego viewport state między nimi.

## Zalecane nazwy w promptach

Używaj:

- `Project model`, nie „global state” bez doprecyzowania.
- `InterfaceObject`, nie „element” jeśli chodzi o obiekt danych.
- `app shell docking`, gdy chodzi o split/merge paneli aplikacji.
- `canvas object layout`, gdy chodzi o układ obiektów w projekcie.
- `graph viewport`, gdy chodzi o pan/zoom node graphu.
- `region_rectangle`, gdy region jest osobnym obiektem.
- `region_data`, gdy region jest właściwością obiektu.
- `command-backed action`, gdy akcja eventu wywołuje command.

Unikaj:

- „panel” bez doprecyzowania: panel object czy dock panel?
- „node” bez doprecyzowania: state graph node czy UI primitive?
- „layout” bez doprecyzowania: app shell, canvas object czy graph viewport?
- „component” jako React component, jeśli chodzi o reusable UI asset.

## Krótka forma dla tasków

Przy zadaniu używaj wzoru:

```text
Feature: [nazwa]
Type: [primitive/group/component/dock/workspace/event/graph]
Data owner: [Project/InterfaceObject/DockLayout/StateGraph]
Command path: [...]
View: [...]
Validation: [...]
Maturity: [L0-L5]
Default UI: [yes/no/debug/workspace only]
```