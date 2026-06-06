# 03 — Interface element groups, types and tags

Status: draft 00
Scope: UI element classification, groups, roles, tags, examples

## 1. Purpose

This document defines how interface elements should be grouped, typed and tagged in `qubok_interface_core`.

The goal is to prevent ambiguous words like `box`, `window`, `panel`, `node`, `thing`, `button` from meaning different objects in different layers.

Main principle:

```text
classification = type + role + domain + maturity + behavior + export status
```

A useful element description should include:

```text
technical type
semantic role
parent/group
interaction role
layout role
export behavior
roadmap phase
state_machine targetability
```

## 2. Main element groups

| Group | Meaning | Examples |
|---|---|---|
| Primitive Object | minimal editable object | rectangle, text_rectangle, line, region_rectangle |
| Structural Helper | non-final helper | empty, empty_rectangle, guide, anchor |
| Interaction Region | invisible reaction zone | hit region, drag region, resize region |
| Layout Object | object participating in layout | section box, content box, grid cell |
| Visual Element | rendered object | rectangle, text, line, icon shape |
| Control Group | UI control made from primitives | button, slider, checkbox, input |
| Panel Part | known part of panel | frame, header, content, footer, resize handle |
| Component Asset | reusable saved group/panel | metric card, button, panel monitor |
| Workspace UI | editor shell element | top bar, left tool panel, right sidebar |
| Debug Element | runtime diagnostic overlay | bbox overlay, hover HUD, validation marker |
| Logic Element | event/state graph object | event node, action node, condition node |

## 3. Object type categories

### 3.1 Structural

| Type | Use |
|---|---|
| `empty` | anchor/pivot/marker |
| `empty_rectangle` | layout placeholder or draft box |
| `group` | semantic container |
| `panel` | structured group with panel roles |

### 3.2 Visual

| Type | Use |
|---|---|
| `rectangle` | visible shape/body/background/fill |
| `text_rectangle` | text bounded by rectangle |
| `line` | divider/connector/guide/icon stroke |
| `path` | later editable vector shape |
| `curve` | later curved line/path |

### 3.3 Interaction

| Type | Use |
|---|---|
| `region_rectangle` | hit/hover/drag/drop/resize/scroll/snap/content region |
| `transform_handle` | runtime handle, not saved as normal object by default |
| `selection_overlay` | runtime overlay, not project object |

### 3.4 Component/control

| Type | Use |
|---|---|
| `button_group` | rectangle + text + region |
| `slider_group` | track + fill + handle + value + regions |
| `input_group` | background + text + focus region |
| `toolbar_group` | row/column of controls |
| `panel_component` | structured panel asset |

In implementation these may still be `group` or `panel` objects with `group_data.group_kind` or `component_data.component_kind`.

## 4. Semantic roles

Semantic role describes what an element means inside its parent.

| Role | Common type | Example name |
|---|---|---|
| frame | rectangle | `PanelMonitor_Frame_Rectangle` |
| header | group | `PanelMonitor_Header_Group` |
| title | text_rectangle | `PanelMonitor_Title_Text` |
| body | group/rectangle | `Card_Body_Group` |
| content | group/region | `PanelMonitor_Content_Group` |
| footer | group | `PanelMonitor_Footer_Group` |
| label | text_rectangle | `RowCPU_Label_Text` |
| value | text_rectangle | `RowCPU_Value_Text` |
| icon | group/line/rectangle | `IconGauge_Group` |
| track | rectangle | `Slider_Track_Rectangle` |
| fill | rectangle | `Slider_Fill_Rectangle` |
| handle | rectangle/group | `Slider_Handle_Group` |
| divider | line | `Inspector_Divider_Line` |
| hit | region_rectangle | `Button_Click_Region` |
| drag | region_rectangle | `Panel_Header_Drag_Region` |
| resize | region_rectangle | `Panel_RightResize_Region` |
| drop | region_rectangle | `ComponentShelf_Drop_Region` |

## 5. Domain tags

Domain tag says which subsystem primarily uses the element.

