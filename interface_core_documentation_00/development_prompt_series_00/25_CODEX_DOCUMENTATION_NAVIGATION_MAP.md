# 25 — Codex documentation navigation map

status: active
version: v2.1
doc_type: codex_navigation_map
last_updated: 2026-06-10

## Cel

Ten plik jest mapą poruszania się po dokumentacji `qubok_interface_core` dla Codex / AI. Ma ograniczyć zużycie tokenów i zapobiec sytuacji, w której Codex czyta wszystkie dokumenty, zamiast dobrać minimalny zestaw właściwy dla danego zadania.

Jeżeli masz wskazać Codexowi jeden plik nawigacyjny, wskaż ten plik.

## Najkrótsza instrukcja dla Codex

```text
Read first:
1. interface_core_documentation_00/development_prompt_series_00/25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md

Then choose exactly one task route from this file.
Do not read every documentation file.
Read only files listed under the selected route.
If files conflict, use conflict priority from this map.
Implement only the selected sprint/task.
```

## Repo and documentation root

| Item | Value |
|---|---|
| Repository | `qubokjunior/qubok_interface_core` |
| Documentation root | `interface_core_documentation_00/development_prompt_series_00/` |
| Current documentation version | v2.1 |
| Main source of truth | `00_CURRENT_SOURCE_OF_TRUTH.md` |
| Current near-term development target | `04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md` |
| Roadmap acceptance map | `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` |

## Global conflict priority

If documents conflict, use this order:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
2. `00_CURRENT_SOURCE_OF_TRUTH.md`
3. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
4. `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md`
5. `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md`
6. `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`
7. task-specific policy/matrix/glossary document
8. current sprint prompt
9. older sprint prompts
10. legacy notes / archive/reference material

## Core invariants to keep in memory

These rules apply to every route:

1. Project JSON / Project model is source of truth.
2. `src/core` must not import React or `creator`.
3. `creator` may import `core`, not reverse.
4. Persistent mutations go through command layer.
5. Command payloads stay serializable.
6. Command history / undo contract comes before heavy canvas editing.
7. Core selectors are the shared read boundary for canvas, inspector, hierarchy, status and target resolver.
8. `visual_bbox`, `layout_bbox` and `interaction_region` are separate.
9. Rectangle renders; Region interacts.
10. Render adapter separates Project from SVG/HTML renderer.
11. Event/action registry is headless and separate from visual state graph.
12. External libraries are adapters, not source of truth.
13. Default UI exposes only L3/L4 features.
14. Debug, graph, docking and experimental systems must not dominate the default screen.
15. Panel_Monitor sample must be Project data, not JSX mockup.

## Route selector

Choose one route only.

| Task type | Route | Read these files |
|---|---|---|
| Start or continue implementation sprint | Route A | 25, 00, 01, current sprint file, 27, 28 |
| Repair failed sprint/build | Route B | 25, 00, 01, 26, 27, current sprint file, failing log/code context only |
| Diagnose architecture/roadmap | Route C | 25, 00, 15, 16, 18, 22, 23, 24, 29 |
| Add or evaluate external library | Route D | 25, 00, 01, 17, 28, related sprint file |
| Work on core/model/commands/history/selectors | Route E | 25, 00, 01, 04, 22, 23, 24, 27, 28, 29 |
| Work on canvas/primitives/selection/transform | Route F | 25, 00, 01, 05, 22, 23, 24, 27, 28, 29 |
| Work on inspector/hierarchy/region debug | Route G | 25, 00, 01, 06, 22, 23, 24, 27, 28, 29 |
| Work on component proof/save/export | Route H | 25, 00, 01, 07, 22, 23, 24, 27, 28, 29 |
| Work on default UI/component library | Route I | 25, 00, 01, 08, 22, 23, 24, 27, 28, 29 |
| Work on layout/snap/panel builder | Route J | 25, 00, 01, 09, 22, 23, 24, 27, 28, 29 |
| Work on docking shell | Route K | 25, 00, 01, 10, 17, 22, 23, 24, 27, 28, 29 |
| Work on events/actions/target resolver | Route L | 25, 00, 01, 11, 18, 22, 23, 24, 27, 28, 29 |
| Work on state graph workspace | Route M | 25, 00, 01, 12, 17, 18, 22, 23, 24, 27, 28, 29 |
| Work on advanced/post-MVP feature | Route N | 25, 00, 01, 13, 17, 22, 23, 24, 27, 28, 29 |
| Update documentation itself | Route O | 25, 00, 19, 20, 21, 28, affected docs |
| Clarify terminology/naming | Route P | 25, 00, 23, 24 |
| Verify roadmap stage completion | Route Q | 25, 00, 16, 27, 29, current sprint file |

