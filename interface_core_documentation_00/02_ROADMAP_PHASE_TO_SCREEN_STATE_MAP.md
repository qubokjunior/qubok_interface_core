# QUBOK_INTERFACE_CORE — ROADMAP PHASE TO SCREEN STATE MAP

Version: 2026-06-11B
Status: canonical roadmap clarification + current stabilization map

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

Canonical long-term order:

```text
source-of-truth model -> command layer -> canvas/hierarchy/inspector sync -> bbox/regions -> component proof -> save/export -> default UI cleanup -> component library -> panel builder -> app shell docking -> headless events/rules/references -> node_graph workspace -> advanced procedural systems
```

Current local implementation state requires a stabilization bridge before continuing advanced phases:

```text
0 Safety baseline -> 1 Selection Operation Target Contract -> 2 InteractionSession -> 3 Canvas Instance Guardrail -> 4 Document/Session/View split audit -> 5 Core browser boundary audit -> 6 Default UI visibility cleanup -> 7 Versioned persistence/migrations -> 8 dedicated node_graph naming cleanup
```

## Stabilization bridge tasks

These tasks do not replace Phase A-O. They are the required bridge from the current prototype-heavy local state back to the canonical roadmap.

| Bridge | Dominant mechanism | Related phase | Target maturity | Gate |
|---:|---|---|---|---|
| S0 | Safety baseline / repo cleanliness | A/I | L2 -> L3 | build status known, noisy local root documented, rollback path clear |
| S1 | Selection Operation Target Contract | E/F | L2/L3 -> L4 | multi-delete and multi-drag use same target resolver |
| S2 | InteractionSession performance path | D/E | L2 -> L3/L4 | pointermove uses transient preview; commit happens on pointerup/blur/enter |
| S3 | Canvas Instance Guardrail | D/L | L2 -> L3 | one primary interactive canvas per `viewId`; mirrors do not dispatch commands |
| S4 | Document / Session / View / Workspace / Cache split audit | C/D/F/L | L1/L2 -> L3 | ownership of each state field is explicit |
| S5 | Core browser boundary audit | C | L2 -> L4 | `src/core` has no React/DOM/browser/storage ownership |
| S6 | Default UI visibility cleanup | I | L3 -> L4 | L2 debug visible only as compact status or debug workspace |
| S7 | Versioned persistence and migrations | H/J/M | L1 -> L3 | saved documents contain schema version and migration path |
| S8 | `node_graph` naming cleanup | N support | L1/L2 -> L3 | new public naming uses `node_graph`; legacy kept only where isolated |

## Codex task chain for current stabilization

Codex prompts should reference one bridge task at a time. Do not combine S1/S2/S3 with graph, docking polish, rules, arrays or visual redesign.

| Step | Codex focus | Inspect first | Avoid |
|---:|---|---|---|
| 1 | S0 safety baseline | `package.json`, `src/`, root sprint notes, `_diagnostics`, build logs | deleting user backups without explicit request |
| 2 | S1 selection resolver | canvas selection, hierarchy selection, delete, drag move, active object logic | global model rename |
| 3 | S2 interaction session | pointer handlers, drag/resize paths, validation/autosave/log calls | changing visual style |
| 4 | S3 canvas instance guardrail | docking content resolver, canvas viewport mounting, panel content mapping | rewriting docking model |
| 5 | S4 state ownership audit | project model, app state, local component state, persistence | broad App.tsx decomposition before contracts |
| 6 | S5 core boundary | all `src/core` imports and browser API usage | moving UI logic into core |
| 7 | S6 default visibility | shell panels, bottom shelf, debug/log/graph/docking visibility | hiding broken runtime bugs with CSS only |
| 8 | S7 persistence | save/load/export, localStorage, schema version | changing object schema without migration |
| 9 | S8 naming cleanup | legacy `stateGraph` files/imports/docs | mixing semantic `state` with graph naming |

## Acceptance principle

A step is not complete because the UI looks better. It is complete only when the relevant contract is testable:

- selection target contract is testable by multi-select delete and drag;
- interaction session is testable by drag performance and single commit log;
- canvas guardrail is testable by opening multiple dock panes;
- state ownership is testable by inspecting where each field is stored;
- core boundary is testable by import/API scan;
- visibility cleanup is testable by default UI and debug workspace modes;
- persistence is testable by save/load across schema version;
- naming cleanup is testable by grep and build.
