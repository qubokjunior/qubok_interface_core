# 04 — Sprint 02: Core model + commands + validation skeleton

## Cel sprintu

Zbudować stabilny headless core: Project, InterfaceObject, command layer i podstawową walidację. To jest fundament całego projektu. UI może jeszcze tylko wyświetlać wynik, ale nie powinien być właścicielem danych.

## Prompt do użycia

```text
SPRINT 02 — Core model, command layer and validation skeleton

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Shell exists and builds.
- Default UI has stable zones.
- Debug/state graph/docking are not dominant.

GLOBAL RULES
- Project JSON / data model is source of truth.
- src/core must not import React or creator modules.
- Creator reads core state and dispatches commands.
- All persistent mutations must go through command layer.

CURRENT PHASE
- Phase: Core model + command layer + validation skeleton.
- Maturity target: L1 headless core, optional L2 command/validation status display.
- Data owner: Project and InterfaceObject in src/core/model.

IMPLEMENT ONLY
1. Add or normalize core model types:
   - Project
   - InterfaceObject
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
4. Add validation skeleton:
   - duplicate IDs
   - missing parent
   - child without parent backlink
   - circular parent relation
   - invalid size
   - broken region linked_visual_id
5. Add minimal selectors if useful:
   - getObjectById
   - getSelectedObjects
   - getActiveObject
   - getRootObjects
6. Wire current app state to create/use Project if a store already exists.

DO NOT IMPLEMENT
- Do not build full inspector UI yet.
- Do not build full canvas tools yet.
- Do not add state graph runtime beyond reserved types if already present.
- Do not implement docking.
- Do not create sample UI as hardcoded JSX.
- Do not store persistent object changes in React-only local state.

FILES / MODULES
Allowed:
- src/core/model/**
- src/core/commands/**
- src/core/validation/**
- src/creator/state/** if needed to hold Project and dispatch commands
- src/app/** for initial state glue
Forbidden:
- src/core importing React
- src/core importing src/creator
- graph visual editor
- docking runtime

ACCEPTANCE
- npm run build passes.
- A Project can be created headlessly.
- object.create adds object to objects_by_id and parent/root list.
- object.patch updates object through applyCommand.
- selection.set updates Project.selection.
- transform.update changes transform through command.
- validateProject returns useful errors/warnings without UI.
- Response lists changed files and manual/headless tests.
```

## Done oznacza

- Jest jeden model prawdy.
- Jest jedna ścieżka mutacji.
- Walidacja zaczyna pilnować relacji zanim UI urośnie.
