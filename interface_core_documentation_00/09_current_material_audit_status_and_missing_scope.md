# 09 — Current material audit, status and missing scope

Status: draft 00
Scope: audit of currently discussed material, relation to existing documentation, missing topics that need separate documents
Mode: private / local-first / semi-offline

## 1. Purpose

This document consolidates the current state of the `qubok_interface_core` documentation after many chat sessions, text documents and visual atlases.

It does not replace documents `01–08`. It maps what is already covered, what was updated by later visual material, and what still needed to be added to GitHub.

Main conclusion:

```text
The core architecture is already stable.
The missing area is not the foundation, but the next layer of authoring systems:
local persistence, presets, parameter bindings, component templates, local data sources,
macros, developer/versioning tools, and aware curve_node routing for state_graph.
```

## 2. Current canonical position

`qubok_interface_core` is not a simple UI kit. It is a parametric interface engine where every interface element is a data object. The already accepted chain remains:

```text
primitive
-> bbox
-> transform/style
-> region/layout
-> group/panel
-> exposed parameters
-> library asset
-> reusable UI component
-> event/action binding
-> state_graph behavior
-> application workspace
```

The core rules are unchanged:

```text
Project data model = source of truth
Canvas / Inspector / Hierarchy / Debug / Library / StateGraph = views or tools over the model
Region handles interaction
Rectangle renders visible shape
Commands are the only persistent mutation path
State graph emits commands, never mutates DOM/canvas directly
App shell docking != canvas object layout != graph viewport layout
```

## 3. Existing documentation already on GitHub

Current documentation in `interface_core_documentation_00` already covers:

| Existing doc | Covered scope |
|---|---|
| `01_project_definition_and_principles.md` | project identity, principles, source-of-truth direction |
| `02_roadmap_and_implementation_order.md` | early roadmap and staged implementation idea |
| `03_primitive_and_object_model.md` | primitive/object model foundations |
| `04_regions_events_and_interaction_model.md` | region/event/interaction concepts |
| `05_element_property_panel_schema_and_preview.md` | schema-driven inspector and live previews |
| `06_next_documentation_targets_and_work_packages.md` | work packages for next documentation blocks |
| `07_state_machine_events_actions_registry_catalog.md` | event/action registry, target resolver, conflicts, logic spreadsheet |
| `08_debug_inspect_parameter_catalog.md` | debug/inspect parameters, source modes, value/source display |

Documents `07` and `08` are especially important because they define the next layer of logic:

```text
event -> target resolver -> action -> command -> Project model -> rerender / validation
```

## 4. What is already stable enough

The following topics are now treated as canonical and should not be redesigned without a clear reason.

| Topic | Status | Meaning |
|---|---|---|
| Source of truth | stable | `Project` / data model owns persistent state |
| Command layer | stable | all persistent edits go through commands |
| Primitive model | stable base | empty, rectangle, text, line, region, group, panel |
| Region separation | stable | visual, layout and interaction areas are different systems |
| Inspector schema | stable direction | inspector should be schema-driven, not one-off hardcoded panels |
| Debug inspect | stable direction | debug reads model/runtime diagnostics and shows source chain |
| Event/action registry | stable direction | actions are command wrappers with metadata |
| Target resolver | stable direction | resolves event targets, selected objects, parent/children, query targets |
| Maturity levels | stable | L0-L5 prevent advanced systems from polluting default MVP |
| Default UI direction | stable | compact dark interface creator, not debug/API dashboard |
| Offline/private direction | stable update | no cloud/team/server-first features in core scope |

## 5. Material from recent visual atlases

Recent visual atlases introduced or clarified the following topics:

| Atlas topic | New/updated implication |
|---|---|
| Project status board | need a living status/maturity map for documentation and implementation |
| Persistence and presets | local project save, snapshots, preset packs, theme variations, override chains |
| Bindings and formulas | value-to-value links, formulas, dependency graph, dirty propagation, cycle blocking |
| Components and templates | structure variants versus theme/style variants, slots and exposed parameters |
| Local data sources | JSON/CSV/TXT/SVG/bitmap metadata as local sources only |
| Automations and macros | user macros, event hooks, scheduled local tasks, action queues, dry-run mode |
| Curve node aware routing | `curve_node` as spatially-aware `StateGraphEdge` with routing context and solver |
| Developer/versioning tools | local snapshots, validation, migration, archives, recovery, no team/cloud workflow |

