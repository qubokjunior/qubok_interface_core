# 10 — Sprint 08: App shell docking

Cel: dodać Blender-like split/merge/resize dla paneli aplikacji. To jest osobny model od canvas object layout i od graph viewport layout.

SPRINT 08 — App shell docking MVP

Assume previous sprint successful:
- Layout and panel builder affect Project objects only.
- Default UI remains clean.
- npm run build passed.

Global rules:
- Docking controls application workspace areas.
- Docking must not change InterfaceObject transforms.
- Dock layout is stored separately from Project object layout.
- Canvas object layout and app shell layout must not share one data model.

Current phase:
- Phase: App shell docking.
- Maturity target: L3/L4 for shell layout, L2 for diagnostics.
- Data owner: DockLayout / workspace_layout / app shell state.

Implement only:
1. Docking model: split node, leaf node, direction, ratio, content_id, min size, active area.
2. Shell commands or reducers: split horizontal, split vertical, merge area, resize ratio, set area content, reset layout.
3. UI shell integration: splitter drag, area header, content selector, active area marker.
4. Content resolver: Inspector, Hierarchy, Canvas, Preview, Export, Components, Validation, Command Log, Logic Debug as selectable content where available.
5. Persistence: save layout to workspace layout or local storage / manifest if existing system supports it.
6. Diagnostics: compact panel showing active area, content id, split tree summary.

Do not implement:
- Do not modify Project object transforms.
- Do not implement graph viewport docking.
- Do not add state graph editor.
- Do not make docking workbench the default face if normal shell works.
- Do not remove existing stable panels.

Allowed modules:
- src/core/docking/**
- src/creator/layout/DockArea.tsx
- src/creator/layout/SplitPane.tsx
- src/creator/layout/WorkspaceShell.tsx
- src/creator/panels/DockingWorkbenchPanel.tsx
- src/app/defaultWorkspaceLayout.ts

Forbidden modules:
- src/core/layout/** except shared size helpers if unavoidable
- src/core/stateGraph/**
- Project object transform commands

Acceptance:
- npm run build passes.
- Split area horizontal and vertical works.
- Merge area works.
- Drag splitter changes panel sizes.
- Area content can be changed without losing Project objects.
- Reset layout restores default shell.
- Canvas objects do not resize when app panels resize.
