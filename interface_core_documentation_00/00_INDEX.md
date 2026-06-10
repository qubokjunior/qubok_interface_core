# QUBOK_INTERFACE_CORE — interface_core_documentation_00

Status: working documentation folder
Created: 2026-06-06
Repository: qubok_interface_core
Last update: 2026-06-11 — source of truth, node_graph canon, roadmap mapping, maturity matrix, Codex protocol

## Cel folderu

`interface_core_documentation_00` jest roboczym katalogiem dokumentacji dla projektu `qubok_interface_core`.

Folder służy do gromadzenia:

- decyzji architektonicznych,
- definicji pojęć,
- roadmapy i faz implementacji,
- opisów funkcjonalności,
- standardów wizualizacji UI,
- protokołów developmentowych,
- materiałów referencyjnych do późniejszej dokumentacji docelowej.

## Główne założenie projektu

`qubok_interface_core` ma być parametrycznym silnikiem interfejsu, a nie zwykłym zestawem gotowych kontrolek UI.

Każdy istotny element interfejsu powinien być opisywany jako obiekt danych, który może mieć między innymi:

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
- `relation_data`,
- `instance_generator_data`,
- powiązania z eventami, command layer i `node_graph`.

## Canonical entrypoint

Czytaj w tej kolejności:

| Plik | Cel |
|---|---|
| `00_CURRENT_SOURCE_OF_TRUTH.md` | aktualny kanon projektu, scope i precedence dokumentów |
| `01_TERMINOLOGY_CANON_NODE_GRAPH.md` | finalna decyzja: `node_graph` zastępuje stare `state_graph` |
| `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md` | rozdzielenie Phase A-O i Screen State 01-08 |
| `03_FEATURE_MATURITY_MATRIX.md` | poziomy L0-L5 i gate dla funkcji |
| `04_CODEX_DEVELOPMENT_PROTOCOL.md` | protokół promptów/sprintów dla Codex i nowych czatów |

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
-> event/action/node_graph behavior
-> application workspace
```

## Zasada źródła prawdy

```text
Project JSON / data model = source of truth
```

Canvas, inspector, hierarchy, export, component library, debug, event registry, rule engine, reference map, preview modes i `node_graph` są widokami albo warstwami operacyjnymi modelu, a nie osobnymi źródłami sprzecznego stanu.

## Decyzja nazewnicza 2026-06-11

`node_graph` jest ostateczną nazwą systemu graph.

| Stare | Nowe |
|---|---|
| `state_graph` | `node_graph` |
| `state graph` | `node_graph` |
| `StateGraphWorkspace` dla nowych plików | `NodeGraphWorkspace` |
| `stateGraphTypes` dla nowych plików | `nodeGraphTypes` |

`state` pozostaje poprawnym pojęciem dla stanów obiektu/komponentu, na przykład: hover, pressed, disabled, selected, dirty, error, `states_slot`, `state_transition`.

## Fazy roadmapy implementacyjnej

| Faza | Cel | Przykładowe funkcje |
|---|---|---|
| A. Foundation | ustalić język projektu i granice | glossary, maturity levels, scope MVP/post-MVP |
| B. Scaffold + shell | stworzyć docelowy układ aplikacji od początku | top bar, left tools, center canvas, right inspector, bottom shelf |
| C. Core model + commands | zbudować source of truth i jedną ścieżkę mutacji | Project, InterfaceObject, applyCommand, validation skeleton |
| D. Canvas + viewport | renderować model bez osobnego stanu UI | SVG/HTML canvas, grid, pan, zoom |
| E. Primitive + selection | stworzyć alfabet UI i edycję obiektów | rectangle, text, line, region, select, box select, transform |
| F. Inspector + hierarchy | zsynchronizować widoki modelu | active object inspector, hierarchy tree, status bar |
| G. BBox + regions + preview modes | oddzielić widoczność, layout, interakcję i overlaye | visual_bbox, layout_bbox, interaction_region, debug/rules/references overlays |
| H. Component proof | udowodnić składanie komponentów z primitive | button_group, Panel_Monitor, save/load/export |
| I. Default MVP cleanup | usunąć chaos debugowy z default UI | clean interface creator, debug hidden by default |
| J. Component library | zapisywać i instancjonować komponenty | component asset, preview, metadata, new IDs |
| K. Layout / panel builder / size policies | przyspieszyć składanie paneli i reguły rozmiaru | snap, align, box arranger, panel builder |
| L. App shell docking | split/merge paneli aplikacji | docking tree, splitter drag, workspace layout |
| M. Events/actions/references/rules headless | przygotować logikę i zależności bez dominacji graph UI | event registry, action registry, target resolver, links, rules |
| N. Node graph workspace | osobny edytor graph | node_graph viewport, profiles, ports, output contracts, debug watch |
| O. Advanced systems / instance-on-points | późniejsze composable subsystemy | procedural icons, path editor, instance-on-points, external bridges |

## Screen State 01-08

Screen State 01-08 to wizualne stany dokumentacyjne fullscreen interface. Nie są ścisłym sprint planem kodu.

| Screen State | Sens |
|---|---|
| 01 | Foundation / model / primitive |
| 02 | Canvas / primitive / inspector sync |
| 03 | Regions / bbox / interaction debug |
| 04 | Spreadsheet / filters / parameter visibility |
| 05 | Layers / hierarchy / tags / style sorting |
| 06 | Panel builder / component structure / states |
| 07 | Actions / events / command layer / node_graph |
| 08 | Docking / pinning / export / final MVP view |

## Reguły dokumentacji

1. Każda funkcjonalność powinna mieć przypisany etap roadmapy.
2. Każda funkcjonalność powinna mieć poziom dojrzałości: L0-L5.
3. Dokumentacja powinna rozdzielać app shell docking, canvas object layout i graph viewport layout.
4. `node_graph` nie powinien być defaultowym ekranem MVP.
5. Panel nie jest pojedynczym rectangle, tylko strukturą: frame, header, content, regions, sections, controls.
6. Region jest oddzielony od shape: rectangle renderuje, region reaguje.
7. Debug jest core feature, ale nie dominuje defaultowego interfejsu.
8. Preview mode zmienia widoczność danych diagnostycznych i referencyjnych; nie zmienia danych projektowych.
9. Linki, referencje, rules, states i instance-on-points muszą przechodzić przez source-of-truth model, command layer i validation.
10. Nowe dokumenty i prompty nie powinny wprowadzać nowego użycia `state_graph`.

## Dokumenty referencyjne

| Plik | Cel |
|---|---|
| `10_atlas_value_rules_references_instances.md` | parameter links, object references, rules, size policies, instance-on-points, preview modes |
| `11_VISUAL_DIAGNOSTICS_AND_STYLE_STANDARD_2026_06_10.txt` | standard realnego screenshotu narzędzia zamiast infografiki |
| `12_RULES_STATES_RELATIONS_PROCEDURAL_UI_2026_06_10.txt` | rules, states, responsiveness, value relations, relation graph, shape graph, array |
| `13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt` | registries, customization, tokens, Preview Service, Node Adapter Registry |
| `14_DEBUG_DOCKING_WORKSPACE_KERNEL_GRAPH_PROFILES_2026_06_10.txt` | debug_command, docking ownership, Workspace Kernel, node_graph profiles |

## Precedence

Gdy dokumenty są sprzeczne, obowiązuje kolejność:

1. `00_CURRENT_SOURCE_OF_TRUTH.md`
2. `01_TERMINOLOGY_CANON_NODE_GRAPH.md`
3. `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
4. `03_FEATURE_MATURITY_MATRIX.md`
5. `04_CODEX_DEVELOPMENT_PROTOCOL.md`
6. pliki tematyczne `10-14`
7. starsze dated notes i prompty graficzne
