# 01 — Naming, taxonomy and display order

Status: draft 00
Scope: vocabulary, naming rules, type hierarchy, display order, hierarchy order, inspector order

## 1. Purpose

This document defines the first naming system for `qubok_interface_core` documentation.

The goal is to make feedback and development precise. A user, developer or later state machine should be able to say exactly which kind of element is being referenced:

- primitive shape,
- interaction region,
- layout box,
- group,
- panel,
- component,
- workspace panel,
- graph node,
- command,
- event,
- style token,
- debug overlay.

The main rule:

```text
name should describe role first, implementation second
```

Example:

```text
Header_Drag_Region
CPU_Bar_Fill_Rectangle
Panel_Monitor_Group
Inspector_RightSidebar_WorkspacePanel
```

Not:

```text
rect01
thing_big
blue_box
clicker
```

## 2. Canonical language split

| Layer | Owns data? | User sees it? | Example |
|---|---:|---:|---|
| Project model | yes | indirectly | Project, InterfaceObject, Selection |
| Primitive | yes | yes/debug | rectangle, text_rectangle, region_rectangle |
| Group/component | yes | yes | button_group, panel_monitor |
| Workspace shell | yes, separate model | yes | left tool panel, right sidebar, bottom shelf |
| Tool | no object by itself | yes | select tool, rectangle tool, region tool |
| Event/action/state graph | yes, later | debug/workspace | onClick -> SetFill -> object.patch |
| Debug overlay | no permanent project object | yes in debug | visual bbox overlay, region overlay |

## 3. Naming pattern for project objects

Recommended pattern:

```text
[Context]_[Role]_[Kind]
```

Where:

- `Context` = parent/domain/component, for example `PanelMonitor`, `ButtonPrimary`, `RowCPU`.
- `Role` = semantic role, for example `Header`, `Title`, `Track`, `Fill`, `Hit`, `Drag`.
- `Kind` = technical object category, for example `Rectangle`, `Text`, `Region`, `Group`, `Line`.

Examples:

| Object name | Meaning |
|---|---|
| `PanelMonitor_Frame_Rectangle` | visible panel background rectangle |
| `PanelMonitor_Header_Group` | group containing header elements |
| `PanelMonitor_Header_Drag_Region` | invisible drag region for moving panel |
| `PanelMonitor_Title_Text` | text object used as title |
| `RowCPU_Bar_Background_Rectangle` | visible background of metric bar |
| `RowCPU_Bar_Fill_Rectangle` | visible fill of metric bar |
| `RowCPU_Bar_Hit_Region` | interaction region for bar hover/click |
| `ButtonPrimary_Body_Rectangle` | visible button body |
| `ButtonPrimary_Label_Text` | button label |
| `ButtonPrimary_Click_Region` | hit region for click interaction |

## 4. Technical type names

Use lowercase snake case for internal type identifiers.

| Concept | Type identifier |
|---|---|
| Empty anchor | `empty` |
| Empty rectangle / placeholder | `empty_rectangle` |
| Visible rectangle | `rectangle` |
| Text box | `text_rectangle` |
| Line / separator | `line` |
| Invisible interaction rectangle | `region_rectangle` |
| Container | `group` |
| Structured UI panel | `panel` |
| Reusable asset instance | `component_instance` |
| Graph node | `state_graph_node` |
| Graph edge | `state_graph_edge` |
| Workspace panel | `workspace_panel` |
| Dock area | `dock_area` |

## 5. Human display names

Use short Title Case in UI labels.

| Type | UI label |
|---|---|
| `empty` | Empty |
| `empty_rectangle` | Empty Rectangle |
| `rectangle` | Rectangle |
| `text_rectangle` | Text Rectangle |
| `line` | Line |
| `region_rectangle` | Region |
| `group` | Group |
| `panel` | Panel |
| `component_instance` | Component Instance |

## 6. Type hierarchy

Recommended hierarchy from smallest to largest:

```text
Token
Property
Primitive
Region
Group
Panel
Component
Workspace
Application
```

Meaning:

