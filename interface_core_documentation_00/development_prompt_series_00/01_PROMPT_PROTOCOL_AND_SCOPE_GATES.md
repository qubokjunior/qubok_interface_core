# 01 — Prompt protocol and scope gates

Używaj tego dokumentu jako stałej warstwy kontrolnej dla każdego kolejnego promptu developmentowego.

## Minimalny format każdego promptu

```text
SPRINT: [numer] — [jeden cel]

ASSUME PREVIOUS SPRINT SUCCESSFUL
- npm run build passed.
- Manual acceptance from previous sprint passed.
- No unresolved TypeScript errors.
- No known broken source-of-truth sync.

GLOBAL RULES
- Project JSON / data model is source of truth.
- Core has no React imports.
- Creator imports core, not reverse.
- Persistent mutations go through command layer.
- visual_bbox, layout_bbox and interaction_region remain separate.
- Rectangle renders; Region interacts.
- Default UI exposes only L3/L4 features.

CURRENT PHASE
- Phase: [roadmap phase]
- Maturity target: [L1/L2/L3/L4]
- Data owner: [Project / InterfaceObject / workspace UI state / DockLayout / StateGraph]

IMPLEMENT ONLY
1. ...
2. ...
3. ...

DO NOT IMPLEMENT
- ...
- ...

FILES / MODULES
Allowed:
- ...
Forbidden:
- ...

ACCEPTANCE
- npm run build passes.
- [manual test]
- [sync test]
- [validation test]
- Response lists changed files, what was done, and how to test.
```

## Scope gates

| Gate | Must be true before moving on |
|---|---|
| Core gate | model, commands and validation build without React imports |
| View gate | canvas, inspector, hierarchy and status read the same selection |
| Region gate | visual/layout/interaction overlays are distinguishable |
| Component gate | button_group and Panel_Monitor are Project data, not JSX mockups |
| Export gate | Project JSON / Component JSON / SVG survive validation |
| UI gate | default screen is clean, debug not dominant |
| Library gate | component save and instantiate generate valid IDs |
| Docking gate | app shell layout is separate from canvas object layout |
| Logic gate | events/actions output commands only |
| Graph gate | graph viewport has pan/zoom/multiselect before full graph complexity |

## Token optimization rules

1. Do not paste full documentation into every prompt.
2. Paste only this protocol plus the current sprint file.
3. When a sprint fails, next prompt should be a repair prompt for that sprint only.
4. Never ask Codex to implement state graph, docking, panel builder and component library in one pass.
5. Each prompt should mention explicitly what not to touch.
6. Prefer complete files or safe patches over scattered fragments.
7. Require build and manual acceptance at the end of every sprint.

## Anti-patterns

| Anti-pattern | Result | Correction |
|---|---|---|
| “Add all UI features” | huge noisy patch | one feature owner per sprint |
| “Make it like Blender” | vague docking/layout confusion | specify app shell vs canvas layout |
| “Add state graph” | premature visual graph chaos | headless event/action registry first |
| “Improve UI” | visual polish without architecture | target shell/spacing/tokens only |
| “Fix everything” | unstable patch | diagnose one failing gate |

## Mandatory final response format from Codex/chat

```text
Co zrobiono
- ...

Zmienione pliki
- ...

Jak testować
1. ...
2. ...

Kryteria done
- ...

Czego celowo nie ruszałem
- ...
```
