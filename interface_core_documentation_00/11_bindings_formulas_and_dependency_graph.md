# 11 — Bindings, formulas and dependency graph

Status: draft 00
Scope: parameter bindings, formulas, value links, dependency graph, dirty propagation, cycle detection, UI creation flow
Mode: private / local-first / command-based

## 1. Purpose

This document defines how values can be linked, transformed and synchronized inside `qubok_interface_core`.

Typical examples:

```text
slider handle position -> numeric value text
numeric input text -> slider value
panel property -> all sliders inside panel
local data source column -> chart bar height
state graph output -> element style
curve_node execution signal -> debug value preview
```

Core rule:

```text
Bindings do not mutate UI directly.
Bindings resolve values, validate them, then emit commands or computed runtime outputs.
```

## 2. Why bindings are needed

The editor must support building functional interface elements from primitive/group structures.

Example: a slider is not one magical object. It is a group containing:

```text
track rectangle
fill rectangle
handle rectangle/shape
value text optional
interaction region / drag region
exposed value parameter
binding rules
constraint rules
```

The relationship between handle position and displayed value is a binding graph.

## 3. Binding categories

| Binding type | Meaning | Example |
|---|---|---|
| `value_to_value` | one parameter drives another | slider.value -> text.string |
| `value_to_style` | value affects visual style | progress.value -> fill.width/color |
| `style_to_style` | one style token drives another | panel.textColor -> child labels |
| `position_to_value` | transform position becomes value | handle.local_x -> normalized_value |
| `value_to_position` | value becomes transform | numeric input 0.5 -> handle.local_x |
| `region_to_state` | interaction region changes state | hover region -> button.hover |
| `event_to_action` | event triggers action/command | click -> toggle panel |
| `formula_binding` | expression computes target | clamp(map_range(x), 0, 1) |
| `data_source_binding` | local data field drives UI | csv.sales -> chart bar height |
| `graph_output_binding` | state graph output drives parameter | action result -> panel status |

## 4. Binding object model

Recommended model:

```ts
export type ParameterBinding = {
  id: string;
  name: string;
  enabled: boolean;

  source: BindingSource;
  transform?: BindingTransform;
  target: BindingTarget;

  direction: "one_way" | "two_way" | "read_only";
  update_mode: "immediate" | "on_commit" | "throttled" | "debounced" | "manual";

  priority: number;
  conflict_policy: "exclusive" | "merge" | "last_writer_wins" | "blocked" | "warn";

  validation: BindingValidation;
  debug: BindingDebugState;
  metadata: BindingMetadata;
};
```

## 5. Source and target paths

Bindings should use stable parameter paths.

Examples:

```text
objects.slider_01.parameter_data.value
objects.slider_01.children.handle.transform.local_x
objects.slider_01.children.value_text.text.string
objects.panel_01.style.color_main
objects.panel_01.query(role=slider).style.color_main
localData.salesCsv.rows[0].sales
stateGraph.graph_01.outputs.result
```

Path must resolve through the Project model or local data source registry. It should not point to a DOM node.

## 6. Direction rules

### One-way binding

```text
source -> transform/formula -> target
```

Example:

```text
Slider.value -> format(value, 0.00) -> ValueText.string
```

### Two-way binding

Two-way binding is not one circular binding. It should be represented as two guarded assignments sharing one canonical value.

Example:

```text
handle drag -> normalized_value
numeric text commit -> normalized_value
normalized_value -> handle position
normalized_value -> displayed text
```

Canonical value:

```text
Slider.parameter_data.value
```

Input sources update the canonical value; output bindings render it.

## 7. Slider value binding model

Recommended slider architecture:

```text
Slider_Group
  Track_Rectangle
  Fill_Rectangle
  Handle_Group
    Handle_Rectangle
    Handle_Drag_Region
  Value_Text optional
  Min_Text optional
  Max_Text optional
  parameter_data.value
  parameter_data.min
  parameter_data.max
  parameter_data.step
```

Bindings:

```text
Handle_Drag_Region.pointer_drag_move
-> read region_local_x
-> clamp to track bbox
-> normalize 0..1
-> set Slider.value

Slider.value
-> map_range(min,max,track width)
-> set Handle.local_x

Slider.value
-> map_range(min,max,track width)
-> set Fill.width

Slider.value
-> format number
-> set Value_Text.string
```

Important guard:

```text
Binding source = drag does not recursively trigger text input source.
Binding source = text input does not recursively re-enter drag source.
```

## 8. Formula model

Formula bindings should be simple and inspectable.

Initial supported formula families:

| Family | Examples |
|---|---|
| clamp | `clamp(x, min, max)` |
| map range | `map_range(x, in_min, in_max, out_min, out_max)` |
| remap | `remap01(x, min, max)` |
| math | `+`, `-`, `*`, `/`, `pow`, `sqrt`, `abs`, `floor`, `ceil`, `round` |
| compare | `==`, `!=`, `>`, `>=`, `<`, `<=` |
| logic | `and`, `or`, `not` |
| condition | `if(cond, a, b)` |
| format | `format_number(value, decimals)` |
| vector | `vec2`, `vec3`, `dot`, `length`, `normalize` |
| color | `mix_color(a,b,t)`, `with_alpha(color,a)` |
| string | `concat`, `format`, `contains` |

Formula MVP should avoid arbitrary JavaScript strings. The formula should be parsed into safe expression nodes or selected from operators.

