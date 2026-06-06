# 07 — State machine events and actions registry catalog

Status: draft 00
Scope: event registry, action registry, logic spreadsheet, ordering, weights, conflict priority, tags

## 1. Purpose

This document defines the first canonical catalog of events, actions, conditions, flow operators and debug panels for `qubok_interface_core` state machine / logic layer.

The system must support two authoring levels:

```text
L1 — headless registry / simple event assignments
L2 — visual state graph workspace later
```

The state graph is not the first default MVP screen. It is a later Logic / Events workspace. However, the registries, event names, action names and metadata should be defined early so that buttons, sliders, panels and regions can already be described and debugged.

Core rule:

```text
state_machine reads Project model
state_machine resolves targets
state_machine emits commands
commands mutate Project model
views rerender from Project model
```

Forbidden:

```text
state_machine directly mutates DOM / canvas / SVG
```

## 2. Registry object model

Every event/action/condition/operator should have registry metadata.

Recommended fields:

```text
id
label
category
group
kind
tags
maturity
weight
priority
scope
source_domain
target_policy
parameter_schema
when_available
conflict_policy
command_output
history_policy
debug_visibility
short_description
long_description
```

Meaning:

| Field | Meaning |
|---|---|
| `id` | stable technical identifier, snake_case |
| `label` | user-facing name |
| `category` | event, action, condition, operator, value, debug, command |
| `group` | pointer, keyboard, region, layout, style, component, project, etc. |
| `kind` | input, output, mutation, query, branch, timer, state, utility |
| `tags` | searchable tags |
| `maturity` | spec, headless, debug, MVP, polished, advanced |
| `weight` | default ordering weight in menus and spreadsheet |
| `priority` | conflict/runtime resolution priority |
| `scope` | project, workspace, canvas, object, region, graph, shell |
| `source_domain` | where event/action is produced or used |
| `target_policy` | what can be affected |
| `parameter_schema` | required fields and value types |
| `when_available` | activation conditions |
| `conflict_policy` | what happens if similar items run together |
| `command_output` | emitted command type(s) |
| `history_policy` | undo/redo behavior |
| `debug_visibility` | hidden, compact, full, spreadsheet |
| `short_description` | one-line description |
| `long_description` | detailed documentation |

## 3. Naming rules

Technical identifiers use lowercase snake_case.

Examples:

```text
pointer_click
keyboard_key_down
region_enter
set_style
transform_update
value_compare
flow_if
command_emit
```

User-facing labels may use compact Title Case:

```text
Pointer Click
Keyboard Input
Region Enter
Set Style
Change Position
Compare
Delay
```

## 4. Major registry groups

| Group | Contains |
|---|---|
| Pointer events | move, click, drag, wheel, cursor position |
| Keyboard events | key down/up, shortcut, text input |
| Region events | enter, leave, focus, blur, drag over, drop |
| Selection events | selection change, active object change, box select |
| Transform events | transform start/move/end, resize, rotate |
| Value events | value change, value commit, parameter changed |
| Component events | open/close, dropdown expand, item selected |
| Panel/window events | dock, undock, resize, collapse, visibility |
| Project events | load, save, export, validation, dirty state |
| Timer events | delay, interval, frame tick, throttle, debounce |
| Conditions | if, compare, and, or, not, exists, has_tag |
| Actions | set style, set value, transform, select, open, toggle |
| Commands | object.patch, transform.update, selection.set, project.patch |
| Debug events/actions | trace, highlight, flash, log, inspect, watch |

## 5. Weight convention

Weights define menu ordering and spreadsheet sorting priority.

| Weight range | Meaning |
|---:|---|
| 0–99 | critical runtime/system events/actions |
| 100–199 | common user interactions |
| 200–299 | region and component events |
| 300–399 | transform/layout actions |
| 400–499 | style/value/text actions |
| 500–599 | project/library/export actions |
| 600–699 | timer/flow/condition utilities |
| 700–799 | debug/watch/trace tools |
| 800–899 | advanced/procedural/later |
| 900+ | experimental/hidden |

Default display order in action picker:

```text
recently_used
project_used
context_relevant
core_common
category_weight
alphabetical
```

## 6. Conflict and priority model

If two similar or conflicting actions are triggered at the same time, resolution uses priority layers.

Recommended priority order:

