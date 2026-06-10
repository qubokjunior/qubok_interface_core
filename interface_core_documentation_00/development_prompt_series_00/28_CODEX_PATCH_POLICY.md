# 28 — Codex patch policy

status: active
version: v2.2
doc_type: codex_patch_policy
last_updated: 2026-06-10

## Cel

Ten dokument określa, jak Codex powinien zmieniać pliki w repozytorium, aby development był przewidywalny, mały i łatwy do review.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## Główna zasada

Każdy patch ma realizować jeden sprint, jedną warstwę albo jeden repair. Nie mieszać kilku właścicieli danych w jednym patchu.

## Patch size policy

| Patch type | Allowed scope | Forbidden |
|---|---|---|
| Sprint patch | files listed in current sprint | unrelated refactor |
| Repair patch | smallest files needed to fix failure | next sprint work |
| Docs patch | affected docs + indexes | code changes unless requested |
| Adapter patch | adapter boundary only | making external library source of truth |
| Test patch | tests for current owner | unrelated snapshot rewrites |

## Before editing

Codex should identify:

```text
Task route:
Current sprint:
Data owner:
Allowed files:
Forbidden files:
Acceptance commands:
Manual smoke test:
```

If these are unknown, read:

- `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
- `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
- current sprint file
- `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md`

## During editing

Do:

- prefer small complete files or focused patches,
- preserve module boundaries,
- add/update tests when the sprint touches core behavior,
- update exports/imports deliberately,
- keep commands serializable,
- keep Project JSON exportable,
- keep React out of `src/core`,
- keep external libraries behind adapters,
- keep debug hidden unless current sprint is debug-specific,
- keep node_graph naming consistent.

Do not:

- reformat entire unrelated files,
- rename broad concepts without updating glossary/docs,
- introduce old `state_graph` naming,
- use temporary project names ending with `_00`,
- remove validation to silence errors,
- use broad `any` to bypass model errors,
- add new dependencies without adapter policy check,
- implement visual node graph before event/action registry,
- implement docking by mutating InterfaceObject transforms,
- commit local UI state as source of truth.

## File ownership guardrails

| Path / module | Owner | Rule |
|---|---|---|
| `src/core/model/**` | data model | no React, serializable data |
| `src/core/commands/**` | mutations | pure/applyCommand, serializable commands |
| `src/core/selectors/**` | reads | no mutation, reusable by UI/workspaces |
| `src/core/validation/**` | consistency | detect, do not hide broken links |
| `src/core/render/**` | render adapter | no React dependency if possible |
| `src/core/events/**` | headless events/actions | no visual node graph dependency |
| `src/core/nodeGraph/**` | node graph model/runtime | consumes events/actions, outputs commands |
| `src/core/docking/**` | app shell layout model | not canvas object layout |
| `src/creator/**` | React UI/workspaces | dispatch commands, read selectors |
| `tests/**` | regression safety | cover current sprint behavior |
| `interface_core_documentation_00/**` | docs | update indexes when navigation changes |

## Commit/report discipline

Every response should report:

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

For repair tasks, use `26_CODEX_REPAIR_PLAYBOOK.md` response format.

## Refactor policy

Refactor is allowed only when:

- it is required by the current sprint acceptance,
- it reduces duplication inside the same owner boundary,
- it does not change behavior unexpectedly,
- tests/build are run after it,
- response explains why refactor was necessary.

Avoid broad cleanup unless the sprint is explicitly a cleanup sprint.

## Dependency policy

Before adding any dependency:

1. Read `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md`.
2. State why dependency is needed.
3. State whether it is adapter/helper/runtime.
4. Confirm it will not own canonical Project data.
5. Add only if current sprint allows it.

## Documentation update policy

If a patch changes architecture, roadmap order, data owner, command flow, naming, local path assumptions or file navigation, update the relevant docs:

- `00_CURRENT_SOURCE_OF_TRUTH.md`
- `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
- `19_DOC_STATUS_AND_ARCHIVE_INDEX.md`
- `22_FEATURE_DEPENDENCY_MATRIX.md`
- `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md`
- `24_CANONICAL_GLOSSARY.md`
- `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md`

Do not update all docs by default. Update only affected docs.