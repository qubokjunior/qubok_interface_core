# AGENTS.md — qubok_interface_core

status: active
version: v2.1
doc_type: codex_root_instruction
last_updated: 2026-06-10

## Primary instruction

Before working on this repository, read this navigation file first:

`interface_core_documentation_00/development_prompt_series_00/25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`

Then choose exactly one route from that file. Do not read every documentation file.

## Default route

If the user does not specify another task, use Route E:

`core/model/commands/history/selectors`

Read only the files listed for Route E in the navigation map.

## Required operational docs

For most implementation routes, also respect:

- `26_CODEX_REPAIR_PLAYBOOK.md` when build/test/typecheck fails.
- `27_PROJECT_COMMANDS_AND_TESTS.md` before guessing build/test commands.
- `28_CODEX_PATCH_POLICY.md` before editing files.
- `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` before claiming a roadmap stage is complete.

## Core invariants

1. Project JSON / Project model is source of truth.
2. `src/core` must not import React or `creator`.
3. `creator` may import `core`, not reverse.
4. Persistent mutations go through command layer.
5. Commands must stay serializable.
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

## Work rules

- Implement only the selected route/sprint.
- Do not expand scope.
- Do not start with full state graph UI, docking shell, Tauri, procedural icons, bitmap/GPU graph or linked component instances unless the selected route explicitly allows it.
- Prefer small safe patches.
- Use `27_PROJECT_COMMANDS_AND_TESTS.md` to discover package location and commands before running build/test.
- Run build/tests when available.
- If build fails, use `26_CODEX_REPAIR_PLAYBOOK.md` and repair the current sprint only.
- Use `29_ROADMAP_SUCCESS_ACCEPTANCE_MAP.md` before reporting a roadmap stage as done.

## Required response format

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
