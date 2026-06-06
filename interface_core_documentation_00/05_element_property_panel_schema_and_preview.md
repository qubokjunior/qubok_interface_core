# 05 — Element property panel schema and preview

Status: draft 00
Scope: parameter panels, inspector schema, element preview, rectangle / region / text / curve / panel properties

## 1. Purpose

This document defines the first visual and data schema for element parameter panels in `qubok_interface_core`.

The attached reference panels describe how the inspector can display:

- the selected element type,
- transform and size fields,
- style fields,
- stroke and opacity fields,
- type-specific fields,
- preview of the current element,
- panel-level metadata,
- inherited/internal/dictated parameter sources.

Core idea:

```text
property panel = schema-driven inspector view + live preview of selected element
```

The panel should not be manually hardcoded per object forever. Each element type should expose an inspector schema that determines which fields appear and in what order.

## 2. Common field order

Most visible/interactable elements share a base field order.

Recommended order:

```text
element_type
size
pos
pos_pivot
rotation
corner_radius
stroke_width
color_main
color_stroke
opacity_main
opacity_stroke
stroke_type
stroke_mode
```

This order should remain stable across `rectangle`, `region`, `text`, `curve`, panel frame and many derived components.

## 3. Shared base parameters

| Field | Meaning | Typical type |
|---|---|---|
| `element_type` | technical object type | enum/string |
| `size` | object size | vector2/vector3 |
| `pos` | object position | vector3 |
| `pos_pivot` | pivot position/offset | vector3 |
| `rotation` | object rotation | vector3 |
| `corner_radius` | per-corner radius | vector4 |
| `stroke_width` | outline width | float |
| `color_main` | fill/main color | color/token |
| `color_stroke` | stroke color | color/token |
| `opacity_main` | main/fill opacity | normalized float or percent |
| `opacity_stroke` | stroke opacity | normalized float or percent |
| `stroke_type` | stroke render pattern | enum/preset/custom sequence |
| `stroke_mode` | stroke placement | inside/middle/outside |

## 4. Rectangle property panel

Reference element:

```text
element_type: rectangle
size: x50px, y50px
pos: 0x 0y 0z
pos_pivot: 0x 0y 0z
rotation: 0x 0y 0z
corner_radius: 4x 4y 0z 0w
stroke_width: 0
color_main: dark swatch
color_stroke: dark/black swatch
opacity_main: 100%
opacity_stroke: 0
stroke_type: 0
stroke_mode: 0
```

Visual preview:

- dark rounded rectangle,
- preview area below parameters,
- no debug overlay in normal preview.

Implementation note:

`rectangle` is a visible shape. It should expose style parameters and support SVG export.

Recommended schema:

| Section | Fields |
|---|---|
| Identity | element_type, name, id, tags later |
| Transform | size, pos, pos_pivot, rotation |
| Geometry | corner_radius |
| Style | color_main, color_stroke, opacity_main, opacity_stroke |
| Stroke | stroke_width, stroke_type, stroke_mode |
| Preview | live rectangle render |

## 5. Region property panel

Reference element:

```text
element_type: region
size: x50px, y50px
pos: 0x 0y 0z
pos_pivot: 0x 0y 0z
rotation: 0x 0y 0z
corner_radius: 4x 4y 0z 0w
stroke_width: 2
color_main: black
color_stroke: blue
opacity_main: 30%
opacity_stroke: 100%
stroke_type: 2dash, 2gap
stroke_mode: inside
```

Visual preview:

- dashed blue outline rectangle,
- darker semi-transparent fill,
- debug-like representation by default.

Implementation note:

`region` / `region_rectangle` is primarily interaction data, not final visual export. It may use style fields for debug representation, but normal SVG export should exclude it unless explicitly enabled for debug export.

Additional region-specific fields should include:

| Field | Meaning |
|---|---|
| `region_type` | hit, hover, drag, focus, resize, drop, scroll, snap, content, layout |
| `linked_visual_id` | visual object controlled/covered by region |
| `priority` | hit-test priority |
| `cursor_type` | cursor shown over region |
| `event_bindings` | attached events/actions |
| `enabled` | active/inactive region |

Recommended schema:

