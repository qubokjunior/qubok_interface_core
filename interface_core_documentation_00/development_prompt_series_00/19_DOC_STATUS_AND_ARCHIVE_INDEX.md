# 19 — Documentation status and archive index

status: active
version: v2.2
doc_type: documentation_index
last_updated: 2026-06-24

## Cel

Ten dokument określa status plików dokumentacji, aby AI/Codex i człowiek nie traktowali wszystkich plików jako równorzędnych. Nie usuwa historii projektu. Rozdziela dokumenty aktywne, aktywne z poprawką v2.2, pomocnicze i archiwalne.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare graph naming jest superseded.

## Statusy

| Status | Znaczenie |
|---|---|
| ACTIVE | aktualne źródło prawdy lub aktywny plik operacyjny |
| ACTIVE_WITH_V2_2_AMENDMENT | plik nadal użyteczny, ale musi być czytany przez korektę v2.2 / 2026-06-24 amendment |
| POLICY | aktywna reguła / polityka techniczna |
| PROMPT_ONLY | prompt wykonawczy, nie pełna architektura |
| REFERENCE | materiał pomocniczy / kontekst |
| SUPERSEDED | zastąpione przez nowszy dokument |
| ARCHIVE | historia, nie używać bezpośrednio do implementacji |

## Aktualne pliki canonical / active

| Plik | Status | Priorytet | Uwagi |
|---|---|---:|---|
| `00_CURRENT_SOURCE_OF_TRUTH.md` | ACTIVE | 1 | główny entry point v2.2 + 2026-06-24 amendment |
| `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md` | ACTIVE | 2 | protokół promptów v2.2 |
| `15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md` | ACTIVE_WITH_V2_2_AMENDMENT | 2 | uzasadnienie zmian v2.1/v2.2; czytać przez 00 i 30 |
| `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md` | ACTIVE_WITH_V2_2_AMENDMENT | 2 | aktualna kolejność techniczna z nazewnictwem v2.2; runtime/event/action przed node_graph |
| `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md` | POLICY | 2 | biblioteki tylko jako adaptery |
| `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md` | ACTIVE_WITH_V2_2_AMENDMENT | 2 | korekta starszych promptów |
| `20_AI_CONTEXT_MINIMAL.md` | ACTIVE | 1 | minimalny kontekst dla AI, z feature_core update |
| `21_DOCUMENTATION_HEALTH_CHECKLIST.md` | ACTIVE | 3 | checklist utrzymania docs, z Figma/feature_core/node_graph checks |
| `22_FEATURE_DEPENDENCY_MATRIX.md` | ACTIVE_WITH_V2_2_AMENDMENT | 2 | zależności funkcji; czytać przez 30 dla feature_core/database |
| `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md` | ACTIVE_WITH_V2_2_AMENDMENT | 2 | mapa owner/command/view/validation; owner może być też token/feature/runtime/view/debug |
| `24_CANONICAL_GLOSSARY.md` | ACTIVE | 2 | słownik pojęć z feature_core/tokens/runtime rules |
| `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` | ACTIVE | 1 | główna mapa Codex |
| `26_CODEX_REPAIR_PLAYBOOK.md` | ACTIVE | 3 | repair flow |
| `27_PROJECT_COMMANDS_AND_TESTS.md` | ACTIVE | 3 | build/test discovery |
| `28_CODEX_PATCH_POLICY.md` | ACTIVE | 3 | patch policy |
| `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` | ACTIVE_WITH_V2_2_AMENDMENT | 2 | success/acceptance map; czytać przez 00 i 30 dla runtime-rules/database |
| `30_FEATURE_CORE_DATABASE_AND_RUNTIME_RULES.md` | ACTIVE | 1 | canonical reference dla feature_core, database, tokens, panel rules i runtime order |

## Prompt chain files

