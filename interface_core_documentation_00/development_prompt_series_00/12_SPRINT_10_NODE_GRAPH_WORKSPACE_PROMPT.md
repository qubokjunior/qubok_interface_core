# 12 — Sprint 10: Node graph workspace

status: active
version: v2.2
doc_type: sprint_prompt
last_updated: 2026-06-10

Cel: dopiero po headless event/action registry dodać osobny workspace dla node graphu. Node graph ma własny viewport i generuje commands. Nie jest defaultowym ekranem MVP.

## Naming rule

Use `node_graph`, `nodeGraph`, `NodeGraph` and “node graph”. Do not use old `state_graph` naming.

## SPRINT 10 — Node graph workspace MVP

Assume previous sprint successful:
- Event registry exists.
- Action registry exists.
- Target resolver works.
- Logic Debug can test command-backed actions.
- npm run build passed.

Global rules:
- Node graph outputs commands.
- Node graph does not mutate DOM or canvas directly.
- Node graph viewport is separate from canvas viewport and app shell docking.
- Node graph workspace is separate from default interface creator.

Current phase:
- Phase: Node graph workspace.
- Maturity target: L2/L3 first, L4 only after navigation and selection are stable.
- Data owner: `Project.node_graphs` or equivalent `NodeGraph` model.

Implement only:
1. `NodeGraph` model:
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
3. Node graph viewport:
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
5. Node graph execution preview:
   - select/test event
   - show command output
   - show validation errors
6. Workspace integration:
   - Logic/Events workspace route or tab
   - full-size node graph area
   - side inspector for selected node
   - watch/log panel

Do not implement:
- Do not show node graph in default MVP screen.
- Do not mix node graph viewport with canvas viewport.
- Do not use node graph to patch DOM.
- Do not build advanced node library.
- Do not build procedural icons or bitmap graph.

Allowed modules:
- `src/core/nodeGraph/**`
- `src/creator/workspaces/LogicEventsWorkspace.tsx`
- `src/creator/panels/LogicDebugPanel.tsx`
- `src/creator/panels/NodeGraphInspectorPanel.tsx` if needed
- node graph components only under node graph/workspace folder

Forbidden modules:
- app shell docking model except workspace content registration
- canvas object layout logic except target preview/read-only integration
- external bridges

Acceptance:
- npm run build passes.
- Logic workspace opens full node graph area.
- Node graph pan/zoom works.
- Add/select/move node works.
- Connect compatible sockets if implemented; otherwise edge creation is marked pending clearly.
- Test node graph outputs command preview.
- Node graph validation prevents invalid target/action output.
- Default UI remains interface creator, not node graph editor.