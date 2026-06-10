# 11 — Sprint 09: Events, actions, target resolver

Cel: przygotować logikę bez pełnego graph editor. Ten sprint dodaje headless event/action registry oraz target resolver. Outputem akcji są commands.

SPRINT 09 — Headless events, actions and target resolver

Assume previous sprint successful:
- App shell docking works or is explicitly deferred.
- Canvas, inspector, hierarchy, component library and layout are stable.
- npm run build passed.

Global rules:
- Event/action layer must not mutate DOM or canvas directly.
- Actions are wrappers around existing commands.
- Target resolver reads Project selection, event target and object queries.
- Full state graph editor is not part of this sprint.

Current phase:
- Phase: Headless event/action registry.
- Maturity target: L1/L2, optional L3 for simple event assignment UI.
- Data owner: Project.event_assignments or equivalent, InterfaceObject.event_bindings, core/stateGraph or core/events headless model.

Implement only:
1. Event registry types:
   - click
   - hover enter/leave
   - drag start/end
   - selection change
   - value change
   - project save/load
   - shortcut
   - timer tick placeholder
2. Action registry entries:
   - SetPosition -> transform.update
   - SetSize -> transform.update
   - SetFill -> object.patch
   - SetText -> object.patch
   - SetVisible -> object.patch
   - SetLocked -> object.patch
   - AddObject -> object.create
   - DeleteObject -> object.delete
   - GroupSelected -> group.create
   - AlignSelected -> layout.align
   - ValidateProject -> debug.validate / project.validate
3. Target resolver:
   - event_target
   - active_object
   - first_selected
   - all_selected
   - object_by_id
   - parent_of_target
   - children_of_target
   - objects_by_type
   - objects_with_tag placeholder
4. Event assignment model:
   - id
   - object_id or region_id
   - event_type
   - target_rule
   - action_id
   - parameter_bindings
   - enabled
   - priority
5. Logic Debug panel:
   - list event assignments
   - show resolved target for selected object
   - test event button if safe
   - show command preview
6. First proof:
   - selected region click or test event can call SetFill on target through command layer.

Do not implement:
- Do not implement node graph viewport.
- Do not add visual nodes.
- Do not allow arbitrary JS strings as actions.
- Do not bypass validation.
- Do not make Logic Debug dominate default UI.

Allowed modules:
- src/core/stateGraph/actionRegistry.ts or src/core/events/actionRegistry.ts
- src/core/stateGraph/eventRegistry.ts or src/core/events/eventRegistry.ts
- src/core/stateGraph/targetResolver.ts or src/core/events/targetResolver.ts
- src/core/commands/** for missing command wrappers
- src/creator/panels/LogicDebugPanel.tsx
- src/creator/panels/EventsInspector.tsx

Forbidden modules:
- visual graph workspace
- graph node canvas
- app shell docking changes unless needed to expose hidden panel

Acceptance:
- npm run build passes.
- Action registry lists command-backed actions.
- Event assignment can resolve active/selected/event target.
- Test event emits a command and updates Project.
- No direct DOM/canvas mutation in event/action layer.
- Logic Debug remains secondary/collapsed in default UI.
