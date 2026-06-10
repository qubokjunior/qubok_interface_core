# QUBOK_INTERFACE_CORE — ROADMAP PHASE TO SCREEN STATE MAP

Version: 2026-06-11
Status: canonical roadmap clarification

## Purpose

The project uses two roadmap views:

1. `Phase A-O` — implementation and dependency order.
2. `Screen State 01-08` — visual documentation states of the fullscreen application.

These two views are complementary, not interchangeable.

Use `Phase A-O` for development planning, Codex prompts, task splitting and acceptance gates.
Use `Screen State 01-08` for screenshots, interface mockups, infographic prompts and visual communication.

## Canonical implementation phases

| Phase | Name | Goal | Gate |
|---|---|---|---|
| A | Foundation | glossary, terminology, MVP/post-MVP split, maturity levels | no feature enters default UI without maturity status |
| B | Scaffold + shell | stable fullscreen app shell | top/left/center/right/bottom/status layout exists |
| C | Core model + commands | source of truth and mutation path | model can be changed without React UI |
| D | Canvas + viewport | render model with grid, pan, zoom | viewport does not mutate object transforms |
| E | Primitive + selection | create and edit primitive objects | object appears in canvas, hierarchy and inspector |
| F | Inspector + hierarchy | sync active object across views | canvas, hierarchy, inspector and status agree |
| G | BBox + regions + preview modes | separate visual/layout/interaction and overlays | rectangle renders, region reacts |
| H | Component proof | prove primitive -> group -> component | button group and Panel_Monitor survive save/export |
| I | Default MVP cleanup | make default UI clear and concept-aligned | debug and graph do not dominate default screen |
| J | Component library | save and instantiate reusable assets | new IDs, preview and metadata work |
| K | Layout / panel builder | build structured panels faster | panel is structure, not a single rectangle |
| L | App shell docking | split/merge/resize app panels | docking does not touch canvas object layout |
| M | Headless events/actions/references/rules | prepare logic without graph UI dominance | events/actions resolve targets and emit commands |
| N | Node graph workspace | full node graph editor | graph has pan/zoom, ports, profiles and output contract |
| O | Advanced systems | procedural shapes, array, instance-on-points, bridges | feature gated and not default chaos |

## Canonical screen states

| Screen State | Visual purpose | Typical phases represented |
|---|---|---|
| 01 | Foundation / model / primitive visible in full shell | A-B-C-E |
| 02 | Canvas / primitive / inspector sync | D-E-F |
| 03 | Regions / bbox / interaction debug | G |
| 04 | Spreadsheet / filters / parameter visibility | F-G-M |
| 05 | Layers / hierarchy / tags / style sorting | F-G-J |
| 06 | Panel builder / component structure / states | H-J-K + reserved state schema |
| 07 | Actions / events / command layer / node_graph | M-N |
| 08 | Docking / pinning / export / final MVP view | I-L + H-J |

## Mapping table

| Implementation phase | Primary screen state | Secondary visual references |
|---|---|---|
| A Foundation | 01 | none |
| B Scaffold + shell | 01 | 08 for final shell target |
| C Core model + commands | 01 | 02 for visible sync proof |
| D Canvas + viewport | 02 | 03 for overlays |
| E Primitive + selection | 02 | 01 for initial simple objects |
| F Inspector + hierarchy | 02 | 04 and 05 |
| G BBox + regions + preview modes | 03 | 04 |
| H Component proof | 06 | 08 for export proof |
| I Default MVP cleanup | 08 | all states as visual consistency check |
| J Component library | 06 | 08 |
| K Layout / panel builder | 06 | 03 for region overlays |
| L App shell docking | 08 | none |
| M Headless events/actions/references/rules | 04 or 07 | 06 for rules/states on components |
| N Node graph workspace | 07 | 03 for target/debug overlays |
| O Advanced systems | 06 or 07 | depends on subsystem |

## Development rule

A Codex/development task must name the implementation phase, not only the screen state.

Bad:

```text
implement screen 07
```

Good:

```text
Implement Phase M: headless event/action registry target resolver.
Use Screen State 07 only as visual reference.
Do not implement full node_graph editor yet.
```

## Visual documentation rule

A visual prompt can name the screen state, but must not imply that all functions shown are already MVP-ready.

Bad:

```text
show final MVP with state graph, docking, array, relations, rules all working
```

Good:

```text
show Screen State 07 as node_graph workspace concept.
Mark graph as separate workspace and show command output contract.
Keep default interface creator clean.
```

## Scope safety rule

When a screen state shows multiple systems, the implementation task still selects one system.

Example:

Screen State 08 may show docking, pinned preview, export, validation and active canvas selection.
A sprint may implement only docking shell split/merge. Export and preview remain existing context, not part of the task.

## Current priority order

For actual development, prioritize:

1. source of truth model,
2. command layer,
3. canvas / hierarchy / inspector sync,
4. bbox and region separation,
5. component proof,
6. save/export,
7. default UI cleanup,
8. component library,
9. panel builder,
10. app shell docking,
11. headless events/actions/rules/references,
12. node_graph workspace,
13. advanced procedural systems.
