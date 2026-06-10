# 14 — Next chat starter template

status: active
version: v2.2
doc_type: prompt_starter
last_updated: 2026-06-10

Użyj tego jako krótkiego promptu startowego dla nowego czatu/Codex. Dołącz tylko jeden dokument sprintowy z tej serii oraz, jeśli to możliwe, `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`, `00_CURRENT_SOURCE_OF_TRUTH.md` i `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`.

```text
Projekt: qubok_interface_core / interface_core
Repo: qubokjunior/qubok_interface_core
Lokalna ścieżka PC: I:\Art\_AI\app_development\qubok_interface_core
Technologia: TypeScript + React + Vite + SVG/HTML + JSON

TRAKTUJ TO JAKO AKTUALNE REGUŁY v2.2
- Project JSON / Project model is source of truth.
- Core has no React imports.
- Creator imports core, not reverse.
- Persistent mutations go through command layer.
- Command payloads stay serializable.
- Command history / undo contract must exist before heavy canvas editing.
- Core selectors are the read boundary for canvas, inspector, hierarchy, status and target resolver.
- visual_bbox, layout_bbox and interaction_region are separate.
- Rectangle renders shape; Region handles interaction.
- Render MVP can be SVG/HTML, but Project is not the renderer model.
- Event/action registry is headless and separate from visual node graph.
- External libraries are adapters, not source of truth.
- Use node_graph / nodeGraph / NodeGraph / node graph naming.
- Do not use old state_graph naming or temporary project names ending with _00.
- Default UI exposes only L3/L4 features.
- Debug/node graph/docking/experiments must not dominate default screen unless current sprint explicitly says so.

TRYB PRACY
- Zakładaj, że poprzedni sprint z serii zakończył się sukcesem.
- Wykonaj tylko sprint opisany poniżej.
- Nie rozszerzaj scope poza dokument.
- Jeśli wykryjesz błąd z poprzedniego sprintu, napraw minimalnie tylko to, co blokuje obecny sprint.
- Jeżeli starszy dokument jest sprzeczny z v2.2, preferuj 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md, 00_CURRENT_SOURCE_OF_TRUTH.md i 18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md.
- Po zmianach podaj: co zrobiono, zmienione pliki, jak testować, kryteria done, czego celowo nie ruszałeś, ryzyka/pending.

MINIMALNY KONTEKST DO WKLEJENIA
1. 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
2. 00_CURRENT_SOURCE_OF_TRUTH.md
3. 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
4. [jeden aktualny dokument sprintowy]

WKLEJONY SPRINT
[tu wklej jeden dokument z development_prompt_series_00]
```

## Reguła wyboru kolejnego sprintu

- Jeśli build nie przechodzi: nie idź dalej, zrób repair prompt dla aktualnego sprintu.
- Jeśli brakuje command history/selectors/validation po Sprint 02: wróć do Sprint 02 v2.2.
- Jeśli canvas/inspector/hierarchy nie są zsynchronizowane: wróć do Sprint 04.
- Jeśli Panel_Monitor nie jest Project data: wróć do Sprint 05.
- Jeśli default UI wygląda jak debug dashboard: wróć do Sprint 06.
- Jeśli docking zmienia obiekty na canvasie: wróć do Sprint 08 i napraw granicę modeli.
- Jeśli event/action registry wymaga node graph do działania: wróć do Sprint 09.
- Jeśli node graph patchuje DOM/canvas bez command layer: wróć do Sprint 09/10 i napraw output commands.
- Jeśli external library przejmuje model danych: zastosuj adapter policy z dokumentu 17.

## Najbliższy zalecany sprint

Jeżeli projekt nie ma jeszcze stabilnego core v2.2, użyj:

`04_SPRINT_02_CORE_MODEL_COMMANDS_VALIDATION_PROMPT.md`

Ten sprint powinien dostarczyć: Project model, commands, command history, selectors, validation, tests i render adapter types.