## File index by number

| Index | File | Role | Read when |
|---:|---|---|---|
| 00 | `00_CURRENT_SOURCE_OF_TRUTH.md` | canonical current source | always after this map |
| 01 | `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md` | prompt format, gates, response format | every implementation route |
| 02 | `02_SPRINT_00_FOUNDATION_AUDIT_PROMPT.md` | foundation audit | first project audit only |
| 03 | `03_SPRINT_01_SCAFFOLD_CONCEPT_SHELL_PROMPT.md` | shell/tokens | scaffold or UI shell sprint |
| 04 | `04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md` | core/model/commands/history/selectors/tests | current near-term priority |
| 05 | `05_SPRINT_03_CANVAS_PRIMITIVES_SELECTION_PROMPT.md` | canvas/primitives/selection/transform | after Sprint 02 v2.1 done |
| 06 | `06_SPRINT_04_INSPECTOR_HIERARCHY_REGION_DEBUG_PROMPT.md` | inspector/hierarchy/region debug | after canvas/selection stable |
| 07 | `07_SPRINT_05_COMPONENT_PROOF_SAVE_EXPORT_PROMPT.md` | Panel_Monitor/save/export | component proof/export sprint |
| 08 | `08_SPRINT_06_DEFAULT_UI_CLEANUP_COMPONENT_LIBRARY_PROMPT.md` | UI cleanup/component library | after component proof |
| 09 | `09_SPRINT_07_LAYOUT_PANEL_BUILDER_PROMPT.md` | layout/snap/panel builder | after component library base |
| 10 | `10_SPRINT_08_DOCKING_SHELL_PROMPT.md` | app shell docking | after core creator workflow stable |
| 11 | `11_SPRINT_09_EVENTS_ACTION_REGISTRY_PROMPT.md` | events/actions/target resolver | before state graph |
| 12 | `12_SPRINT_10_STATE_GRAPH_WORKSPACE_PROMPT.md` | state graph workspace | after events/actions |
| 13 | `13_SPRINT_11_ADVANCED_POST_MVP_PROMPT.md` | advanced features | post-MVP only |
| 14 | `14_NEXT_CHAT_STARTER_TEMPLATE.md` | new chat starter | when starting a fresh chat manually |
| 15 | `15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md` | architecture rationale | architecture diagnosis |
| 16 | `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md` | revised technical roadmap | planning/roadmap decisions |
| 17 | `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md` | library adapter policy | any external library decision |
| 18 | `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md` | v2.1 corrections for older prompts | when using old sprint prompts |
| 19 | `19_DOC_STATUS_AND_ARCHIVE_INDEX.md` | doc status/archive policy | docs maintenance |
| 20 | `20_AI_CONTEXT_MINIMAL.md` | ultra-short AI context | when token budget is very low |
| 21 | `21_DOCUMENTATION_HEALTH_CHECKLIST.md` | docs quality checklist | docs update/review |
| 22 | `22_FEATURE_DEPENDENCY_MATRIX.md` | feature dependencies | planning any feature |
| 23 | `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md` | owner/command/view/validation map | writing implementation plan |
| 24 | `24_CANONICAL_GLOSSARY.md` | terminology | naming/feedback/clarification |
| 25 | `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` | navigation map | first file for Codex |
| 26 | `26_CODEX_REPAIR_PLAYBOOK.md` | repair flow | when build/test/typecheck fails |
| 27 | `27_PROJECT_COMMANDS_AND_TESTS.md` | build/test command discovery and smoke tests | every implementation/repair route |
| 28 | `28_CODEX_PATCH_POLICY.md` | safe patch rules | every implementation/repair route |
| 29 | `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` | per-roadmap-stage success/acceptance | after each stage or before moving on |

## Roadmap cross-reference

| Roadmap stage | Main doc | Supporting docs | Do not skip gate |
|---|---|---|---|
| Foundation | 02 | 00, 01, 16, 19, 21, 29 | Documentation gate |
| Shell | 03 | 00, 01, 20, 24, 27, 28, 29 | UI gate |
| Core model / commands / history | 04 | 00, 01, 16, 22, 23, 24, 27, 28, 29 | Core gate, Test gate |
| Canvas / primitives / selection | 05 | 00, 01, 22, 23, 24, 27, 28, 29 | Render gate, View gate |
| Inspector / hierarchy / region debug | 06 | 00, 01, 22, 23, 24, 27, 28, 29 | Sync gate, Region gate |
| Component proof / save / export | 07 | 00, 01, 22, 23, 24, 27, 28, 29 | Component gate, Export gate |
| Default UI / component library | 08 | 00, 01, 22, 23, 24, 27, 28, 29 | UI gate, Library gate |
| Layout / snap / panel builder | 09 | 00, 01, 22, 23, 24, 27, 28, 29 | Layout gate, Region/performance gate |
| Docking shell | 10 | 00, 01, 17, 22, 23, 24, 27, 28, 29 | Docking gate, External adapter gate |
| Events / actions | 11 | 00, 01, 18, 22, 23, 24, 27, 28, 29 | Logic gate |
| State graph | 12 | 00, 01, 17, 18, 22, 23, 24, 27, 28, 29 | Graph gate, External adapter gate |
| Advanced | 13 | 00, 01, 17, 22, 23, 24, 27, 28, 29 | Advanced gate |

