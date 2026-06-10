# 07 — Sprint 05: Component proof + Panel_Monitor + save/export

## Cel sprintu

Udowodnić, że system buduje komponenty z primitive. Ten sprint zamyka pierwszy pełny proof: rectangle + text_rectangle + region_rectangle -> button_group -> inspector/hierarchy/debug -> Project JSON / Component JSON / SVG.

## Prompt do użycia

```text
SPRINT 05 — Component proof, Panel_Monitor sample, save and export

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Canvas, inspector, hierarchy and status sync through Project.selection.
- Region debug overlays work.
- Inspector edits dispatch commands.
- npm run build passed.

GLOBAL RULES
- Sample data must be Project data, not hardcoded JSX mockup.
- Component/group hierarchy must preserve parent/children links.
- SVG export includes visual objects only.
- Component JSON preserves internal hierarchy and exposed parameters.

CURRENT PHASE
- Phase: Component proof + persistence/export.
- Maturity target: L3 for group/save/export, L3/L4 for Panel_Monitor sample readability.
- Data owner: Project.objects_by_id, group_data, component_data.

IMPLEMENT ONLY
1. Group MVP:
   - selected objects -> group
   - group has children_ids and computed bbox
   - children keep valid parent_id
2. Button proof:
   - rectangle body
   - text label
   - region hit area
   - group named Button_Group_Proof
3. Panel_Monitor sample as Project data:
   - root panel group
   - frame rectangle
   - header group
   - title text
   - stat rows CPU/RAM/MEM
   - bar backgrounds/fills
   - footer group
   - header drag region
   - details hit region
   - panel hit region
4. Default startup scene:
   - load/create Panel_Monitor sample
   - select Panel_Monitor by default
   - hierarchy expanded enough to be useful
5. Save/export:
   - Project JSON
   - Component JSON for selected group/panel
   - SVG for visual shapes
   - validation before export
6. Bottom shelf integration:
   - Preview tab shows selected group/panel preview or placeholder
   - Export tab exposes Project JSON / Component JSON / SVG actions

DO NOT IMPLEMENT
- Do not add full component library cards yet.
- Do not add linked component instances.
- Do not add state graph event assignment.
- Do not add docking.
- Do not add panel builder tools yet.
- Do not flatten live primitives to paths.

FILES / MODULES
Allowed:
- src/core/commands/groupCommands.ts
- src/core/geometry/computedBBox.ts
- src/core/export/**
- src/core/library/** for component serialization only
- src/samples/createPanelMonitorSample.ts
- src/samples/createButtonGroupSample.ts
- src/creator/panels/ExportPanel.tsx
- src/creator/panels/PreviewPanel.tsx
- src/creator/layout/BottomShelf.tsx
Forbidden:
- graph editor
- docking shell
- procedural icon engine

ACCEPTANCE
- npm run build passes.
- On app start, Panel_Monitor appears on canvas and is selected.
- Hierarchy shows nested Panel_Monitor structure.
- Inspector shows selected panel/group.
- Button proof validates cleanly.
- Project JSON preserves IDs, parent/children, regions and viewport.
- SVG export contains visual shapes.
- Component JSON preserves hierarchy.
```

## Done oznacza

- Projekt ma pierwszy realny komponent testowy.
- Default scene nie jest chaotycznym test canvasem.
- Save/export zaczyna zabezpieczać długoterminową architekturę.
