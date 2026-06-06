# QUBOK_INTERFACE_CORE — interface_core_documentation_00

Status: working documentation folder
Created: 2026-06-06
Repository: qubok_interface_core

## Cel folderu

`interface_core_documentation_00` jest roboczym katalogiem dokumentacji dla projektu `qubok_interface_core`.

Folder służy do gromadzenia:
- notatek projektowych,
- opisów funkcjonalności,
- decyzji architektonicznych,
- wersji roboczych dokumentacji,
- map roadmapy,
- przypisań funkcji do etapów implementacji,
- definicji pojęć używanych w projekcie,
- materiałów referencyjnych do późniejszej dokumentacji docelowej.

## Główne założenie projektu

`qubok_interface_core` ma być parametrycznym silnikiem interfejsu, a nie zwykłym zestawem gotowych kontrolek UI.

Każdy element interfejsu powinien być opisywany jako obiekt danych, który może mieć między innymi:
- identyfikator,
- typ,
- transformację,
- styl,
- `visual_bbox`,
- `layout_bbox`,
- `interaction_region`,
- hierarchię,
- walidację,
- parametry eksportu,
- potencjalne powiązania z eventami i command layer.

## Roboczy pipeline pojęciowy

```text
primitive
-> bbox
-> transform/style
-> region/layout
-> group/panel
-> exposed parameters
-> library asset
-> reusable UI component
-> event/action/state behavior
-> application workspace
```

## Zasada źródła prawdy

Docelowo dokumentacja w tym folderze powinna konsekwentnie trzymać zasadę:

```text
Project JSON / data model = source of truth
```

Canvas, inspector, hierarchy, export, component library, debug, event registry i state graph powinny być widokami albo warstwami operacyjnymi modelu, a nie osobnymi źródłami sprzecznego stanu.

## Zakres roboczych dokumentów

Proponowany podział kolejnych plików w tym folderze:

| Plik | Cel |
|---|---|
| `00_INDEX.md` | indeks, cel folderu, aktualny zakres dokumentacji |
| `01_project_scope_and_core_principles.md` | definicja projektu, granice MVP, zasady rdzenia |
| `02_canonical_roadmap_from_zero.md` | roadmapa od zera, fazy implementacji, zależności |
| `03_functionality_catalog.md` | katalog funkcji, ich cel, dane wejściowe, wynik i miejsce w systemie |
| `04_data_model_and_command_layer.md` | model danych, commands, walidacja, source of truth |
| `05_regions_bbox_events.md` | visual/layout/interaction bbox, regiony, event flow |
| `06_tools_workspaces_panels.md` | narzędzia, workspaces, panele i ich role |
| `07_default_mvp_interface.md` | domyślny wygląd MVP, układ interface, panel monitor sample |
| `08_implementation_guardrails.md` | testy, kryteria akceptacji, czego nie mieszać za wcześnie |
| `09_notes_and_decisions_log.md` | bieżący log decyzji, zmian, pytań i nowych wniosków |

## Wstępne fazy roadmapy

| Faza | Cel | Przykładowe funkcje |
|---|---|---|
| A. Foundation | ustalić język projektu i granice | glossary, maturity levels, scope MVP/post-MVP |
| B. Scaffold + shell | stworzyć docelowy układ aplikacji od początku | top bar, left tools, center canvas, right inspector, bottom shelf |
| C. Core model + commands | zbudować source of truth i jedną ścieżkę mutacji | Project, InterfaceObject, applyCommand, validation skeleton |
| D. Canvas + viewport | renderować model bez osobnego stanu UI | SVG/HTML canvas, grid, pan, zoom |
| E. Primitive + selection | stworzyć alfabet UI i edycję obiektów | rectangle, text, line, region, select, box select, transform |
| F. Inspector + hierarchy | zsynchronizować widoki modelu | active object inspector, hierarchy tree, status bar |
| G. BBox + regions | oddzielić widoczność, layout i interakcję | visual_bbox, layout_bbox, interaction_region, debug overlays |
| H. Component proof | udowodnić składanie komponentów z primitive | button_group, Panel_Monitor, save/load/export |
| I. Default MVP cleanup | usunąć chaos debugowy z default UI | clean interface creator, debug hidden by default |
| J. Component library | zapisywać i instancjonować komponenty | component asset, preview, metadata, new IDs |
| K. Layout / panel builder | przyspieszyć składanie paneli | snap, align, distribute, box arranger, panel builder |
| L. App shell docking | split/merge paneli aplikacji | docking tree, splitter drag, workspace layout |
| M. Events/actions | przygotować logikę bez dominacji graph UI | event registry, action registry, target resolver |
| N. State graph workspace | osobny edytor logiki | graph viewport, nodes, commands output, debug watch |
| O. Advanced systems | późniejsze eksperymenty | procedural icons, path editor, external bridges |

## Reguły dokumentacji

1. Każda funkcjonalność powinna mieć przypisany etap roadmapy.
2. Każda funkcjonalność powinna mieć poziom dojrzałości: spec, headless core, debug view, MVP workflow, polished tool albo advanced/composable.
3. Dokumentacja powinna rozdzielać:
   - app shell docking,
   - canvas object layout,
   - graph viewport layout.
4. State graph nie powinien być opisywany jako pierwszy ekran MVP.
5. Panel nie powinien być opisywany jako pojedynczy rectangle, tylko jako struktura: frame, header, content, regions, sections, controls.
6. Region powinien być oddzielony od shape: rectangle renderuje, region reaguje.
7. Debug powinien być core feature, ale nie powinien dominować defaultowego interfejsu.

## Najbliższy sensowny następny dokument

Najbliższy dokument do dodania:

`01_project_scope_and_core_principles.md`

Powinien zawierać:
- czym jest `qubok_interface_core`,
- czym nie jest,
- zakres MVP,
- zakres post-MVP,
- główne pojęcia,
- pierwsze guardrails architektoniczne,
- canonical proof: rectangle + text + region -> button_group -> component -> export.
