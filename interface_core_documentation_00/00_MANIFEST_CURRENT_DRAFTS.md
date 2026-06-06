# 00 — Current documentation manifest

Status: draft 00
Created: 2026-06-06

## Purpose

This manifest lists the current documentation drafts inside `interface_core_documentation_00`.

The folder is a working documentation space for naming, functionality definitions, roadmap phases and implementation boundaries of `qubok_interface_core`.

## Current files

| File | Purpose |
|---|---|
| `00_INDEX.md` | Initial folder index, project purpose, roadmap phase sketch |
| `01_naming_taxonomy_and_display_order.md` | Naming rules, hierarchy order, display order, inspector order, tag naming |
| `02_primitive_shapes_and_interface_object_types.md` | Definition of primitive shapes and basic interface object types |
| `03_interface_element_groups_types_tags.md` | Classification of interface elements into groups, roles, tags and domains |
| `04_state_machine_relation_to_core_functionality.md` | Relation between state machine, events, target resolver, actions, commands and core features |

## Current documentation focus

The current documentation group focuses on predefining selected elements before implementation:

- naming and vocabulary,
- primitive shapes,
- object type taxonomy,
- interface element grouping,
- tags and semantic roles,
- display order,
- hierarchy order,
- inspector order,
- how state machine relates to basic functionality,
- how events become commands,
- what should be MVP and what should stay later/debug.

## Most important current decisions

| Decision | Meaning |
|---|---|
| Rectangle renders, region reacts | Visible shape and interaction area are separate concepts |
| Panel is structure, not one rectangle | Panel contains frame, header, content, regions, sections and controls |
| State machine emits commands | It does not mutate DOM/SVG/canvas directly |
| Project model is source of truth | Canvas, inspector, hierarchy, export and debug read from the same model |
| Tags classify behavior and maturity | Tags help inspector, hierarchy, export, debug and state machine targeting |
| Workspace panel is not canvas panel | Editor shell layout and designed UI objects have separate data models |
| Graph viewport is separate | State graph node layout is not canvas object layout and not app docking layout |

## Suggested next documents

| Proposed file | Purpose |
|---|---|
| `05_roadmap_feature_assignment_matrix.md` | Map each function/type/tag group to roadmap phase and maturity level |
| `06_inspector_schema_and_field_order.md` | Define inspector tabs, fields, field paths and command mapping |
| `07_hierarchy_display_and_grouping_rules.md` | Define tree grouping, sorting, icons, warning markers, region grouping |
| `08_event_action_command_examples.md` | Concrete examples of event assignments and command outputs |
| `09_panel_component_naming_examples.md` | Canonical naming examples for button, slider, panel, toolbar and inspector section |

## Minimal continuation rule

When adding a new documentation file, define:

```text
what is being named
what data owns it
where it appears in UI
which roadmap phase introduces it
whether state_machine can target it
whether it exports to JSON/SVG
how it appears in hierarchy and inspector
```