| Section | Fields |
|---|---|
| Identity | element_type, name, id, tags |
| Transform | size, pos, pos_pivot, rotation |
| Region | region_type, linked_visual_id, priority, cursor_type, enabled |
| Debug Style | corner_radius, stroke_width, color_main, color_stroke, opacity_main, opacity_stroke, stroke_type, stroke_mode |
| Events | event_bindings |
| Preview | dashed region render |

## 6. Text property panel

Reference element:

```text
element_type: text
string: -default-
size: 12px
pos: 0x 0y 0z
pos_pivot: 0x 0y 0z
rotation: 0x 0y 0z
stroke_width: 0
color_main: white
color_stroke: black
opacity_main: 100%
opacity_stroke: 0
stroke_type: 0
stroke_mode: 0

font_type: roboto
alignment:
  align_x: left / center / right
  align_y: top / center / bottom
  pivot_point: midpoint+
spacing:
  character: 1
  word: 1
  line: 1
text_box:
  flow_type: overflow
  width: 0m
  height: 0m
```

Visual preview:

- dark preview area,
- rendered text sample `-default-`,
- text aligned according to settings.

Implementation note:

`text_rectangle` should share transform/style fields with other objects but also expose typography and text box behavior.

Recommended schema:

| Section | Fields |
|---|---|
| Identity | element_type, name, id, tags |
| Text | string, font_type, size |
| Transform | pos, pos_pivot, rotation |
| Style | color_main, color_stroke, opacity_main, opacity_stroke |
| Stroke | stroke_width, stroke_type, stroke_mode |
| Alignment | align_x, align_y, pivot_point |
| Spacing | character, word, line |
| Text Box | flow_type, width, height |
| Preview | live text render |

Potential `flow_type` values:

```text
overflow
clip
wrap
auto_size
auto_fit
```

## 7. Curve property panel

Reference element:

```text
element_type: curve
width: 2px
pos: 0x 0y 0z
pos_pivot: 0x 0y 0z
rotation: 0x 0y 0z
corner_radius: 0x 0y 0z
stroke_width: 0
color_main: cyan/green
color_stroke: black
opacity_main: 100%
opacity_stroke: 0
stroke_type: 0
stroke_mode: 0

resolution: 4 point
type: poly / bezier
point_xx preview: 4 id
size: 100%
pos: 1x 1y 0z
rotation: 0x 0y 0z
```

Visual preview:

- small graph-like curve preview,
- visible points,
- line segments,
- direction/point markers.

Implementation note:

`curve` is a visible/vector primitive with both object-level transform and point-level data.

Recommended schema:

| Section | Fields |
|---|---|
| Identity | element_type, name, id, tags |
| Transform | pos, pos_pivot, rotation, size |
| Curve Style | width, color_main, color_stroke, opacity_main, opacity_stroke, stroke_type, stroke_mode |
| Curve Data | resolution, type, point_count, selected_point_id |
| Point Data | point position, handle mode, per-point size/rotation later |
| Tools | trim, resample, convert poly/bezier later |
| Preview | curve with points |

Important separation:

```text
object transform = transform of whole curve object
point data = local curve geometry
```

## 8. Panel properties display

Reference element:

```text
panel_properties
name: panel_name_01
id: 01
tags: default, toolbar

element_type: rectangle
size: x200px, y275px
pos: 0x 0y 0z
pos_pivot: 0x 0y 0z
rotation: 0x 0y 0z
corner_radius: 4x 4y 4z 4w
stroke_width: 1
color_main: dark swatch
color_stroke: white swatch
opacity_main: 80%
opacity_stroke: 80%
stroke_type: line
stroke_mode: inside

size_minMax: 1x1y min / 1.5x1.5y max
align: x center, y top
```

Additional section:

```text
parameter read:
internal / inherited / dictated

read:
min/max size
corner_radius
stroke_width
color_main
color_stroke
opacity_main
opacity_stroke
stroke_type
stroke_mode
```

Meaning:

The panel property view shows not only current values, but also where the values come from.

## 9. Parameter source model

The reference panel defines three parameter source modes:

| Source mode | Meaning |
|---|---|
| `internal` | local/default value of this element |
| `inherited` | sampled/inherited from parent/component/theme |
| `dictated` | forced/manual/overriding value |