## 6. Missing GitHub documentation after the visual pass

Before this audit, GitHub had solid foundations but did not yet fully document the following late updates:

| Missing document area | Needed because |
|---|---|
| local/private/offline operation | user explicitly excludes cloud/team/server workflows |
| persistence and presets | projects, snapshots, preset packs, theme variations and local library must be defined |
| advanced bindings and formulas | slider-value links and formula graphs need a stable model |
| component/template authoring | panels, slots, exposed parameters and structural variants need exact rules |
| local data sources | CSV/JSON/SVG/image metadata must be integrated without remote services |
| macros and local automations | automation must be local, safe, replayable and command-based |
| developer/versioning/archive tools | local snapshots, migrations and recovery must replace team/cloud assumptions |
| curve_node spatial routing awareness | recent graph-edge concept needs architecture and parameters |

This audit therefore creates the next documentation group:

```text
09_current_material_audit_status_and_missing_scope.md
10_local_private_persistence_presets_and_versions.md
11_bindings_formulas_and_dependency_graph.md
12_component_templates_panel_authoring_and_slots.md
13_local_data_sources_macros_and_developer_tools.md
14_curve_node_spatial_routing_awareness.md
```

## 7. Updated scope exclusions

The project is designed for private/semi-offline/internal use. The following are out of scope for this documentation layer:

```text
cloud sync
online collaboration
team roles / permissions
multi-user editing
public marketplace
remote telemetry
server-hosted asset libraries
account-based sharing
production SaaS workflows
```

Allowed only as optional future import/export boundary, not core behavior:

```text
manual file import/export
local folder package
local loopback endpoint on the same machine
portable archive
offline backup
```

## 8. Status map: already defined vs next definition

| Area | Current status | Next documentation target |
|---|---|---|
| Primitive/object model | defined | keep stable, refine exact field schemas later |
| Regions/events | defined | connect to component templates and macros |
| State machine registry | defined in catalog | connect to bindings/formulas and curve_node graph edges |
| Debug inspect parameters | defined in catalog | connect to source chains, presets, versioning and local data |
| Panel property schema | defined | expand into component template and slot model |
| Library concept | partially defined | add local library, presets, snapshots and variants |
| Tags/filtering | partially defined | connect to presets, actions, data sources and debug spreadsheets |
| Persistence | missing / visual only | define local project, presets, snapshots, migrations |
| Bindings/formulas | missing / visual only | define dependency graph, formula nodes, cycle protection |
| Local data sources | missing / visual only | define source schema, refresh/cache/mapping |
| Macros/automations | missing / visual only | define local event hooks, action queue and safety modes |
| Developer tools | missing / visual only | define validation, migration, snapshots, profiles |
| Curve node awareness | missing / visual only | define graph routing context and solver |

## 9. Updated implementation priority

The new documentation does not mean all new systems enter MVP immediately.

Recommended maturity placement:

| System | First maturity | Default UI? |
|---|---|---|
| local project save/load | L3 MVP | yes |
| snapshots/backups | L2/L3 | compact yes |
| local presets | L3 | yes after component library |
| formula bindings | L1/L2 first | no, debug/workspace first |
| component templates/slots | L2/L3 | yes in Panel Builder / Library |
| local data sources | L1/L2 first | no, later workspace/panel |
| macros | L2 first | no, debug/workspace first |
| developer versioning | L2/L3 | compact/debug only |
| curve_node aware routing | L1/L2 first | only in Logic/Graph workspace |

## 10. Final audit statement

Current documentation is not missing the philosophical base anymore. It is missing exact operational specs for the second layer:

```text
how things persist locally
how presets override values
how values bind to values
how components expose parameters
how panels are built from reusable templates
how local data enters UI objects
how user macros replay command chains
how graph edges route themselves spatially
```

Those topics should be documented as local/private, command-based, model-first systems before they become UI features.
