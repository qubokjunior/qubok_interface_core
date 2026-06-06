# 14 — Curve node spatial routing awareness

Status: draft 00
Scope: procedural `curve_node` as state graph edge, spatial routing awareness, node/edge avoidance, attraction/repulsion, bundling, signal overlay
Mode: graph viewport / state_graph workspace / local debug

## 1. Purpose

This document updates the `curve_node` concept.

`curve_node` is not only a procedural curve between two sockets. It is a spatially-aware connector in the `state_graph` workspace. It can generate a rendered path based on socket anchors, manual dots, routing rules, other node bounding boxes, other edge paths, lane preferences and debug/runtime signal overlays.

Short definition:

```text
curve_node = procedural, debug-visible and spatially-aware StateGraphEdge.
```

## 2. Classification

`curve_node` should be classified as:

```text
StateGraphEdge / graph connector / graph viewport object
```

It should not be classified as:

```text
ordinary canvas primitive
ordinary panel object
ordinary region
ordinary UI layout object
```

It may reuse geometry/path utilities from `curve` and `path`, but its owner is the state graph model.

## 3. Layer model

```text
StateGraphEdge
  semantic connection between sockets
  source/target references
  connection type and data type
  routing policy
  manual dots
  style/decorations/signal settings
  validation status

GraphRoutingContext
  runtime/spatial data for graph viewport
  node bbox map
  socket anchor map
  existing edge paths
  obstacle map
  density field
  lane map
  bundle groups

GraphRoutingSolver
  computes resolved path
  avoids hard obstacles
  applies soft edge repulsion
  applies attraction/bundling
  reduces crossings
  caches path

EdgeRenderer
  renders base path
  renders decorations
  renders hit region
  renders signal segment
  renders debug overlay
```

## 4. Why awareness does not change ownership

Spatial awareness does not make `curve_node` a general canvas layout object. It only means the routing solver computes its path using a wider graph context.

Incorrect model:

```text
edge_A directly knows edge_B and mutates itself when B changes
```

Correct model:

```text
edge_A has routing policy
GraphRoutingContext reads all nodes/edges
GraphRoutingSolver computes edge_A resolved path
edge_A stores or receives resolved path cache
renderer displays result
```

This prevents unstable feedback loops where edges continuously push each other without central arbitration.

## 5. Main data entities

| Entity | Meaning |
|---|---|
| `StateGraphEdge` | logical connection between sockets |
| `curve_node` | visual/procedural representation of the edge |
| `socket_anchor` | start/end point derived from socket position/direction |
| `dot` | manual control point created by user |
| `route_control_point` | generated helper point |
| `edge_path` | resolved sampled path for render/hit/signal |
| `GraphRoutingContext` | derived graph spatial environment |
| `GraphRoutingSolver` | routing algorithm/heuristic |
| `signal_segment` | moving path segment representing runtime flow |
| `flow_marker` | arrow/icon instance on path |
| `bundle_group` | group of edges routed together |
| `route_lane` | preferred corridor through graph workspace |

## 6. Persisted edge data

Store in graph/project:

```text
edge_id
source_node_id
source_socket_id
target_node_id
target_socket_id
connection_kind
data_type
routing_mode
routing_awareness settings
manual dots
style preset
signal settings
decoration preset
validation status compact
```

Optional cache:

```text
resolved_path cache
cache_hash
solver_cost
last_routed_at
```

Do not store as primary truth:

```text
spatial index
full density field
temporary force iteration state
runtime signal offset unless recorded replay
hover/candidate debug overlay state
```

## 7. Routing awareness settings

Recommended type:

```ts
export type CurveNodeRoutingAwareness = {
  awareness_enabled: boolean;

  avoid_node_bboxes: boolean;
  avoid_edge_paths: boolean;
  avoid_labels: boolean;

  node_padding_px: number;
  edge_padding_px: number;

  edge_repulsion_strength: number;
  edge_attraction_strength: number;
  crossing_penalty: number;

  bundle_enabled: boolean;
  bundle_group_rule:
    | "none"
    | "same_data_type"
    | "same_source_node"
    | "same_target_node"
    | "same_source_and_target"
    | "same_tag"
    | "same_connection_kind";

  route_lane_policy:
    | "none"
    | "prefer_horizontal"
    | "prefer_vertical"
    | "orthogonal_lanes"
    | "auto_lanes";

  manual_dots_policy:
    | "manual_wins"
    | "solver_may_adjust"
    | "manual_as_soft_constraint";

  reroute_mode:
    | "on_node_move"
    | "on_edge_create"
    | "on_graph_idle"
    | "manual_only"
    | "force_simulate";

  route_cache_policy:
    | "none"
    | "cache_until_dirty"
    | "cache_with_debug";
};
```