| Priority | Source | Example |
|---:|---|---|
| 1000 | critical validation block | locked object cannot transform |
| 900 | active modal/tool capture | text input captures keyboard |
| 800 | active drag/resize session | drag region owns pointer move |
| 700 | selected transform handle | resize handle beats body click |
| 600 | explicit region priority | high-priority region handles click |
| 500 | active state override | pressed state beats hover state |
| 400 | user-authored rule priority | dictated panel rule beats inherited |
| 300 | layer/render order | topmost visual wins |
| 200 | parent context | child interaction may bubble to parent |
| 100 | default behavior | fallback click/select |
| 0 | passive debug/watch | observe only |

Conflict policies:

| Policy | Meaning |
|---|---|
| `exclusive` | only highest priority action runs |
| `merge` | compatible actions combine into one command batch |
| `queue` | second action runs after first finishes |
| `bubble` | event moves to parent if child does not consume it |
| `cancel_previous` | newer event cancels previous runtime action |
| `ignore_if_active` | action ignored while same action is active |
| `debug_warn` | both blocked and warning is shown |

Examples:

```text
hover + mouse_down -> active/pressed wins over hover
text_input + shortcut -> focused text field captures key unless shortcut is global
resize_handle_drag + panel_drag -> resize region wins because it has higher explicit priority
locked_object + transform.update -> validation blocks transform command
```

## 7. Event registry — pointer events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `pointer_move` | Pointer Move | pointer | input | 100 | 100 | canvas/window pointer moves | pointer, cursor, hover | UI event |
| `pointer_down` | Pointer Down | pointer | input | 101 | 500 | pointer pressed | pointer, mouse, pen, touch | UI event |
| `pointer_up` | Pointer Up | pointer | input | 102 | 500 | pointer released | pointer, mouse, pen, touch | UI event |
| `pointer_click` | Pointer Click | pointer | input | 103 | 450 | down/up on same target | click, select, activate | UI event/action trigger |
| `pointer_double_click` | Pointer Double Click | pointer | input | 104 | 460 | second click within threshold | edit, enter, open | UI event |
| `pointer_context_click` | Context Click | pointer | input | 105 | 450 | right click / context gesture | menu, context | UI event |
| `pointer_wheel` | Pointer Wheel | pointer | input | 106 | 300 | wheel/trackpad scroll | scroll, zoom, value | UI event |
| `pointer_drag_start` | Drag Start | pointer | input | 110 | 800 | threshold exceeded after down | drag, transform | UI event |
| `pointer_drag_move` | Drag Move | pointer | input | 111 | 800 | active drag session | drag, transform, continuous | UI event |
| `pointer_drag_end` | Drag End | pointer | input | 112 | 800 | drag released/cancelled | drag, commit | UI event |
| `pointer_hover_start` | Hover Start | pointer | input | 120 | 200 | pointer enters hit area | hover, state | state input |
| `pointer_hover_end` | Hover End | pointer | input | 121 | 200 | pointer leaves hit area | hover, state | state input |
| `cursor_position_changed` | Cursor Position Changed | pointer | value | 130 | 100 | pointer move | cursor, coordinate, value | value output |

Key payload fields:

```text
screen_pos
world_pos
local_pos optional
pointer_id
button
buttons
modifiers
delta_screen
delta_world
pressure optional
timestamp
```

## 8. Event registry — keyboard and text events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `keyboard_key_down` | Key Down | keyboard | input | 150 | 600 | key pressed | keyboard, shortcut | UI event |
| `keyboard_key_up` | Key Up | keyboard | input | 151 | 600 | key released | keyboard | UI event |
| `keyboard_shortcut` | Shortcut | keyboard | input | 152 | 700 | registered key chord | shortcut, command | command/action |
| `keyboard_text_input` | Text Input | keyboard | input | 153 | 900 | text field focused | text, typing | text edit event |
| `keyboard_escape` | Escape | keyboard | input | 154 | 800 | Esc key | cancel, close, modal | cancel event |
| `keyboard_enter` | Enter | keyboard | input | 155 | 800 | Enter key | commit, confirm | commit event |
| `keyboard_tab` | Tab Focus | keyboard | input | 156 | 650 | Tab key | focus, navigation | focus event |

Conflict rule:

```text
focused text input captures keyboard_text_input
registered global shortcut may override only if global_scope = true
```

