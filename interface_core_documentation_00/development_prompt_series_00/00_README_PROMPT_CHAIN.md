# QUBOK_INTERFACE_CORE — development prompt chain 00

Cel pakietu: seria dokumentów do prowadzenia developmentu `qubok_interface_core` prompt po promptcie, z założeniem że poprzedni prompt zakończył się sukcesem: build przechodzi, testy manualne przeszły, a scope nie rozlał się poza fazę.

Ten pakiet nie zastępuje pełnej dokumentacji architektury. Jest warstwą wykonawczą: ma skracać kontekst, zmniejszać zużycie tokenów i poprawiać jakość kodu przez małe, zamknięte sprinty.

## Zasady nadrzędne

1. `Project JSON / data model` jest source of truth.
2. `src/core` nie importuje React ani `creator`.
3. React UI nie mutuje modelu bezpośrednio; wszystkie trwałe zmiany idą przez command layer.
4. `visual_bbox`, `layout_bbox` i `interaction_region` są oddzielnymi pojęciami.
5. `Rectangle` renderuje shape; `Region` obsługuje hit/hover/drag/drop/resize/snap/scroll.
6. Default UI pokazuje tylko funkcje L3/L4. Debug, graph, docking workbench i eksperymenty nie dominują ekranu startowego.
7. Każdy sprint ma jeden dominujący cel, listę zakazów i kryteria akceptacji.

## Poziomy dojrzałości

| Level | Nazwa | Znaczenie | Widoczność |
|---|---|---|---|
| L0 | SPEC_ONLY | tylko opis / typy planowane | brak UI |
| L1 | HEADLESS_CORE | typy i funkcje testowalne bez React | brak lub dev only |
| L2 | DEBUG_VIEW | workbench/panel diagnostyczny | nie default |
| L3 | USER_WORKFLOW_MVP | działa w głównym workflow | default allowed |
| L4 | POLISHED_TOOL | stabilny UX, inspector, validation, shortcuts | default allowed |
| L5 | ADVANCED_COMPOSABLE | asset/graph/plugin/preset-ready | zależnie od fazy |

## Kolejność użycia dokumentów

1. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md` — stała reguła pisania kolejnych promptów.
2. `02_SPRINT_00_FOUNDATION_AUDIT_PROMPT.md` — audyt struktury i decyzji.
3. `03_SPRINT_01_SCAFFOLD_CONCEPT_SHELL_PROMPT.md` — shell aplikacji i tokens.
4. `04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md` — model danych, commands, validation.
5. `05_SPRINT_03_CANVAS_PRIMITIVES_SELECTION_PROMPT.md` — canvas, primitive, selection, transform.
6. `06_SPRINT_04_INSPECTOR_HIERARCHY_REGION_DEBUG_PROMPT.md` — sync, regiony, debug overlays.
7. `07_SPRINT_05_COMPONENT_PROOF_SAVE_EXPORT_PROMPT.md` — button_group, Panel_Monitor, save/export.
8. `08_SPRINT_06_DEFAULT_UI_CLEANUP_COMPONENT_LIBRARY_PROMPT.md` — default UI pass i library MVP.
9. `09_SPRINT_07_LAYOUT_PANEL_BUILDER_PROMPT.md` — layout, snap, box arranger, panel builder.
10. `10_SPRINT_08_DOCKING_SHELL_PROMPT.md` — app shell docking.
11. `11_SPRINT_09_EVENTS_ACTION_REGISTRY_PROMPT.md` — headless events/actions/target resolver.
12. `12_SPRINT_10_STATE_GRAPH_WORKSPACE_PROMPT.md` — graph workspace dopiero po registry.
13. `13_SPRINT_11_ADVANCED_POST_MVP_PROMPT.md` — advanced/post-MVP bez psucia core.
14. `14_NEXT_CHAT_STARTER_TEMPLATE.md` — krótki szablon nowego czatu.

## Reguła pracy

Nie łączyć dwóch dokumentów sprintowych w jeden prompt, jeśli oba dotykają innych właścicieli danych. Lepszy jest mały sprint z czystym buildem niż duży sprint z ukrytym długiem architektonicznym.
