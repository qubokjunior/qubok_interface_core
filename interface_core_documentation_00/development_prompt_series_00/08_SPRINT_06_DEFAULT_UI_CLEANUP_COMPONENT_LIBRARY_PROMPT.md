# 08 — Sprint 06: Default UI cleanup + component library MVP

## Cel sprintu

Uporządkować ekran startowy po wprowadzeniu Panel_Monitor oraz dodać prosty przepływ biblioteki komponentów: zapisz wybraną grupę lub panel jako asset, pokaż go w panelu biblioteki, wstaw nową kopię do projektu.

## Prompt do użycia

```text
SPRINT 06 — Default UI cleanup and component library MVP

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Panel_Monitor exists as Project data.
- Button/group proof validates.
- Project JSON, Component JSON and SVG export exist.
- npm run build passed.

GLOBAL RULES
- Default UI should look like interface_creator, not a debug console.
- Component asset is serialized group or panel data with metadata and preview.
- New component instance must receive fresh IDs and valid parent/children links.
- Debug and logic panels remain secondary.

CURRENT PHASE
- Phase: Default MVP interface cleanup + Component library MVP.
- Maturity target: L4 for default UI clarity, L3 for component library.
- Data owner: Project.library_assets or equivalent asset store, Project.objects_by_id for instances.

IMPLEMENT ONLY
1. Default UI cleanup:
   - compact dark shell pass
   - normal mode labels off
   - Panel_Monitor visible and selected
   - inspector, hierarchy and library layout readable
   - bottom shelf not oversized
2. Component asset type:
   - asset_id
   - name
   - category
   - tags
   - version
   - created_at / updated_at
   - component data
   - preview placeholder
3. Save selected group or panel as component:
   - validate selected root
   - store asset
   - show asset row/card
4. Insert component:
   - clone asset hierarchy
   - generate fresh IDs
   - add root to Project
   - select new root
   - validate links
5. Component library panel:
   - list/card view
   - name/category/status
   - insert action
   - minimal preview or placeholder

DO NOT IMPLEMENT
- Do not add remote sync.
- Do not add linked instance versioning.
- Do not add full exposed parameter editor.
- Do not add state graph.
- Do not add docking.
- Do not add panel builder.

FILES / MODULES
Allowed:
- src/core/library/**
- src/core/commands/componentCommands.ts
- src/creator/panels/ComponentLibraryPanel.tsx
- src/creator/layout/RightSidebar.tsx
- src/creator/layout/BottomShelf.tsx
- src/styles/**
Forbidden:
- src/core/stateGraph/**
- src/core/docking/**

ACCEPTANCE
- npm run build passes.
- Default screen is clean and readable.
- Save selected Panel_Monitor or Button_Group_Proof as component.
- Component appears in library panel.
- Insert component creates fresh object IDs.
- New instance appears on canvas and in hierarchy.
- Validation reports no duplicate IDs or broken parent links.
```

## Done oznacza

- System przechodzi od pojedynczego sample do reusable component workflow.
- Default UI wygląda jak narzędzie, nie jak zbiór debug paneli.