## 9. Event registry — region events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `region_enter` | Region Enter | region | input | 200 | 300 | pointer enters region | region, hover | state input |
| `region_leave` | Region Leave | region | input | 201 | 300 | pointer leaves region | region, hover | state input |
| `region_hit` | Region Hit | region | input | 202 | 500 | pointer down/click resolves region | hit, click | event target |
| `region_focus` | Region Focus | region | input | 203 | 600 | focusable region activated | focus, keyboard | state input |
| `region_blur` | Region Blur | region | input | 204 | 600 | focus leaves region | focus | state input |
| `region_drag_enter` | Drag Enter Region | region | input | 205 | 700 | dragged payload enters region | drag, drop | drop preview |
| `region_drag_leave` | Drag Leave Region | region | input | 206 | 700 | dragged payload leaves region | drag, drop | drop preview |
| `region_drag_over` | Drag Over Region | region | input | 207 | 700 | payload moves over drop region | drag, drop | drop preview |
| `region_drop` | Drop On Region | region | input | 208 | 750 | payload released over drop region | drop, insert | command/action |
| `region_resize_start` | Resize Start | region | input | 209 | 850 | resize region grabbed | resize | transform session |
| `region_resize_move` | Resize Move | region | input | 210 | 850 | active resize session | resize | transform preview/command |
| `region_resize_end` | Resize End | region | input | 211 | 850 | resize committed | resize | command |
| `region_scroll` | Region Scroll | region | input | 212 | 650 | scroll region wheel/drag | scroll | command/value |
| `region_snap_candidate` | Snap Candidate | region | query | 213 | 300 | snap engine queries region | snap, layout | value output |

Region event payload:

```text
region_id
region_type
object_id
linked_visual_id
priority
local_pos
world_pos
cursor_type
accepted_payload optional
```

## 10. Event registry — selection and focus events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `selection_changed` | Selection Changed | selection | state | 230 | 400 | selected IDs changed | selection, inspector, hierarchy | update event |
| `active_object_changed` | Active Object Changed | selection | state | 231 | 400 | active_id changed | active, inspector | update event |
| `selection_box_start` | Box Select Start | selection | input | 232 | 700 | box select begins | select, marquee | selection session |
| `selection_box_move` | Box Select Move | selection | input | 233 | 700 | box select moves | select, continuous | preview |
| `selection_box_end` | Box Select End | selection | input | 234 | 700 | box select commits | select, commit | selection.set |
| `focus_changed` | Focus Changed | focus | state | 235 | 600 | active focus target changes | focus, keyboard | update event |

## 11. Event registry — transform/layout events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `transform_start` | Transform Start | transform | session | 300 | 800 | move/resize/rotate starts | transform | session event |
| `transform_move` | Transform Move | transform | session | 301 | 800 | object moves during session | move, continuous | preview/command |
| `transform_resize` | Transform Resize | transform | session | 302 | 850 | resize during session | resize | preview/command |
| `transform_rotate` | Transform Rotate | transform | session | 303 | 820 | rotate during session | rotate | preview/command |
| `transform_commit` | Transform Commit | transform | session | 304 | 800 | transform accepted | commit | transform.update |
| `transform_cancel` | Transform Cancel | transform | session | 305 | 900 | transform cancelled | cancel | revert preview |
| `layout_rule_applied` | Layout Rule Applied | layout | state | 320 | 400 | panel/layout rule updates children | layout, rule | update event |
| `snap_applied` | Snap Applied | layout | state | 321 | 450 | snap correction applied | snap | transform metadata |
| `guide_candidate_found` | Guide Candidate Found | layout | query | 322 | 250 | guide system finds candidate | guide, snap | debug/value |

## 12. Event registry — component, panel, dropdown and window events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `component_instantiated` | Component Instantiated | component | state | 400 | 300 | component dropped/created | component, library | update event |
| `component_saved` | Component Saved | component | state | 401 | 300 | selected group saved | component, library | update event |
| `component_variant_changed` | Component Variant Changed | component | state | 402 | 350 | theme/variant changed | preset, variant | update event |
| `drop_down_opened` | Dropdown Opened | component | state | 410 | 650 | dropdown changes to open | dropdown, open | state event |
| `drop_down_closed` | Dropdown Closed | component | state | 411 | 650 | dropdown closes | dropdown, close | state event |
| `drop_down_extended` | Dropdown Extended | component | state | 412 | 660 | extended content visible | dropdown, panel | state event |
| `drop_down_item_selected` | Dropdown Item Selected | component | input | 413 | 700 | item chosen | dropdown, select, value | value/action |
| `panel_opened` | Panel Opened | panel | state | 420 | 500 | panel becomes visible | panel, visibility | state event |
| `panel_closed` | Panel Closed | panel | state | 421 | 500 | panel hidden/closed | panel, visibility | state event |
| `panel_collapsed` | Panel Collapsed | panel | state | 422 | 500 | content collapsed | panel, layout | layout event |
| `panel_expanded` | Panel Expanded | panel | state | 423 | 500 | content expanded | panel, layout | layout event |
| `window_docked` | Window Docked | shell | state | 430 | 500 | workspace panel docked | shell, docking | workspace command |
| `window_floating` | Window Floating | shell | state | 431 | 500 | workspace panel undocked/floats | shell, docking | workspace command |
| `workspace_panel_resized` | Workspace Panel Resized | shell | state | 432 | 600 | dock/shell panel resized | shell, resize | workspace command |

