# 04 — State machine relation to core functionality

Status: draft 00
Scope: state machine, event/action model, command layer relation, target resolving, examples

## 1. Core definition

The state machine is not a replacement for the core editor tools.

It is a logic layer that reacts to events, resolves targets and emits commands.

Canonical flow:

```text
event
-> target resolver
-> state machine / action assignment
-> command
-> applyCommand
-> Project model update
-> dirty flags
-> validation
-> rerender / export state update
```

Forbidden flow:

```text
event
-> graph node
-> direct DOM/SVG mutation
```

## 2. Main rule

```text
state_machine does not mutate UI directly
state_machine emits commands
commands mutate Project model
views rerender from Project model
```

This keeps canvas, inspector, hierarchy, export and debug synchronized.

## 3. What belongs to built-in core

Built-in core functionality exists before state machine and does not need graph logic to work.

| Core functionality | State machine role |
|---|---|
| create object | may call object.create command later |
| select object | may call selection.set command later |
| transform object | may call transform.update command later |
| edit style/text | may call object.patch command later |
| group/ungroup | may call group.create / group.ungroup later |
| snap/align | may call layout.align / transform.update later |
| save/export | may call project.save / export commands later |
| validation | may trigger validate command or react to result |
| component instantiate | may call component.instantiate later |

The editor must remain usable without a visual graph.

## 4. State machine maturity

State machine is later than basic MVP workflow.

Recommended maturity path:

| Level | Meaning |
|---|---|
| L0 spec | described in docs |
| L1 headless registry | event/action types exist |
| L2 debug panel | events/actions visible in debug |
| L3 event assignments | user can assign simple events to actions |
| L4 graph workspace | full node editor with pan/zoom/multiselect |
| L5 composable logic asset | reusable graph presets/components |

## 5. Event types

Events are normalized signals from UI, system or project runtime.

| Event | Source | Example |
|---|---|---|
| `pointer_enter` | region/canvas | hover begins |
| `pointer_leave` | region/canvas | hover ends |
| `pointer_move` | pointer | hover debug updates |
| `click` | region/object | button clicked |
| `double_click` | region/object | enter component edit mode later |
| `drag_start` | region/handle | begin moving panel/object |
| `drag_move` | pointer | update transform preview |
| `drag_end` | pointer | commit transform command |
| `resize_start` | resize region | start resize |
| `resize_move` | pointer | update resize preview |
| `resize_end` | pointer | commit resize |
| `drop` | drop region | component inserted |
| `selection_change` | project | inspector/hierarchy update |
| `value_change` | inspector/field | update property |
| `project_save` | system | save completed |
| `validation_run` | validation | validation result changed |
| `shortcut` | keyboard | user pressed command shortcut |
| `timer_tick` | system | every x seconds, later |

## 6. Event payload

Minimal event payload:

```text
event_id
event_type
source_domain
target_id optional
region_id optional
pointer_world_position optional
pointer_screen_position optional
modifiers
value optional
timestamp
```

Example:

```json
{
  "event_type": "click",
  "source_domain": "canvas",
  "target_id": "ButtonPrimary_Group",
  "region_id": "ButtonPrimary_Click_Region",
  "pointer_world_position": { "x": 420, "y": 180 },
  "modifiers": { "shift": false, "alt": false, "ctrl": false }
}
```

## 7. Target resolver

Target resolver decides which object or set of objects an event/action should affect.

Target rules:

| Target rule | Meaning |
|---|---|
| `event_target` | object resolved by hit-test/event |
| `event_region` | region that received event |
| `active_object` | Project.selection.active_id |
| `first_selected` | first selected object |
| `all_selected` | all selected objects |
| `parent_of_target` | parent of event target |
| `children_of_target` | children of event target |
| `object_by_id` | explicit object id |
| `objects_by_type` | all objects of type |
| `objects_with_tag` | all objects with tag |
| `nearest_snap_candidate` | object/guide resolved by snap system |

Resolver flow:

```text
receive event
-> read Project
-> read selection
-> read hit-test result
-> apply target rule
-> validate target availability
-> return target list
```

## 8. Action registry

Action registry exposes safe command wrappers.

| Action | Command output | Typical target |
|---|---|---|
| `SetFill` | `object.patch` | rectangle/group style target |
| `SetStroke` | `object.patch` | rectangle/line |
| `SetOpacity` | `object.patch` | visual object/group |
| `SetText` | `object.patch` | text_rectangle |
| `SetVisible` | `object.patch` | any object |
| `SetLocked` | `object.patch` | any object |
| `SetPosition` | `transform.update` | object/group |
| `SetSize` | `transform.update` | rectangle/group/panel |
| `MoveBy` | `transform.update` | object/group |
| `SelectObject` | `selection.set` | any selectable object |
| `ValidateProject` | `debug.validate` | project |
| `InstantiateComponent` | `component.instantiate` | component asset + canvas position |
| `AssignEvent` | `stateGraph.assign_event` | region/object |

Action definition should include:

```text
action_id
label
category
parameters schema
target policy
command mapping
validation rules
debug description
```

## 9. Command output

Every state machine output should become one or more commands.

Example output:

```json
{
  "type": "object.patch",
  "payload": {
    "object_id": "ButtonPrimary_Body_Rectangle",
    "patch": {
      "style.fill": "theme.accent_active"
    }
  },
  "source": "state_machine"
}
```

