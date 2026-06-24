# 00 — Current source of truth for qubok_interface_core

status: active
version: v2.2
doc_type: canonical_entry_point
last_updated: 2026-06-24

## Cel

Ten dokument jest głównym źródłem prawdy aktualnej dokumentacji `qubok_interface_core`. Jeżeli inne dokumenty są sprzeczne, ten plik oraz dokumenty v2.2 mają wyższy priorytet niż starsze opisy sprintów.

Jeżeli pracujesz z Codex i chcesz oszczędzać tokeny, zacznij od `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`. Ten plik mówi, które dokumenty czytać dla danego typu zadania i których nie czytać.

## Nazewnictwo obowiązujące od v2.2

- Używać `node_graph`, `nodeGraph`, `NodeGraph` i „node graph”.
- Nie dodawać nowych publicznych nazw opartych na starym graph naming.
- Jeżeli starszy dokument lub kod używa starej nazwy, traktować ją jako superseded i migrować do `node_graph`.
- Właściwa nazwa projektu: `qubok_interface_core` albo skrócone `interface_core`, zależnie od kontekstu.
- Nie używać tymczasowych nazw projektu kończących się `_00` dla aktualnego projektu.

## Lokalna ścieżka projektu na PC

```text
I:\Art\_AI\app_development\qubok_interface_core
```

Ta ścieżka jest lokalną ścieżką roboczą użytkownika. GitHub repo nadal pozostaje `qubokjunior/qubok_interface_core`.

## Aktualny werdykt

`qubok_interface_core` pozostaje parametrycznym silnikiem UI, nie UI kitem i nie pojedynczą aplikacją. Każdy element interface powinien być obiektem danych w Project modelu. Canvas, inspector, hierarchy, export, component library, debug, events i node graph są widokami albo warstwami operacyjnymi nad tym modelem.

## 2026-06-24 update — feature_core, database, tokens, node_graph and documentation merge

Aktualny model architektury:

```text
design tokens
-> feature_core
-> runtime shell
-> interaction layer
-> rule engine
-> node_graph
-> debug/inspection
```

- Design tokens opisują wizualne wartości: colors, type, layout, parts, node_graph i runtime_feedback. Figma variables są visual mirror; runtime/development source of truth pozostaje w repo: JSON/TypeScript, schema i generowane artefakty.
- `feature_core` jest funkcjonalną bazą definicji dla zachowania engine. Grupuje `runtime/*`, `layout/*`, `events/*`, `actions/*`, `nodes/*`, `colors/*`, `debug/*`.
- `feature_core` nie zastępuje Project modelu. Project JSON / Project model nadal zapisuje konkretny dokument użytkownika i pozostaje persistent source of truth.
- Runtime rules są pierwsze: dock shell, split/merge/resize paneli, panel_resize_rule, panel_readable_size, compact_when_default, collapse_when_small, overflow_rule, value field math, command palette, event/action runtime, debug trace / dry run i lekka logika theme/color.
- `node_graph` ma później wizualnie edytować istniejącą logikę runtime/events/actions/rules. Nie jest drugim engine i nie powinien przejmować ownership event/action runtime.
- Rekomendowana przyszła struktura repo to `src/core/database/{tokens,features,nodes,schemas,generated}` oraz `scripts/database/{generate,validate,export}`. To kierunek architektury, nie wymóg tego patcha.

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
10. Event/action registry jest osobną headless warstwą, nie częścią visual node graph.
11. External libraries mogą być tylko adapterami, nie właścicielami danych.
12. Default UI pokazuje tylko L3/L4. Debug, graph, docking workbench i eksperymenty nie dominują ekranu startowego.
13. Panel_Monitor sample musi być Project data, nie hardcoded JSX mockup.
14. Tauri, bridges, procedural icons, bitmap graph i advanced node graph tools są post-MVP albo experimental.

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
-> Node graph workspace
-> Advanced post-MVP
```

## Aktualny najbliższy priorytet

Najbliższy development powinien wykonać `Sprint 02 v2.2`:

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
| `00_CURRENT_SOURCE_OF_TRUTH.md` | active | główne źródło prawdy |
| `15_ARCHITECTURE_UPDATE_V2_1_IMPLEMENTATION_DIAGNOSIS.md` | active_with_v2_2_naming | uzasadnienie techniczne v2.1/v2.2 |
| `16_ROADMAP_V2_1_REVISED_IMPLEMENTATION_ORDER.md` | active_with_v2_2_naming | aktualna roadmapa techniczna |
| `17_EXTERNAL_ADAPTER_POLICY_AND_LIBRARY_DECISIONS.md` | active_with_v2_2_naming | zasady bibliotek zewnętrznych |
| `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md` | active_with_v2_2_naming | korekty dla starszych promptów |
| `20_AI_CONTEXT_MINIMAL.md` | active | najkrótszy kontekst dla AI/Codex |
| `22_FEATURE_DEPENDENCY_MATRIX.md` | active_with_v2_2_naming | zależności funkcjonalności |
| `23_DATA_OWNER_COMMAND_VIEW_VALIDATION_MAP.md` | active_with_v2_2_naming | mapa owner -> command -> view -> validation |
| `24_CANONICAL_GLOSSARY.md` | active_with_v2_2_naming | słownik pojęć |
| `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` | active | pierwsza mapa nawigacyjna dla Codex |
| `30_FEATURE_CORE_DATABASE_AND_RUNTIME_RULES.md` | active | feature_core, database, tokens i runtime rules |

## Dokumenty prompt chain

Dokumenty `01`–`14` są aktywne jako prompt chain, ale należy czytać je przez korektę v2.2 z `18_IMPLEMENTATION_CHANGELOG_FOR_EXISTING_PROMPTS.md`.

## Reguła konfliktu

Jeżeli dokumenty są sprzeczne, stosuj priorytet:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` dla wyboru dokumentów do czytania.
2. Ten plik dla reguł canonical.
3. `30_FEATURE_CORE_DATABASE_AND_RUNTIME_RULES.md` dla feature_core, database, tokens i runtime rules.
4. Roadmap v2.2.
5. Architecture update v2.1/v2.2.
6. Implementation changelog v2.2.
7. External adapter policy.
8. Current sprint prompt.
9. Starsze prompt files.
10. Archiwalne notatki / wcześniejsze visual prompts.

## Czego obecnie nie robić

- Nie zaczynać od full node graph UI.
- Nie dodawać docking jako pierwszej dużej funkcji.
- Nie dodawać external UI/graph/docking libraries przed ich adapter boundary.
- Nie budować sample scene jako JSX mockup.
- Nie mieszać app shell layout, canvas object layout i graph viewport layout.
- Nie robić toolbaru jako długiej listy wszystkich komend.
- Nie pokazywać debug labels jako default screen.
- Nie przenosić source of truth do Figma variables.
- Nie traktować `feature_core` jako zamiennika Project modelu.

## Definicja sukcesu dokumentacji

Nowy czat albo Codex powinien móc przeczytać:

1. `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`
2. `00_CURRENT_SOURCE_OF_TRUTH.md`
3. `01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md`
4. aktualny sprint file

...i wykonać jeden ograniczony etap bez czytania całej historii projektu.