## 8. Hard obstacles, soft obstacles and fields

| Type | Examples | Solver behavior |
|---|---|---|
| `hard_obstacle` | node bbox, locked area, socket body | route should not pass through |
| `soft_obstacle` | other edge path, label, debug marker | route may pass but cost increases |
| `repulsor` | unrelated edge/path/dense area | route moves away |
| `attractor` | similar edge/type/source | route may move toward bundle |
| `route_lane` | preferred corridor | route prefers lane |
| `manual_override` | user dot | user intent wins or becomes strong constraint |

Important distinction:

```text
node bbox = obstacle
other curve_node = field influence unless explicitly locked
manual dot = user intent
```

## 9. Routing modes

| Mode | Description | Awareness level |
|---|---|---|
| `debug_direct` | straight line | none/minimal |
| `bezier_soft` | soft cubic curve | low |
| `segmented` | hard polyline | medium |
| `orthogonal` | 90-degree path | medium/high |
| `rope` | hanging/soft cable | low/visual |
| `hybrid_auto` | auto route with manual dots | high |
| `aware_bezier` | Bezier with obstacle/edge awareness | high |
| `aware_orthogonal` | lanes + node avoidance | high |
| `bundled` | shared routes for similar edges | high |
| `force_relaxed` | post-solver relaxation | advanced |
| `manual_locked` | only anchors and manual dots | user-controlled |

## 10. Solver pipeline

```text
edge dirty
-> read source/target socket anchors
-> read manual dots
-> build GraphRoutingContext
-> classify obstacles/fields
-> choose initial route by routing_mode
-> avoid hard obstacles
-> apply soft repulsion from other edges
-> apply attraction/bundling for matching edges
-> reduce crossings
-> smooth / segment / quantize path
-> compute tangents/normals/length
-> cache resolved path
-> render base path + decorations + signal + debug overlay
```

## 11. Node bbox awareness

Node bboxes should be treated as hard obstacles by default.

Fields:

```text
avoid_node_bboxes: true
node_padding_px: 12
respect_socket_ports: true
avoid_node_labels: optional
```

If no route exists:

```text
validation_status = blocked or degraded
show route error/warning
allow direct fallback only in debug/direct mode
```

## 12. Edge-to-edge awareness

Other edges are normally soft obstacles.

Use cases:

```text
reduce visual overlap
avoid crossing important command edges
bundle same type connections
push unrelated edges apart
keep debug signal readable
```

Do not solve edge awareness by each edge directly modifying every other edge. Use a shared solver/context.

## 13. Edge attraction / repulsion

Better technical names:

```text
edge_attraction
edge_repulsion
edge_bundling
route_relaxation
crossing_penalty
```

User-facing labels can be:

```text
Avoid edges
Attract similar
Bundle cables
Reduce crossings
Relax route
```

## 14. Bundling

Bundling groups similar edges into cable-like paths.

Bundle rules:

```text
same_data_type
same_source_node
same_target_node
same_source_and_target
same_connection_kind
same_tag
manual bundle group
```

Bundle behavior:

```text
shared trunk path
separated ends near sockets
parallel offset by index
color/style preserved per edge
hover/select can isolate one edge
```

## 15. Rope / hanging cable mode

Rope mode is mostly visual. It makes edges feel like hanging lines between points.

Parameters:

```text
sag_amount
tension
gravity_direction
anchor_strength
manual_dot_weight
smoothing
```

Recommended use:

```text
organic/visual graphs
non-critical debug views
style preset for aesthetic cable graphs
```

Avoid using rope mode for dense technical graphs unless readability remains high.

## 16. Sharp segmented line mode