Recommended expanded source model:

| Source | Meaning |
|---|---|
| `default` | system default for type |
| `local` | value stored directly on object |
| `inherited` | value from parent/group/component |
| `theme_token` | value from preferences/theme |
| `component_parameter` | exposed component parameter |
| `state_override` | temporary or active state variant |
| `graph_output` | state machine / graph generated value |
| `dictated` | forced override, higher priority |

The panel should initially show simplified source labels, then later expose full source chain in advanced/debug mode.

## 10. Parameter source display

The example shows values like `0/1/2` next to fields. This can become a compact source indicator.

Proposed mapping:

```text
0 = internal/default/local
1 = inherited/parent/component/theme
2 = dictated/override/graph/state
```

Alternative readable chips:

```text
INT
INH
OVR
```

Recommended UI:

| Field | Value | Source chip |
|---|---|---|
| `corner_radius` | `4 4 4 4` | `INT` |
| `color_main` | `theme.panel.dark` | `INH` |
| `opacity_main` | `80%` | `OVR` |

## 11. Property row structure

Each inspector row should eventually support:

```text
label
value editor
unit
source indicator
reset/revert button
lock/override state
validation marker
optional mini preview
```

Example:

```text
color_main | [swatch] #202020 | theme.panel.dark | INH | reset
```

## 12. Preview area behavior

Each property panel should include live preview.

| Element type | Preview |
|---|---|
| rectangle | rendered rounded rectangle with current style |
| region | dashed/debug region representation |
| text | rendered text sample in box |
| curve | small curve preview with points/handles |
| panel | mini panel frame preview |

Preview should support:

- normal view,
- selected view,
- debug view,
- invalid/warning state overlay.

## 13. Inspector implementation recommendation

Inspector should be schema-driven.

Example conceptual schema:

```json
{
  "type": "rectangle",
  "sections": [
    { "id": "identity", "fields": ["element_type", "name", "id", "tags"] },
    { "id": "transform", "fields": ["size", "pos", "pos_pivot", "rotation"] },
    { "id": "geometry", "fields": ["corner_radius"] },
    { "id": "stroke", "fields": ["stroke_width", "stroke_type", "stroke_mode"] },
    { "id": "style", "fields": ["color_main", "color_stroke", "opacity_main", "opacity_stroke"] },
    { "id": "preview", "renderer": "rectangle_preview" }
  ]
}
```

Field values are read from Project model and changed only through commands.

## 14. Command relation

Changing an inspector value should create command output.

Examples:

| UI edit | Command |
|---|---|
| change rectangle fill | `object.patch style.color_main` |
| change size | `transform.update` or `object.patch transform.size` |
| change region stroke debug style | `object.patch region debug style` |
| change text string | `object.patch text_data.string` |
| change curve point position | `curve.point.patch` later or `object.patch path_data.points` |
| change parameter source | `parameter.set_source` later |

Minimum MVP can use `object.patch` for most fields, but architecture should reserve specialized commands for transform, curve points and parameter source changes.

## 15. Roadmap placement

| Feature | Phase |
|---|---|
| base property panel layout | F — Inspector / hierarchy / sync |
| rectangle schema | F |
| region schema | G — BBox / region / debug |
| text schema | F |
| curve schema | O advanced, partial earlier if curve exists |
| panel properties | H/K — panel proof and panel builder |
| parameter source display | K/M — components and event/value binding |
| dictated/inherited/internal source resolution | K/M/O depending on depth |
| live preview | F/H basic, later polished |

## 16. Acceptance rule

The element property panel is valid when:

```text
it shows element_type
it shows shared base parameters in stable order
it shows type-specific parameters only where relevant
it includes live preview
it edits Project model through commands
it shows parameter source at least in debug/advanced mode
it can be driven by schema, not one-off custom panels only
```

## 17. Design conclusion

The attached panels define the first inspector language for `qubok_interface_core`:

```text
common parameters first
type-specific parameters second
preview always visible
source of value visible or debuggable
all edits route through command layer
```

This allows `rectangle`, `region`, `text`, `curve`, `panel` and future derivatives to share one coherent editing model.
