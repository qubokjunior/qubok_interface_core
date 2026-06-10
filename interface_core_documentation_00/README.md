# interface_core_documentation_00

Folder roboczy dla skondensowanej dokumentacji `qubok_interface_core`: decyzje, roadmapa, standardy UI, mechaniki engine, aktualizacje po grafikach i protokoły developmentowe.

## Canonical entrypoint

Czytaj w tej kolejności:

| Plik | Cel |
|---|---|
| `00_CURRENT_SOURCE_OF_TRUTH.md` | aktualny kanon projektu i kolejność pierwszeństwa dokumentów |
| `01_TERMINOLOGY_CANON_NODE_GRAPH.md` | finalna decyzja: `node_graph` zastępuje stare `state_graph` |
| `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md` | rozdzielenie roadmapy implementacyjnej Phase A-O i wizualnych Screen State 01-08 |
| `03_FEATURE_MATURITY_MATRIX.md` | poziomy L0-L5 i gate dla funkcji |
| `04_CODEX_DEVELOPMENT_PROTOCOL.md` | protokół promptów/sprintów dla Codex i nowych czatów developmentowych |

## Recent reference files

| Plik | Cel |
|---|---|
| `10_atlas_value_rules_references_instances.md` | parameter links, object references, rules, panel size policies, instance-on-points, preview modes |
| `11_VISUAL_DIAGNOSTICS_AND_STYLE_STANDARD_2026_06_10.txt` | standard: realny screenshot roboczego narzędzia zamiast infografiki/postera |
| `12_RULES_STATES_RELATIONS_PROCEDURAL_UI_2026_06_10.txt` | rules, states, responsiveness, value relations, relation graph, shape graph, instance on points, array |
| `13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt` | registry-driven customization, tokens, preview service, node adapters |
| `14_DEBUG_DOCKING_WORKSPACE_KERNEL_GRAPH_PROFILES_2026_06_10.txt` | debug_command, docking ownership, workspace kernel, graph profiles and output contracts |

## Current canonical decisions

- `Project JSON / data model` jest źródłem prawdy.
- Canvas, inspector, hierarchy, spreadsheet, export, debug, component library, event registry, rules, references, preview modes i `node_graph` są widokami albo narzędziami nad modelem.
- Zmiany trwałe przechodzą przez command layer.
- Rectangle renderuje shape; region obsługuje hit/hover/drag/drop/resize/snap/scroll/content/layout.
- Panel to struktura: frame, header, content, footer, sections, rows, regions, exposed parameters.
- `node_graph` jest ostateczną nazwą systemu graph.
- `state_graph` jest nazwą legacy; nie używać jej w nowych dokumentach ani publicznych nazwach.
- `state` pozostaje poprawnym pojęciem dla wariantów obiektu/komponentu: hover, pressed, disabled, dirty, selected, error.
- App shell docking, canvas object layout i graph viewport layout to trzy oddzielne systemy.
- Default UI ma być compact, dark, docked, techniczny i realnie używalny w fullscreen.
- Każda większa funkcja powinna mieć phase, maturity level, owner, command path, validation i test.

## Documentation precedence

Gdy dokumenty są sprzeczne, obowiązuje kolejność:

1. `00_CURRENT_SOURCE_OF_TRUTH.md`
2. `01_TERMINOLOGY_CANON_NODE_GRAPH.md`
3. `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
4. `03_FEATURE_MATURITY_MATRIX.md`
5. `04_CODEX_DEVELOPMENT_PROTOCOL.md`
6. pliki tematyczne `10-14`
7. starsze dated notes i prompty graficzne