| Level | Meaning | Example |
|---|---|---|
| Token | reusable style/default value | `theme.accent`, `radius.small` |
| Property | single editable field | `x`, `width`, `fill`, `text` |
| Primitive | minimal object | `rectangle`, `line`, `text_rectangle` |
| Region | interaction/layout zone | `hit`, `drag`, `resize`, `drop` |
| Group | parent container | button group, stat row group |
| Panel | structured group | monitor panel, inspector section |
| Component | saved reusable group/panel | button asset, panel asset |
| Workspace | editor area | interface creator, logic workspace |
| Application | product using core | future qubok apps |

## 7. Display order on canvas

Canvas render order should be deterministic.

Recommended order inside a parent:

1. background and frame primitives,
2. structural helper shapes if visible,
3. content primitives,
4. labels/text,
5. icons/lines/connectors,
6. visual state overlays,
7. selection outline,
8. transform handles,
9. debug overlays,
10. hover/debug HUD.

Runtime regions are not rendered in Normal mode. They are displayed only in debug modes.

## 8. Hierarchy display order

Hierarchy should show semantic structure, not raw render stack chaos.

Recommended order inside a panel:

```text
Panel_Group
  Frame
  Header_Group
  Content_Group
  Footer_Group
  Interaction_Regions
  Debug_Helpers
```

For repeated rows:

```text
Stats_Group
  Row_CPU_Group
  Row_RAM_Group
  Row_MEM_Group
```

Inside one row:

```text
Row_CPU_Group
  Icon_CPU
  Label_CPU
  Bar_Background
  Bar_Fill
  Value_Text
  Row_Hit_Region
```

## 9. Inspector tab order

Recommended inspector order:

1. Identity,
2. Transform,
3. Geometry,
4. Style,
5. Text,
6. Region,
7. Layout,
8. Group / Component,
9. Events,
10. Export,
11. Validation,
12. Debug.

Reason:

- identity and transform answer what/where,
- geometry/style/text answer what is visible,
- region/layout answer what reacts and how it occupies space,
- component/events/export/validation are higher-level concerns.

## 10. Workspace display order

Default MVP workspace order:

```text
TopAppBar
LeftToolPanel
CenterCanvas
RightSidebar
BottomShelf
StatusBar
```

RightSidebar internal order:

```text
Inspector
Hierarchy
ComponentLibrary
```

BottomShelf tab order:

```text
Preview
Export
Components
Validation
CommandLog
LogicDebug
```

LogicDebug is present, but collapsed by default.

## 11. Tag naming

Tags use lowercase snake case.

Recommended tag groups:

| Tag group | Examples |
|---|---|
| role | `role_header`, `role_body`, `role_footer`, `role_label`, `role_value` |
| behavior | `can_select`, `can_drag`, `can_resize`, `can_drop`, `can_scroll` |
| structure | `is_panel_part`, `is_component_root`, `is_region`, `is_helper` |
| maturity | `mvp_active`, `debug_only`, `post_mvp`, `experimental_hidden` |
| export | `export_svg`, `export_json`, `no_svg_export`, `runtime_only` |
| state | `state_hoverable`, `state_disabled`, `state_warning`, `state_error` |

## 12. Reserved prefixes

| Prefix | Use |
|---|---|
| `Root_` | top-level scene/project object |
| `Panel_` | structured panel component |
| `Group_` | generic container only when semantic name is unknown |
| `Region_` | explicit region object |
| `Debug_` | debug-only helper |
| `Sample_` | documentation/sample assets |
| `Tmp_` | temporary operation object; must not be saved unless converted |
| `Ghost_` | preview object during drag/drop/insert |
| `State_` | state variant or graph runtime object |

## 13. Bad names and replacements

| Bad name | Better name |
|---|---|
| `rect1` | `PanelMonitor_Frame_Rectangle` |
| `text2` | `PanelMonitor_Title_Text` |
| `click_area` | `ButtonPrimary_Click_Region` |
| `box` | `DashboardSection_EmptyRectangle` |
| `window` | `Inspector_RightSidebar_WorkspacePanel` or `PanelMonitor_Group` depending on meaning |
| `layout` | `CanvasObjectLayout` / `AppShellDockingLayout` / `GraphViewportLayout` |
| `node` | `StateGraphNode` / `HierarchyRow` / `CanvasObject` depending on meaning |

## 14. Minimal rule for future documentation

Every described element should answer:

```text
What is it called?
What technical type is it?
Where is it displayed?
Who owns its data?
Can it be selected?
Can it be exported?
Can state_machine target it?
Which roadmap phase introduces it?
```
