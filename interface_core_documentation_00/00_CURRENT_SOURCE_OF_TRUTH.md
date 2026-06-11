# QUBOK_INTERFACE_CORE — CURRENT SOURCE OF TRUTH

Version: 2026-06-11B
Status: canonical entry point for planning, Codex prompts, roadmap decisions and terminology.

## 1. Project definition

`qubok_interface_core` is a parametric interface engine for the qubok suite. It is not a UI kit, one-off app, screenshot collection or decorative mockup library.

It defines how interface elements are represented as data, edited, validated, debugged, previewed, exported and reused.

```text
primitive -> bbox -> transform/style -> region/layout -> group/panel -> exposed parameters -> links/references/rules -> library asset -> reusable component -> generated helpers -> event/action/node_graph behavior -> workspace
```

## 2. Canonical naming

| Rule | Decision |
|---|---|
| Graph system name | `node_graph` |
| Deprecated names | `state_graph`, `state graph` |
| Prose form | `node graph` is readable; use `node_graph` for identifiers/docs canon |
| Valid `state` usage | visual/interactivity states: `hover`, `pressed`, `disabled`, `dirty`, `states_slot`, `state_transition`, `state_change` |

Rule: use `node_graph` for the graph system. Use `state` only for object/component states.

Do not do a global `stateGraph -> nodeGraph` rename during stabilization. Legacy migration belongs to a dedicated cleanup sprint after selection, interaction and performance contracts are stable.

## 3. Source of truth

```text
Project JSON / data model = source of truth for persistent document data.
```

Canvas, inspector, hierarchy, spreadsheet, export, component library, validation, debug, preview modes, rules, references, event registry and `node_graph` are views/tools over the same persistent model. Persistent mutations go through the command layer.

Implementation must still separate data ownership:

| Layer | Owns | Persistence rule |
|---|---|---|
| `DocumentModel` | objects, hierarchy, components, style/token refs, rules, relations, schema versions | saved/exported |
| `SessionState` | selected ids, active id, hover id, active tool, current drag/session | local/session, not document truth |
| `ViewState` | canvas camera, zoom, grid, overlay mode, scroll positions | local or workspace preset |
| `WorkspaceLayout` | docking tree, pane contents, split ratios | saved separately from document object layout |
| `RuntimeCache` | computed bbox, resolved tokens, preview cache, validation cache | regenerated, not authoritative |
| `History/CommandLog` | undo/redo records, debug trace, command chain | optional/dev; not object truth |

## 4. Architectural boundaries

| Boundary | Canonical rule |
|---|---|
| Core vs Creator | `src/core` is headless TypeScript; no React/creator UI imports and no DOM/browser/storage ownership. |
| Shape vs Region | Shape renders; region handles hit/hover/drag/drop/resize/snap/scroll/content/layout. |
| Canvas vs Model | Canvas renders/interacts with model data; it is not the source of truth. |
| Inspector vs Model | Drafts are allowed; committed values go through commands. |
| App shell docking | Splits/merges/floats/docks app panels only. |
| Canvas object layout | Moves/resizes/aligns objects inside DocumentModel only. |
| Graph viewport layout | Pans/zooms/selects graph nodes only. |
| Node graph | Emits commands, values, shape outputs, validation results or state changes; it does not mutate DOM/canvas directly. |
| Pointer interactions | Use `InteractionSession`: snapshot operation targets, preview transiently, commit one transaction. |
| Canvas instances | One primary interactive canvas per `viewId`; mirrors/snapshots must not dispatch full canvas commands. |

## 5. InterfaceObject model slots

Important UI elements should fit into `InterfaceObject` slots. Empty slots are allowed and should be reserved before polished tools exist.

| Slot group | Fields/examples |
|---|---|
| Identity | `id`, `name`, `type`, `subtype`, `schema_version` |
| Hierarchy | `parent_id`, `children_ids`, `layer_index` |
| Transform | `x`, `y`, `width`, `height`, `rotation`, `scale`, `pivot` |
| Bounds/regions | `visual_bbox`, `layout_bbox`, `interaction_region`, `computed_bbox` |
| Visual | `style`, `text_data`, `path_data`, `token_refs` |
| Interaction | `region_data`, `event_bindings` |
| Structure | `layout_data`, `group_data`, `panel_data`, `component_data` |
| Parametric | `parameter_data`, `reference_data`, `rule_assignments`, `relation_data` |
| Generated/preview | `instance_generator_data`, `preview_data` |
| Runtime/debug | `state_data`, `validation_status`, `metadata` |

Avoid storing duplicate source-of-truth flags. Example: selection should live in selection/session state, not as both `object.selected` and `selection.selected_ids` unless one is clearly derived.

## 6. MVP scope