## Navigation by question

| If Codex needs to answer... | Read |
|---|---|
| “What is currently true?” | 25 -> 00 |
| “What should I implement next?” | 25 -> 00 -> 16 -> 04 |
| “How should I write a sprint prompt?” | 25 -> 01 -> current sprint file |
| “What files should I touch?” | 25 -> current sprint file -> 23 -> 28 |
| “How do I build/test?” | 25 -> 27 |
| “How do I repair a failed sprint?” | 25 -> 26 -> 27 |
| “How do I know stage is done?” | 25 -> 29 |
| “Can I add React Flow/FlexLayout/XState/Tauri?” | 25 -> 17 |
| “Is this old prompt still valid?” | 25 -> 18 -> 19 |
| “What does this term mean?” | 25 -> 24 |
| “What depends on this feature?” | 25 -> 22 |
| “Who owns this data?” | 25 -> 23 |
| “How do I update docs safely?” | 25 -> 19 -> 21 -> 28 |

## Token budget modes

### Minimal mode

Use when task is simple or context is expensive:

```text
Read only:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- current sprint file
```

### Normal implementation mode

```text
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
- current sprint file
- 27_PROJECT_COMMANDS_AND_TESTS.md
- 28_CODEX_PATCH_POLICY.md
- 23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md if feature touches Project data
```

### Architecture mode

```text
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md
- 16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md
- 22_FEATURE_DEPENDENCY_MATRIX.md
- 23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md
- 24_CANONICAL_GLOSSARY.md
- 29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md
```

### External library mode

```text
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md
- 28_CODEX_PATCH_POLICY.md
- related sprint file
```

### Repair mode

```text
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
- 26_CODEX_REPAIR_PLAYBOOK.md
- 27_PROJECT_COMMANDS_AND_TESTS.md
- current sprint file
- failing log/code context only
```

### Documentation maintenance mode

```text
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 19_DOC_STATUS_AND_ARCHIVE_INDEX.md
- 21_DOCUMENTATION_HEALTH_CHECKLIST.md
- 28_CODEX_PATCH_POLICY.md
- affected docs only
```

## Current next-step pointer

As of v2.1 documentation state, the recommended next development target is:

```text
Route E — core/model/commands/history/selectors
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
- 04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md
- 22_FEATURE_DEPENDENCY_MATRIX.md
- 23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md
- 24_CANONICAL_GLOSSARY.md
- 27_PROJECT_COMMANDS_AND_TESTS.md
- 28_CODEX_PATCH_POLICY.md
- 29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md
```

Expected implementation area:

```text
src/core/model/**
src/core/commands/**
src/core/selectors/**
src/core/validation/**
src/core/render/**
tests/core/**
```

Do not start with:

```text
state graph UI
docking shell
external graph/docking library
Tauri file system
procedural icons
bitmap/GPU graph
linked component instances
```

## Required Codex response format

For implementation tasks, Codex should answer with:

```text
Co zrobiono
- ...

Zmienione pliki
- ...

Jak testować
1. ...

Kryteria done
- ...

Czego celowo nie ruszałem
- ...

Ryzyka / pending
- ...
```

For completed roadmap stages, use `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` and include:

```text
Roadmap stage completed
- [stage]

Implemented
- ...

Related modules
- ...

Enabled next
- ...

Tests / acceptance
- ...
```

For failed sprint/build/test, use `26_CODEX_REPAIR_PLAYBOOK.md`.

## Docs maintenance rule

When adding, editing, or superseding docs:

1. Update `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` if navigation changes.
2. Update `00_CURRENT_SOURCE_OF_TRUTH.md` if canonical rules change.
3. Update `19_DOC_STATUS_AND_ARCHIVE_INDEX.md` if a file status changes.
4. Update `20_AI_CONTEXT_MINIMAL.md` if the minimal context changes.
5. Update `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` if roadmap acceptance changes.
6. Use `21_DOCUMENTATION_HEALTH_CHECKLIST.md` before committing documentation changes.
7. Use `28_CODEX_PATCH_POLICY.md` to keep docs changes focused.