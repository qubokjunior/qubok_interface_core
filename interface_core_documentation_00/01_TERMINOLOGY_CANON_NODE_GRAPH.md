# QUBOK_INTERFACE_CORE — TERMINOLOGY CANON: NODE_GRAPH

Version: 2026-06-11
Status: canonical terminology decision

## Decision

`node_graph` is the canonical term for the graph system.

Use it for: visual graph editor, runtime, viewport, nodes, profiles, output contracts, workspace, palette and node adapters.

## Naming table

| Legacy/deprecated | Canonical |
|---|---|
| `state_graph` | `node_graph` |
| `state graph` | `node_graph` |
| `StateGraph` as new public class/system name | `NodeGraph` |
| `stateGraph` as new public variable/module name | `nodeGraph` |

Older documents may still contain legacy wording. Read it as `node_graph` unless it clearly describes visual/interactivity states.

## Valid `state` usage

`state` remains valid for component/object variants:

| Valid examples | Meaning |
|---|---|
| `states_slot`, `state_variant`, `state_transition`, `state_resolver` | state system data |
| `hover_state`, `pressed_state`, `disabled_state`, `dirty_state` | interaction/visual states |
| `state_change` | valid graph output type |

A button has states. The graph system is `node_graph`.

## Concept split

| Concept | Meaning |
|---|---|
| `state` | visual/interactivity/runtime variant of an object/component |
| `transition` | interpolation/timing between states |
| `node_graph` | graph editor + graph runtime |
| `node_graph profile` | domain-specific graph mode |
| `node_graph output` | required emitted result |

## Profiles and outputs

| Profile | Purpose | Required output |
|---|---|---|
| `interaction` | pointer, keyboard, hover, drag reactions | `command` or `state_change` |
| `rules` | conditional decisions | `decision` or `property_override` |
| `relations` | value drivers and parameter links | `value_change` |
| `animation` | time-based values and transitions | `timeline_value` or `transition_result` |
| `object_create` | procedural object creation | `object_create_command` |
| `shape` | procedural shape generation | `shape_output` |
| `signal` | pulse/realtime/simulation signal | `signal_output` |
| `debug` | diagnostics and checks | `validation_result` |

Every `node_graph` must have a visible named output. If no output exists, the graph is invalid.

## Visual standard

A `node_graph` view should use:

- left input sockets, right output sockets,
- named and typed sockets,
- selected outline for active node,
- orthogonal routing, reroute points and wire lanes,
- left-to-right flow,
- no wires through node body, labels or socket rows,
- visible output node or output panel.

## Recommended new code names

Prefer:

- `src/core/nodeGraph/`
- `NodeGraphWorkspace.tsx`
- `nodeGraphTypes.ts`
- `nodeGraphProfileTypes.ts`
- `nodeGraphOutputContracts.ts`
- `nodeAdapterRegistry.ts`

Do not create new public APIs using old `stateGraph` naming. Migrate legacy code only in a dedicated cleanup sprint.
