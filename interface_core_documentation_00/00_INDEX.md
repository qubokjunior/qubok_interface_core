# QUBOK_INTERFACE_CORE — documentation index

Status: working documentation folder
Created: 2026-06-06
Last update: 2026-06-11 — editorial pass / condensed canonical index
Repository: `qubok_interface_core`

## Purpose

`interface_core_documentation_00` contains working documentation for `qubok_interface_core`: architectural decisions, terminology, roadmap, visual standards, feature specs and development protocols.

The project is a parametric interface engine, not a normal UI kit. The full canonical definition is in `00_CURRENT_SOURCE_OF_TRUTH.md`.

## Read first

| Order | File | Role |
|---:|---|---|
| 00 | `00_CURRENT_SOURCE_OF_TRUTH.md` | current canon, scope, source-of-truth, boundaries, MVP, precedence |
| 01 | `01_TERMINOLOGY_CANON_NODE_GRAPH.md` | final graph naming: `node_graph`, legacy handling for `state_graph` |
| 02 | `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md` | Phase A-O implementation order vs Screen State 01-08 visual states |
| 03 | `03_FEATURE_MATURITY_MATRIX.md` | feature maturity levels L0-L5 and promotion gates |
| 04 | `04_CODEX_DEVELOPMENT_PROTOCOL.md` | required structure for Codex/new-chat implementation prompts |

## Core rules

| Area | Rule |
|---|---|
| Source of truth | `Project JSON / data model` owns persistent state. |
| Mutation path | Persistent changes go through command layer. |
| Naming | `node_graph` is canonical; `state_graph` is legacy. |
| Shape/region split | Shape renders; region handles interaction/layout behavior. |
| Panel concept | Panel is a structured component, not a single rectangle. |
| Layout separation | app shell docking, canvas object layout and graph viewport layout are separate systems. |
| MVP | Default MVP stays focused and readable; advanced systems remain schema/workspace gated. |
| Documentation | Each feature should have phase, maturity level, owner, command path, validation and tests. |

## Pipeline summary

```text
primitive -> bbox -> transform/style -> region/layout -> group/panel -> exposed parameters -> links/references/rules -> library asset -> reusable component -> generated helpers -> event/action/node_graph behavior -> workspace
```

## Implementation roadmap

| Phase | Focus |
|---|---|
| A | Foundation / glossary / maturity / scope |
| B | Application shell |
| C | Core model + command layer |
| D | Canvas + viewport |
| E | Primitives + selection |
| F | Inspector + hierarchy sync |
| G | BBox + regions + preview modes |
| H | Component proof + save/export |
| I | Default MVP cleanup |
| J | Component library |
| K | Layout / panel builder / size policies |
| L | App shell docking |
| M | Headless events/actions/references/rules |
| N | Node graph workspace |
| O | Advanced procedural systems |

## Visual roadmap states

Screen State 01-08 are visual documentation states, not strict sprint tasks.

| State | Purpose |
|---|---|
| 01 | Foundation / model / primitive |
| 02 | Canvas / primitive / inspector sync |
| 03 | Regions / bbox / interaction debug |
| 04 | Spreadsheet / filters / parameter visibility |
| 05 | Layers / hierarchy / tags / style sorting |
| 06 | Panel builder / component structure / states |
| 07 | Actions / events / command layer / node_graph |
| 08 | Docking / pinning / export / final MVP view |

## Reference documents

| File | Use when working on |
|---|---|
| `10_atlas_value_rules_references_instances.md` | parameter links, references, rules, size policies, instance-on-points, preview modes |
| `11_VISUAL_DIAGNOSTICS_AND_STYLE_STANDARD_2026_06_10.txt` | visual output quality, UI screenshot standards, node graph visuals, bbox debug |
| `12_RULES_STATES_RELATIONS_PROCEDURAL_UI_2026_06_10.txt` | rules, states, responsiveness, value relations, relation graph, shape graph, array |
| `13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt` | registries, customization, tokens, Preview Service, Node Adapter Registry |
| `14_DEBUG_DOCKING_WORKSPACE_KERNEL_GRAPH_PROFILES_2026_06_10.txt` | debug command, docking ownership, Workspace Kernel, node_graph profiles |

## Precedence

When files conflict, follow:

`00_CURRENT_SOURCE_OF_TRUTH.md` -> `01_TERMINOLOGY_CANON_NODE_GRAPH.md` -> `02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md` -> `03_FEATURE_MATURITY_MATRIX.md` -> `04_CODEX_DEVELOPMENT_PROTOCOL.md` -> references `10-14` -> older notes/prompts.
