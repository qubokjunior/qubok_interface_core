# QUBOK_INTERFACE_CORE — development prompt chain 00

status: active
version: v2.2
doc_type: prompt_chain_index
last_updated: 2026-06-10

Cel pakietu: seria dokumentów do prowadzenia developmentu `qubok_interface_core` prompt po promptcie, z założeniem że poprzedni prompt zakończył się sukcesem: build przechodzi, testy manualne przeszły, a scope nie rozlał się poza fazę.

Ten pakiet jest jednocześnie warstwą wykonawczą i aktualnym indeksem dokumentacji. Dla Codex pierwszym plikiem nawigacyjnym jest `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`. Główny canonical entry point z regułami projektu to `00_CURRENT_SOURCE_OF_TRUTH.md`.

## Nazewnictwo v2.2

| Obszar | Obowiązująca nazwa |
|---|---|
| Project | `qubok_interface_core` |
| Short name | `interface_core` |
| Local PC path | `I:\Art\_AI\app_development\qubok_interface_core` |
| Graph system | `node_graph`, `nodeGraph`, `NodeGraph`, `node graph` |

Nie używać nowych referencji do starego nazewnictwa `state_graph` ani tymczasowych nazw projektu zakończonych `_00`.

## Status po aktualizacji v2.2

Weryfikacja techniczna i rozwój dokumentacji nie zmieniają głównego kierunku projektu. Potwierdzają go, ale doprecyzowują kilka wcześniejszych braków:

1. Command history / undo / redo musi wejść wcześniej, razem z command layer, a nie jako późniejszy dodatek.
2. Headless tests dla core, commands, validation i samples muszą wejść przed rozbudową UI.
3. Selectors powinny być osobną warstwą między Project model a canvas/inspector/hierarchy.
4. Render MVP może pozostać SVG/HTML, ale potrzebny jest render adapter, aby później nie zamknąć projektu na jeden renderer.
5. Event/action registry powinno być osobnym headless layer, nie częścią visual node graph.
6. Zewnętrzne biblioteki, takie jak graph/docking helpers, mogą być użyte tylko jako adaptery UI, nie jako source of truth.
7. Performance baseline powinien pojawić się wcześniej: object count tiers, throttling pointer move, CSS containment, później spatial index.

## Zasady nadrzędne

1. `Project JSON / data model` jest source of truth.
2. `src/core` nie importuje React ani `creator`.
3. React UI nie mutuje modelu bezpośrednio; wszystkie trwałe zmiany idą przez command layer.
4. Command history / undo contract musi istnieć przed ciężką edycją canvasu.
5. Selectors są wspólną warstwą odczytu dla canvas, inspector, hierarchy, status i target resolver.
6. `visual_bbox`, `layout_bbox` i `interaction_region` są oddzielnymi pojęciami.
7. `Rectangle` renderuje shape; `Region` obsługuje hit/hover/drag/drop/resize/snap/scroll.
8. Render adapter oddziela Project od SVG/HTML renderer.
9. Default UI pokazuje tylko funkcje L3/L4. Debug, graph, docking workbench i eksperymenty nie dominują ekranu startowego.
10. Każdy sprint ma jeden dominujący cel, listę zakazów i kryteria akceptacji.
11. Każdy external library choice musi odpowiedzieć: czy to adapter widoku, czy właściciel danych. Właścicielem danych pozostaje Project/core.

## Poziomy dojrzałości

| Level | Nazwa | Znaczenie | Widoczność |
|---|---|---|---|
| L0 | SPEC_ONLY | tylko opis / typy planowane | brak UI |
| L1 | HEADLESS_CORE | typy i funkcje testowalne bez React | brak lub dev only |
| L2 | DEBUG_VIEW | workbench/panel diagnostyczny | nie default |
| L3 | USER_WORKFLOW_MVP | działa w głównym workflow | default allowed |
| L4 | POLISHED_TOOL | stabilny UX, inspector, validation, shortcuts | default allowed |
| L5 | ADVANCED_COMPOSABLE | asset/graph/plugin-ready subsystem | zależnie od fazy |

