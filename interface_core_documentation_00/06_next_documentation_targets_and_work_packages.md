# 06 — Next documentation targets and work packages

Status: draft 00
Scope: planned documentation topics, dependencies, implementation meaning

## 1. Purpose

This document reorganizes the next documentation targets for `qubok_interface_core`.

The topics may be developed in random order during conversation, but they should still form one coherent model:

```text
primordials
-> editable/detectable parameters
-> component breakdowns
-> panels/rules/parameter source
-> library assets
-> tags/filtering/search
-> state_machine/actions/events
-> implementation-ready MVP defaults
```

The goal is not to implement everything immediately. The goal is to define enough structure so implementation can start from MVP without blocking later expansion.

## 2. Work package map

| Work package | Main question | Output document |
|---|---|---|
| WP01 Primordials | What are the smallest objects/operators? | `07_primordials_exact_definition.md` |
| WP02 Parameters | What can the program edit/detect/read? | `08_editable_detectable_parameter_catalog.md` |
| WP03 Components | How are slider/button/switch/dropdown broken into primitives? | `09_component_breakdown_patterns.md` |
| WP04 Panel rules | How do panels override/inherit/dictate parameters? | `10_panel_rules_alignment_parameter_source.md` |
| WP05 Library | How are groups/tools/functions/actions stored and reused? | `11_library_assets_groups_tools_functions.md` |
| WP06 Tags/filtering | How are names/tags/types used for search/filter/debug? | `12_tags_filtering_and_query_system.md` |
| WP07 State machine | How are actions/states/events created and connected? | `13_state_machine_behavior_authoring_model.md` |
| WP08 MVP defaults | What must exist before in-app authoring exists? | `14_mvp_default_assets_and_bootstrap_logic.md` |

## 3. Dependency order

Even if documentation is written in random order, implementation dependencies should remain clear.

```text
A. Primordials must exist before components can be decomposed.
B. Editable parameters must exist before inspector/rules/library can be useful.
C. Component breakdowns must exist before component library presets are reliable.
D. Rules and parameter sources must exist before inherited/dictated panel behavior.
E. Tags must exist before powerful filtering, batch edits and library search.
F. State machine can target objects only after events, regions, actions and command layer exist.
G. Some default assets/actions must be bootstrapped before the app can author them internally.
```

## 4. WP01 — Primordials

Goal:
Define the smallest objects and operators used to construct every interface element.

Important distinction:

```text
shape primitive != interaction primitive != procedural primitive
```

Recommended categories:

| Category | Types |
|---|---|
| Object primitives | `empty`, `rectangle`, `line`, `curve`, `text_rectangle` |
| Interaction primitives | `region`, `region_rectangle` |
| Layout primitives | `empty_rectangle`, `layout_box`, `guide_line` |
| Procedural primitives/operators | `instance_on_points`, `instance_array`, `curve_resample` |
| Value primitives | `integer`, `float`, `vector2`, `vector3`, `color`, `string`, `boolean` |

Documentation should define for each:

```text
technical type
visual representation
editable fields
runtime behavior
export behavior
can be selected?
can be grouped?
can be targeted by state_machine?
roadmap phase
```

## 5. WP02 — Editable and detectable parameters

Goal:
Define all parameters the program can edit, read, detect, sample, expose or bind.

Groups:

| Parameter group | Examples |
|---|---|
| Identity | id, name, type, tags, role |
| Transform | pos, size, rotation, pivot, scale |
| Geometry | corner_radius, curve points, handles, resolution |
| Style | fill, stroke, opacity, blend_mode, gradient, shadow later |
| Text | string, font, alignment, spacing, flow_type |
| Region | region_type, linked_visual_id, priority, cursor, event bindings |
| Layout | margin, padding, anchor, alignment, gap, size policy |
| State | normal, hover, active, focus, disabled, warning, error |
| Export | svg visibility, json visibility, runtime only/debug only |
| Source | internal, inherited, dictated, theme, graph output |
| Detection | hover target, pointer position, local position, drag delta, selected count |

Important separation:

```text
editable = user can change it
detectable = runtime can read it
bindable = graph/rule can connect it
computed = system derives it
exportable = saved/rendered externally
```

Example:

```text
Slider_Handle.local_x is detectable and editable.
Slider_Value.normalized is computed and bindable.
Slider_Value_Text.string is editable and graph-output-driven.
```

## 6. WP03 — Component breakdown patterns

Goal:
Describe how derived controls are decomposed into primitives, regions, value bindings and rules.

Initial components to document:

| Component | Required breakdown |
|---|---|
| `button` | body rectangle, label text, optional icon, click region, hover/active states |
| `switch` | track rectangle, knob shape, click region, boolean value, on/off states |
| `slider` | track, fill, handle, drag region, value text optional, constraint rule |
| `slider_minMax` | track, range fill, min handle, max handle, two drag regions, min/max value rules |
| `drop_down` | header, arrow icon, click region, content group, open/closed state |
| `progress` | background track, fill rectangle, value binding, optional label |
| `swatch` | color rectangle, border, pick region, value binding |
| `toolbar` | layout group, buttons, icon/text variants, active tool state |
| `interface_shelf` | panel group, rows/sections, scroll/drop/content regions |

Each component should specify:

```text
internal hierarchy
required primitive types
regions
state variants
parameter bindings
constraints
library asset behavior
export behavior
```

## 7. Component constraints reference

Blender-like constraints should be represented as rules attached to regions, handles or components.

Example: slider handle constraint

```text
Slider_Handle_Drag_Region.drag
-> target Slider_Handle
-> constraint: local_x between Slider_Track.min_x and Slider_Track.max_x
-> local_y locked to Slider_Track.center_y
-> output: normalized_value 0..1
```

Do not encode this as an invisible hardcoded behavior inside the rectangle.

Recommended model:

| Concept | Object owning it |
|---|---|
| what can be grabbed | `region_rectangle` with `region_type = drag` |
| what moves | `target_rule`, usually handle object or parent group |
| where it can move | constraint rule, local bounds or linked bbox |
| what value changes | value binding / graph output |
| what text updates | action/command targeting text object |

Example relation:

```text
Region: Slider_Handle_Drag_Region
Target: Slider_Handle_Group
Constraint: lock_y, clamp_x_to Track_InteractionBBox
Value: normalized = local_x / track_width
Actions: SetPositionX, SetText, SetFillWidth
```

## 8. WP04 — Panel rules and parameter sources

Goal:
Describe how panels influence contained groups/elements and how values are selected as internal/inherited/dictated.

Panel can define:

| Rule type | Meaning |
|---|---|
| layout rule | margin, padding, gap, alignment, anchor, size policy |
| style rule | fill, stroke, opacity, text color, radius |
| behavior rule | hover/click/focus/active/disabled handling |
| region rule | auto-create hit/drag/resize/content regions |
| constraint rule | min/max size, locked axis, clamp to bbox |
| source rule | internal/inherited/dictated/theme/component parameter |
| visibility rule | show/hide by state, type, tag or condition |

Parameter source modes:

| Mode | Meaning | Example |
|---|---|---|
| internal | value is owned locally | button radius is local 4px |
| inherited | value is read from parent/theme/component | text color from panel theme |
| dictated | value is forced by panel/rule/state | all sliders in panel become green |

Panel should allow managing many child elements by:

```text
selection filter
query by tag/type/role
batch edit
rule assignment
preview affected elements
debug spreadsheet view
```

Example:

```text
Panel_Settings rule:
  target query: type=slider_group inside this panel
  parameter: color_main
  source: dictated
  value: theme.slider.activeFill
```

## 9. WP05 — Library panel / group / tool / function / state_graph

Goal:
Define one library system that can store visual assets, tool presets and behavior/action presets.

Library asset types:

| Asset type | Example |
|---|---|
| primitive preset | rounded rectangle preset, dashed region preset |
| group component | button, slider, dropdown |
| panel component | inspector section, shelf, toolbar |
| icon/shape asset | folder icon, arrow icon, star icon |
| style preset | dark button, warning state, selected outline |
| theme preset | semantic color set |
| rule preset | clamp x to parent bbox, inherit text color |
| action preset | SetFill on hover, ToggleVisible on click |
| event assignment preset | mouse_click -> action |
| state graph preset | slider value binding graph |
| tool preset | draw dashed curve, arrow curve preset |

Important rule:

```text
every visual asset is built from primordials
every behavior asset emits commands through state_graph/action system
```

However, MVP may ship with default hardcoded/built-in assets before the app can author them internally.

Bootstrap distinction:

| Term | Meaning |
|---|---|
| built-in default | implemented in code, available at startup |
| library asset | saved JSON/component/action/rule preset |
| user-created asset | created in app and saved by user |
| editable later | can be modified only after corresponding editor exists |

## 10. WP06 — Tags, names, groups and filtering

Goal:
Define a query/filter model that allows batch editing, library search, debug inspection and event/action audits.

Filtering examples:

```text
all sliders in selected panel
all buttons with variant=primary
all actions triggered by mouse_click
all elements using theme.warning
all regions of type drag
all components saved as toolbar presets
all state variants using hover
all text fields with inherited color
```

