# 13 — Sprint 11: Advanced / post-MVP expansion

Cel: dodać regułę ekspansji po stabilnym MVP. Ten dokument nie jest jednym sprintem implementacyjnym, tylko bramką dla późniejszych funkcji: shape/path, procedural icons, reaction layer, external bridges, bitmap/GPU experiments.

SPRINT 11 — Advanced post-MVP feature gate

Assume previous sprint successful:
- Core, canvas, inspector, hierarchy, component library, panel builder, docking, events and state graph are stable or explicitly marked by maturity level.
- npm run build passed.

Global rules:
- Advanced features enter through maturity levels.
- No experimental feature enters default UI at L0/L1/L2.
- Every advanced feature must declare data owner, command path, validation and UI exposure.

Candidate advanced features:
1. Shape/path editor:
   - live rectangle
   - radius per corner
   - convert to path
   - point editing
   - boolean operations later
2. Value widgets:
   - slider
   - range
   - map range
   - curve editor
   - color/gradient preview
3. Reaction layer:
   - hover/active/disabled variants
   - warning flash
   - pulse/highlight overlays
   - no permanent base style mutation unless command says so
4. Procedural icons:
   - icon primitive recipes
   - temporary numeric icon atlas
   - export to component/SVG
5. External bridges:
   - Blender/Geometry Nodes references
   - Substance/Houdini style graph references
   - import/export adapters
6. Bitmap/GPU experiments:
   - experimental only
   - never required by MVP

Required mini-prompt for each advanced feature:
- Feature name.
- Roadmap phase.
- Maturity start and target.
- Data owner.
- Commands needed.
- Validation needed.
- UI exposure: hidden, debug, workspace, default.
- What this feature must not touch.
- Acceptance test.

Do not implement:
- Do not add all advanced features together.
- Do not bypass command layer.
- Do not introduce direct DOM mutations.
- Do not rewrite the existing MVP architecture.
- Do not make experiments mandatory for build.

Acceptance for any advanced feature:
- npm run build passes.
- Feature has a maturity level.
- Feature can be disabled or hidden if not L3/L4.
- Existing canonical tests still pass: selection sync, Panel_Monitor, component instance, save/export, validation.
