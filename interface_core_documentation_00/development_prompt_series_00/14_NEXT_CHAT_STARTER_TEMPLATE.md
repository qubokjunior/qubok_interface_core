# 14 — Next chat starter template

Użyj tego jako krótkiego promptu startowego dla nowego czatu/Codex. Dołącz tylko jeden dokument sprintowy z tej serii.

```text
Projekt: qubok_interface_core / qubok_interface_creator
Repo: qubokjunior/qubok_interface_core
Technologia: TypeScript + React + Vite + SVG/HTML + JSON

TRAKTUJ TO JAKO AKTUALNE REGUŁY
- Project JSON / data model is source of truth.
- Core has no React imports.
- Creator imports core, not reverse.
- Persistent mutations go through command layer.
- visual_bbox, layout_bbox and interaction_region are separate.
- Rectangle renders shape; Region handles interaction.
- Default UI exposes only L3/L4 features.
- Debug/state graph/docking/experiments must not dominate default screen unless current sprint explicitly says so.

TRYB PRACY
- Zakładaj, że poprzedni sprint z serii zakończył się sukcesem.
- Wykonaj tylko sprint opisany poniżej.
- Nie rozszerzaj scope poza dokument.
- Jeśli wykryjesz błąd z poprzedniego sprintu, napraw minimalnie tylko to, co blokuje obecny sprint.
- Po zmianach podaj: co zrobiono, zmienione pliki, jak testować, kryteria done, czego celowo nie ruszałeś.

WKLEJONY SPRINT
[tu wklej jeden dokument z development_prompt_series_00]
```

## Reguła wyboru kolejnego sprintu

- Jeśli build nie przechodzi: nie idź dalej, zrób repair prompt dla aktualnego sprintu.
- Jeśli canvas/inspector/hierarchy nie są zsynchronizowane: wróć do Sprint 04.
- Jeśli Panel_Monitor nie jest Project data: wróć do Sprint 05.
- Jeśli default UI wygląda jak debug dashboard: wróć do Sprint 06.
- Jeśli docking zmienia obiekty na canvasie: wróć do Sprint 08 i napraw granicę modeli.
- Jeśli graph patchuje DOM/canvas bez command layer: wróć do Sprint 09/10 i napraw output commands.
