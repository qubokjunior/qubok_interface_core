# qubok_interface_core

interface_engine

## Documentation

Robocza dokumentacja projektu znajduje się w:

- `interface_core_documentation_00/`

Aktualny kanon dokumentacji zaczyna się od:

- `interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md`
- `interface_core_documentation_00/01_TERMINOLOGY_CANON_NODE_GRAPH.md`
- `interface_core_documentation_00/02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
- `interface_core_documentation_00/03_FEATURE_MATURITY_MATRIX.md`
- `interface_core_documentation_00/04_CODEX_DEVELOPMENT_PROTOCOL.md`

## Current canonical decisions

- `Project JSON / data model` jest źródłem prawdy.
- Zmiany trwałe przechodzą przez command layer.
- Rectangle renderuje shape; region obsługuje interakcję.
- Panel to struktura: frame, header, content, footer, sections, rows, regions, exposed parameters.
- `node_graph` jest ostateczną nazwą systemu graph.
- `state_graph` jest nazwą legacy i nie powinna być używana w nowych dokumentach ani publicznych nazwach.
- `state` pozostaje poprawnym pojęciem dla wariantów obiektu/komponentu: hover, pressed, disabled, dirty, selected, error.
- App shell docking, canvas object layout i graph viewport layout to trzy oddzielne systemy.
- Funkcje advanced muszą przechodzić przez maturity levels: L0 spec, L1 headless, L2 debug, L3 MVP workflow, L4 polished, L5 advanced/composable.

## Recent documentation updates

Aktualizacja 2026-06-10 dodała standard wizualizacji interface, diagnozę błędów w generowanych grafikach, zasady node graph UI, registry-driven customization, Preview Service, debug_command, Workspace Kernel i profile graph z output contracts.

Aktualizacja 2026-06-11 dodała bieżący source of truth, finalną decyzję nazewniczą `node_graph`, mapowanie Phase A-O do Screen State 01-08, matrycę dojrzałości funkcji oraz protokół przygotowywania promptów/sprintów developmentowych.

## Important files

- `interface_core_documentation_00/11_VISUAL_DIAGNOSTICS_AND_STYLE_STANDARD_2026_06_10.txt`
- `interface_core_documentation_00/12_RULES_STATES_RELATIONS_PROCEDURAL_UI_2026_06_10.txt`
- `interface_core_documentation_00/13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt`
- `interface_core_documentation_00/14_DEBUG_DOCKING_WORKSPACE_KERNEL_GRAPH_PROFILES_2026_06_10.txt`
- `interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md`
- `interface_core_documentation_00/01_TERMINOLOGY_CANON_NODE_GRAPH.md`
- `interface_core_documentation_00/02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
- `interface_core_documentation_00/03_FEATURE_MATURITY_MATRIX.md`
- `interface_core_documentation_00/04_CODEX_DEVELOPMENT_PROTOCOL.md`
