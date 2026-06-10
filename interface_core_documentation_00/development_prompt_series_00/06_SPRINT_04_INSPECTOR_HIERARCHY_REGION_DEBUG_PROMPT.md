# 06 — Sprint 04: Inspector + hierarchy + region debug

## Cel sprintu

Zamknąć podstawową triadę edytora: canvas, inspector i hierarchy muszą stale wskazywać ten sam aktywny obiekt. Następnie urealnić rozdzielenie `visual_bbox`, `layout_bbox` i `interaction_region` przez debug overlays oraz region inspector.

## Prompt do użycia

```text
SPRINT 04 — Inspector, hierarchy, sync and region debug overlays

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Canvas renders Project objects.
- Primitive creation works through object.create.
- Selection and transform work through commands.
- npm run build passed.

GLOBAL RULES
- Project.selection is the single source of active selection.
- Inspector and hierarchy read Project state; they do not own object data.
- Edits from inspector dispatch commands.
- Region is explicit data; rectangle does not own interaction logic by magic.

CURRENT PHASE
- Phase: Inspector + hierarchy + bbox/region debug.
- Maturity target: L3 for inspector/hierarchy sync, L2/L3 for debug overlays.
- Data owner: Project.objects_by_id, Project.selection, InterfaceObject.region_data and bbox fields.

IMPLEMENT ONLY
1. Inspector panel:
   - no selection: Project summary
   - one selection: Object Inspector
   - multi selection: Mixed Inspector summary
   - tabs or sections: Identity, Transform, Geometry, Style, Text, Region, Layout, Validation
2. Inspector fields:
   - name
   - visible/locked
   - x/y/width/height
   - fill/stroke/radius for rectangle
   - text value/font size/color for text_rectangle
   - region_type/priority/linked_visual_id/cursor/debug opacity for region_rectangle
3. Hierarchy panel:
   - tree or flat tree with parent/children indentation
   - selected row highlight
   - visibility toggle
   - lock toggle
   - warning marker from validation
   - click row selects object
4. Sync:
   - canvas click updates hierarchy and inspector
   - hierarchy click updates canvas and inspector
   - inspector edit rerenders canvas
   - status bar shows selected count and active object x/y/w/h
5. Debug overlays:
   - Normal: no all labels
   - Selected Debug: active object bbox/id/name
   - Layout: layout_bbox
   - Interaction: interaction_region / region rectangles
   - Validation: warnings/errors
6. Hover debug card if cheap:
   - object id/name/type
   - bbox summary
   - region type
   - validation state

DO NOT IMPLEMENT
- Do not add full event/action registry.
- Do not add state graph.
- Do not add component library.
- Do not add panel builder.
- Do not make all debug labels default.
- Do not add docking.

FILES / MODULES
Allowed:
- src/creator/panels/InspectorPanel.tsx and related inspector components
- src/creator/panels/HierarchyPanel.tsx
- src/creator/canvas/DebugOverlay.tsx
- src/creator/canvas/HoverInspectOverlay.tsx if needed
- src/creator/layout/StatusBar.tsx
- src/core/validation/** for warning integration
- src/core/geometry/** for bbox helpers
Forbidden:
- src/core/stateGraph/**
- src/core/docking/**
- component library runtime

ACCEPTANCE
- npm run build passes.
- Click canvas object -> inspector and hierarchy show same object.
- Click hierarchy row -> canvas selection and inspector update.
- Change x/y/fill/text in inspector -> canvas updates through command.
- Region rectangle is invisible in Normal mode and visible in Interaction debug mode.
- Validation mode shows warnings without flooding normal view.
```

## Done oznacza

- Główna pętla edycji działa.
- Regiony i bboxy są debugowalne.
- Default UI pozostaje czysty.