Note:

```text
window_docked / window_floating apply to app shell workspace panels, not canvas object panels.
```

## 13. Event registry — project, validation, library and export events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `project_loaded` | Project Loaded | project | state | 500 | 400 | project JSON loaded | project, load | update event |
| `project_saved` | Project Saved | project | state | 501 | 400 | save succeeds | project, save | update event |
| `project_dirty_changed` | Dirty State Changed | project | state | 502 | 400 | dirty flags change | project, dirty | status event |
| `project_validated` | Project Validated | validation | state | 510 | 500 | validation run completes | validation | validation event |
| `validation_error_added` | Validation Error Added | validation | state | 511 | 600 | new error appears | error, validation | debug event |
| `validation_warning_added` | Validation Warning Added | validation | state | 512 | 550 | new warning appears | warning, validation | debug event |
| `library_asset_added` | Library Asset Added | library | state | 520 | 350 | asset created/imported | library, component | update event |
| `library_asset_used` | Library Asset Used | library | state | 521 | 350 | asset instantiated | library, usage | usage counter |
| `export_started` | Export Started | export | state | 530 | 450 | export begins | export | status event |
| `export_completed` | Export Completed | export | state | 531 | 450 | export succeeds | export | status event |
| `export_failed` | Export Failed | export | state | 532 | 650 | export fails | export, error | error event |

## 14. Event registry — time and flow trigger events

| ID | Label | Group | Kind | Weight | Priority | When available | Tags | Output |
|---|---|---|---|---:|---:|---|---|---|
| `delay_elapsed` | Delay Elapsed | timer | flow | 600 | 200 | delay timer completes | delay, time | trigger |
| `interval_tick` | Interval Tick | timer | flow | 601 | 200 | repeating timer interval | timer, loop | trigger |
| `frame_tick` | Frame Tick | timer | flow | 602 | 100 | animation/update frame | frame, runtime | trigger |
| `debounce_elapsed` | Debounce Elapsed | timer | flow | 603 | 250 | no new event within delay | debounce | trigger |
| `throttle_tick` | Throttle Tick | timer | flow | 604 | 250 | throttled interval allows event | throttle | trigger |

Timer parameters:

```text
duration_ms
repeat_count optional
repeat_mode once/repeat/while_true
cancel_on_event optional
scope graph/object/project
```

Example:

```text
pointer_enter -> delay 2000ms -> if still hover -> show tooltip
```

## 15. Condition and flow operators

| ID | Label | Group | Kind | Weight | Tags | Description |
|---|---|---|---|---:|---|---|
| `flow_if` | If | condition | branch | 620 | if, branch | run branch when condition is true |
| `flow_else` | Else | condition | branch | 621 | else, branch | fallback branch |
| `flow_then` | Then | flow | sequence | 622 | then, sequence | execute next action |
| `logic_and` | And | logic | boolean | 623 | and, boolean | true if all inputs true |
| `logic_or` | Or | logic | boolean | 624 | or, boolean | true if any input true |
| `logic_not` | Not | logic | boolean | 625 | not, boolean | invert boolean |
| `logic_compare` | Compare | logic | compare | 626 | compare, value | compare numbers/strings/enums |
| `logic_switch` | Switch / Match | logic | branch | 627 | switch, enum | choose branch by value |
| `logic_exists` | Exists | logic | query | 628 | exists, null | checks if target/value exists |
| `logic_has_tag` | Has Tag | logic | query | 629 | tag, query | checks if object has tag |
| `logic_in_range` | In Range | logic | compare | 630 | range, value | min <= value <= max |
| `logic_changed` | Changed | logic | compare | 631 | diff, changed | true if value changed since last event |

Compare operators:

```text
==
!=
>
>=
<
<=
contains
starts_with
ends_with
in
not_in
approx_equal
```

## 16. Value and read operators