## 9. Dependency graph

Every binding creates a dependency edge.

```text
source parameter -> formula/transform -> target parameter
```

DependencyGraph stores:

```text
nodes: parameter paths / local data paths / graph outputs
edges: binding ids
status: clean / dirty / error / orphan
last_update
validation state
```

When a source changes:

```text
mark source dirty
find dependent bindings
validate source value
evaluate formula
validate target value
emit command or computed output
mark targets dirty
rerender affected views
```

## 10. Dirty propagation

Dirty flags should distinguish domains:

```text
visual_dirty
layout_dirty
interaction_dirty
binding_dirty
validation_dirty
export_dirty
graph_dirty
debug_dirty
```

Example:

```text
Slider.value changed
-> visual_dirty for Fill_Rectangle and Handle_Group
-> text_dirty for Value_Text
-> binding_dirty for dependent values
-> validation_dirty if out of range
```

## 11. Cycle detection

Cycles must be detected before execution.

Example invalid cycle:

```text
A.value -> B.value
B.value -> C.value
C.value -> A.value
```

Allowed controlled cycle patterns:

```text
canonical parameter + guarded input/output bindings
event-triggered binding with source guard
state memory node with explicit delay/tick
```

Cycle policy:

| Situation | Result |
|---|---|
| direct cycle without guard | block and show error |
| cycle through formula | block and show source chain |
| user intentionally creates feedback loop | requires explicit state/memory/delay node later |
| canonical two-way control | allowed if input/output directions are separated |

## 12. Binding UI / creation flow

### Quick binding

Use for simple direct links.

```text
select source parameter
click/link drag to target parameter
choose transform: direct / format / map_range / clamp
preview result
save binding
```

### Formula binding

Use for more complex relations.

```text
select target parameter
open Binding panel
add sources
choose formula operators
preview resolved value
validate
save
```

### Graph binding

Use in Logic/Events workspace.

```text
event/value node -> formula/action node -> command/output node
```

## 13. Binding inspector

The inspector should show:

```text
binding id
source path
target path
formula / transform
current source value
resolved value
last update
dirty state
validation status
cycle status
source chain
used times
```

Example row:

```text
source: Slider_Handle.local_x
target: Slider.value
transform: map_range(local_x, 0..track.width -> 0..1)
resolved_value: 0.63
status: OK
```

## 14. Debug spreadsheet

Recommended columns:

| Column | Meaning |
|---|---|
| `id` | binding id |
| `enabled` | active/inactive |
| `source_path` | source parameter |
| `target_path` | target parameter |
| `formula` | transform/expression |
| `source_value` | current input |
| `resolved_value` | computed output |
| `dirty` | needs recompute |
| `last_update` | timestamp |
| `status` | OK/warning/error/orphan/cycle |
| `source_chain` | full chain |
| `command_output` | command emitted if persistent |

## 15. Conflict rules

Multiple bindings may target the same parameter.

Conflict resolution:

| Source | Priority suggestion |
|---|---:|
| locked explicit override | 1000 |
| local user edit | 900 |
| dictated panel rule | 800 |
| active state override | 700 |
| graph runtime output | 650 |
| component exposed parameter | 600 |
| binding/formula output | 500 |
| preset/theme/inherited/default | lower |

If two bindings with similar priority target the same value:

```text
show conflict warning
block if both are persistent and incompatible
allow if they affect different subfields
allow if one is preview/runtime only
```

## 16. Value editing and reverse binding

Example: numeric field next to slider.

Recommended model:

```text
NumericInput.commit
-> parse number
-> clamp to slider min/max
-> set Slider.value
-> output bindings update handle/fill/text
```

Do not directly set handle position from the numeric input if the slider has canonical value. Otherwise the system creates fragile parallel truths.

## 17. Local data source binding

Local data sources are allowed later. They should enter the same binding model.

Example:

```text
localData.salesCsv.rows[3].sales
-> map_range(0..max_sales -> 0..chartHeight)
-> ChartBar_03.transform.height
```

The data source does not directly draw a chart. It feeds bound parameters of UI objects.

## 18. Persistence

Bindings are persisted in project JSON or component JSON.

Store:

```text
binding id
source path
target path
formula / operator graph
update mode
direction
priority
conflict policy
validation rules
metadata
```

Do not store:

```text
runtime hover value
current animation frame
temporary formula preview result
spatial cache
```

## 19. Roadmap placement

| Feature | Maturity start | Target |
|---|---|---|
| direct value binding | L1/L2 | L3 after component proof |
| slider canonical value binding | L2/L3 | panel/control proof |
| formula parser/operator graph | L1/L2 | later Logic workspace |
| binding debug spreadsheet | L2 | debug workspace |
| cycle detection | L1 | required before formulas are editable |
| local data bindings | L1/L2 | after local data source system |
| graph visual binding editor | L2/L3 | state graph workspace |

## 20. Acceptance rule

The binding system is valid when:

```text
source and target are stable parameter paths
canonical values prevent circular UI truth
formulas are safe and inspectable
changes propagate through dirty flags
persistent changes emit commands
cycles are detected and blocked
debug view shows source_chain and resolved_value
```

## 21. Final decision

Bindings should be treated as a dependency graph over Project parameters and local data paths. They should not be hidden callbacks inside UI components. This keeps sliders, panels, formulas, data sources and state graph outputs compatible with one model.