| Plik | Status | Uwagi |
|---|---|---|
| `02_SPRINT_00_FOUNDATION_AUDIT_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | czytać z dokumentem 18 oraz 30 jeśli dotyczy feature_core/database |
| `03_SPRINT_01_SCAFFOLD_CONCEPT_SHELL_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | czytać z dokumentem 18; Figma/tokens tylko jako mirror/visual layer |
| `04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | core v2.2 naming obowiązuje |
| `05_SPRINT_03_CANVAS_PRIMITIVES_SELECTION_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | musi używać selectors/render adapter |
| `06_SPRINT_04_INSPECTOR_HIERARCHY_REGION_DEBUG_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | schema-driven inspector, selectors |
| `07_SPRINT_05_COMPONENT_PROOF_SAVE_EXPORT_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | sample tests, render model export |
| `08_SPRINT_06_DEFAULT_UI_CLEANUP_COMPONENT_LIBRARY_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | deep copy instances only; compact default hiding jako runtime/layout rule |
| `09_SPRINT_07_LAYOUT_PANEL_BUILDER_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | performance guardrails; panel rules przez 30 |
| `10_SPRINT_08_DOCKING_SHELL_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | DockLayout source of truth; panel resize rules przez 30 |
| `11_SPRINT_09_EVENTS_ACTION_REGISTRY_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | prefer `core/events` mental model; event/action runtime przed node_graph |
| `12_SPRINT_10_NODE_GRAPH_WORKSPACE_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | node graph workspace po events/actions; nie drugi engine |
| `12_SPRINT_10_STATE_GRAPH_WORKSPACE_PROMPT.md` | SUPERSEDED | removed/replaced by node graph prompt |
| `13_SPRINT_11_ADVANCED_POST_MVP_PROMPT.md` | ACTIVE_WITH_V2_2_AMENDMENT | hidden/spec/headless until L3/L4; path/curve graph later |
| `14_NEXT_CHAT_STARTER_TEMPLATE.md` | ACTIVE_WITH_V2_2_AMENDMENT | zaktualizowany do v2.2; minimal context 20 zawiera nowszy 2026-06-24 dodatek |

## Reference / superseded root documentation notes

| Plik / grupa | Status | Canonical replacement / czytać przez |
|---|---|---|
| `interface_core_documentation_00/01_TERMINOLOGY_CANON_NODE_GRAPH.md` | REFERENCE | `24_CANONICAL_GLOSSARY.md` + naming in `00` |
| `interface_core_documentation_00/02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md` | REFERENCE | `16`, `29`, plus `00` and `30` for latest runtime-rules/database amendment |
| `interface_core_documentation_00/13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt` | REFERENCE | `30_FEATURE_CORE_DATABASE_AND_RUNTIME_RULES.md`; tokens/Figma treated as visual mirror |
| `interface_core_documentation_00/14_curve_node_spatial_routing_awareness.md` | REFERENCE / POST-MVP | path/curve subsystem later than core runtime; rename legacy graph language mentally to node_graph |
| older visual prompts / generated infographic notes | ARCHIVE / REFERENCE | useful for visual reference only, not implementation order |

## Co uważać za superseded mentalnie

| Stare założenie | Zastąpione przez |
|---|---|
| Graph workspace jako miejsce event/action registry | `core/events` jako headless layer, `nodeGraph` później |
| stare graph naming | `node_graph` / `nodeGraph` / `NodeGraph` |
| Sprint 10 graph workspace przed event/action runtime | Sprint 09 headless events/actions, potem Sprint 10 node graph workspace |
| Sprint 02 bez command history/selectors/tests | Sprint 02 v2.2 |
| SVG jako jedyny model renderowania | render adapter + SVG renderer |
| Docking jako szybki UI feature | DockLayout po stabilnym creator core |
| Component library z linked instances od razu | deep copy instances MVP, linking later |
| Debug labels jako default readability tool | debug overlay modes, normal mode clean |
| External library as model owner | external library as adapter |
| Figma variables jako source of truth | repo JSON/TypeScript/schema as source, Figma as visual mirror |
| node_graph jako drugi runtime engine | node_graph edits/visualizes existing runtime/event/action/rule logic |
| feature_core jako Project replacement | feature_core as functional configuration/definition database |

## Reguła dla AI/Codex

Przed implementacją czytaj minimalnie:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
2. `00_CURRENT_SOURCE_OF_TRUTH.md`
3. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
4. current sprint file
5. jeżeli sprint dotyczy biblioteki zewnętrznej: `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md`
6. jeżeli sprint dotyczy zmian w starym zakresie: `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`
7. jeżeli sprint dotyczy tokens/database/feature_core/panel rules/runtime rules: `30_FEATURE_CORE_DATABASE_AND_RUNTIME_RULES.md`

## Reguła archiwizacji

Nie kasować starych plików, jeśli zawierają wartościowy kontekst. Zamiast tego:

- oznaczyć status,
- dodać canonical replacement,
- przenieść do archive folder później,
- albo dopisać notkę `ACTIVE_WITH_V2_2_AMENDMENT`.

Stary plik `12_SPRINT_10_STATE_GRAPH_WORKSPACE_PROMPT.md` został usunięty, ponieważ sama nazwa pliku utrwalała superseded naming.

## Pliki do potencjalnej przyszłej reorganizacji folderów

Docelowo można przenieść:

- `00_CURRENT_SOURCE_OF_TRUTH.md`, `15`, `16`, `22`, `23`, `24`, `29`, `30` -> `00_CANONICAL/`
- `01`, `02`-`14`, `18`, `20`, `25` -> `01_PROMPT_CHAIN/`
- `17`, `21`, `26`, `27`, `28` -> `02_POLICIES/`
- starsze eksperymenty -> `04_ARCHIVE/`

Na razie pozostają w jednym folderze, żeby nie łamać istniejących linków i workflow.
