# 12 — Sprint 10: State graph workspace

Cel: dopiero po headless event/action registry dodać osobny workspace dla graphu. Graph ma własny viewport i generuje commands. Nie jest defaultowym ekranem MVP.

SPRINT 10 — State graph workspace MVP

Assume previous sprint successful:
- Event registry exists.
- Action registry exists.
- Target resolver works.
- Logic Debug can test command-backed actions.
- npm run build passed.

Global rules:
- Graph outputs commands.
- Graph does not mutate DOM or canvas directly.
- Graph viewport is separate from canvas viewport and app shell docking.
- State graph workspace is separate from default interface creator.

Current phase:
- Phase: State graph workspace.
- Maturity target: L2/L3 first, L4 only after navigation and selection are stable.
- Data owner: Project.state_graphs or equivalent StateGraph model.

Implement only:
1. StateGraph model:
   - graph_id
   - name
   - nodes_by_id
   - edges
   - variables
   - viewport
   - validation_state
2. Node types MVP:
   - Event Input
   - Target Resolver
   - Condition
   - Get Value
   - Set Property
   - Emit Command
   - Debug Log
3. Graph viewport:
   - pan
   - zoom
   - node positions
   - node selection
   - box select or explicit pending marker if not feasible
4. Node UI:
   - compact dark nodes
   - visible sockets
   - clear output direction
   - labels by type/category
5. Graph execution preview:
   - select/test event
   - show command output
   - show validation errors
6. Workspace integration:
   - Logic/Events workspace route or tab
   - full-size graph area
   - side inspector for selected node
   - watch/log panel

Do not implement:
- Do not show graph in default MVP screen.
- Do not mix graph viewport with canvas viewport.
- Do not use graph to patch DOM.
- Do not build advanced node library.
- Do not build procedural icons or bitmap graph.

Allowed modules:
- src/core/stateGraph/**
- src/creator/workspaces/LogicEventsWorkspace.tsx
- src/creator/panels/LogicDebugPanel.tsx
- src/creator/panels/GraphInspectorPanel.tsx if needed
- src/creator/canvas-like graph components only under graph/workspace folder

Forbidden modules:
- app shell docking model except workspace content registration
- canvas object layout logic except target preview/read-only integration
- external bridges

Acceptance:
- npm run build passes.
- Logic workspace opens full graph area.
- Graph pan/zoom works.
- Add/select/move node works.
- Connect compatible sockets if implemented; otherwise edge creation is marked pending clearly.
- Test graph outputs command preview.
- Graph validation prevents invalid target/action output.
- Default UI remains interface creator, not graph editor.
