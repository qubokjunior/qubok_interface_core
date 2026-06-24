# 20 — AI context minimal

status: active
version: v2.2
doc_type: ai_minimal_context
last_updated: 2026-06-24

## Cel

To jest najkrótszy kontekst, jaki powinien wystarczyć AI/Codex do rozpoczęcia pracy nad jednym sprintem bez czytania całej historii dokumentacji.

Jeżeli Codex ma sam zdecydować, które dokumenty czytać, użyj najpierw `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`. Ten plik jest krótkim kontekstem, a plik 25 jest mapą wyboru ścieżki.

## Projekt

`qubok_interface_core` to parametryczny silnik UI dla przyszłych aplikacji QUBOK. Nie jest to UI kit, theme ani pojedynczy app mockup. Każdy element interface jest obiektem danych w Project modelu.

Short name: `interface_core`.

Local PC path:

```text
I:\Art\_AI\app_development\qubok_interface_core
```

## Naming v2.2

- Use `node_graph`, `nodeGraph`, `NodeGraph` and `node graph`.
- Do not add new references using old graph naming.
- Do not use temporary project names ending with `_00`.

## Stack

- TypeScript
- React
- Vite
- SVG/HTML renderer MVP
- JSON project/component export
- Tauri later, not MVP

## Najważniejsze reguły

1. Project JSON / Project model is source of truth.
2. `src/core` has no React imports.
3. `creator` imports `core`, not reverse.
4. Persistent mutations go through command layer.
5. Commands stay serializable.
6. Command history / undo contract comes before heavy canvas editing.
7. Selectors are the read boundary for canvas, inspector, hierarchy, status and target resolver.
8. `visual_bbox`, `layout_bbox` and `interaction_region` are separate.
9. Rectangle renders. Region interacts.
10. Render adapter separates Project from SVG/HTML renderer.
11. Event/action registry is headless and separate from visual node graph.
12. External libraries are adapters only.
13. Default UI exposes only L3/L4.
14. Debug, graph, docking and experiments do not dominate the default screen.
15. Panel_Monitor sample must be Project data, not JSX mockup.

## 2026-06-24 minimal additions

- Current mental model: `design tokens -> feature_core -> runtime shell -> interaction layer -> rule engine -> node_graph -> debug/inspection`.
- Design tokens are visual values. Repo JSON/TypeScript is the development source of truth; Figma variables are a visual mirror, not runtime ownership.
- `feature_core` groups functional definitions into `runtime/*`, `layout/*`, `events/*`, `actions/*`, `nodes/*`, `colors/*`, `debug/*`.
- `feature_core` does not replace Project model. Project stores the concrete document; `feature_core` defines available behavior/configuration.
- Runtime rules come first: dock shell, split/merge/resize, panel readable rules, `compact_when_default`, value field math, command palette, event/action runtime, debug trace and lightweight color logic.
- `node_graph` comes later as visual editing/inspection for existing runtime/event/action/rule logic. It must not become a second runtime engine.

## Aktualna kolejność implementacji

```text
Foundation
-> Shell
-> Core model
-> Commands
-> Command history
-> Selectors
-> Validation + tests
-> Render adapter
-> Canvas
-> Primitives
-> Selection / transform
-> Inspector / hierarchy
-> Regions / debug
-> Performance baseline
-> Component proof
-> Save / export
-> UI cleanup
-> Component library
-> Layout / panel builder
-> Docking shell
-> Events / actions
-> Node graph
-> Advanced
```

## Najbliższy zalecany sprint

`04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md`

Ten sprint powinien zbudować:

- Project model,
- InterfaceObject types,
- command types,
- applyCommand,
- command history,
- selectors,
- validation,
- render adapter types,
- headless tests.

## Czego nie robić teraz

- Full node graph UI.
- Docking jako pierwsza duża funkcja.
- External graph/docking library jako source of truth.
- Tauri file system jako wymóg MVP.
- Procedural icons / bitmap graph / external bridges.
- Linked component instances.
- Debug labels jako default screen.
- Hardcoded JSX sample scene.

## Minimalny prompt do Codex

```text
Read:
1. 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
2. 00_CURRENT_SOURCE_OF_TRUTH.md
3. 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
4. current sprint file

Follow v2.2 rules.
Implement only current sprint.
Do not expand scope.
Run/build/test if available.
Return: what changed, changed files, how to test, done criteria, what was intentionally not touched, risks/pending.
```

## Token-saving prompt to Codex

```text
Use 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md as the first navigation file. Select one route only. Do not read every documentation file. Read only the files listed for the selected route, then implement the current sprint.
```

## Conflict priority

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` for choosing what to read
2. `00_CURRENT_SOURCE_OF_TRUTH.md` for canonical rules
3. `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md`
4. `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`
5. Current sprint file
6. Older notes / archives
