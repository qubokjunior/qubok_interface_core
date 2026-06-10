# 09 — Sprint 07: Layout, snap, box arranger, panel builder

Cel: dodać narzędzia do układania obiektów na canvasie i składania paneli jako struktury danych. Ten sprint dotyczy canvas object layout, nie app shell docking.

SPRINT 07 — Layout, snap, box arranger and panel builder MVP

Assume previous sprint successful:
- Component library MVP works.
- Component instances receive fresh IDs.
- Default UI is clean.
- npm run build passed.

Global rules:
- This sprint edits objects inside Project, not application windows.
- layout_bbox is used for layout operations.
- interaction_region is used for drop/content/resize zones.
- Panel is a group/component structure, not one rectangle.

Current phase:
- Phase: Layout / snap / panel builder.
- Maturity target: L3 user workflow MVP.
- Data owner: InterfaceObject.layout_data, bbox fields, group_data, region_data.

Implement only:
1. Layout tools: align left/center/right/top/middle/bottom, distribute horizontal/vertical, grid size control, snap on/off.
2. Snap MVP: snap to grid, snap to sibling edges/centers, guide line preview, transform.update on commit.
3. Box arranger: create placeholder empty rectangles, create rows/columns inside selected parent bbox, margin/padding/gap inputs.
4. Panel builder MVP: create panel shell, frame rectangle, header group, content group/region, optional footer, optional resize regions, content/drop regions.
5. Inspector integration: layout fields for selected group/panel, region fields for content/drop regions.
6. Validation: invalid layout sizes, broken region links, simple panel warnings.

Do not implement:
- Do not implement app shell split/merge.
- Do not modify workspace panels as docking.
- Do not add state graph.
- Do not add procedural shape/path editor.
- Do not create full responsive constraint solver.

Allowed modules:
- src/core/layout/**
- src/core/commands/layoutCommands.ts
- src/core/geometry/**
- src/creator/tools/layoutTool.ts
- src/creator/tools/panelBuilderTool.ts
- src/creator/panels/LayoutInspector.tsx
- src/creator/panels/RegionInspector.tsx
- src/creator/panels/PrimitiveLibraryPanel.tsx

Forbidden modules:
- src/core/docking/**
- src/core/stateGraph/**

Acceptance:
- npm run build passes.
- Align/distribute selected objects works through commands.
- Drag move can snap to grid.
- Guide overlay appears during snap.
- Box arranger creates rows/columns as Project objects.
- Panel builder creates structured panel hierarchy.
- Validation catches broken region links.

Done means:
- Można szybciej budować układy paneli.
- Panel staje się strukturą danych, nie pojedynczym kształtem.
- Nadal nie mieszamy tego z dockingiem aplikacji.
