# 19 — Documentation status and archive index

status: active
version: v2.2
doc_type: documentation_index
last_updated: 2026-06-10

## Cel

Ten dokument określa status plików dokumentacji, aby AI/Codex i człowiek nie traktowali wszystkich plików jako równorzędnych. Nie usuwa historii projektu. Rozdziela dokumenty aktywne, aktywne z poprawką v2.2, pomocnicze i archiwalne.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## Statusy

| Status | Znaczenie |
|---|---|
| ACTIVE | aktualne źródło prawdy lub aktywny plik operacyjny |
| ACTIVE_WITH_V2_2_AMENDMENT | plik nadal użyteczny, ale musi być czytany przez korektę v2.2 |
| POLICY | aktywna reguła / polityka techniczna |
| PROMPT_ONLY | prompt wykonawczy, nie pełna architektura |
| REFERENCE | materiał pomocniczy / kontekst |
| SUPERSEDED | zastąpione przez nowszy dokument |
| ARCHIVE | historia, nie używać bezpośrednio do implementacji |

## Aktualne pliki canonical / active

| Plik | Status | Priorytet | Uwagi |
|---|---|---:|---|
| `00_CURRENT_SOURCE_OF_TRUTH.md` | ACTIVE | 1 | główny entry point v2.2 |
| `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md` | ACTIVE | 2 | protokół promptów v2.2 |
| `15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md` | ACTIVE | 2 | uzasadnienie zmian v2.1/v2.2 |
| `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md` | ACTIVE | 2 | aktualna kolejność techniczna z nazewnictwem v2.2 |
| `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md` | POLICY | 2 | biblioteki tylko jako adaptery |
| `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md` | ACTIVE | 2 | korekta starszych promptów |
| `20_AI_CONTEXT_MINIMAL.md` | ACTIVE | 1 | minimalny kontekst dla AI |
| `21_DOCUMENTATION_HEALTH_CHECKLIST.md` | ACTIVE | 3 | checklist utrzymania docs |
| `22_FEATURE_DEPENDENCY_MATRIX.md` | ACTIVE | 2 | zależności funkcji |
| `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md` | ACTIVE | 2 | mapa owner/command/view/validation |
| `24_CANONICAL_GLOSSARY.md` | ACTIVE | 2 | słownik pojęć |
| `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` | ACTIVE | 1 | główna mapa Codex |
| `26_CODEX_REPAIR_PLAYBOOK.md` | ACTIVE | 3 | repair flow |
| `27_PROJECT_COMMANDS_AND_TESTS.md` | ACTIVE | 3 | build/test discovery |
| `28_CODEX_PATCH_POLICY.md` | ACTIVE | 3 | patch policy |
| `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` | ACTIVE | 2 | success/acceptance map |

## Prompt chain files

| Plik | Status | Uwagi |
|---|---|---|
| `02_SPRINT_00_FOUNDATION_AUDIT_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | czytać z dokumentem 18 |
| `03_SPRINT_01_SCAFFOLD_CONCEPT_SHELL_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | czytać z dokumentem 18 |
| `04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | core v2.2 naming obowiązuje |
| `05_SPRINT_03_CANVAS_PRIMITIVES_SELECTION_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | musi używać selectors/render adapter |
| `06_SPRINT_04_INSPECTOR_HIERARCHY_REGION_DEBUG_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | schema-driven inspector, selectors |
| `07_SPRINT_05_COMPONENT_PROOF_SAVE_EXPORT_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | sample tests, render model export |
| `08_SPRINT_06_DEFAULT_UI_CLEANUP_COMPONENT_LIBRARY_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | deep copy instances only |
| `09_SPRINT_07_LAYOUT_PANEL_BUILDER_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | performance guardrails |
| `10_SPRINT_08_DOCKING_SHELL_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | DockLayout source of truth |
| `11_SPRINT_09_EVENTS_ACTION_REGISTRY_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | prefer `core/events` mental model |
| `12_SPRINT_10_NODE_GRAPH_WORKSPACE_PROMPT.md` | ACTIVE | node graph workspace |
| `12_SPRINT_10_STATE_GRAPH_WORKSPACE_PROMPT.md` | SUPERSEDED | removed/replaced by node graph prompt |
| `13_SPRINT_11_ADVANCED_POST_MVP_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | hidden/spec/headless until L3/L4 |
| `14_NEXT_CHAT_STARTER_TEMPLATE.md` | ACTIVE | zaktualizowany do v2.2 |

## Co uważać za superseded mentalnie

| Stare założenie | Zastąpione przez |
|---|---|
| StateGraph jako miejsce event/action registry | `core/events` jako headless layer, `nodeGraph` później |
| `state_graph` / `stateGraph` / `StateGraph` | `node_graph` / `nodeGraph` / `NodeGraph` |
| Sprint 10 state graph workspace | Sprint 10 node graph workspace |
| Sprint 02 bez command history/selectors/tests | Sprint 02 v2.2 |
| SVG jako jedyny model renderowania | render adapter + SVG renderer |
| Docking jako szybki UI feature | DockLayout po stabilnym creator core |
| Component library z linked instances od razu | deep copy instances MVP, linking later |
| Debug labels jako default readability tool | debug overlay modes, normal mode clean |
| External library as model owner | external library as adapter |

## Reguła dla AI/Codex

Przed implementacją czytaj minimalnie:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
2. `00_CURRENT_SOURCE_OF_TRUTH.md`
3. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
4. current sprint file
5. jeżeli sprint dotyczy biblioteki zewnętrznej: `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md`
6. jeżeli sprint dotyczy zmian w starym zakresie: `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`

## Reguła archiwizacji

Nie kasować starych plików, jeśli zawierają wartościowy kontekst. Zamiast tego:

- oznaczyć status,
- dodać canonical replacement,
- przenieść do archive folder później,
- albo dopisać notkę `ACTIVE_WITH_V2_2_AMENDMENT`.

Stary plik `12_SPRINT_10_STATE_GRAPH_WORKSPACE_PROMPT.md` został usunięty, ponieważ sama nazwa pliku utrwalała superseded naming.

## Pliki do potencjalnej przyszłej reorganizacji folderów

Docelowo można przenieść:

- `00_CURRENT_SOURCE_OF_TRUTH.md`, `15`, `16`, `22`, `23`, `24`, `29` -> `00_CANONICAL/`
- `01`, `02`-`14`, `18`, `20`, `25` -> `01_PROMPT_CHAIN/`
- `17`, `21`, `26`, `27`, `28` -> `02_POLICIES/`
- starsze eksperymenty -> `04_ARCHIVE/`

Na razie pozostają w jednym folderze, żeby nie łamać istniejących linków i workflow.