| Domain tag | Meaning |
|---|---|
| `domain_canvas_object` | object on interface design canvas |
| `domain_workspace_shell` | part of editor shell |
| `domain_component_library` | asset/library object |
| `domain_state_graph` | graph/editor logic object |
| `domain_debug_overlay` | diagnostic only |
| `domain_export` | export-related object |
| `domain_layout` | layout/snap/arranger object |
| `domain_region` | interaction region object |
| `domain_preference` | preference/default token object |

## 6. Role tags

Role tags use `role_` prefix.

| Tag | Meaning |
|---|---|
| `role_frame` | outer visual frame |
| `role_header` | top panel/header area |
| `role_content` | main content area |
| `role_footer` | bottom/footer area |
| `role_label` | descriptive text |
| `role_value` | value/readout text |
| `role_icon` | icon or icon group |
| `role_button_body` | visible button body |
| `role_track` | slider/progress track |
| `role_fill` | fill/progress element |
| `role_handle` | draggable handle |
| `role_hit_region` | click/hit zone |
| `role_drag_region` | drag zone |
| `role_resize_region` | resize zone |
| `role_drop_region` | drop zone |
| `role_scroll_region` | scroll zone |
| `role_snap_target` | snap candidate/target |

## 7. Behavior tags

Behavior tags say what an object can do.

| Tag | Meaning |
|---|---|
| `can_select` | selectable on canvas/hierarchy |
| `can_transform` | can move/resize/rotate |
| `can_drag` | acts as drag source/handle |
| `can_resize` | acts as resize region/handle |
| `can_drop` | accepts dropped objects/components |
| `can_scroll` | scrollable region |
| `can_snap` | snap target or snap participant |
| `can_export_svg` | visible in SVG export |
| `can_export_json` | stored in JSON |
| `can_bind_event` | can have event bindings |
| `can_be_state_target` | state_machine can target it |

## 8. Maturity tags

| Tag | Meaning |
|---|---|
| `maturity_spec_only` | documentation only |
| `maturity_headless_core` | core model/helper exists, no full UI |
| `maturity_debug_view` | visible in debug/workbench |
| `maturity_mvp_workflow` | usable in main MVP workflow |
| `maturity_polished_tool` | refined, documented, stable |
| `maturity_advanced_composable` | reusable as graph/component/plugin/tool |
| `maturity_experimental_hidden` | hidden experiment |

## 9. Export tags

| Tag | Meaning |
|---|---|
| `export_project_json` | saved in full project JSON |
| `export_component_json` | saved in component asset JSON |
| `export_svg` | visible in SVG export |
| `export_svg_optional` | only if explicitly enabled |
| `runtime_only` | never saved as normal project object |
| `debug_only` | saved only in debug report or not saved |
| `no_svg_export` | not part of visual SVG output |

## 10. State machine tags

| Tag | Meaning |
|---|---|
| `state_target_style` | graph may change style fields |
| `state_target_transform` | graph may change position/size/rotation |
| `state_target_text` | graph may change text content |
| `state_target_visibility` | graph may hide/show |
| `state_target_region` | graph may enable/disable region or bind behavior |
| `state_target_component_parameter` | graph may set exposed parameter |
| `state_emits_command_only` | graph action must use command layer |

## 11. Element classification examples

### Button primary

| Field | Value |
|---|---|
| object name | `ButtonPrimary_Group` |
| type | `group` |
| group kind | `button_group` |
| tags | `domain_canvas_object`, `role_button`, `can_select`, `can_transform`, `can_bind_event`, `maturity_mvp_workflow` |
| children | body rectangle, label text, click region |
| state_machine | can target group or click region |
| export | project JSON, component JSON, SVG children |

### Button body

| Field | Value |
|---|---|
| object name | `ButtonPrimary_Body_Rectangle` |
| type | `rectangle` |
| tags | `role_button_body`, `can_export_svg`, `state_target_style` |
| state_machine | may set fill/stroke/opacity through command |

### Click region

| Field | Value |
|---|---|
| object name | `ButtonPrimary_Click_Region` |
| type | `region_rectangle` |
| tags | `role_hit_region`, `can_bind_event`, `state_target_region`, `no_svg_export` |
| state_machine | event source for click/hover |

## 12. Panel monitor sample classification

