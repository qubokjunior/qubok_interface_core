# 12 — Component templates, panel authoring and slots

Status: draft 00
Scope: component anatomy, panel builder, templates, slots, exposed parameters, structural variants, library workflow
Mode: private / local-first

## 1. Purpose

This document defines how reusable components, templates and panels should be structured in `qubok_interface_core`.

The system should allow a user to build interface elements such as:

```text
button
slider
slider_minMax
switch
dropdown
input
levels/range widget
scroll panel
toolbar
inspector section
panel shell
state graph node card
curve_node widget
```

from smaller primitive/group structures while keeping them reusable, inspectable and editable.

## 2. Core distinction

```text
Primitive = smallest object type.
Group = hierarchy container.
Component = saved reusable group with exposed parameters.
Template = reusable structure / blueprint.
Preset = parameter values for an existing structure.
Slot = declared placeholder area for child content.
Panel = specialized component with frame/header/content/sections/regions.
```

Important rule:

```text
Preset changes parameters.
Template/structural variant changes object structure.
```

## 3. Component anatomy

A component should be represented as a group with extra component data.

Recommended fields:

```text
component_id
asset_id optional
component_type
name
version
structure_signature
root_object_id
internal_roles
slots
exposed_parameters
hidden_internal_objects
default_bindings
default_regions
default_states
style_presets
validation_rules
tags
preview
```

Component root still behaves as an `InterfaceObject` and exists in hierarchy.

## 4. Internal hierarchy and roles

Every important child should have a semantic role.

Example button:

```text
Button_Group role=button/root
  Button_Body_Rectangle role=button/body
  Button_Label_Text role=button/label
  Button_Icon_Slot optional role=slot/icon
  Button_Click_Region role=region/click
  Button_Hover_Region optional role=region/hover
```

Example slider:

```text
Slider_Group role=slider/root
  Slider_Track_Background role=slider/track_bg
  Slider_Fill_Rectangle role=slider/fill
  Slider_Handle_Group role=slider/handle
    Slider_Handle_Rectangle role=slider/handle_visual
    Slider_Handle_Drag_Region role=region/drag
  Slider_Value_Text optional role=slider/value_text
  Slider_Min_Text optional role=slider/min_text
  Slider_Max_Text optional role=slider/max_text
```

## 5. Slots

Slots are declared insertion points for other objects/components.

Examples:

```text
icon_slot
left_label_slot
right_label_slot
header_actions_slot
content_slot
footer_slot
list_item_slot
node_port_slot
```

Slot fields:

```text
slot_id
name
role
accepted_types
accepted_tags
required
max_children
default_child optional
layout_policy
inherit_style_policy
empty_state_visual
validation_status
```

A slot is not only visual empty space. It is a typed placement contract.

## 6. Exposed parameters

Components should expose selected internal parameters rather than every child field.

Examples:

| Component | Exposed parameters |
|---|---|
| Button | label, icon, color, disabled, variant |
| Slider | min, max, value, step, label, color, show_value |
| Dropdown | selected_value, item_list, opened, placeholder |
| Panel | title, width, height, padding, gap, theme_variant |
| Node card | node_title, socket list, state, validation_status |

Exposed parameter entry:

```text
parameter_id
label
value_type
default_value
maps_to internal parameter path(s)
source_mode default/local/inherited/dictated
read/write access
validation
ui_editor type
```

## 7. Component surface versus hidden internals

The user usually edits the component surface.

Surface:

```text
Button.label
Button.color
Slider.value
Panel.title
Panel.padding
Dropdown.items
```

Hidden internals:

```text
Button_Body_Rectangle.style.color_main
Slider_Handle_Drag_Region.region.priority
Panel_Content_Region.region.content_rules
```

Advanced mode can reveal internals, but the normal inspector should prefer the exposed parameter surface.

## 8. Panel as specialized component

Panel structure:

```text
Panel_Group role=panel/root
  Panel_Frame_Rectangle role=panel/frame
  Panel_Header_Group role=panel/header
    Panel_Title_Text role=panel/title
    Panel_Header_Actions_Slot role=slot/header_actions
    Panel_Drag_Region role=region/drag
  Panel_Content_Region role=region/content
  Panel_Content_Group role=panel/content
    Panel_Section_Group[] role=panel/section
  Panel_Footer_Group optional role=panel/footer
  Panel_Scroll_Region optional role=region/scroll
  Panel_Resize_Region[] role=region/resize
```

Panel is not one rectangle. It is a structured component with regions and layout rules.

## 9. Panel authoring workflow

Recommended flow:

```text
1. Create panel shell from template or draw empty_rectangle.
2. Assign title and size.
3. Create frame/header/content/footer groups.
4. Add content region and layout rules.
5. Drag components from library into content region.
6. Define slots and exposed parameters.
7. Attach resize/scroll/drag regions if needed.
8. Validate structure.
9. Save as component/template.
10. Instantiate in another project or panel.
```

## 10. Dragging library objects onto a panel

Drop flow:

