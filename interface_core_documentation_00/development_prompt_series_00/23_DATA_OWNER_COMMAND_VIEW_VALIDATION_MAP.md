# 23 — Data owner / command / view / validation map

status: active
version: v2.1
doc_type: canonical_matrix
last_updated: 2026-06-10

## Cel

Ten dokument mapuje funkcje na właściciela danych, komendy, widoki i walidację. Używać przy pisaniu promptów, żeby każdy sprint jasno odpowiadał: kto posiada dane, kto je zmienia, kto je pokazuje i kto je waliduje.

## Główna zasada

```text
Data owner -> Command -> Selector -> View -> Validation
```

Niepoprawne jest:

```text
View local state -> direct mutation -> partial Project update
```

## Mapa podstawowa

| Obszar | Data owner | Commands | Selectors | View / UI | Validation |
|---|---|---|---|---|---|
| Project metadata | Project | project.patch | getProjectMeta | TopAppBar, StatusBar | validateProjectMeta |
| Object identity | InterfaceObject | object.create, object.patch, object.delete | getObjectById | Inspector, Hierarchy | validateObjectIdentity |
| Hierarchy | Project.root_children, children_ids, parent_id | group.create, group.ungroup, object.reparent | getRootObjects, getChildrenOfObject, getParentChain | HierarchyPanel, Canvas layer order | validateHierarchy |
| Selection | Project.selection | selection.set, selection.clear, selection.add, selection.remove | getSelectedObjects, getActiveObject | Canvas outline, Inspector, StatusBar | validateSelection |
| Transform | InterfaceObject.transform | transform.update | getObjectById, getSelectionBBox | Canvas handles, Inspector | validateTransform |
| Visual bbox | InterfaceObject.bbox.visual | object.patch, transform.update | getSelectionBBox, getObjectsForRender | Canvas, Selected Debug | validateBBox |
| Layout bbox | InterfaceObject.bbox.layout | layout.update, object.patch | getLayoutObjects, getSelectionBBox | Layout Debug, Layout Inspector | validateLayoutBBox |
| Interaction region | InterfaceObject.region_data / region_rectangle | region.patch, object.patch | getRegionsForHitTest, getRegionById | Interaction Debug, Region Inspector | validateRegions |
| Style | InterfaceObject.style | style.patch, object.patch | getObjectById, getObjectsForRender | Canvas, Inspector | validateStyle |
| Text | InterfaceObject.text_data | text.patch, object.patch | getObjectById, getObjectsForRender | Canvas text, Text Inspector | validateText |
| Group data | InterfaceObject.group_data | group.create, group.ungroup, group.patch | getChildrenOfObject, getGroupBBox | Hierarchy, Inspector | validateGroup |
| Component asset | Project.library_assets | component.save, component.insert, component.delete | getLibraryAssets, getComponentById | ComponentLibraryPanel | validateComponentAsset |
| Project save/load | Project | project.save, project.load | getProjectSnapshot | ExportPanel | validateProject |
| SVG export | Render model | export.svg | getObjectsForRender | ExportPanel, PreviewPanel | validateExport |
| Command history | Project.history | history.undo, history.redo, history.clear | getCommandLog, getUndoState | CommandLogPanel, StatusBar | validateHistory |
| Viewport | Project.viewport or workspace viewport | viewport.pan, viewport.zoom | getViewport | CanvasViewport | validateViewport |
| Docking shell | DockLayout | docking.split, docking.merge, docking.resize, docking.setContent | getDockTree, getActiveDockArea | WorkspaceShell, DockingPanel | validateDockLayout |
| Event assignment | Project.event_assignments | event.assign, event.unassign, event.patch | getEventsForObject, getEventAssignments | LogicDebugPanel, EventsInspector | validateEventAssignments |
| Action registry | core/events registry | action.register only if dynamic; usually static | getActionRegistry | LogicDebugPanel | validateActionRegistry |
| Target resolver | core/events resolver | no direct persistent command | resolveTarget | LogicDebugPanel, Graph preview | validateTargetRule |
| State graph model | Project.state_graphs | graph.create, graph.patch, graph.delete, graph.node.patch, graph.edge.patch | getGraphById, getGraphNodes | StateGraphWorkspace | validateGraph |
| Graph viewport | StateGraph.viewport | graph.viewport.pan, graph.viewport.zoom | getGraphViewport | StateGraphWorkspace | validateGraphViewport |

## Przykład: region_rectangle

```text
Feature: region_rectangle
Data owner: InterfaceObject.region_data or InterfaceObject type=region_rectangle
Commands: object.create, region.patch, object.patch
Selectors: getRegionsForHitTest, getObjectById
View: Canvas normal mode hidden/subtle, Interaction Debug visible, RegionInspector editable
Validation: validateRegions, validateBrokenLinkedVisualId
Export: Project JSON yes, SVG no by default unless debug export enabled
Maturity: L3
```

## Przykład: Panel_Monitor

```text
Feature: Panel_Monitor sample
Data owner: Project.objects_by_id + hierarchy
Commands: sample.createProject or object.create/group.create commands
Selectors: getRootObjects, getChildrenOfObject, getObjectsForRender
View: Canvas, HierarchyPanel, Inspector, PreviewPanel
Validation: validateProject, validateHierarchy, validateRegions, validateComponentCandidate
Export: Project JSON, Component JSON, SVG visual shapes
Maturity: L4 sample proof
```

## Przykład: docking shell

```text
Feature: app shell docking
Data owner: DockLayout, not Project.objects_by_id
Commands: docking.split, docking.merge, docking.resize, docking.setContent
Selectors: getDockTree, getActiveDockArea
View: WorkspaceShell, SplitPane, DockAreaHeader
Validation: validateDockLayout
Export: workspace preset optional, not Component JSON
Maturity: L3/L4
```

## Przykład: state graph node move

```text
Feature: state graph node move
Data owner: Project.state_graphs[graph_id].nodes_by_id[node_id].position
Commands: graph.node.patch or graph.node.move
Selectors: getGraphById, getGraphNodeById
View: StateGraphWorkspace, graph adapter view
Validation: validateGraph, validateGraphNodePosition
Export: Project JSON yes
Maturity: L2/L3 first
```

## Prompt writing rule

Każdy prompt implementacyjny powinien zawierać blok:

```text
DATA FLOW
- Data owner:
- Commands:
- Selectors:
- View:
- Validation:
- Export/persistence:
```

Jeżeli nie da się wypełnić tego bloku, feature jest jeszcze za słabo określony i powinien zostać doprecyzowany przed implementacją.