```text
PanelMonitor_Group
  type: panel/group
  tags: domain_canvas_object, role_panel, can_select, can_transform, can_export_json

PanelMonitor_Frame_Rectangle
  type: rectangle
  tags: role_frame, export_svg, state_target_style

PanelMonitor_Header_Group
  type: group
  tags: role_header, can_select, can_transform

PanelMonitor_Header_Drag_Region
  type: region_rectangle
  tags: role_drag_region, can_bind_event, no_svg_export

PanelMonitor_Stats_Group
  type: group
  tags: role_content

RowCPU_Group
  type: group
  tags: role_row, can_select

RowCPU_Bar_Fill_Rectangle
  type: rectangle
  tags: role_fill, state_target_style, state_target_transform, export_svg

RowCPU_Bar_Hit_Region
  type: region_rectangle
  tags: role_hit_region, can_bind_event, no_svg_export
```

## 13. Workspace shell classification

Workspace shell elements are not the same as canvas objects.

| Shell element | Type | Stored where | Meaning |
|---|---|---|---|
| Top app bar | `workspace_panel` | app/workspace state | global actions |
| Left tool panel | `workspace_panel` | app/workspace state | tools and primitive library |
| Center canvas | `workspace_panel` | app/workspace state | renders project objects |
| Right sidebar | `workspace_panel` | app/workspace state | inspector/hierarchy/library |
| Bottom shelf | `workspace_panel` | app/workspace state | preview/export/debug tabs |
| Status bar | `workspace_panel` | app/workspace state | current mode/status |

Do not store editor shell panels as normal design-canvas `InterfaceObject` unless building a meta-editor mode later.

## 14. Three different meanings of panel

| Word | Correct meaning | Data owner |
|---|---|---|
| Canvas panel | panel object designed on canvas | Project.objects_by_id |
| Editor panel | UI panel of the application shell | app shell/workspace state |
| Dock panel | resizable area in app layout | docking model |

Rule:

```text
always say canvas panel, workspace panel, or dock area when ambiguity is possible
```

## 15. Three different meanings of layout

| Layout type | Meaning | Example |
|---|---|---|
| Canvas object layout | positions of designed UI objects | panel sections, rectangles, groups |
| App shell docking layout | split/merge arrangement of editor panels | Blender-like split panels |
| Graph viewport layout | positions of logic graph nodes | state graph editor |

Do not mix their data models.

## 16. Minimal schema for classification

Every object may eventually expose:

```text
type
subtype
role
kind
domain_tags
role_tags
behavior_tags
maturity_tags
export_tags
state_machine_tags
```

Example:

```json
{
  "type": "region_rectangle",
  "subtype": "interaction_region",
  "role": "click_region",
  "domain_tags": ["domain_canvas_object", "domain_region"],
  "role_tags": ["role_hit_region"],
  "behavior_tags": ["can_bind_event", "can_be_state_target"],
  "maturity_tags": ["maturity_mvp_workflow"],
  "export_tags": ["export_project_json", "export_component_json", "no_svg_export"],
  "state_machine_tags": ["state_target_region", "state_emits_command_only"]
}
```

## 17. How this affects UI display

The UI can use tags to decide:

| UI area | Tag usage |
|---|---|
| Canvas | render only visible/exportable objects in Normal mode |
| Hierarchy | group by semantic role and show type icons |
| Inspector | choose tabs from type/role/tags |
| Debug overlay | show hidden regions/helpers |
| Component library | filter by component kind/tags |
| State graph | expose only objects with state target tags |
| Export panel | show export eligibility and warnings |

## 18. Roadmap phase assignment

| Element group | First roadmap phase |
|---|---|
| Primitive object | E |
| Structural helper | E/K |
| Interaction region | E/G |
| Layout object | G/K |
| Visual element | E |
| Control group | H |
| Panel part | H/K |
| Component asset | J |
| Workspace UI | B/I |
| Debug element | G/I |
| Logic element | M/N |

## 19. Acceptance rule

A category is useful only if it improves at least one of these:

- naming clarity,
- inspector grouping,
- hierarchy display,
- event targeting,
- export rules,
- roadmap planning,
- debug readability,
- component reuse.