| ID | Label | Group | Kind | Weight | Tags | Output |
|---|---|---|---|---:|---|---|
| `read_property` | Read Property | value | query | 640 | property, read | value |
| `read_style` | Read Style | value | query | 641 | style, read | style value |
| `read_transform` | Read Transform | value | query | 642 | transform, read | transform value |
| `read_bbox` | Read BBox | value | query | 643 | bbox, read | visual/layout/interaction bbox |
| `read_region` | Read Region | value | query | 644 | region, read | region field |
| `read_selection` | Read Selection | value | query | 645 | selection, read | selected IDs |
| `read_pointer_position` | Read Cursor Position | value | query | 646 | cursor, position | screen/world/local pos |
| `read_screen_px_size` | Read Screen Pixel Size | value | query | 647 | screen, px, viewport | px scale at zoom |
| `read_viewport` | Read Viewport | value | query | 648 | viewport, zoom | camera/zoom data |
| `read_theme_token` | Read Theme Token | value | query | 649 | theme, token | resolved token value |
| `read_usage_count` | Read Usage Count | value | query | 650 | used_times, referred_times | count |
| `read_parameter_source` | Read Parameter Source | value | query | 651 | internal, inherited, dictated | source info |

Coordinate outputs should distinguish:

```text
screen_pos_px
world_pos
object_local_pos
region_local_pos
normalized_region_pos 0..1
```

## 17. Action registry — style and visual actions

| ID | Label | Group | Kind | Weight | Priority | Tags | Command output |
|---|---|---|---|---:|---:|---|---|
| `set_style` | Set Style | style | mutation | 400 | 500 | style, patch | `object.patch` |
| `change_style` | Change Style | style | mutation | 401 | 500 | style, delta | `object.patch` |
| `set_color_main` | Set Main Color | style | mutation | 402 | 500 | color, fill | `object.patch style.color_main` |
| `set_color_stroke` | Set Stroke Color | style | mutation | 403 | 500 | stroke, color | `object.patch style.color_stroke` |
| `set_opacity_main` | Set Main Opacity | style | mutation | 404 | 500 | opacity | `object.patch style.opacity_main` |
| `set_opacity_stroke` | Set Stroke Opacity | style | mutation | 405 | 500 | opacity, stroke | `object.patch style.opacity_stroke` |
| `set_stroke_width` | Set Stroke Width | style | mutation | 406 | 500 | stroke, width | `object.patch style.stroke_width` |
| `set_stroke_type` | Set Stroke Type | style | mutation | 407 | 500 | stroke, dash | `object.patch style.stroke_type` |
| `set_stroke_mode` | Set Stroke Mode | style | mutation | 408 | 500 | inside, outside, middle | `object.patch style.stroke_mode` |
| `set_corner_radius` | Set Corner Radius | style | mutation | 409 | 500 | radius, corner | `object.patch geometry.corner_radius` |
| `set_blend_mode` | Set Blend Mode | style | mutation | 410 | 450 | blend, layer | `object.patch style.blend_mode` |
| `set_theme_variation` | Set Theme Variation | style | mutation | 411 | 450 | theme, variant, preset | `component.set_variant` or `object.patch` |
| `apply_style_preset` | Apply Style Preset | style | mutation | 412 | 450 | preset, theme | command batch |

Theme variation rule:

```text
changing only parameters of existing objects = style/theme/preset variation
changing number or structure of child objects = new group/component/panel variant
```

## 18. Action registry — transform, layout and geometry actions

| ID | Label | Group | Kind | Weight | Priority | Tags | Command output |
|---|---|---|---|---:|---:|---|---|
| `set_position` | Set Position | transform | mutation | 300 | 700 | pos, move | `transform.update` |
| `change_position` | Change Position | transform | mutation | 301 | 700 | delta, move | `transform.update` |
| `set_size` | Set Size | transform | mutation | 302 | 700 | size, resize | `transform.update` |
| `change_size` | Change Size | transform | mutation | 303 | 700 | resize, delta | `transform.update` |
| `set_rotation` | Set Rotation | transform | mutation | 304 | 650 | rotation | `transform.update` |
| `set_pivot` | Set Pivot | transform | mutation | 305 | 650 | pivot | `transform.update` |
| `lock_axis` | Lock Axis | constraint | rule | 306 | 800 | constraint, lock | rule state / transform metadata |
| `clamp_to_bbox` | Clamp To BBox | constraint | rule | 307 | 850 | constraint, bbox | transform constraint |
| `align_objects` | Align Objects | layout | mutation | 320 | 600 | align | `layout.align` |
| `distribute_objects` | Distribute Objects | layout | mutation | 321 | 600 | distribute | `layout.distribute` |
| `set_margin` | Set Margin | layout | mutation | 322 | 500 | margin | `object.patch layout.margin` |
| `set_padding` | Set Padding | layout | mutation | 323 | 500 | padding | `object.patch layout.padding` |
| `set_gap` | Set Gap | layout | mutation | 324 | 500 | gap | `object.patch layout.gap` |
| `set_alignment` | Set Alignment | layout | mutation | 325 | 500 | alignment | `object.patch layout.alignment` |
| `set_size_policy` | Set Size Policy | layout | mutation | 326 | 500 | fit, fill, fixed | `object.patch layout.size_policy` |

