# QUBOK_INTERFACE_CORE — CURRENT SOURCE OF TRUTH

Version: 2026-06-11
Status: canonical documentation entry point
Scope: current planning, Codex prompts, roadmap decisions and terminology normalization.

## 1. Canonical project definition

`qubok_interface_core` is a parametric interface engine for the qubok suit.
It is not a UI kit, not a single application, not a collection of screenshots and not a decorative mockup library.

The engine defines how interface elements are represented as data, edited, validated, debugged, previewed, exported and reused as components.

Core pipeline:

```text
primitive
-> bbox
-> transform / style
-> region / layout
-> group / panel
-> exposed parameters
-> parameter links / object references / rules
-> library asset
-> reusable UI component
-> generated helpers / instance-on-points overlays
-> event / action / node_graph behavior
-> application workspace
```

## 2. Final naming decision

`node_graph` is the final canonical name.

Old names are deprecated:

| Deprecated | Canonical replacement | Meaning |
|---|---|---|
| `state_graph` | `node_graph` | visual graph editor / graph runtime system |
| `state graph` | `node_graph` | same system, old prose form |
| `node graph` | `node_graph` | acceptable in prose, but use `node_graph` in model/docs identifiers |

`state` remains valid only as a domain concept inside the node graph, for example:

- `states_slot`,
- `state_variant`,
- `state_transition`,
- `node_graph_profile = states`,
- `state_change` output.

Rule: write `node_graph` for the system. Write `state` only for visual/interactivity states.

## 3. Source of truth rule

```text
Project JSON / data model = source of truth
```

Canvas, inspector, hierarchy, spreadsheet, export, component library, validation, debug, preview modes, rules, references, event registry and `node_graph` are views, tools or operational layers over the same model. They must not keep conflicting object state.

Every persistent model mutation must go through the command layer.

## 4. Hard architectural boundaries

| Boundary | Canonical rule |
|---|---|
| Core vs Creator | `src/core` is headless TypeScript. It must not import React or creator UI. |
| Shape vs Region | Rectangle renders visual shape. Region handles hit/hover/drag/drop/resize/snap/scroll/content/layout. |
| Canvas vs Model | Canvas renders and interacts with model data. Canvas is not the source of truth. |
| Inspector vs Model | Inspector can keep temporary field drafts, but committed values go through commands. |
| App shell docking | Split/merge/float/dock application panels only. Does not transform canvas objects. |
| Canvas object layout | Moves/resizes objects inside Project. Does not split app shell panels. |
| Graph viewport layout | Pans/zooms/selects graph nodes. Does not change canvas object layout or app shell docking. |
| Node graph | Emits commands, values, shape outputs, validation results or state changes. It must not mutate DOM/canvas directly. |

## 5. Canonical object model expectations

Each important UI element should be representable as an `InterfaceObject` with the relevant subset of:

- identity: `id`, `name`, `type`, `subtype`,
- hierarchy: `parent_id`, `children_ids`, `layer_index`,
- transform: `x`, `y`, `width`, `height`, `rotation`, `scale`, `pivot`,
- bbox: `visual_bbox`, `layout_bbox`, `interaction_region`, `computed_bbox`,
- visual data: `style`, `text_data`, `path_data`,
- interaction data: `region_data`, `event_bindings`,
- structure data: `layout_data`, `group_data`, `panel_data`, `component_data`,
- parametric data: `parameter_data`, `reference_data`, `rule_assignments`, `relation_data`,
- generated data: `instance_generator_data`, `preview_data`,
- runtime/debug data: `state_data`, `validation_status`, `metadata`.

Empty slots are allowed. The model should reserve the slot before the polished tool exists.

## 6. MVP scope

MVP should prove the engine, not every advanced subsystem.

MVP includes:

- project model,
- command layer,
- validation skeleton,
- canvas renderer and viewport,
- primitive creation,
- selection / box select / transform,
- inspector / hierarchy / status sync,
- visual/layout/interaction bbox separation,
- region debug,
- group/ungroup,
- button group proof,
- `Panel_Monitor` sample as Project data,
- save/load Project JSON,
- export SVG,
- default compact dark interface creator shell,
- basic component save/instantiate path.

MVP should reserve schema for:

- rules,
- states,
- value relations,
- object references,
- preview service,
- registry-driven functions,
- `node_graph` profiles,
- app shell docking.

MVP should not expose as default primary workflow:

- full node graph editor,
- advanced procedural icon engine,
- external Blender/Substance/Houdini bridges,
- bitmap/GPU graph,
- complex array / instance-on-points editors,
- noisy raw debug registries.

## 7. Implementation order

Canonical implementation order:

| Phase | Name | Primary goal |
|---|---|---|
| A | Foundation | glossary, terminology, maturity levels, scope boundaries |
| B | Scaffold + shell | stable fullscreen interface creator shell |
| C | Core model + commands | Project / InterfaceObject / applyCommand / validation skeleton |
| D | Canvas + viewport | model renderer, grid, pan, zoom, overlays |
| E | Primitive + selection | rectangle, text, line, region, box select, transform |
| F | Inspector + hierarchy | canvas / inspector / hierarchy / status sync |
| G | BBox + regions + preview modes | visual/layout/interaction separation and debug overlays |
| H | Component proof | button group, Panel_Monitor, save/load/export |
| I | Default MVP cleanup | concept-aligned clean interface, debug hidden by default |
| J | Component library | save selected group, preview, instantiate with new IDs |
| K | Layout / panel builder / size policies | snap, align, box arranger, panel structures |
| L | App shell docking | split/merge/resize app panels only |
| M | Headless events/actions/references/rules | registries and runtime target resolver without graph UI dominance |
| N | Node graph workspace | full graph viewport with profiles, ports, output contracts |
| O | Advanced systems | procedural shapes, array, instance-on-points, external bridges |

## 8. Roadmap view distinction

There are two valid roadmap representations:

1. `Phase A-O` = implementation/dependency order.
2. `Screen State 01-08` = visual documentation states of the fullscreen application.

Do not treat Screen State 01-08 as a strict sprint plan. Use it to guide mockups, screenshots and UI communication.

## 9. Visual documentation standard

Every new UI visualization should look like a real running tool, not a poster.

Rules:

- one dominant mechanism per screen,
- stable app shell: top bar, left rail, center workspace, right inspector, bottom shelf, status bar,
- runtime panels instead of infographic cards,
- solid dark backplates for text, panels, chips and nodes,
- clear active selection / active node / active rule / active state,
- visible output: command, value, shape, asset, target or validation result,
- node graphs use socket rows, typed ports, orthogonal routing, wire lanes and named outputs,
- bottom shelf contains logs, validation, command queue or execution trace, not poster summaries.

## 10. Documentation precedence

When documents conflict, use this order:

1. `00_CURRENT_SOURCE_OF_TRUTH.md`,
2. `01_TERMINOLOGY_CANON_NODE_GRAPH.md`,
3. `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`,
4. `03_FEATURE_MATURITY_MATRIX.md`,
5. `04_CODEX_DEVELOPMENT_PROTOCOL.md`,
6. specific topic documents `10-14`,
7. older dated notes and prompts.

If an older file says `state_graph`, read it as legacy alias for `node_graph`, unless it clearly refers to visual states such as hover/pressed/disabled.

## 11. Current highest-priority cleanup tasks

1. Keep terminology canonical: `node_graph` replaces `state_graph`.
2. Use maturity levels before implementing advanced features.
3. Keep default MVP focused on model, commands, canvas, inspector, hierarchy, regions, component proof and export.
4. Keep graph, docking, rules, relations and generated systems as reserved schema or separate workspaces until their gates are passed.
5. Every development prompt must state touched files, maturity level, forbidden scope, manual tests and acceptance criteria.