```text
pointer_drag_start library asset
-> payload contains asset_id, asset_type, accepted target rules
-> hover over panel content region
-> target resolver returns drop region
-> validate accepted type/tag
-> show ghost insert preview
-> on drop emit component.instantiate or object.create batch
-> parent new objects under content group/slot
-> update hierarchy and inspector
```

Drop target is usually an `interaction_region` or `content_region`, not the visible rectangle.

## 11. Temporary insert / ghost preview

Before commit, UI should show a ghost placement.

Ghost preview contains:

```text
proposed parent
slot/content region
ghost bbox
insert index
validation status
snap/alignment hints
warnings
```

Ghost preview does not mutate Project until drop commit.

## 12. Layout rules inside panels

Panel layout should be rule-based.

Fields:

```text
layout.mode: free / stack_vertical / stack_horizontal / grid / wrap / table
layout.margin
layout.padding
layout.gap
layout.align_x
layout.align_y
layout.size_policy
layout.min_size
layout.max_size
layout.overflow
layout.clip_children
```

Rules may be applied to:

```text
all children
children by role
children by type
children with tag
slot contents
current selection
```

Example:

```text
inside Panel_Content_Group
where role=slider
set layout.width_policy = fill
set style.color_main source = inherited from panel theme
```

## 13. Internal / inherited / dictated in panels

Panel can control child parameters through source modes.

| Mode | Meaning in panel context |
|---|---|
| `internal` | child owns its own value |
| `inherited` | child reads from panel/theme/component |
| `dictated` | panel forces value and locks local editing |

Example:

```text
Panel rule:
  query: role=button inside panel
  parameter: text.color_main
  source: inherited
  value: panel.theme.text_primary
```

Example dictated rule:

```text
Panel warning state:
  query: role=value_text inside panel
  parameter: style.color_main
  source: dictated
  value: theme.warning
```

## 14. Component variants

### Style/theme variant

Same structure, different exposed parameter values.

```text
Button/primary
Button/secondary
Slider/minimal_green
Panel/dark_compact
```

### Structural variant

Different structure, roles, slots or regions.

```text
Button/text_only
Button/icon_left
Slider/single
Slider/minMax
Dropdown/simple
Dropdown/searchable
Panel/basic
Panel/with_footer
```

Structure signature should detect whether object count/roles/slots changed.

## 15. Component validation

Validation checks:

```text
root object exists
all children exist
required roles exist
required slots exist
required exposed parameters resolve
region links valid
slot accepted types valid
no duplicate role where unique role required
no broken preset/theme references
structure_signature matches template version
```

Warnings:

```text
unused slot
hidden internal object selected accidentally
parameter exposed but target missing
preset older than component schema
component contains unsupported object for export
```

## 16. Library asset card

A component/template card should show:

```text
preview
name
type/category
variant
tags
validation status
version
used_times
referred_times
source local file
```

Actions:

```text
drag instantiate
edit metadata
open template
save variant
export local file
duplicate
validate
```

## 17. Example component: slider_minMax

Structure:

```text
SliderMinMax_Group
  Track_Background
  Range_Fill
  Min_Handle_Group
    Min_Handle_Visual
    Min_Drag_Region
  Max_Handle_Group
    Max_Handle_Visual
    Max_Drag_Region
  Min_Value_Text optional
  Max_Value_Text optional
```

Rules:

```text
min_value <= max_value
min_handle.x <= max_handle.x
both handles clamp to track bbox
range_fill.x = min_handle.x
range_fill.width = max_handle.x - min_handle.x
```

Bindings:

```text
Min_Drag_Region -> min_value
Max_Drag_Region -> max_value
min_value/max_value -> handle positions
min_value/max_value -> text labels
```

## 18. Example component: dropdown

Structure:

```text
Dropdown_Group
  Header_Rectangle
  Header_Label_Text
  Arrow_Icon
  Header_Click_Region
  List_Panel_Group
    List_Content_Region
    List_Item_Group[]
```

States:

```text
closed
open
hover_item
selected_item
disabled
```

Actions:

```text
pointer_click header -> toggle open
pointer_click item -> set selected_value + close
escape -> close
outside click -> close
```

## 19. Roadmap placement

| Feature | Maturity start | Target |
|---|---|---|
| group component proof | L3 | MVP proof |
| save component | L3 | component library phase |
| exposed parameters | L2/L3 | component library phase |
| slots | L2 | panel builder phase |
| structural templates | L2/L3 | panel builder phase |
| style variants | L3 | presets/theme phase |
| advanced component editor | L2/L3 | post-MVP workspace |

## 20. Acceptance rule

Component/template system is valid when:

```text
component is still normal hierarchy data
exposed parameters map to internal paths
slots validate accepted content
library drag/drop uses regions and commands
style variation does not create structural variant
structural variant updates structure signature
component instance can be edited and saved locally
```

## 21. Final decision

Panels, sliders, buttons and dropdowns should be documented as component structures, not as primitive types. Their behavior emerges from primitives, regions, exposed parameters, bindings and commands. This keeps the system editable, inspectable and reusable.