Slider constraint example:

```text
region: Slider_Handle_Drag_Region
target: Slider_Handle_Group
action: set_position
constraint: clamp_to_bbox(track_interaction_bbox), lock_axis(y)
value_output: normalized_x 0..1
commands: transform.update + object.patch value/text if bound
```

## 19. Action registry — text, value and data actions

| ID | Label | Group | Kind | Weight | Priority | Tags | Command output |
|---|---|---|---|---:|---:|---|---|
| `set_text` | Set Text | text | mutation | 430 | 500 | text, string | `object.patch text_data.string` |
| `set_font` | Set Font | text | mutation | 431 | 400 | font | `object.patch text_data.font_family` |
| `set_text_alignment` | Set Text Alignment | text | mutation | 432 | 400 | alignment | `object.patch text_data.align` |
| `set_value` | Set Value | value | mutation | 440 | 500 | value | `object.patch parameter_data.value` |
| `map_range` | Map Range | value | compute | 441 | 300 | map, normalize | computed value |
| `normalize_value` | Normalize Value | value | compute | 442 | 300 | normalize, 0-1 | computed value |
| `clamp_value` | Clamp Value | value | compute | 443 | 600 | clamp, min, max | computed value or validation |
| `format_value` | Format Value | value | compute | 444 | 300 | format, text | computed string |
| `set_parameter_source` | Set Parameter Source | value | mutation | 445 | 600 | internal, inherited, dictated | `parameter.set_source` |
| `link_parameter` | Link Parameter | value | mutation | 446 | 550 | binding, reference | `parameter.link` |
| `unlink_parameter` | Unlink Parameter | value | mutation | 447 | 550 | binding, unlink | `parameter.unlink` |

## 20. Action registry — object, selection, group and component actions

| ID | Label | Group | Kind | Weight | Priority | Tags | Command output |
|---|---|---|---|---:|---:|---|---|
| `create_object` | Create Object | object | mutation | 500 | 500 | create, primitive | `object.create` |
| `delete_object` | Delete Object | object | mutation | 501 | 800 | delete | `object.delete` |
| `duplicate_object` | Duplicate Object | object | mutation | 502 | 500 | duplicate | `object.duplicate` |
| `patch_object` | Patch Object | object | mutation | 503 | 500 | patch | `object.patch` |
| `set_visibility` | Set Visibility | object | mutation | 504 | 600 | visible, hidden | `object.patch visible` |
| `set_locked` | Set Locked | object | mutation | 505 | 700 | lock | `object.patch locked` |
| `bring_to_front` | Bring To Front | hierarchy | mutation | 506 | 500 | layer, top | `hierarchy.reorder` |
| `send_to_back` | Send To Back | hierarchy | mutation | 507 | 500 | layer, bottom | `hierarchy.reorder` |
| `select_object` | Select Object | selection | mutation | 510 | 600 | select | `selection.set` |
| `select_add` | Add To Selection | selection | mutation | 511 | 600 | selection | `selection.add` |
| `select_remove` | Remove From Selection | selection | mutation | 512 | 600 | selection | `selection.remove` |
| `clear_selection` | Clear Selection | selection | mutation | 513 | 600 | selection | `selection.clear` |
| `group_create` | Create Group | group | mutation | 520 | 500 | group | `group.create` |
| `group_ungroup` | Ungroup | group | mutation | 521 | 500 | group | `group.ungroup` |
| `component_save` | Save Component | component | mutation | 530 | 500 | library, asset | `component.save` |
| `component_instantiate` | Instantiate Component | component | mutation | 531 | 500 | library, dragdrop | `component.instantiate` |
| `component_apply_variant` | Apply Component Variant | component | mutation | 532 | 450 | variant, theme | `component.apply_variant` |

## 21. Action registry — panel, dropdown, window and workspace actions

