# QUBOK_INTERFACE_CORE — FEATURE MATURITY MATRIX

Version: 2026-06-11
Status: canonical maturity gate

## Purpose

Every feature must declare maturity before implementation, UI exposure, prompt usage or default workflow placement.

## Levels

| Level | Name | Meaning | Default UI visibility |
|---|---|---|---|
| L0 | SPEC_ONLY | documented idea, no implementation | hidden |
| L1 | HEADLESS_CORE | model/types/functions, no polished UI | hidden or dev-only |
| L2 | DEBUG_VIEW | diagnostic/workbench view | collapsed/debug/workspace only |
| L3 | USER_WORKFLOW_MVP | usable in main workflow | visible when relevant |
| L4 | POLISHED_TOOL | stable UI, shortcuts, validation, docs | visible / production-ready |
| L5 | ADVANCED_COMPOSABLE | reusable as component, preset, graph profile or subsystem | workspace/palette/advanced |

Default interface creator should show L3/L4 and compact L2 status only. It should not be dominated by raw registries, workbenches, full `node_graph` before Phase N or advanced systems.

## Matrix

| Feature | Phase | Start | MVP target | Final |
|---|---|---:|---:|---:|
| Project model | C | L1 | L4 | L5 |
| InterfaceObject model | C | L1 | L4 | L5 |
| Command layer | C | L1 | L4 | L5 |
| Validation skeleton | C | L1 | L3 | L4 |
| Canvas renderer | D | L2 | L3 | L4 |
| Viewport pan/zoom/grid | D | L2 | L3 | L4 |
| Primitive creation | E | L2 | L3 | L4 |
| Selection / box select | E | L2 | L3 | L4 |
| Transform / resize | E | L2 | L3 | L4 |
| Inspector | F | L2 | L3 | L4 |
| Hierarchy / layers | F | L2 | L3 | L4 |
| visual_bbox | G | L1 | L3 | L4 |
| layout_bbox | G | L1 | L3 | L4 |
| interaction_region | G | L1 | L3 | L4 |
| Region debug overlay | G | L2 | L3 | L4 |
| Preview modes | G | L1/L2 | L2/L3 | L4 |
| Group/ungroup | H | L2 | L3 | L4 |
| button_group proof | H | L2 | L3 | L4 |
| Panel_Monitor sample | H | L2 | L3 | L4 |
| Save/load Project JSON | H | L1 | L3 | L4 |
| SVG export | H | L1 | L3 | L4 |
| Default MVP cleanup | I | L3 | L4 | L4 |
| Component library | J | L2 | L3 | L4/L5 |
| Layout snap/align/distribute | K | L2 | L3 | L4 |
| Box arranger | K | L1/L2 | L2/L3 | L4 |
| Panel builder | K | L1/L2 | L2/L3 | L4/L5 |
| App shell docking | L | L1/L2 | L3 | L4 |
| Event registry | M | L1 | L2/L3 | L4 |
| Action registry | M | L1 | L2/L3 | L4 |
| Target resolver | M | L1 | L2/L3 | L4 |
| Object references | M | L1 | L2 | L4 |
| Parameter links | M | L1 | L2 | L4/L5 |
| Rule sets | M | L1 | L2 | L4/L5 |
| States slot | M | L1 | L2/L3 | L4 |
| Value relations | M/O | L1 | L2 | L5 |
| Node graph workspace | N | L1/L2 | post-MVP L3 | L4/L5 |
| Node adapter registry | N | L1 | L2 | L5 |
| Relation graph | N/O | L0/L1 | L2 | L5 |
| Shape graph | N/O | L0/L1 | L2 | L5 |
| Responsiveness | O | L0/L1 | L2 | L4 |
| Array modifier | O | L0/L1 | L2 | L4/L5 |
| Instance on points | O | L0/L1 | L2 | L5 |
| Procedural icons | O | L0/L1 | L2 | L5 |
| External bridges | O | L0 | hidden | L5 |

## Promotion gates

| Promotion | Gate |
|---|---|
| L0 -> L1 | model slot named, owner known, persistence impact known, validation risks listed |
| L1 -> L2 | types/functions exist, core remains headless, validation path exists or is stubbed |
| L2 -> L3 | workflow clear, command layer used, sync preserved, default UI readable |
| L3 -> L4 | stable compact UI, visible warnings, coherent shortcuts/status, save/export behavior defined |
| L4 -> L5 | reusable as component, preset, `node_graph` action/profile, template, adapter or subsystem |

## Sprint annotation template

```text
Feature:
Phase:
Current maturity:
Target maturity:
Default UI visibility:
Touched model fields:
Touched commands:
Touched views:
Validation:
Manual tests:
Out-of-scope:
```