Tag dimensions:

| Dimension | Examples |
|---|---|
| type | rectangle, region, group, panel, action, event |
| role | button, slider, handle, label, track, fill, header |
| domain | canvas_object, library, state_graph, debug, export |
| behavior | hoverable, draggable, resizable, clickable |
| state | normal, hover, active, focus, disabled, warning |
| source | internal, inherited, dictated, theme, graph_output |
| maturity | mvp, debug, post_mvp, experimental |
| asset | preset, component, theme, tool, action, rule |

Query example:

```text
inside: PanelSettings
where role=slider and parameter=color_main
set source=dictated
set value=theme.slider.activeFill
```

Debug spreadsheet example:

```text
event_type | source_region | target | action | command | last_run | status
mouse_click | ButtonSave_ClickRegion | Project | SaveProject | project.save | 12:03:22 | success
hover_enter | SliderA_HandleRegion | SliderA_Handle | SetFill | object.patch | 12:03:30 | active
```

## 11. WP07 — State machine behavior authoring

Goal:
Describe how all actions and interface states are created/assigned through a state machine or simplified action assignment system.

State variants:

| State | Example behavior |
|---|---|
| normal | default visual state |
| hover_over | change fill/stroke, show tooltip |
| mouse_down | pressed state, offset/shadow change |
| active | selected/toggled state |
| focus | keyboard/input focus state |
| disabled | opacity lower, blocked region |
| warning | color warning + validation marker |
| error | red marker + action blocked |

Behavior authoring flow:

```text
choose event source
-> choose target resolver
-> choose action from registry
-> set parameters / value bindings
-> preview command output
-> validate
-> save as assignment or graph preset
```

Example button hover:

```text
Button_ClickRegion.pointer_enter
-> target Button_Body_Rectangle
-> action SetFill(theme.button.hover)
-> command object.patch
```

Example slider drag:

```text
Slider_Handle_DragRegion.drag_move
-> target Slider_Handle
-> action SetLocalX(clamped to Slider_Track_BBox)
-> compute normalized value
-> action SetText(Slider_Value_Text)
-> command transform.update + object.patch
```

## 12. WP08 — MVP default assets and bootstrap logic

Goal:
Define which default elements and functions must exist before the application can create/edit them internally.

MVP must include built-in versions of:

| Built-in default | Why needed |
|---|---|
| base primitives | cannot build anything without them |
| inspector fields | needed to edit primitives |
| hierarchy/layers | needed to manage groups |
| selection/transform | needed to manipulate objects |
| region hit-test | needed for interaction and state targets |
| command layer | needed for all changes |
| save/load JSON | needed for library/project persistence |
| component save/instantiate | needed for library proof |
| action registry minimal | needed for event/action tests |
| default button/slider sample | proves component breakdown and binding |
| theme tokens | proves preference-based styling |

Before in-app authoring exists, these can be shipped as:

```text
hardcoded TypeScript defaults
JSON sample assets
seed library package
built-in action registry
built-in inspector schema
```

Later they become editable through:

```text
primitive editor
component editor
panel builder
state graph workspace
preferences/theme editor
library manager
```

## 13. Recommended immediate documentation sequence

Best next sequence:

1. `07_primordials_exact_definition.md`
2. `08_editable_detectable_parameter_catalog.md`
3. `09_component_breakdown_patterns.md`
4. `10_panel_rules_alignment_parameter_source.md`
5. `12_tags_filtering_and_query_system.md`
6. `11_library_assets_groups_tools_functions.md`
7. `13_state_machine_behavior_authoring_model.md`
8. `14_mvp_default_assets_and_bootstrap_logic.md`

Reason:

```text
first define what exists
then what can be edited/read
then how components are built
then how panels/rules control them
then how to search/filter them
then how library saves/reuses them
then how behavior is authored
then what must be bootstrapped in MVP
```

## 14. Single model tying all topics together

```text
Element
  has type/name/tags
  is built from primordials
  exposes parameters
  may belong to group/panel/component
  may inherit or override panel/theme rules
  may be saved in library
  may have regions
  may emit events
  may be targeted by actions/state_machine
  changes only through commands
  appears in inspector/hierarchy/debug/export according to its type and tags
```

## 15. Implementation guardrail

Do not implement advanced authoring tools before their data model exists.

Correct order:

```text
model/schema first
built-in sample second
basic inspector third
library serialization fourth
visual authoring later
```

Example:

A slider can exist as built-in component JSON before the app can visually author every part of slider logic. But the slider should still be decomposed into primitives, regions, rules and actions so future editing does not require redesign.
