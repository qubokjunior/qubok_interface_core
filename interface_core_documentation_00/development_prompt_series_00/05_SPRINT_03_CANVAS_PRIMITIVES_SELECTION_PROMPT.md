# 05 — Sprint 03: Canvas renderer + primitive tools + selection/transform

## Cel sprintu

Połączyć headless Project model z widocznym canvasem i minimalnym alfabetem obiektów. Od tego momentu użytkownik powinien móc tworzyć primitive, zaznaczać je i transformować przez command layer.

## Prompt do użycia

```text
SPRINT 03 — Canvas renderer, primitive creation, selection and transform

ASSUME PREVIOUS SPRINT SUCCESSFUL
- Core Project model exists.
- Commands exist for object.create, object.patch, selection.set and transform.update.
- Validation skeleton exists.
- npm run build passed.

GLOBAL RULES
- Canvas renders Project objects; canvas is not source of truth.
- Pan/zoom modifies Project.viewport or workspace viewport state, not object transforms.
- Object creation and transform commits go through commands.
- Region objects are allowed as data but not full event system yet.

CURRENT PHASE
- Phase: Canvas renderer + primitive creation + selection/transform.
- Maturity target: L3 user workflow MVP for basic canvas editing.
- Data owner: Project.objects_by_id, Project.root_children, Project.selection, Project.viewport.

IMPLEMENT ONLY
1. Canvas renderer:
   - render rectangle
   - render text_rectangle
   - render line if already supported by model
   - optional render empty/region only in debug placeholder mode
2. Viewport basics:
   - grid visible
   - pan
   - zoom
   - worldToScreen / screenToWorld helpers
3. Primitive tools:
   - Select
   - Rectangle
   - Text Rectangle
   - Line if model supports it safely
   - Region Rectangle as invisible normal-mode object
4. Creation flow:
   - click or drag creates object
   - object.create command
   - new object becomes selected
   - hierarchy/inspector placeholders can read active selection if available
5. Selection:
   - click select
   - shift click add/remove if simple
   - drag box select
   - selected outline
6. Transform:
   - drag move selected object(s)
   - resize handles if feasible in this sprint
   - delete selected if command exists
   - transform.update on commit

DO NOT IMPLEMENT
- Do not add full inspector tabs yet.
- Do not add component library.
- Do not add panel builder.
- Do not add state graph/event registry.
- Do not add app shell docking.
- Do not store duplicated object positions in component local state after commit.

FILES / MODULES
Allowed:
- src/creator/canvas/**
- src/creator/tools/**
- src/creator/state/**
- src/core/geometry/**
- src/core/layout/grid.ts if needed
- src/core/commands/** only for missing small command support
Forbidden:
- src/core/stateGraph/**
- src/core/docking/**
- large inspector implementation

ACCEPTANCE
- npm run build passes.
- Create rectangle on canvas.
- Create text rectangle on canvas.
- Click selects object; selected outline appears.
- Box select can select multiple objects or is explicitly documented as pending if not feasible.
- Move selected object; Project transform changes through command.
- Pan/zoom does not alter object transform.
- Validation still runs after object creation.
```

## Done oznacza

- Model jest widoczny i edytowalny.
- Canvas, tools i selection pracują na Project.
- Nie ma jeszcze potrzeby graphu ani docking, bo podstawowy editor dopiero powstaje.
