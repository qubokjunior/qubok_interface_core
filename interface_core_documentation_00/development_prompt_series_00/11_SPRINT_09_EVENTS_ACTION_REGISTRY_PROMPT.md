# 11 — Sprint 09: Events, actions, target resolver

status: active_with_v2_2_amendment
version: v2.2
doc_type: sprint_prompt
last_updated: 2026-06-10

Cel: przygotować logikę bez pełnego node graph editor. Ten sprint dodaje headless event/action registry oraz target resolver. Outputem akcji są commands.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## SPRINT 09 — Headless events, actions and target resolver

Assume previous sprint successful:
- App shell docking works or is explicitly deferred.
- Canvas, inspector, hierarchy, component library and layout are stable.
- npm run build passed.

Global rules:
- Event/action layer must not mutate DOM or canvas directly.
- Actions are wrappers around existing commands.
- Target resolver reads Project selection, event target and object queries.
- Full node graph editor is not part of this sprint.

Current phase:
- Phase: Headless event/action registry.
- Maturity target: L1/L2, optional L3 for simple event assignment UI.
- Data owner: Project.event_assignments or equivalent, InterfaceObject.event_bindings, `core/events` headless model.

Implement only:
1. Event registry types.
2. Action registry entries backed by existing command layer.
3. Target resolver.
4. Event assignment model.
5. Logic Debug panel.
6. First proof that an event can emit a command through command layer.

Do not implement:
- Do not implement node graph viewport.
- Do not add visual nodes.
- Do not bypass validation.
- Do not make Logic Debug dominate default UI.

Allowed modules:
- `src/core/events/actionRegistry.ts`
- `src/core/events/eventRegistry.ts`
- `src/core/events/targetResolver.ts`
- `src/core/commands/**` for missing command wrappers
- `src/creator/panels/LogicDebugPanel.tsx`
- `src/creator/panels/EventsInspector.tsx`

Forbidden modules:
- visual node graph workspace
- node graph canvas
- app shell docking changes unless needed to expose hidden panel

Acceptance:
- npm run build passes.
- Action registry lists command-backed actions.
- Event assignment can resolve active/selected/event target.
- Test event emits a command and updates Project.
- No direct DOM/canvas mutation in event/action layer.
- Logic Debug remains secondary/collapsed in default UI.