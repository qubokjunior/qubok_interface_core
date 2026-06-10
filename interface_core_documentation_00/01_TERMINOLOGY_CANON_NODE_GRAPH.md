# QUBOK_INTERFACE_CORE — TERMINOLOGY CANON: NODE_GRAPH

Version: 2026-06-11
Status: canonical terminology decision

## Final decision

`node_graph` is the final canonical term for the graph system.

Use `node_graph` for:

- visual graph editor,
- graph runtime,
- graph viewport,
- graph nodes,
- graph profiles,
- graph output contracts,
- graph workspace,
- graph palette and node adapters.

## Deprecated names

| Deprecated | Replacement |
|---|---|
| `state_graph` | `node_graph` |
| `state graph` | `node_graph` |
| `StateGraph` as new class/system name | `NodeGraph` |
| `stateGraph` as new variable/module name | `nodeGraph` |

Older documents may still contain `state_graph`. Treat this as a legacy alias for `node_graph` unless the text clearly talks about visual states such as hover, pressed or disabled.

## What remains valid

The word `state` remains valid for object/component states:

- `states_slot`,
- `state_variant`,
- `state_transition`,
- `state_resolver`,
- `hover_state`,
- `pressed_state`,
- `disabled_state`,
- `dirty_state`,
- `state_change` output.

A button has states. A graph system is `node_graph`.

## Concept split

| Concept | Meaning |
|---|---|
| `state` | visual/interactivity/runtime variant of an object or component |
| `transition` | interpolation or timing between states |
| `node_graph` | graph editor and graph runtime system |
| `node_graph profile` | filtered graph mode for a domain |
| `node_graph output` | required emitted result of the graph |

## Node graph profiles

The graph editor is one system with multiple profiles.

| Profile | Purpose | Required output |
|---|---|---|
| `interaction` | pointer, keyboard, hover, drag reactions | `command` or `state_change` |
| `rules` | conditional decisions | `decision` or `property_override` |
| `relations` | value drivers and parameter links | `value_change` |
| `animation` | time-based values and transitions | `timeline_value` or `transition_result` |
| `object_create` | procedural object creation | `object_create_command` |
| `shape` | procedural shape generation | `shape_output` |
| `signal` | pulse, realtime or simulation-like signal | `signal_output` |
| `debug` | diagnostics and checks | `validation_result` |

## Output contract rule

Every node graph must have a visible named output. If no output exists, the graph is invalid.

Valid output classes:

- `command`,
- `state_change`,
- `property_override`,
- `value_change`,
- `shape_output`,
- `asset`,
- `target`,
- `validation_result`,
- `signal_output`.

## Visual standard

Every node graph view should use:

- input sockets on the left,
- output sockets on the right,
- named and typed sockets,
- selected outline for active node,
- orthogonal routing,
- reroute points,
- wire lanes,
- left-to-right flow,
- no wires through node body, labels or socket rows,
- visible output node or output panel.

## Migration rule

Legacy wording should be read as:

| Legacy wording | Canonical reading |
|---|---|
| `state_graph system` | `node_graph system` |
| `state_graph workspace` | `node_graph workspace` |
| `state_graph viewport` | `node_graph viewport` |
| `state_graph emits commands` | `node_graph emits commands` |

State-related terms remain unchanged when they describe component states.

## Recommended new file/module names

Prefer:

- `src/core/nodeGraph/`,
- `NodeGraphWorkspace.tsx`,
- `nodeGraphTypes.ts`,
- `nodeGraphProfileTypes.ts`,
- `nodeGraphOutputContracts.ts`,
- `nodeAdapterRegistry.ts`.

Avoid creating new public APIs using the old `stateGraph` naming.
