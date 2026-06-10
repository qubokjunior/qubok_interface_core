# 03 — Sprint 01: Scaffold + concept shell

## Cel sprintu

Ustawić docelowy układ aplikacji wcześnie, zanim funkcje zaczną tworzyć chaotyczną listę paneli. Shell może mieć puste lub placeholderowe panele, ale powinien mieć właściwe strefy: top bar, left tools, center canvas, right sidebar, bottom shelf, status bar.

## Prompt do użycia

```text
SPRINT 01 — Scaffold concept shell and design tokens

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Foundation audit exists.
- Feature maturity matrix exists.
- Module boundaries are documented.

GLOBAL RULES
- Project JSON / data model is source of truth, even if model is still minimal.
- Core has no React imports.
- This sprint is about app shell layout, not canvas object layout.
- Default UI must look like interface_creator, not a debug/API dashboard.

CURRENT PHASE
- Phase: Project scaffold + concept shell.
- Maturity target: L3 shell placeholders, L1/L2 style tokens.
- Data owner: creator workspace UI state; not Project object layout.

IMPLEMENT ONLY
1. Create or clean the main layout skeleton:
   - TopAppBar
   - LeftToolPanel
   - CenterCanvas area
   - RightSidebar
   - BottomShelf
   - StatusBar
2. Add compact dark design tokens:
   - app background
   - panel background
   - border
   - text primary/secondary/muted
   - accent
   - success/warning/error
   - graph/disabled/grid/guides
3. Add placeholder content only where needed:
   - tools list placeholder
   - inspector placeholder
   - hierarchy placeholder
   - preview/export placeholder
4. Ensure default debug/state graph/docking content is hidden or collapsed.
5. Keep text compact and technical. Avoid marketing headings.

DO NOT IMPLEMENT
- Do not add real docking split/merge.
- Do not add state graph editor.
- Do not add full primitive creation.
- Do not hardcode sample rectangles as random visual tests.
- Do not add heavy shadows, glow, bevel or large decorative UI.

FILES / MODULES
Allowed:
- src/App.tsx if needed
- src/creator/layout/**
- src/styles/**
- src/app/** for workspace mode constants
Forbidden:
- src/core/model deep implementation unless a minimal placeholder already exists
- src/core/stateGraph/**
- src/core/docking/** except comments/docs if already present

ACCEPTANCE
- npm run build passes.
- App opens to a stable 16:9 friendly shell.
- Visible zones: top, left, center, right, bottom, status.
- Debug/logic/docking are not visually dominant.
- No new runtime feature pretends to be finished.
```

## Done oznacza

- UI ma docelowy szkielet.
- Przyszłe funkcje mają gdzie wejść bez tworzenia kolejnych losowych paneli.
- Projekt wizualnie zaczyna przypominać interface creator.
