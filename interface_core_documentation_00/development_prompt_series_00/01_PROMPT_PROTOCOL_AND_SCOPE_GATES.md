# 01 — Prompt protocol and scope gates

status: active
version: v2.2
doc_type: prompt_protocol
last_updated: 2026-06-10

Używaj tego dokumentu jako stałej warstwy kontrolnej dla każdego kolejnego promptu developmentowego.

## Naming rules v2.2

- Project name: `qubok_interface_core`.
- Short name: `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Use `node_graph`, `nodeGraph`, `NodeGraph` and “node graph”.
- Do not introduce old `state_graph` naming or temporary project names ending with `_00`.

## Minimalny format każdego promptu

```text
SPRINT: [numer] — [jeden cel]

ASSUME PREVIOUS SPRINT SUCCESSFUL
- npm run build passed.
- Manual acceptance from previous sprint passed.
- No unresolved TypeScript errors.
- No known broken source-of-truth sync.
- If this sprint is after Sprint 02 v2.2, command history, selectors, validation and render adapter types exist or are explicitly marked pending.

GLOBAL RULES
- Project JSON / Project model is source of truth.
- Core has no React imports.
- Creator imports core, not reverse.
- Persistent mutations go through command layer.
- Command payloads stay serializable.
- Command history / undo contract exists before heavy canvas editing.
- Core selectors are the shared read boundary for canvas, inspector, hierarchy, status and target resolver.
- visual_bbox, layout_bbox and interaction_region remain separate.
- Rectangle renders; Region interacts.
- Render MVP can be SVG/HTML, but Project is not the renderer model.
- Event/action registry is headless and separate from visual node graph.
- External libraries are adapters, not source of truth.
- Default UI exposes only L3/L4 features.

CURRENT PHASE
- Phase: [roadmap phase]
- Maturity target: [L1/L2/L3/L4]
- Data owner: [Project / InterfaceObject / workspace UI state / DockLayout / EventRegistry / NodeGraph]

IMPLEMENT ONLY
1. ...
2. ...
3. ...

DO NOT IMPLEMENT
- ...
- ...

FILES / MODULES
Allowed:
- ...
Forbidden:
- ...

ACCEPTANCE
- npm run build passes.
- [manual test]
- [sync test]
- [validation test]
- [history/selector/render adapter test if relevant]
- Response lists changed files, what was done, and how to test.
```

## Scope gates

| Gate | Must be true before moving on |
|---|---|
| Documentation gate | current source of truth and active sprint are known |
| Core gate | model, commands, command history, selectors and validation build without React imports |
| Test gate | headless tests exist or missing test command is explicitly documented |
| Render gate | Project model is mapped to render model; renderer is not source of truth |
| View gate | canvas, inspector, hierarchy and status read the same selection through selectors |
| Region gate | visual/layout/interaction overlays are distinguishable |
| Component gate | button_group and Panel_Monitor are Project data, not JSX mockups |
| Export gate | Project JSON / Component JSON / SVG survive validation |
| UI gate | default screen is clean, debug not dominant |
| Library gate | component save and instantiate generate valid IDs |
| Layout gate | canvas object layout is separate from app shell docking |
| Docking gate | app shell layout is separate from canvas object layout and graph viewport layout |
| Logic gate | events/actions output commands only and do not require visual node graph |
| Graph gate | node graph viewport has pan/zoom/multiselect before full node graph complexity |
| External adapter gate | external library is mapped through adapter and does not own canonical data |
| Advanced gate | advanced feature is hidden/spec/headless until it reaches L3/L4 |

## Token optimization rules

1. Do not paste full documentation into every prompt.
2. For implementation, paste only:
   - `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`,
   - `00_CURRENT_SOURCE_OF_TRUTH.md`,
   - this protocol,
   - current sprint file.
3. For architecture diagnosis, additionally include relevant policy or roadmap file.
4. When a sprint fails, next prompt should be a repair prompt for that sprint only.
5. Never ask Codex to implement node graph, docking, panel builder and component library in one pass.
6. Each prompt should mention explicitly what not to touch.
7. Prefer complete files or safe patches over scattered fragments.
8. Require build and manual acceptance at the end of every sprint.
9. If a document conflicts with v2.2, prefer `00_CURRENT_SOURCE_OF_TRUTH.md` and `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`.

## Anti-patterns

| Anti-pattern | Result | Correction |
|---|---|---|
| “Add all UI features” | huge noisy patch | one feature owner per sprint |
| “Make it like Blender” | vague docking/layout confusion | specify app shell vs canvas layout |
| “Add node graph” | premature visual graph chaos | headless event/action registry first |
| “Improve UI” | visual polish without architecture | target shell/spacing/tokens only |
| “Fix everything” | unstable patch | diagnose one failing gate |
| “Use React Flow as the graph model” | external state becomes source of truth | use React Flow only as adapter |
| “Use docking library as app state” | lost Project/workspace boundary | map library events to DockLayout commands |
| “Patch object state in component local state” | canvas/inspector drift | dispatch command and read via selectors |

## Mandatory final response format from Codex/chat

```text
Co zrobiono
- ...

Zmienione pliki
- ...

Jak testować
1. ...
2. ...

Kryteria done
- ...

Czego celowo nie ruszałem
- ...

Ryzyka / pending
- ...
```

## Conflict resolution priority

1. Project/core source of truth.
2. Command layer and command history.
3. Validation and tests.
4. Selectors.
5. BBox/region separation.
6. Render adapter.
7. UI/workspace implementation.
8. External adapters.
9. Advanced features.