MVP proves the engine, not every advanced subsystem.

| Include in MVP | Reserve schema for later | Keep out of default MVP workflow |
|---|---|---|
| project/document model | rules | full node graph editor |
| command layer | states | procedural icon engine |
| validation skeleton | value relations | external Blender/Substance/Houdini bridges |
| canvas renderer/viewport | object references | bitmap/GPU graph |
| primitive creation | Preview Service | complex array/instance-on-points editors |
| selection / box select / transform | registry-driven functions | noisy raw debug registries |
| inspector/hierarchy/status sync | `node_graph` profiles | advanced procedural systems |
| visual/layout/interaction bbox | app shell docking |  |
| group/ungroup, button group proof, `Panel_Monitor` | schema migrations |  |
| save/load Project JSON, SVG export, basic component instantiate | canvas mirror/snapshot modes |  |

## 7. Current implementation diagnosis

The current local repository structure shows a working prototype with many sprint note files in the project root, build logs, diagnostics, backups, `dist`, `node_modules` and generated artifacts. This is normal after rapid prototyping, but it creates noisy context for Codex and makes roadmap navigation harder.

Current stage classification:

```text
Phase E/F/G/H stabilization + Phase I preparation
```

The project should not currently be treated as Phase N `node_graph` implementation, even if graph/debug references exist visually or in legacy files.

Primary runtime risks:

| Risk | Likely cause | Required contract |
|---|---|---|
| multi-delete affects one object | active/hit id used instead of batch targets | Selection Operation Target Contract |
| multi-drag affects one object | target list recalculated or reduced at drag start | `operation_target_ids` snapshot |
| drag/resize freezes | command/validation/render/log/autosave on pointermove | `InteractionSession` + transient preview |
| docked canvases are expensive | multiple panes mount full interactive renderers | Canvas Instance Guardrail |
| debug overload | raw registries/logs visible in default UI | maturity-gated debug workspace |
| legacy naming noise | old `stateGraph` names in code/docs | dedicated naming cleanup later |

## 8. Implementation order

Canonical phase order is defined in `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`:

```text
A Foundation -> B Shell -> C Model/Commands -> D Canvas -> E Primitives/Selection -> F Inspector/Hierarchy -> G BBox/Regions -> H Component Proof -> I MVP Cleanup -> J Component Library -> K Panel Builder -> L Docking -> M Headless Events/Rules/Refs -> N Node Graph -> O Advanced Systems
```

Screen State 01-08 is visual documentation only, not a sprint plan.

Current stabilization order before new advanced work:

```text
0 Safety baseline -> 1 Selection Operation Target Contract -> 2 InteractionSession -> 3 Canvas Instance Guardrail -> 4 Document/Session/View split audit -> 5 Core browser boundary audit -> 6 Default UI visibility cleanup -> 7 Versioned persistence/migrations -> 8 dedicated node_graph naming cleanup
```

## 9. Visual documentation standard

Every UI visualization should look like a running tool, not a poster.

| Required | Avoid |
|---|---|
| one dominant mechanism per screen | everything visible at once |
| stable shell: top/left/center/right/bottom/status | infographic cards pretending to be UI |
| runtime panels/logs/validation | poster summaries in bottom panels |
| clear active selection/node/rule/state | decorative labels over content |
| visible output: command/value/shape/asset/target/validation | graph with no output |
| typed sockets, lanes, orthogonal wires | spaghetti wires through text/body |

## 10. Documentation precedence

When documents conflict, use this order:

1. `00_CURRENT_SOURCE_OF_TRUTH.md`
2. `01_TERMINOLOGY_CANON_NODE_GRAPH.md`
3. `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
4. `03_FEATURE_MATURITY_MATRIX.md`
5. `04_CODEX_DEVELOPMENT_PROTOCOL.md`
6. `05_CURRENT_IMPLEMENTATION_STABILIZATION_MAP.md`
7. `06_LOCAL_REPO_STRUCTURE_AND_CODEX_ACCESS_MAP.md`
8. topic references `10-14`
9. older dated notes and visual prompts

Legacy `state_graph` means `node_graph`, unless the text clearly describes visual/interactivity states.

## 11. Development readiness checklist

Before implementation, every prompt/task should state:

| Required field | Purpose |
|---|---|
| Phase | maps task to A-O roadmap |
| Feature | single dominant mechanism |
| Maturity | current and target L0-L5 |
| Touched files | limits patch scope |
| Out-of-scope | prevents feature creep |
| Model/commands/views affected | protects source-of-truth path |
| Validation/manual tests | defines acceptance |
| Runtime hot path impact | prevents pointermove/render/autosave regressions |
| Data ownership | identifies document/session/view/workspace/cache owner |
