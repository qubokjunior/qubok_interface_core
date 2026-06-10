# 29 — Roadmap success acceptance map

status: active
version: v2.2
doc_type: roadmap_acceptance_map
last_updated: 2026-06-10

## Cel

Ten dokument opisuje, co dokładnie oznacza sukces każdego etapu roadmapy: co zostało zaimplementowane, z czym się wiąże, jakie moduły są dotknięte i jak testować. Ma pomagać po każdym kroku developmentu ocenić, czy można przejść dalej.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## Jak używać

Po zakończeniu sprintu znajdź odpowiedni etap i sprawdź:

1. Expected implementation.
2. Related modules.
3. What it enables.
4. What must not happen.
5. Build/test/manual acceptance.

Jeżeli acceptance nie przechodzi, użyj `26_CODEX_REPAIR_PLAYBOOK.md` i nie przechodź do następnego etapu.

## Roadmap success map

| Stage | Expected implementation | Related modules | Enables | Test / acceptance |
|---|---|---|---|---|
| Foundation | documentation status, source of truth, roadmap, scope gates, maturity levels | docs | safe planning, token control | docs have active status, current source of truth exists, route is known |
| Shell | app frame, top bar, side zones, canvas area, bottom/status zones, base tokens | app, creator/layout, styles | predictable UI zones | app opens, default screen is clean, debug not dominant |
| Core model | Project, InterfaceObject, bbox, region, layout, selection, viewport types | src/core/model | all future features | Project can be created headlessly, data is JSON-safe |
| Command layer | serializable commands, applyCommand, object/selection/transform commands | src/core/commands | safe mutation, inspector, events, graph | object.create/patch/select works only through commands |
| Command history | command log, undo/redo contract, history_domain | src/core/commands, src/core/model | undo/redo, repair/debug, safe drag/edit | object edit logs history; viewport history is separate or explicitly excluded |
| Selectors | shared read functions for active/selected/root/children/render | src/core/selectors | canvas/inspector/hierarchy sync | selectors return consistent selected/active/root data |
| Validation + tests | validateProject/hierarchy/regions/commands and headless tests | src/core/validation, tests/core | safe save/export/component graph | broken parent/region IDs produce useful errors; tests/build pass |
| Render adapter | neutral render descriptors from Project/selectors | src/core/render | SVG renderer, export, future renderers | Project maps to render model without React import |
| Canvas renderer | SVG/HTML view from render model, grid, viewport | src/creator/canvas, src/core/render | primitive tools and selection | objects render from Project; pan/zoom does not alter object transform |
| Primitive creation | rectangle, text, line, region, empty through commands | src/creator/tools, src/core/commands | component proof, panel builder | create tool adds valid InterfaceObject and validates |
| Selection / transform | click/shift/box select, move/resize/delete through commands | canvas, commands, selectors | inspector, hierarchy, layout tools | canvas/inspector/hierarchy show same selected object; transform is undoable or logged |
| Inspector / hierarchy sync | schema-driven inspector, hierarchy tree, status sync | creator/panels, core/selectors | object editing, group/component workflows | inspector patch dispatches command; hierarchy reflects Project links |
| BBox / region debug | visual/layout/interaction overlays and region inspector/debug modes | core/render, core/validation, creator/debug | hit-test, events, layout validation | overlays are distinguishable and hidden/subtle in normal mode |
| Performance baseline | object count tiers, pointer throttling/local drag session, first hit-test strategy | canvas, selectors, geometry | larger projects, node graph/workspaces | 100 objects smooth; debug does not render all labels by default |
| Component proof | Button_Group and Panel_Monitor as Project data | samples, core/model, validation | component library, export proof | Panel_Monitor validates and is not hardcoded JSX |
| Save / export | Project JSON, Component JSON, SVG, debug report | core/export, samples | persistence, sharing, regression | export/import roundtrip does not break IDs/parents/regions |
| Default UI cleanup | compact concept-aligned interface creator, debug hidden, readable panels | creator/layout, styles | usable MVP | default UI is not debug dashboard; main actions visible and compact |
| Component library | save group/panel as component, insert with fresh IDs | core/library, creator/panels | reusable UI assets | component insert creates valid unique IDs and passes validation |
| Layout / panel builder | align, distribute, snap, box arranger, panel creator | core/layout, canvas/tools | structured UI creation | layout commands affect selected objects, not app docking |
| Docking shell | split/merge/resize app panels, content registry, DockLayout | core/docking, creator/shell | flexible workspace | docking changes do not alter InterfaceObject transforms |
| Events / actions | headless event registry, action registry, target resolver, assignments | core/events | node graph, interaction logic | event assignment can output command without visual node graph |
| Node graph workspace | node graph model, node graph viewport, nodes/edges, command output | core/nodeGraph, creator/workspaces | visual logic authoring | node graph pan/zoom/selection works; node graph does not patch DOM/canvas directly |
| Advanced post-MVP | gated features: procedural icons, reactions, bridges, bitmap/GPU, Tauri | advanced modules | future expansion | each feature has maturity, owner, adapter policy and hidden/default decision |

## Detailed acceptance by stage

### Foundation

Implemented:
- source of truth document,
- roadmap,
- doc status/archive policy,
- prompt protocol,
- Codex navigation map.

Test:
- Codex can identify current route without reading all docs.
- Each active doc has status/version/type.

### Core model / commands / history / selectors

Implemented:
- Project model,
- InterfaceObject model,
- commands,
- applyCommand,
- command history or explicit contract,
- selectors,
- validation skeleton,
- headless tests.

Test:
- create Project,
- object.create,
- object.patch,
- selection.set,
- validateProject,
- undo/redo log or contract,
- no React imports in core.

### Canvas / primitives / selection

Implemented:
- render adapter to SVG/HTML view,
- viewport pan/zoom,
- primitive creation,
- selection and transform tools.

Test:
- add rectangle/text/region,
- select object,
- transform object,
- inspector/hierarchy later can read same selection,
- viewport pan/zoom does not mutate object transform.

### Inspector / hierarchy / region debug

Implemented:
- inspector reads selected object via selectors,
- hierarchy reads Project tree,
- region/bbox debug overlays.

Test:
- canvas selection updates inspector and hierarchy,
- inspector changes dispatch command,
- broken region linked_visual_id is visible in validation,
- normal mode is not overloaded by debug.

### Component proof / export

Implemented:
- Panel_Monitor sample as Project data,
- component candidate validation,
- JSON/SVG export.

Test:
- Panel_Monitor validates,
- export/import does not duplicate IDs or lose children,
- SVG visual export does not unintentionally include debug internals.

### Docking shell

Implemented:
- DockLayout model,
- split/merge/resize app panels,
- content registry.

Test:
- resize app panels,
- swap content if supported,
- reset layout,
- verify InterfaceObject positions unchanged.

### Events / node graph

Implemented:
- events/actions first,
- node graph model/workspace later,
- node graph outputs command payloads.

Test:
- event assignment can call command-backed action,
- node graph can be serialized,
- node graph viewport is separate from canvas viewport,
- visual node graph is not required for event registry to exist.

## Done report template

After each roadmap stage, report:

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
- command: ...
- result: ...
- manual checks: ...

Not implemented intentionally
- ...

Risks / pending
- ...
```

## Stop rule

If a stage fails its acceptance, stop. Use `26_CODEX_REPAIR_PLAYBOOK.md`. Do not proceed to the next stage in the same task.