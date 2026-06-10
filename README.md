# qubok_interface_core

interface_engine

## Documentation

Robocza dokumentacja projektu znajduje się w:

- `interface_core_documentation_00/`

Aktualizacja 2026-06-06 dodaje:

- skondensowane wnioski po grafikach fullscreen UI,
- osiem etapów widoku programu dla roadmapy,
- prompt do wygenerowania 8 osobnych infografik PNG.

Aktualizacja 2026-06-10 dodaje:

- standard wizualizacji interface: realny screenshot roboczego narzędzia zamiast infografiki/postera,
- diagnozę błędów w generowanych grafikach: kolizje tekstu, posterowe bottom panels, niespójne dane, spaghetti wires, brak jawnego outputu,
- kanon dla node graph: socket rows, typy danych, orthogonal routing, reroute points, wire lanes, jawny output,
- doprecyzowanie mechanik: rules, states, responsiveness, value relations, relation graph, shape through state_graph, instance on points, array,
- priorytet MVP dla parametrycznego interface engine.

Aktualizacja 2026-06-10B dodaje:

- architekturę registry-driven customization: Object Schema Registry, Property Registry, Command Registry, Function Registry, Node Adapter Registry, Theme/Icon/Token Registry, Preview Registry, Debug/Event Bus,
- zasady `states_slot`, transition fallback, computed/effective style i opcjonalnych state variants,
- wspólny `Preview Service` dla canvas preview, node preview, state preview, timeline preview, procedural preview i debug preview,
- model tokenów kolorów, proceduralnych swatchy, semantic icon roles, temporary icons oraz późniejszej migracji placeholderów,
- zasady FunctionDefinition / NodeAdapter / Graph Palette / Node Instance oraz reguły ukrywania funkcji niedozwolonych w graphach,
- model `debug_command`, contextual filters, function/event spreadsheet, history panel i library panel jako data-source + template + repeater,
- rozdzielenie `app_shell_docking`, `canvas_object_layout`, `graph_viewport_layout`, customize_Interface mode i layout store,
- wspólny Workspace Kernel, curve/shape functions, event binding + weights oraz profile state_graph z output contracts.

Nowe pliki:

- `interface_core_documentation_00/11_VISUAL_DIAGNOSTICS_AND_STYLE_STANDARD_2026_06_10.txt`
- `interface_core_documentation_00/12_RULES_STATES_RELATIONS_PROCEDURAL_UI_2026_06_10.txt`
- `interface_core_documentation_00/13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt`
- `interface_core_documentation_00/14_DEBUG_DOCKING_WORKSPACE_KERNEL_GRAPH_PROFILES_2026_06_10.txt`
