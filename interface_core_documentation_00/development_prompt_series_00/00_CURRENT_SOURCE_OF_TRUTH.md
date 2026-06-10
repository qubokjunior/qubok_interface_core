# 00 — Current source of truth for qubok_interface_core

status: active
version: v2.1
doc_type: canonical_entry_point
last_updated: 2026-06-10

## Cel

Ten dokument jest głównym wejściem do aktualnej dokumentacji `qubok_interface_core`. Jeżeli inne dokumenty są sprzeczne, ten plik oraz dokumenty v2.1 mają wyższy priorytet niż starsze opisy sprintów.

## Aktualny werdykt

`qubok_interface_core` pozostaje parametrycznym silnikiem UI, nie UI kitem i nie pojedynczą aplikacją. Każdy element interface powinien być obiektem danych w Project modelu. Canvas, inspector, hierarchy, export, component library, debug, events i state graph są widokami albo warstwami operacyjnymi nad tym modelem.

## Obowiązujące zasady

1. Project JSON / Project model jest source of truth.
2. `src/core` nie importuje React ani `creator`.
3. `creator` może importować `core`, ale nie odwrotnie.
4. Wszystkie trwałe mutacje idą przez command layer.
5. Command history / undo contract musi istnieć przed ciężką edycją canvasu.
6. Selectors są wspólną warstwą odczytu dla canvas, inspector, hierarchy, status i target resolver.
7. `visual_bbox`, `layout_bbox` i `interaction_region` są oddzielne.
8. Rectangle renderuje shape. Region obsługuje hit/hover/drag/drop/resize/snap/scroll.
9. Render MVP może być SVG/HTML, ale core używa render adaptera.
10. Event/action registry jest osobną headless warstwą, nie częścią visual state graph.
11. External libraries mogą być tylko adapterami, nie właścicielami danych.
12. Default UI pokazuje tylko L3/L4. Debug, graph, docking workbench i eksperymenty nie dominują ekranu startowego.
13. Panel_Monitor sample musi być Project data, nie hardcoded JSX mockup.
14. Tauri, bridges, procedural icons, bitmap graph i advanced graph tools są post-MVP albo experimental.

## Aktualna roadmapa wysokiego poziomu

```text
Foundation
-> Shell
-> Core model
-> Command layer
-> Command history
-> Selectors
-> Validation + tests
-> Render adapter types
-> Canvas renderer
-> Primitive creation
-> Selection / transform
-> Inspector / hierarchy sync
-> BBox / region debug
-> Performance baseline
-> Component proof
-> Save / export
-> Default UI cleanup
-> Component library
-> Layout / panel builder
-> Docking shell
-> Events / actions
-> State graph workspace
-> Advanced post-MVP
```

## Aktualny najbliższy priorytet

Najbliższy development powinien wykonać `Sprint 02 v2.1`:

```text
src/core/model
src/core/commands
src/core/selectors
src/core/validation
src/core/render
tests/core
```

Cel: model, commands, command history, selectors, validation, tests i render adapter types przed rozbudową canvasu i inspectora.

## Dokumenty canonical

| Plik | Status | Użycie |
|---|---|---|
| `00_CURRENT_SOURCE_OF_TRUTH.md` | active | główny entry point |
| `15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md` | active | uzasadnienie techniczne v2.1 |
| `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md` | active | aktualna roadmapa techniczna |
| `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md` | active | zasady bibliotek zewnętrznych |
| `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md` | active | korekty dla starszych promptów |
| `20_AI_CONTEXT_MINIMAL.md` | active | najkrótszy kontekst dla AI/Codex |
| `22_FEATURE_DEPENDENCY_MATRIX.md` | active | zależności funkcjonalności |
| `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md` | active | mapa owner -> command -> view -> validation |
| `24_CANONICAL_GLOSSARY.md` | active | słownik pojęć |

## Dokumenty prompt chain

Dokumenty `01`–`14` są aktywne jako prompt chain, ale należy czytać je przez korektę v2.1 z `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`.

## Reguła konfliktu

Jeżeli dokumenty są sprzeczne, stosuj priorytet:

1. Ten plik.
2. Roadmap v2.1.
3. Architecture update v2.1.
4. Implementation changelog v2.1.
5. External adapter policy.
6. Current sprint prompt.
7. Starsze prompt files.
8. Archiwalne notatki / wcześniejsze visual prompts.

## Czego obecnie nie robić

- Nie zaczynać od full state graph UI.
- Nie dodawać docking jako pierwszej dużej funkcji.
- Nie dodawać React Flow/FlexLayout/XState/Tauri przed ich adapter boundary.
- Nie budować sample scene jako JSX mockup.
- Nie mieszać app shell layout, canvas object layout i graph viewport layout.
- Nie robić toolbaru jako długiej listy wszystkich komend.
- Nie pokazywać debug labels jako default screen.

## Definicja sukcesu dokumentacji

Nowy czat albo Codex powinien móc przeczytać:

1. `00_CURRENT_SOURCE_OF_TRUTH.md`
2. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
3. aktualny sprint file

...i wykonać jeden ograniczony etap bez czytania całej historii projektu.