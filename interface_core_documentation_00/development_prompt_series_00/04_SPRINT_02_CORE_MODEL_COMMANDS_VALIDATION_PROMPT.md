# 04 — Sprint 02: Core model + commands + validation skeleton

## Cel sprintu

Zbudować stabilny headless core: Project, InterfaceObject, command layer, command history, selectors, podstawową walidację i testy headless. To jest fundament całego projektu. UI może jeszcze tylko wyświetlać wynik, ale nie powinien być właścicielem danych.

## Dlaczego aktualizacja v2.1 zmienia ten sprint

Pierwotnie command history, selectors i testy mogły zostać potraktowane jako dodatki. Po diagnozie projektu należy je wprowadzić wcześniej, bo późniejszy canvas drag, inspector edits, grouping, export i state graph będą od nich zależeć. Bez tego ryzyko rośnie w trzech miejscach: brak undo/redo, powielanie logiki w UI oraz błędy relacji ID wykrywane za późno.

## Prompt do użycia

```text
SPRINT 02 — Core model, command layer, command history, selectors, validation and headless tests

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Shell exists and builds.
- Default UI has stable zones.
- Debug/state graph/docking are not dominant.

GLOBAL RULES
- Project JSON / data model is source of truth.
- src/core must not import React or creator modules.
- Creator reads core state and dispatches commands.
- All persistent mutations must go through command layer.
- Commands must be serializable.
- Reducers/applyCommand must be pure or clearly isolate side effects outside core.

CURRENT PHASE
- Phase: Core model + command layer + command history + validation skeleton.
- Maturity target: L1 headless core, L1/L2 tests, optional L2 command/validation status display.
- Data owner: Project and InterfaceObject in src/core/model.

IMPLEMENT ONLY
1. Add or normalize core model types:
   - Project
   - InterfaceObject as discriminated union if feasible
   - Transform
   - BBoxSet: visual_bbox, layout_bbox, interaction_bbox, computed_bbox
   - Style
   - TextData
   - RegionData
   - LayoutData
   - GroupData
   - SelectionState
   - ViewportState
   - ValidationState
   - CommandHistoryState
2. Add object defaults/factories:
   - createInitialProject
   - createObjectDefaults
   - createRectangleObject
   - createTextRectangleObject
   - createRegionRectangleObject
3. Add command layer:
   - commandTypes
   - applyCommand
   - object.create
   - object.patch
   - object.delete
   - object.duplicate basic placeholder if safe
   - selection.set / selection.clear
   - transform.update
   - group.create minimal or stubbed with validation notes
4. Add command history:
   - past/present/future or command journal structure
   - undo command if safe, otherwise define interface and acceptance notes
   - redo command if safe, otherwise define interface and acceptance notes
   - history_domain to separate project edits from camera/viewport moves
   - command_log for diagnostics
5. Add selectors as core boundary:
   - getObjectById
   - getSelectedObjects
   - getActiveObject
   - getRootObjects
   - getChildrenOfObject
   - getParentChain
   - getObjectValidationStatus
6. Add validation skeleton:
   - duplicate IDs
   - missing parent
   - child without parent backlink
   - circular parent relation
   - invalid size
   - broken region linked_visual_id
   - hidden object selected
   - locked object edited warning if command context supports it
7. Add minimal render adapter types, not full renderer:
   - RenderObjectDescriptor
   - RenderLayerDescriptor
   - mapProjectToSvgRenderModel placeholder or interface
   - no React in this layer
8. Add headless tests if test setup exists, or add minimal test setup if safe:
   - create Project
   - create object
   - patch object
   - select object
   - validate Project
   - undo/redo if implemented
9. Wire current app state to create/use Project if a store already exists.

DO NOT IMPLEMENT
- Do not build full inspector UI yet.
- Do not build full canvas tools yet.
- Do not add state graph runtime beyond reserved types if already present.
- Do not implement docking.
- Do not create sample UI as hardcoded JSX.
- Do not store persistent object changes in React-only local state.
- Do not import external graph/docking libraries in this sprint.

FILES / MODULES
Allowed:
- src/core/model/**
- src/core/commands/**
- src/core/selectors/**
- src/core/validation/**
- src/core/render/** for adapter types only
- src/creator/state/** if needed to hold Project and dispatch commands
- src/app/** for initial state glue
- tests/core/** or src/**/*.test.ts if existing pattern exists
Forbidden:
- src/core importing React
- src/core importing src/creator
- graph visual editor
- docking runtime
- visual graph/docking external adapters

ACCEPTANCE
- npm run build passes.
- Headless tests pass if test command exists; otherwise document missing test command.
- A Project can be created headlessly.
- object.create adds object to objects_by_id and parent/root list.
- object.patch updates object through applyCommand.
- selection.set updates Project.selection.
- transform.update changes transform through command.
- command history excludes viewport/camera changes unless explicitly configured.
- selectors can be used by canvas/inspector/hierarchy later without duplicating lookup logic.
- validateProject returns useful errors/warnings without UI.
- Response lists changed files and manual/headless tests.
```

## Done oznacza

- Jest jeden model prawdy.
- Jest jedna ścieżka mutacji.
- Jest zalążek undo/redo albo przynajmniej jawny command history contract.
- Canvas/inspector/hierarchy będą mogły używać tych samych selectorów.
- Walidacja zaczyna pilnować relacji zanim UI urośnie.
- Render MVP może pozostać SVG, ale core nie jest zamknięty na jeden renderer.