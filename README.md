# qubok_interface_core

Parametric interface engine for the qubok suite.

`qubok_interface_core` is not a UI kit and not a one-off app. It defines how interface elements are represented as data, edited, validated, debugged, previewed, exported and reused.

## Current source of truth

Working documentation lives in:

- `interface_core_documentation_00/`

Read in this order before preparing Codex work:

1. `interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md`
2. `interface_core_documentation_00/01_TERMINOLOGY_CANON_NODE_GRAPH.md`
3. `interface_core_documentation_00/02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
4. `interface_core_documentation_00/03_FEATURE_MATURITY_MATRIX.md`
5. `interface_core_documentation_00/04_CODEX_DEVELOPMENT_PROTOCOL.md`
6. `interface_core_documentation_00/05_CURRENT_IMPLEMENTATION_STABILIZATION_MAP.md`
7. `interface_core_documentation_00/06_LOCAL_REPO_STRUCTURE_AND_CODEX_ACCESS_MAP.md`

## Current canonical decisions

- `Project JSON / data model` is the persistent source of truth, but implementation should keep document data, session state, view state, workspace layout and runtime cache separated.
- Persistent changes go through the command layer.
- Interactive pointer flows use `InteractionSession`: preview during drag, commit one transaction command on end.
- Selection operations use explicit `operation_target_ids`; `active_id` is focus, not the batch target list.
- Rectangle renders shape; region handles hit/hover/drag/drop/resize/snap/scroll/content/layout.
- Panel is a structured component: frame, header, content, footer, sections, rows, regions and exposed parameters.
- `node_graph` is the canonical graph-system name; `state_graph` is legacy and must not be introduced in new public naming.
- `state` remains valid for object/component variants: hover, pressed, disabled, dirty, selected, error.
- App shell docking, canvas object layout and graph viewport layout are three separate systems.
- A canvas dock pane is not automatically a full interactive renderer; only one primary interactive canvas should exist per `viewId` unless explicitly requested.
- Advanced features must pass maturity levels: L0 spec, L1 headless, L2 debug, L3 MVP workflow, L4 polished, L5 advanced/composable.

## Current implementation priority

The current local project state shows a working but sprint-heavy prototype with many root-level sprint notes, diagnostics, build logs, backups and generated artifacts. The next work should not add another advanced feature layer. It should stabilize the editor core:

1. Safety baseline and repo cleanliness.
2. Selection Operation Target Contract.
3. InteractionSession performance path.
4. Canvas Instance Guardrail.
5. Document / Session / View / Workspace / Cache split audit.
6. Core browser boundary audit.
7. Default UI maturity cleanup.
8. Versioned persistence and migrations.
9. Dedicated `node_graph` naming cleanup later.

## Important reference documents

- `interface_core_documentation_00/11_VISUAL_DIAGNOSTICS_AND_STYLE_STANDARD_2026_06_10.txt`
- `interface_core_documentation_00/12_RULES_STATES_RELATIONS_PROCEDURAL_UI_2026_06_10.txt`
- `interface_core_documentation_00/13_REGISTRY_CUSTOMIZATION_TOKENS_PREVIEW_2026_06_10.txt`
- `interface_core_documentation_00/14_DEBUG_DOCKING_WORKSPACE_KERNEL_GRAPH_PROFILES_2026_06_10.txt`
