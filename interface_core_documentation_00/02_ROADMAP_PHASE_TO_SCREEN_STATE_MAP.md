# QUBOK_INTERFACE_CORE — ROADMAP PHASE TO SCREEN STATE MAP

Version: 2026-06-11
Status: canonical roadmap clarification

## Purpose

The project uses two roadmap views:

| View | Role | Use for |
|---|---|---|
| Phase A-O | implementation/dependency order | development planning, Codex prompts, task splitting, acceptance gates |
| Screen State 01-08 | visual documentation states | screenshots, UI mockups, infographic prompts, visual feedback |

These views are complementary, not interchangeable.

## Implementation phases

| Phase | Name | Goal | Gate |
|---|---|---|---|
| A | Foundation | glossary, terminology, MVP/post-MVP split, maturity levels | no feature enters default UI without maturity status |
| B | Scaffold + shell | stable fullscreen app shell | top/left/center/right/bottom/status layout exists |
| C | Core model + commands | source of truth and mutation path | model can be changed without React UI |
| D | Canvas + viewport | render model with grid, pan, zoom | viewport does not mutate object transforms |
| E | Primitive + selection | create/edit primitives | object appears in canvas, hierarchy and inspector |
| F | Inspector + hierarchy | sync active object across views | canvas, hierarchy, inspector and status agree |
| G | BBox + regions + preview modes | separate visual/layout/interaction | rectangle renders, region reacts |
| H | Component proof | prove primitive -> group -> component | button group and Panel_Monitor survive save/export |
| I | Default MVP cleanup | concept-aligned default UI | debug and graph do not dominate default screen |
| J | Component library | save/instantiate reusable assets | new IDs, preview and metadata work |
| K | Layout / panel builder | build structured panels faster | panel is structure, not one rectangle |
| L | App shell docking | split/merge/resize app panels | docking does not touch canvas object layout |
| M | Headless events/actions/references/rules | logic without graph UI dominance | events/actions resolve targets and emit commands |
| N | Node graph workspace | full graph editor | pan/zoom, ports, profiles and output contract exist |
| O | Advanced systems | procedural shapes, array, instance-on-points, bridges | feature gated and not default chaos |

## Screen states

| Screen State | Visual purpose | Typical phases |
|---|---|---|
| 01 | Foundation / model / primitive in full shell | A-B-C-E |
| 02 | Canvas / primitive / inspector sync | D-E-F |
| 03 | Regions / bbox / interaction debug | G |
| 04 | Spreadsheet / filters / parameter visibility | F-G-M |
| 05 | Layers / hierarchy / tags / style sorting | F-G-J |
| 06 | Panel builder / component structure / states | H-J-K + reserved state schema |
| 07 | Actions / events / command layer / node_graph | M-N |
| 08 | Docking / pinning / export / final MVP view | I-L + H-J |

## Phase to screen mapping

| Phase | Primary screen state | Secondary visual reference |
|---|---|---|
| A Foundation | 01 | - |
| B Scaffold + shell | 01 | 08 |
| C Core model + commands | 01 | 02 |
| D Canvas + viewport | 02 | 03 |
| E Primitive + selection | 02 | 01 |
| F Inspector + hierarchy | 02 | 04, 05 |
| G BBox + regions + preview modes | 03 | 04 |
| H Component proof | 06 | 08 |
| I Default MVP cleanup | 08 | all states as visual consistency check |
| J Component library | 06 | 08 |
| K Layout / panel builder | 06 | 03 |
| L App shell docking | 08 | - |
| M Headless events/actions/references/rules | 04 or 07 | 06 |
| N Node graph workspace | 07 | 03 |
| O Advanced systems | 06 or 07 | depends on subsystem |

## Rules for use

| Context | Rule |
|---|---|
| Development task | name the implementation phase, not only the screen state |
| Visual prompt | name screen state, but do not imply that all visible systems are MVP-ready |
| Scope control | implement one system even if the visual reference shows many systems |

Example:

```text
Implement Phase M: headless event/action registry target resolver.
Use Screen State 07 only as visual reference.
Do not implement full node_graph editor yet.
```

## Current priority order

```text
source-of-truth model -> command layer -> canvas/hierarchy/inspector sync -> bbox/regions -> component proof -> save/export -> default UI cleanup -> component library -> panel builder -> app shell docking -> headless events/rules/references -> node_graph workspace -> advanced procedural systems
```
