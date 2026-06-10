# 02 — Sprint 00: Foundation audit / decyzje / maturity map

## Cel sprintu

Stworzyć albo uporządkować warstwę dokumentacyjno-konfiguracyjną projektu przed implementacją kolejnych funkcji. Ten sprint nie powinien zmieniać runtime funkcjonalności aplikacji poza ewentualnym dodaniem małych plików konfiguracyjnych/metadanych używanych przez kolejne etapy.

## Prompt do użycia

```text
SPRINT 00 — Foundation audit, scope lock and maturity map

ASSUME PREVIOUS SPRINT SUCCESSFUL
This is the first sprint in this chain. Treat current repository state as existing baseline. Do not rewrite the app yet.

GLOBAL RULES
- Project JSON / data model is source of truth.
- Core must remain React-free.
- Creator may import core; core may not import creator.
- Persistent changes must go through command layer in later sprints.
- visual_bbox, layout_bbox and interaction_region are separate concepts.
- Default UI must not become debug registry or feature dump.

CURRENT PHASE
- Phase: Foundation / decisions.
- Maturity target: L0/L1 documentation and metadata only.
- Data owner: documentation + optional app metadata files.

IMPLEMENT ONLY
1. Inspect the repository structure and identify existing folders/modules relevant to core, creator, panels, canvas, stateGraph, docking, validation and samples.
2. Add a concise `docs/dev/FOUNDATION_AUDIT.md` or equivalent if the project has a documentation folder.
3. Add a `docs/dev/FEATURE_MATURITY_MATRIX.md` or equivalent listing major features and their current/target level: L0-L5.
4. Add a `docs/dev/MODULE_BOUNDARIES.md` or equivalent describing allowed and forbidden imports.
5. Do not refactor code unless a tiny import-boundary comment or metadata export is necessary.
6. Include a recommended next sprint based on the actual repo state.

DO NOT IMPLEMENT
- Do not add new UI panels.
- Do not add state graph UI.
- Do not add docking.
- Do not add new primitive tools.
- Do not rename large folder structures.
- Do not rewrite App.tsx.

FILES / MODULES
Allowed:
- docs/** or interface_core_documentation_00/**
- optional small metadata file under src/app/ only if already established
Forbidden:
- src/core runtime refactor
- src/creator runtime refactor
- src/stateGraph runtime refactor
- package-level dependency changes unless necessary for docs scripts

ACCEPTANCE
- npm run build still passes if project is buildable.
- Documentation clearly states current gaps and next sprint.
- Feature maturity levels exist for: model, commands, validation, canvas, primitive, selection, inspector, hierarchy, region, save/export, component library, panel builder, docking, event registry, state graph.
- Response lists changed files and confirms no runtime scope was added.
```

## Done oznacza

- Wiadomo, co jest aktualnie w repo.
- Wiadomo, które funkcje są L0/L1/L2/L3/L4/L5.
- Wiadomo, czego nie wolno eksponować w default UI.
- Następny prompt może wejść w scaffold albo core bez ponownego czytania całej dokumentacji.