Sharp mode creates technical hard lines.

Parameters:

```text
segment_mode: direct / orthogonal / stepped
corner_mode: hard / rounded
corner_radius
lane_snap
angle_step: 45 or 90 degrees
```

Use cases:

```text
logic flows
conditions
command paths
debug diagrams
high-readability state machines
```

## 17. Decorations with instance_on_points

Decorations are not the signal. They are icons/markers placed on the resolved edge path.

Examples:

```text
arrows showing direction
small type icons: float/vector/object/file
warning markers
breakpoints
tick marks
activity dots
bundle markers
```

Parameters:

```text
instance_shape_id
placement_mode: by_count / by_spacing / factor_list / midpoint / near_target
count
spacing_px
factor_list
orient_to_tangent
normal_offset
scale_by_zoom
inherit_edge_color
show_when_selected
```

## 18. Signal overlay

Signal is a moving segment of the edge path, not `instance_on_points`.

```text
edge_path:        ───────────────────────────
signal_segment:             [======]
movement:        0.00 -> 0.25 -> 0.50 -> 1.00
```

Signal fields:

```text
signal_enabled
signal_mode: off / realtime / simulate / force_simulate / recorded_replay / step_debug
signal_length
signal_speed
signal_easing
signal_style_preset
signal_repeat
signal_spawn_policy
signal_value_binding
signal_debug_label
signal_layer
```

`force_simulate` must not mutate the graph/project. It is a debug/runtime overlay only.

## 19. Edge presets by data type

| Data type | Visual preset idea |
|---|---|
| `float` | thin soft green line with single signal |
| `integer` | slightly segmented line |
| `boolean` | stepped binary dash |
| `vector2` | double marker line |
| `vector3` | triple tick marker |
| `vector4` | four mini markers |
| `color` | gradient core or color ribbon |
| `matrix` | grid-like technical line |
| `string` | dotted/text marker line |
| `array/list` | multi-strand/bundled line |
| `object` | solid reference line with object icon |
| `file` | line with file/folder icon |
| `event` | bright impulse line |
| `command` | thick directed line |
| `debug` | dashed low-opacity diagnostic line |

Presets should be local theme/preset assets later.

## 20. Edge states

| State | Meaning |
|---|---|
| `normal` | valid connection |
| `hover` | pointer over edge/hit path |
| `selected` | selected edge |
| `editing` | manual dots visible |
| `active` | currently executing/recently executed |
| `simulated` | showing simulated signal |
| `muted` | connection disabled but present |
| `disabled` | not active |
| `invalid` | broken/invalid connection |
| `warning` | degraded but usable |
| `ghost` | being created by drag |
| `candidate` | potential target socket |
| `blocked` | incompatible target/type |
| `recalculating` | solver updating route |
| `cached` | path loaded from valid route cache |

## 21. Edge creation workflow

```text
user drags from source socket
-> create ghost_curve_node
-> source anchor fixed to socket
-> target follows cursor
-> target resolver finds socket under cursor
-> validate socket compatibility
-> route ghost with lightweight solver
-> show candidate/blocked style
-> pointer up
   -> valid: stateGraph.edge.create
   -> invalid: cancel or open node search menu
-> reroute edge with full solver
-> update inspector/debug
```

## 22. Edge editing workflow

```text
click edge -> select edge
double click segment -> insert dot
drag dot -> move manual point
alt click dot -> delete dot
drag segment -> add/move route dot
right click -> context menu
select edge + inspector -> edit routing/style/signal
```

Manual dot policy decides whether solver can move those points.

## 23. Inspector for curve_node

Tabs:

```text
Identity
Source/Target
Data Type
Routing
Awareness
Style
Decorations
Signal
Validation
Debug Metrics
```

Important fields:

```text
routing_mode
effective_resolution
avoid_node_bboxes
avoid_edge_paths
edge_repulsion_strength
edge_attraction_strength
crossing_penalty
bundle_enabled
route_lane_policy
manual_dots_policy
reroute_mode
route_cache_policy
signal_mode
last_execution_value
solver_cost
```

## 24. Graph routing debug overlay

Overlay modes:

| Overlay | Shows |
|---|---|
| `node_obstacles` | node bbox hard obstacles |
| `edge_density` | edge congestion field |
| `repulsion_field` | force away from other edges |
| `attraction_field` | attraction to bundles |
| `route_lanes` | preferred routing corridors |
| `crossing_points` | route intersections |
| `manual_dots` | user points and locks |
| `solver_cost` | route cost score |
| `route_cache` | cached/dirty state |
| `dirty_edges` | edges needing recompute |
| `signal_overlay` | active simulated/runtime signals |

## 25. Validation

Checks:

```text
source node exists
target node exists
source socket exists
target socket exists
output -> input direction valid
data types compatible or converter available
cycles allowed only where graph permits
manual dots valid
resolved path finite
signal speed valid
style preset exists
instance decoration shape exists
route does not pass through hard obstacles unless degraded/direct mode allows it
```

Error states:

```text
blocked route
invalid socket
missing target
type mismatch
cycle detected
path NaN/Infinity
invalid dots
preset missing
cache stale
```

## 26. Commands

Persistent commands:

```text
stateGraph.edge.create
stateGraph.edge.delete
stateGraph.edge.patch
stateGraph.edge.set_source
stateGraph.edge.set_target
stateGraph.edge.set_routing_mode
stateGraph.edge.insert_dot
stateGraph.edge.move_dot
stateGraph.edge.delete_dot
stateGraph.edge.set_style_preset
stateGraph.edge.set_signal_settings
stateGraph.edge.set_decoration_preset
stateGraph.routing.recompute
stateGraph.routing.clear_cache
```

Runtime/debug actions:

```text
stateGraph.edge.preview_signal
stateGraph.edge.stop_signal_preview
stateGraph.execution.replay_edge
stateGraph.execution.step_edge
stateGraph.debug.flash_edge
stateGraph.routing.show_overlay
```

Runtime/debug actions should not persistently mutate Project/Graph unless explicitly saved.

## 27. Architecture modules

Recommended modules:

```text
core/stateGraph/
  graphTypes.ts
  edgeTypes.ts
  socketTypes.ts
  graphValidation.ts
  graphExecution.ts
  routing/
    graphRoutingTypes.ts
    graphRoutingContext.ts
    graphSpatialIndex.ts
    graphObstacleMap.ts
    graphRouteLanes.ts
    graphEdgeBundling.ts
    graphRoutingSolver.ts
    graphRoutingCache.ts
    graphRoutingValidation.ts

core/geometry/
  pathSampling.ts
  pathLength.ts
  pathTangents.ts
  pathIntersections.ts
  bboxIntersections.ts
  polylineRouting.ts
  bezierRouting.ts

creator/workspaces/LogicEventsWorkspace/
  GraphViewport.tsx
  GraphNodeLayer.tsx
  GraphEdgeLayer.tsx
  GraphEdgeDebugOverlay.tsx
  GraphRoutingDebugPanel.tsx

creator/panels/
  CurveNodeInspector.tsx
  GraphRoutingInspector.tsx
```

## 28. Roadmap placement

| Level | Feature |
|---|---|
| L1 headless | edge types, routing awareness type, geometry utilities, validation |
| L2 debug | render edge, ghost edge, hit path, debug overlay, route metrics |
| L3 graph workspace | drag socket to create edge, manual dots, basic avoidance |
| L4 polished | inspector, stable route cache, crossing reduction, route lanes |
| L5 advanced | bundling, attraction/repulsion fields, force relaxation, replay timeline |

This should not be part of early default MVP. It belongs to the Logic/Events/StateGraph workspace.

## 29. Acceptance rule

`curve_node` routing is valid when:

```text
edge identity is stored as StateGraphEdge
render path is derived from source/target + routing policy
manual dots persist
node bbox avoidance works as hard obstacle
other edges influence routing through solver/context
signal is a moving path segment, not point instances
instance_on_points decorations follow tangent direction
validation explains blocked/degraded routes
graph commands mutate graph model, not DOM directly
```

## 30. Final decision

Spatial awareness strengthens the `curve_node` concept but does not change its architectural home. It remains a state graph edge. Awareness is implemented through `GraphRoutingContext` and `GraphRoutingSolver`, not by turning every edge into an autonomous object that manually tracks every other edge.
