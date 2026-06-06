# 02 — Primitive shapes and interface object types

Status: draft 00
Scope: primitive shapes, primitive objects, object types, visible vs invisible primitives

## 1. Core definition

`primitive_shapes` are the smallest editable building blocks used to construct interface objects.

They are not final UI widgets by themselves. A button, slider, panel, toolbar or card is usually a composition of primitives.

Main principle:

```text
primitive = minimal editable object
component = meaningful reusable composition
```

Example:

```text
button_group = rectangle + text_rectangle + region_rectangle
```

## 2. Primitive versus component

| Category | Meaning | Example |
|---|---|---|
| Primitive | smallest editable piece | rectangle, text, line, region |
| Group | container of primitives | label + icon group |
| Panel | structured group with known roles | frame/header/content/footer |
| Component | saved reusable group/panel | button asset, monitor panel asset |
| Workspace panel | part of the editor shell | inspector panel, hierarchy panel |

## 3. MVP primitive list

| Primitive | Type ID | Visible in Normal mode | Main use | Export SVG |
|---|---|---:|---|---:|
| Empty | `empty` | no, debug only | anchor, pivot, marker, snap point | no |
| Empty Rectangle | `empty_rectangle` | optional/debug | layout placeholder, content reservation, blockout | optional |
| Rectangle | `rectangle` | yes | panel body, button body, card, fill, bar | yes |
| Text Rectangle | `text_rectangle` | yes | label, title, value, text box | yes |
| Line | `line` | yes | separator, connector, guide, icon stroke | yes |
| Region Rectangle | `region_rectangle` | no, debug only | hit, hover, drag, drop, resize, scroll, snap | no |
| Group | `group` | children only | container, button, row, section | as children |
| Panel | `panel` | children only | structured UI panel | as children |

## 4. Post-MVP primitive candidates

| Primitive | Type ID | Use |
|---|---|---|
| Curve | `curve` | curved divider, connector, decorative path |
| Path | `path` | editable vector shape |
| Swatch | `swatch` | color sample object |
| Slider group | `slider` | reusable value control, not raw primitive in MVP |
| Color preview | `color_preview` | preview object for color/value systems |
| Gradient preview | `gradient_preview` | gradient visualization |
| Curve preview | `curve_preview` | value curve visualization |
| Image rectangle | `image_rectangle` | bitmap/media holder |

Important: post-MVP objects should not enter default UI before maturity is defined.

## 5. Empty

Type ID:

```text
empty
```

Purpose:

- anchor,
- pivot,
- snap marker,
- parent transform helper,
- procedural origin,
- debug marker.

Properties:

| Property | Meaning |
|---|---|
| transform.x/y | position |
| transform.rotation | optional orientation |
| visible | if false, visible only in debug |
| metadata.role | pivot, marker, snap_target, origin |

Normal mode behavior:

- not rendered as final UI,
- can be selected only if helper visibility is enabled.

## 6. Empty Rectangle

Type ID:

```text
empty_rectangle
```

Purpose:

- placeholder,
- layout box,
- panel draft,
- content reservation,
- drop target draft,
- section blockout.

Properties:

| Property | Meaning |
|---|---|
| transform.x/y/width/height | box area |
| layout_data.role | placeholder, section, content, grid_cell |
| style.debug_outline | visible only in layout/debug mode |

Use example:

```text
Dashboard_Content_EmptyRectangle
```

Later conversion:

```text
empty_rectangle -> panel section -> group/panel component
```

## 7. Rectangle

Type ID:

```text
rectangle
```

Purpose:

- visible shape,
- panel background,
- button body,
- card,
- input field background,
- progress bar track,
- progress bar fill,
- icon block primitive.

Important rule:

```text
rectangle renders; region reacts
```

A rectangle does not own click behavior by itself. It may be visually linked to a region.

Core properties:

| Property | Meaning |
|---|---|
| transform | x, y, width, height, rotation |
| style.fill | fill color/token |
| style.stroke | border/stroke |
| style.radius | rounded corners |
| bbox.visual_bbox | visible bounds |
| layout_data | optional layout behavior |

Examples:

```text
ButtonPrimary_Body_Rectangle
PanelMonitor_Frame_Rectangle
RowCPU_Bar_Fill_Rectangle
```

## 8. Text Rectangle

Type ID:

```text
text_rectangle
```

Purpose:

- label,
- title,
- value,
- body text,
- button label,
- inspector field label,
- metric readout.

Core properties:

| Property | Meaning |
|---|---|
| text_data.text | text content |
| text_data.font_family | font |
| text_data.font_size | size |
| text_data.align | left/center/right |
| text_data.wrap | text wrapping |
| style.text_color | text color/token |
| transform.width/height | text bounds |

Examples:

```text
PanelMonitor_Title_Text
RowCPU_Value_Text
ButtonPrimary_Label_Text
```

## 9. Line

Type ID:

```text
line
```

Purpose:

- separator,
- connector,
- guide,
- measurement line,
- icon stroke,
- graph edge later.

Core properties:

| Property | Meaning |
|---|---|
| path_data.start | start point |
| path_data.end | end point |
| style.stroke | line color |
| style.stroke_width | line thickness |
| style.dash | optional dash pattern |
| style.cap | line cap |

Examples:

```text
Inspector_SectionDivider_Line
NodeGraph_Connection_Line
LayoutGuide_Measurement_Line
```

## 10. Region Rectangle

Type ID:

```text
region_rectangle
```

Purpose:

- hit-test,
- hover,
- drag,
- drop,
- resize,
- scroll,
- snap,
- layout,
- content area.

Normal mode:

- invisible,
- participates in hit-test if enabled.

Debug mode:

- translucent overlay,
- shows region type, priority and links.

Core properties:

| Property | Meaning |
|---|---|
| region_data.region_type | hit, hover, drag, drop, resize, scroll, snap, layout, content |
| region_data.linked_visual_id | visual object this region belongs to |
| region_data.priority | hit-test priority |
| region_data.cursor_type | cursor on hover |
| region_data.event_bindings | events assigned to region |
| region_data.accepts_drop_types | allowed object/component types |

Examples:

```text
ButtonPrimary_Click_Region
PanelMonitor_Header_Drag_Region
PanelMonitor_RightEdge_Resize_Region
ComponentShelf_Drop_Region
```

## 11. Group

Type ID:

```text
group
```

Purpose:

- parent container,
- multi-object transform,
- local coordinate space,
- semantic unit,
- first step toward component.

Core properties:

| Property | Meaning |
|---|---|
| children_ids | child objects |
| group_data.group_kind | generic, button, row, section, icon, control |
| computed_bbox | result from children |
| parameter_data | exposed parameters later |

Examples:

```text
ButtonPrimary_Group
RowCPU_Group
PanelMonitor_Header_Group
```

## 12. Panel

Type ID:

```text
panel
```

Purpose:

A panel is a specialized group with expected internal roles.

Canonical panel structure:

```text
Panel_Group
  Frame_Rectangle
  Header_Group
    Title_Text
    Header_Drag_Region
  Content_Group
    Content_Region
    Section_Group
  Footer_Group
  Resize_Regions
```

A panel is not a single rectangle.

Minimum panel roles:

| Role | Typical type |
|---|---|
| frame | rectangle |
| header | group |
| title | text_rectangle |
| content | group / region_rectangle |
| footer | group |
| drag region | region_rectangle |
| resize region | region_rectangle |
| scroll region | region_rectangle |

## 13. Primitive composition examples

### Button

```text
ButtonPrimary_Group
  ButtonPrimary_Body_Rectangle
  ButtonPrimary_Label_Text
  ButtonPrimary_Click_Region
```

### Metric bar

```text
RowCPU_Bar_Group
  RowCPU_Bar_Background_Rectangle
  RowCPU_Bar_Fill_Rectangle
  RowCPU_Bar_Hit_Region
```

### Inspector section

```text
InspectorTransform_Section_Group
  InspectorTransform_Header_Rectangle
  InspectorTransform_Title_Text
  InspectorTransform_Content_Group
  InspectorTransform_Collapse_Region
```

### Panel monitor sample

```text
PanelMonitor_Group
  PanelMonitor_Frame_Rectangle
  PanelMonitor_Header_Group
  PanelMonitor_Stats_Group
  PanelMonitor_Footer_Group
  PanelMonitor_InteractionRegions_Group
```

## 14. Primitive creation order in UI

Recommended left tool order:

1. Select,
2. Rectangle,
3. Text,
4. Line,
5. Region,
6. Empty,
7. Empty Rectangle,
8. Group,
9. Panel,
10. Component.

Reason:

- visible primitives first,
- invisible/system primitives after visible basics,
- composition tools after primitives.

## 15. Export behavior

| Object type | Project JSON | Component JSON | SVG |
|---|---:|---:|---:|
| empty | yes | yes | no |
| empty_rectangle | yes | yes | optional/debug only |
| rectangle | yes | yes | yes |
| text_rectangle | yes | yes | yes |
| line | yes | yes | yes |
| region_rectangle | yes | yes | no |
| group | yes | yes | children only/group tag |
| panel | yes | yes | children only/group tag |

## 16. State machine targeting

State machine can target primitives and groups, but it should respect their role.

| Target type | Allowed state machine actions |
|---|---|
| rectangle | set fill, stroke, opacity, transform |
| text_rectangle | set text, color, font size, transform |
| line | set stroke, endpoints, visibility |
| region_rectangle | enable/disable, change cursor, event binding, priority |
| group | transform children as unit, set visibility, set state variant |
| panel | set panel state, title, visibility, exposed parameters |
| empty | move anchor, update pivot, debug marker |
| empty_rectangle | convert to section/panel, update layout bounds |

Forbidden:

```text
state_machine directly mutates DOM/SVG without command layer
```

## 17. Roadmap introduction

| Primitive | Roadmap phase |
|---|---|
| empty | E — Primitive creation |
| empty_rectangle | E — Primitive creation / K — Box arranger expanded |
| rectangle | E — Primitive creation |
| text_rectangle | E — Primitive creation |
| line | E — Primitive creation |
| region_rectangle | E basic / G full region debug |
| group | H — Component proof |
| panel | H sample / K panel builder expanded |
| curve/path | O advanced |
| procedural icons | O advanced |

## 18. Minimal acceptance rule

A primitive is valid when it has:

```text
id
type
name
transform or path data
parent relation
visibility/lock state
validation status
clear export behavior
clear state_machine targeting rules
```