| ID | Label | Group | Kind | Weight | Priority | Tags | Command output |
|---|---|---|---|---:|---:|---|---|
| `open_panel` | Open Panel | panel | mutation | 540 | 500 | panel, visible | `object.patch state/open` |
| `close_panel` | Close Panel | panel | mutation | 541 | 500 | panel, visible | `object.patch state/closed` |
| `toggle_panel` | Toggle Panel | panel | mutation | 542 | 500 | panel, toggle | `object.patch state` |
| `collapse_panel` | Collapse Panel | panel | mutation | 543 | 500 | panel, collapsed | layout/state command |
| `expand_panel` | Expand Panel | panel | mutation | 544 | 500 | panel, expanded | layout/state command |
| `open_dropdown` | Open Dropdown | dropdown | mutation | 550 | 650 | dropdown, open | `component.set_state open` |
| `close_dropdown` | Close Dropdown | dropdown | mutation | 551 | 650 | dropdown, close | `component.set_state closed` |
| `toggle_dropdown` | Toggle Dropdown | dropdown | mutation | 552 | 650 | dropdown, toggle | `component.toggle_state` |
| `select_dropdown_item` | Select Dropdown Item | dropdown | mutation | 553 | 700 | dropdown, select | value + state command batch |
| `set_workspace_panel_docked` | Dock Workspace Panel | shell | mutation | 560 | 600 | dock, shell | `workspaceLayout.dock` |
| `set_workspace_panel_floating` | Float Workspace Panel | shell | mutation | 561 | 600 | floating, shell | `workspaceLayout.float` |
| `split_workspace_area` | Split Workspace Area | shell | mutation | 562 | 600 | split, shell | `workspaceLayout.split` |
| `merge_workspace_area` | Merge Workspace Area | shell | mutation | 563 | 600 | merge, shell | `workspaceLayout.merge` |

## 22. Action registry — debug, trace and diagnostics

| ID | Label | Group | Kind | Weight | Priority | Tags | Command output |
|---|---|---|---|---:|---:|---|---|
| `debug_log` | Debug Log | debug | observe | 700 | 0 | log, trace | debug output |
| `debug_watch_value` | Watch Value | debug | observe | 701 | 0 | watch, spreadsheet | debug output |
| `debug_highlight_object` | Highlight Object | debug | overlay | 702 | 100 | highlight, overlay | runtime overlay |
| `debug_flash_region` | Flash Region | debug | overlay | 703 | 100 | region, validation | runtime overlay |
| `debug_pin_inspector` | Pin Debug Inspector | debug | UI | 704 | 100 | inspect, pin | debug UI state |
| `debug_trace_event` | Trace Event | debug | observe | 705 | 0 | event, trace | spreadsheet row |
| `debug_trace_command` | Trace Command | debug | observe | 706 | 0 | command, trace | spreadsheet row |
| `debug_validate_project` | Validate Project | debug | command | 707 | 500 | validation | `project.validate` |
| `debug_set_overlay_mode` | Set Overlay Mode | debug | mutation | 708 | 200 | overlay, debug | workspace debug state |

Debug actions should not permanently change visible object style unless explicitly routed to a style command.

## 23. Target resolver policies

| ID | Meaning | Example use |
|---|---|---|
| `event_target` | object resolved from event/hit-test | click button body |
| `event_region` | region resolved from event | click region itself |
| `active_object` | current active object in selection | inspector action |
| `first_selected` | first selected object | align reference |
| `all_selected` | all selected objects | batch style change |
| `parent_of_target` | parent group/panel of event target | child click affects parent button |
| `children_of_target` | all children of target | panel batch edit |
| `object_by_id` | explicit object | fixed binding |
| `objects_by_type` | query by type | all sliders |
| `objects_with_tag` | query by tag | all role=button |
| `objects_by_query` | complex query | all sliders inside selected panel |
| `nearest_snap_candidate` | snap engine result | custom snap rule |
| `library_asset_by_id` | library asset | instantiate asset |
| `state_graph_variable` | variable value | graph-driven behavior |

Target resolver should return:

```text
target_ids
target_type
resolution_path
confidence
warnings optional
```

## 24. Action picker / node menu panel

The action/event picker should work like a compact command palette / node add menu.

Display groups:

```text
Recent
Used in Project
Context: Selected Object
Events
Actions
Conditions
Values
Commands
Debug
Advanced
```

Each row should show:

```text
icon
label
id
category/group
tags
maturity badge
usage count
last used
compatibility status
```

Compatibility status examples:

| Status | Meaning |
|---|---|
| `available` | can be used in current context |
| `needs_region` | requires region target |
| `needs_selection` | requires active/selected object |
| `needs_value` | requires value input |
| `advanced_hidden` | hidden unless advanced mode enabled |
| `blocked` | unavailable because of lock/validation/maturity |

Sort order:

```text
1. context compatible
2. recently used
3. already used in project
4. common core actions
5. by weight
6. alphabetical
```