## 10. Basic event assignment without graph

Before full graph editor, simple assignments can exist as data.

Example:

```text
ButtonPrimary_Click_Region.click
-> target: ButtonPrimary_Body_Rectangle
-> action: SetFill
-> parameters: fill = theme.accent_active
```

Possible schema:

```json
{
  "id": "assignment_button_primary_click_fill",
  "source_object_id": "ButtonPrimary_Click_Region",
  "event_type": "click",
  "target_rule": "object_by_id",
  "target_object_id": "ButtonPrimary_Body_Rectangle",
  "action_id": "SetFill",
  "parameters": {
    "fill": "theme.accent_active"
  },
  "enabled": true
}
```

## 11. Relation to primitive objects

| Primitive | State machine relation |
|---|---|
| `rectangle` | can receive style/transform commands |
| `text_rectangle` | can receive text/style/transform commands |
| `line` | can receive stroke/path/visibility commands |
| `region_rectangle` | can be event source and can be enabled/disabled |
| `empty` | can be used as target marker/pivot/snap reference |
| `empty_rectangle` | can be converted or used as layout target |
| `group` | can receive transform/visibility/state commands affecting children |
| `panel` | can receive exposed parameter commands |

## 12. Relation to core UI tools

State machine should reuse the same actions as manual tools.

| Manual operation | State machine equivalent |
|---|---|
| user changes fill in inspector | `SetFill` action -> `object.patch` |
| user moves object | `MoveBy` / `SetPosition` -> `transform.update` |
| user selects row in hierarchy | `SelectObject` -> `selection.set` |
| user clicks validate | `ValidateProject` -> `debug.validate` |
| user drags component to canvas | `InstantiateComponent` -> `component.instantiate` |

This prevents duplicate behavior paths.

## 13. Simple example — hover highlight

Goal:

When hovering button region, highlight button body. When leaving, restore default.

Objects:

```text
ButtonPrimary_Group
  ButtonPrimary_Body_Rectangle
  ButtonPrimary_Label_Text
  ButtonPrimary_Click_Region
```

Assignments:

```text
ButtonPrimary_Click_Region.pointer_enter
-> target ButtonPrimary_Body_Rectangle
-> SetFill(theme.accent_hover)

ButtonPrimary_Click_Region.pointer_leave
-> target ButtonPrimary_Body_Rectangle
-> SetFill(theme.button_default)
```

Important:

This changes model through command. Later reaction layer may do this as temporary state overlay instead of permanent style mutation.

## 14. Simple example — click toggles panel visibility

```text
ButtonDetails_Click_Region.click
-> target PanelDetails_Group
-> ToggleVisible
-> object.patch visible = !visible
```

Command output:

```text
object.patch PanelDetails_Group.visible
```

Validation:

- target exists,
- target can be hidden,
- target is not locked if lock blocks visibility changes.

## 15. Example — custom snap while dragging

This is later/advanced, but illustrates relation to core.

```text
Panel_Header_Drag_Region.drag_move
-> target parent panel
-> compute desired position
-> call snap engine
-> emit transform.update preview or commit command
```

The graph does not implement its own independent snap math if core already has snap helpers.

## 16. Example — validation reaction

Goal:

If validation finds broken region link, flash the region in debug overlay.

```text
validation_run
-> filter errors where type = broken_region_link
-> target region object
-> SetDebugState(warning_flash)
```

Command or runtime overlay:

- if persistent: command to object state data,
- if temporary: reaction overlay state outside permanent style.

## 17. State graph node categories

Later full graph workspace can use these nodes:

| Node category | Purpose |
|---|---|
| Event Input | receives click/hover/shortcut/timer/save events |
| Target Resolver | selects event target/active/all selected/etc. |
| Condition | if/else branching |
| Compare | compare values |
| Get Property | read object field |
| Set Property | emits object.patch |
| Transform | emits transform.update |
| Value Mapping | map range, clamp, normalize |
| State Memory | stores local graph state |
| Emit Command | low-level command output |
| Debug Watch | display value in graph debug |

## 18. Graph viewport separation

State graph workspace has its own viewport:

```text
graph_camera_x
graph_camera_y
graph_zoom
selected_node_ids
selected_edge_ids
```

This is separate from:

- canvas viewport,
- app shell docking layout,
- canvas object layout.

## 19. Debug output for state machine

Logic debug panel should show:

| Field | Meaning |
|---|---|
| last event | newest normalized event |
| resolved target | target resolver result |
| assignment | event/action/graph used |
| command output | commands emitted |
| validation result | accepted/rejected command |
| affected objects | object ids changed |
| dirty flags | visual/layout/export/validation |

## 20. When state machine enters roadmap

| Phase | State machine part |
|---|---|
| C | command layer exists |
| G | regions become reliable event targets |
| M | headless event/action registry |
| M | target resolver |
| M | simple event assignments |
| N | visual graph workspace |
| O | reusable graph presets and advanced reaction layer |

## 21. Acceptance rule

State machine feature is acceptable only when:

```text
it emits commands
it validates targets
it does not bypass Project model
it can be debugged
it does not dominate default MVP screen
it respects primitive/group/component roles
```