## Minimalne wejście dla AI/Codex

Przy implementacji jednego sprintu wystarczy zwykle:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
2. `00_CURRENT_SOURCE_OF_TRUTH.md`
3. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
4. aktualny plik sprintu

## Kolejność użycia dokumentów sprintowych

1. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md` — stała reguła pisania kolejnych promptów.
2. `02_SPRINT_00_FOUNDATION_AUDIT_PROMPT.md` — audyt struktury i decyzji.
3. `03_SPRINT_01_SCAFFOLD_CONCEPT_SHELL_PROMPT.md` — shell aplikacji i tokens.
4. `04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md` — model danych, commands, command history, selectors, validation i headless tests.
5. `05_SPRINT_03_CANVAS_PRIMITIVES_SELECTION_PROMPT.md` — canvas, primitive, selection, transform.
6. `06_SPRINT_04_INSPECTOR_HIERARCHY_REGION_DEBUG_PROMPT.md` — sync, regiony, debug overlays.
7. `07_SPRINT_05_COMPONENT_PROOF_SAVE_EXPORT_PROMPT.md` — button_group, Panel_Monitor, save/export.
8. `08_SPRINT_06_DEFAULT_UI_CLEANUP_COMPONENT_LIBRARY_PROMPT.md` — default UI pass i library MVP.
9. `09_SPRINT_07_LAYOUT_PANEL_BUILDER_PROMPT.md` — layout, snap, box arranger, panel builder.
10. `10_SPRINT_08_DOCKING_SHELL_PROMPT.md` — app shell docking.
11. `11_SPRINT_09_EVENTS_ACTION_REGISTRY_PROMPT.md` — headless events/actions/target resolver.
12. `12_SPRINT_10_NODE_GRAPH_WORKSPACE_PROMPT.md` — node graph workspace dopiero po registry.
13. `13_SPRINT_11_ADVANCED_POST_MVP_PROMPT.md` — advanced/post-MVP bez psucia core.
14. `14_NEXT_CHAT_STARTER_TEMPLATE.md` — krótki szablon nowego czatu.

## Canonical / policy / matrix documents

| Plik | Rola |
|---|---|
| `00_CURRENT_SOURCE_OF_TRUTH.md` | główny entry point i priority rules |
| `15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md` | diagnoza i uzasadnienie zmian v2.1/v2.2 |
| `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md` | aktualna roadmapa techniczna z nazewnictwem v2.2 |
| `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md` | zasady użycia bibliotek zewnętrznych jako adapterów |
| `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md` | korekta starszych promptów |
| `19_DOC_STATUS_AND_ARCHIVE_INDEX.md` | status dokumentów i reguły archiwizacji |
| `20_AI_CONTEXT_MINIMAL.md` | najkrótszy kontekst dla AI/Codex |
| `21_DOCUMENTATION_HEALTH_CHECKLIST.md` | checklist utrzymania dokumentacji |
| `22_FEATURE_DEPENDENCY_MATRIX.md` | zależności funkcjonalności |
| `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md` | mapa owner -> command -> view -> validation |
| `24_CANONICAL_GLOSSARY.md` | słownik pojęć |
| `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` | główna mapa poruszania się po dokumentacji dla Codex |
| `26_CODEX_REPAIR_PLAYBOOK.md` | repair flow |
| `27_PROJECT_COMMANDS_AND_TESTS.md` | komendy i testy |
| `28_CODEX_PATCH_POLICY.md` | polityka patchy |
| `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` | mapa sukcesu etapów roadmapy |

## Reguła pracy

Nie łączyć dwóch dokumentów sprintowych w jeden prompt, jeśli oba dotykają innych właścicieli danych. Lepszy jest mały sprint z czystym buildem niż duży sprint z ukrytym długiem architektonicznym.

## Konflikt dokumentów

Jeżeli dokumenty mówią co innego, priorytet ma:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` dla wyboru, które pliki czytać.
2. `00_CURRENT_SOURCE_OF_TRUTH.md` dla aktualnych reguł canonical.
3. `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md`.
4. `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`.
5. aktualny sprint file.
6. starsze notatki i reference files.