## 25. Logic spreadsheet / project action audit panel

Purpose:

```text
show what actions/events exist in current project/window
show which were used
show order and runtime traces
show conflicts and priority decisions
```

Recommended columns:

| Column | Meaning |
|---|---|
| `enabled` | active/inactive toggle |
| `last_status` | idle/running/success/warning/error/blocked |
| `last_run_at` | timestamp |
| `run_count` | how many times triggered |
| `event_id` | event source |
| `event_source` | object/region/project/workspace |
| `target_policy` | resolver policy |
| `resolved_target` | last target result |
| `action_id` | action used |
| `command_output` | command(s) emitted |
| `priority` | runtime priority |
| `weight` | menu/list weight |
| `conflict_policy` | merge/exclusive/bubble/etc. |
| `tags` | searchable tags |
| `maturity` | registry maturity |
| `scope` | object/project/workspace/etc. |
| `source_asset` | optional library/source preset |
| `notes` | debug notes/warnings |

Recommended filters:

```text
show: all / used / unused / recently triggered / errors / warnings / blocked
scope: project / current workspace / selected object / hovered object / active panel
event group: pointer / keyboard / region / component / project / timer
action group: style / transform / value / layout / debug / component
maturity: MVP / debug / advanced / experimental
source: built-in / library asset / user-created / graph
```

Highlighting:

| Highlight | Meaning |
|---|---|
| blue row | recently run |
| green row | success/valid |
| yellow row | warning/conflict resolved |
| red row | error/blocked |
| purple row | graph-owned action |
| gray row | inactive/unused |

## 26. Example assignment schemas

### Button hover

```json
{
  "id": "assignment_button_hover_style",
  "event_id": "region_enter",
  "source_region_id": "ButtonPrimary_Click_Region",
  "target_policy": "object_by_id",
  "target_object_id": "ButtonPrimary_Body_Rectangle",
  "action_id": "set_style",
  "parameters": {
    "style.color_main": "theme.button.hover"
  },
  "priority": 500,
  "conflict_policy": "exclusive",
  "enabled": true
}
```

### Slider drag value mapping

```json
{
  "id": "assignment_slider_drag_value",
  "event_id": "pointer_drag_move",
  "source_region_id": "Slider_Handle_Drag_Region",
  "target_policy": "object_by_id",
  "target_object_id": "Slider_Handle_Group",
  "actions": [
    { "action_id": "clamp_to_bbox", "parameters": { "bbox_source": "Slider_Track_Interaction_BBox", "axis": "x" } },
    { "action_id": "set_position", "parameters": { "x": "event.local_x", "y": "locked" } },
    { "action_id": "map_range", "parameters": { "input": "event.local_x", "from": [0, "track.width"], "to": [0, 1] } },
    { "action_id": "set_text", "target": "Slider_Value_Text", "parameters": { "string": "format(value, 0.00)" } }
  ],
  "priority": 800,
  "conflict_policy": "exclusive",
  "enabled": true
}
```

### Dropdown open/close

```json
{
  "id": "assignment_dropdown_toggle",
  "event_id": "pointer_click",
  "source_region_id": "DropDown_Header_Click_Region",
  "target_policy": "parent_of_target",
  "action_id": "toggle_dropdown",
  "parameters": {
    "state_closed": "closed",
    "state_open": "open"
  },
  "priority": 650,
  "conflict_policy": "exclusive",
  "enabled": true
}
```

## 27. Default MVP / later split

| Registry part | MVP status | Notes |
|---|---|---|
| pointer_click / hover / drag basic | MVP debug/headless | needed for selection and regions |
| keyboard shortcuts | MVP | basic global shortcuts |
| region_enter/leave/hit/drop/resize | MVP debug/headless | needed for interaction_region |
| object.patch / transform.update / selection.set | MVP | core command layer |
| set_style / set_value / set_position | MVP minimal | action wrappers around commands |
| if / compare / and / or | post-MVP headless | can exist before graph UI |
| delay / timer / debounce | post-MVP | useful for tooltip/reactions |
| full visual state graph | later workspace | not default MVP |
| logic spreadsheet | debug/workspace | useful before full graph |
| advanced procedural actions | later | feature-flagged |

## 28. Acceptance rule

The event/action registry is valid when:

```text
all events/actions have id, category, group, tags, weight, priority, maturity and description
event -> target resolver -> action -> command is traceable
actions never bypass Project model
conflicts have deterministic policy
logic spreadsheet can show used/unused/recent/error rows
node/action picker can filter by context, tags, usage and compatibility
```
