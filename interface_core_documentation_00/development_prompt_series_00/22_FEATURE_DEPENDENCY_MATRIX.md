# 22 — Feature dependency matrix

status: active
version: v2.2
doc_type: canonical_matrix
last_updated: 2026-06-10

## Cel

Mapa zależności funkcji. Używać przed planowaniem sprintu, aby nie implementować funkcji zanim istnieją jej wymagane warstwy.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## Matrix

| Feature | Requires | Blocks / Enables | Data owner | Maturity target |
|---|---|---|---|---|
| Project model | none | all features | Project | L4 |
| InterfaceObject | Project model | canvas, inspector, hierarchy, export | Project.objects_by_id | L4 |
| Command layer | Project model | undo, inspector edits, graph actions, events | core/commands | L4 |
| Command history | command layer | undo/redo, command log, safe drag/edit | Project.history / commandHistory | L3/L4 |
| Selectors | Project model | canvas, inspector, hierarchy, status, target resolver | core/selectors | L4 |
| Validation | Project model, selectors | save/export, component library, graph safety | core/validation | L4 |
| Render adapter | Project model, selectors | SVG canvas, SVG export, future renderer | core/render | L3/L4 |
| Canvas renderer | render adapter, Project model | primitive tools, selection, transform | creator/canvas | L3/L4 |
| Viewport | canvas renderer | pan/zoom, graph/canvas separation | Project.viewport or workspace viewport | L3 |
| Primitive creation | command layer, canvas | component proof, panel builder | InterfaceObject | L3/L4 |
| Selection | selectors, commands, canvas | inspector, hierarchy, transform | Project.selection | L4 |
| Transform | selection, command layer | layout, snap, component editing | InterfaceObject.transform | L3/L4 |
| Inspector | selectors, command layer, validation | object editing, region edit, component params | creator/panels + core schema | L4 |
| Hierarchy | selectors, command layer | group edit, component proof | Project hierarchy | L4 |
| Status bar | selectors, validation | debug feedback | creator/layout | L3 |
| Region debug | region data, render adapter, debug mode | events, target resolver, hit-test | InterfaceObject.region_data | L3 |
| BBox debug | bbox model, render adapter | layout, snap, validation | InterfaceObject.bbox | L3 |
| Panel_Monitor sample | primitives, groups, regions | component proof, default UI, tests | samples/Project data | L4 |
| Group commands | command layer, hierarchy validation | component asset, panel builder | group_data | L3/L4 |
| Project JSON save/load | Project model, validation | persistence, regression tests | Project | L3/L4 |
| Component JSON export | group commands, validation | component library | component asset | L3 |
| SVG export | render adapter, validation | visual export proof | export module | L3 |
| Component library | component JSON, group commands | layout reuse, app building | Project.library_assets | L3 |
| Layout tools | bbox, selection, command layer | panel builder, snap, align | layout_data / commands | L3 |
| Snap tools | bbox, layout, canvas drag | precise editing | geometry/layout | L3 |
| Panel builder | group, layout, region | reusable panels, component library | group_data / region_data | L3 |
| Docking shell | app shell layout, content registry | workspace presets, split/merge panels | DockLayout | L3/L4 |
| Event registry | command layer, target resolver | node graph, interaction logic | core/events | L2/L3 |
| Action registry | command layer | event assignments, node graph | core/events | L2/L3 |
| Target resolver | selectors, regions, selection | event/action registry, graph outputs | core/events | L2/L3 |
| Node graph model | events/actions, commands | node graph workspace | Project.node_graphs | L2/L3 |
| Node graph workspace | node graph model, viewport, adapter policy | visual logic editing | creator/workspaces | L3/L4 later |
| React Flow adapter | node graph model, adapter policy | faster node graph UI | graph UI adapter | L2/L3 |
| External docking adapter | DockLayout, adapter policy | complex dock UI | docking UI adapter | L2/L3 |
| Tauri wrapper | save/load adapters, stable app shell | desktop packaging | desktop shell | post-MVP |
| Procedural icons | primitives, export, component asset | icon system | advanced assets | L1/L2 later |
| Bitmap/GPU graph | render abstraction, advanced pipeline | experiments | advanced runtime | L0/L1 |

## Najważniejsze zależności krytyczne

```text
Project model
  -> commands
  -> command history
  -> selectors
  -> validation
  -> render adapter
  -> canvas / inspector / hierarchy
```

```text
commands + selectors + regions
  -> events/actions
  -> node graph
```

```text
bbox + layout + region
  -> panel builder
  -> component library
```

```text
DockLayout
  -> docking shell
```

DockLayout nie zależy od canvas object layout i nie powinien go modyfikować.

## Funkcje, których nie wolno zaczynać zbyt wcześnie

| Feature | Nie zaczynać przed |
|---|---|
| Full node graph UI | events/actions + node graph model + node graph viewport |
| React Flow adapter | Project.node_graphs model |
| Full docking library | DockLayout model |
| Linked component instances | component library MVP + validation |
| Procedural icon engine | primitive/export/component basics |
| Tauri file integration | stable save/load web adapter |
| Bitmap/GPU experiments | render adapter + MVP editor stable |

## Jak używać matrixu

Przed każdym sprintem odpowiedz:

1. Jaka funkcja jest celem?
2. Jakie `Requires` są już spełnione?
3. Kto jest data owner?
4. Czy zmiana przechodzi przez command layer?
5. Czy istnieje walidacja?
6. Czy feature jest L3/L4 i może być widoczny defaultowo?
7. Czy feature powinien być hidden/debug/workspace-only?