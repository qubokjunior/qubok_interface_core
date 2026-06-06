# QUBOK_INTERFACE_CORE — interface_core_documentation_00

Status: working documentation folder
Created: 2026-06-06
Repository: qubok_interface_core
Last update: 2026-06-07 — value/rules/references/instance atlas update

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
- `parameter_data`,
- `reference_data`,
- `rule_assignments`,
- `instance_generator_data`,
- potencjalne powiązania z eventami i command layer.

## Roboczy pipeline pojęciowy

```text
primitive
-> bbox
-> transform/style
-> region/layout
-> group/panel
-> exposed parameters
-> parameter links / object references / rules
-> library asset
-> reusable UI component
-> generated helpers / instance-on-points overlays
-> event/action/state behavior
-> application workspace
```

## Zasada źródła prawdy

Docelowo dokumentacja w tym folderze powinna konsekwentnie trzymać zasadę:

```text
Project JSON / data model = source of truth
```

Canvas, inspector, hierarchy, export, component library, debug, event registry, rule engine, reference map, preview modes i state graph powinny być widokami albo warstwami operacyjnymi modelu, a nie osobnymi źródłami sprzecznego stanu.

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
| `10_atlas_value_rules_references_instances.md` | aktualizacja po atlasach 01/08–08/08: parameter links, object references, rules, panel size policies, instance-on-points, preview modes |

## Wstępne fazy roadmapy

| Faza | Cel | Przykładowe funkcje |
|---|---|---|
| A. Foundation | ustalić język projektu i granice | glossary, maturity levels, scope MVP/post-MVP |
| B. Scaffold + shell | stworzyć docelowy układ aplikacji od początku | top bar, left tools, center canvas, right inspector, bottom shelf |
| C. Core model + commands | zbudować source of truth i jedną ścieżkę mutacji | Project, InterfaceObject, applyCommand, validation skeleton |
| D. Canvas + viewport | renderować model bez osobnego stanu UI | SVG/HTML canvas, grid, pan, zoom |
| E. Primitive + selection | stworzyć alfabet UI i edycję obiektów | rectangle, text, line, region, select, box select, transform |
| F. Inspector + hierarchy | zsynchronizować widoki modelu | active object inspector, hierarchy tree, status bar |
| G. BBox + regions + preview modes | oddzielić widoczność, layout, interakcję i overlaye | visual_bbox, layout_bbox, interaction_region, default/debug/rules/state/link overlays |
| H. Component proof | udowodnić składanie komponentów z primitive | button_group, Panel_Monitor, save/load/export |
| I. Default MVP cleanup | usunąć chaos debugowy z default UI | clean interface creator, debug hidden by default |
| J. Component library | zapisywać i instancjonować komponenty | component asset, preview, metadata, new IDs |
| K. Layout / panel builder / size policies | przyspieszyć składanie paneli i reguły rozmiaru | snap, align, distribute, box arranger, panel builder, min/max policies |
| L. App shell docking | split/merge paneli aplikacji | docking tree, splitter drag, workspace layout |
| M. Events/actions/references/rules headless | przygotować logikę i zależności bez dominacji graph UI | event registry, action registry, target resolver, parameter links, rule sets, reference maps |
| N. State graph workspace | osobny edytor logiki | graph viewport, nodes, commands output, debug watch |
| O. Advanced systems / instance-on-points | późniejsze composable subsystemy | procedural icons, path editor, instance-on-points handles, generated helpers, external bridges |

## Aktualizacja 2026-06-07 — wnioski po atlasach 01/08–08/08

Nowa seria grafik doprecyzowała, że projekt powinien traktować parametry, referencje, reguły i generowane instancje jako pełnoprawne warstwy modelu, nie jako dekoracyjne overlaye.

Najważniejsze dodatki:

1. `ParameterLink` — jawne połączenie parametru źródłowego z parametrami docelowymi, z trybem propagacji, wyrażeniem, blokadą ręcznej edycji celu i ochroną przed cyklami.
2. `ObjectReference` — przypisywanie obiektów jako źródeł geometrii, osi, bboxów, punktów min/max, centrum, handle object lub target property.
3. `RuleSet` — reguły rozmiaru, semantyki, estetyki, interakcji i zachowania z warunkami, priorytetem, zakresem, dziedziczeniem i walidacją konfliktów.
4. `Panel × group size policy` — jawne tryby: respect child min/max, ignore child min/max + align, force panel size + child adaptation.
5. `InstanceOnPoints` — subsystem do generowania uchwytów, helperów, markerów i overlayów na punktach / segmentach / bboxach / krzywych.
6. `Orientation modes` — facing center, outward, tangent, normal, fixed world, mirrored auto, context-aware, per-point override.
7. `PreviewMode` — Default, Debug, Rules, State Machine, Link Elements & References jako osobne tryby z filtrami overlay.

Te funkcje powinny być implementowane zgodnie z maturity levels: najpierw typy, model i walidacja; potem debug/workbench; dopiero później polished user workflow.

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
8. Preview mode zmienia widoczność danych diagnostycznych i referencyjnych; nie zmienia danych projektowych.
9. Linki, referencje, rules i instance-on-points muszą przechodzić przez source-of-truth model, command layer i validation.

## Najbliższe sensowne dokumenty do dodania

1. `01_project_scope_and_core_principles.md`
2. `02_canonical_roadmap_from_zero.md`
3. `03_functionality_catalog.md`
4. `04_data_model_and_command_layer.md`
5. `05_regions_bbox_events.md`
6. `06_tools_workspaces_panels.md`
7. `07_default_mvp_interface.md`
8. `08_implementation_guardrails.md`
9. `09_notes_and_decisions_log.md`

Nowe dokumenty powinny importować decyzje z `10_atlas_value_rules_references_instances.md`, szczególnie w rozdziałach o `parameter_data`, `reference_data`, `rule_assignments`, `layout_data`, `preview_data` i `instance_generator_data`.
