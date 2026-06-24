# 30 — Feature core database and runtime rules

status: active
version: v2.2 + 2026-06-24 amendment
doc_type: canonical_reference
last_updated: 2026-06-24

## Cel

Ten dokument jest kompaktowym odniesieniem dla `feature_core`, przyszłej struktury baz danych repozytorium, design tokens i runtime rules. Ma ograniczyć duplikację w starszych notatkach o tokenach, Figma variables, node graph i panel rules.

## Pozycja w architekturze

```text
design tokens
-> feature_core
-> runtime shell
-> interaction layer
-> rule engine
-> node_graph
-> debug/inspection
```

| Warstwa | Odpowiada za | Nie jest |
|---|---|---|
| design tokens | wygląd: kolory, typografia, spacing, części UI, feedback | runtime behavior |
| feature_core | definicje funkcjonalne i konfiguracja zachowania silnika | Project model replacement |
| runtime shell | faktyczne wykonanie UI: panele, dock, resize, command dispatch | edytor graficzny logiki |
| interaction layer | hover/click/drag/value edit/selection routing | visual style database |
| rule engine | warunki, gates, panel rules, transforms, command emission | node graph canvas |
| node_graph | wizualna edycja istniejącej logiki | drugi silnik runtime |
| debug/inspection | trace, dry run, preview, raporty spójności | default screen |

## feature_core — cel

`feature_core` to funkcjonalna baza definicji dla zachowania `interface_core`. Grupuje zmienne, reguły, akcje, konfiguracje i definicje funkcji tak, aby engine mógł je czytać, walidować i później edytować wizualnie przez `node_graph`.

Nie zastępuje `Project model`. Project nadal zapisuje konkretny dokument/interfejs użytkownika. `feature_core` opisuje dostępne mechanizmy i ich konfigurację.

## Minimalna struktura feature_core

```text
feature_core/
  runtime/*
  layout/*
  events/*
  actions/*
  nodes/*
  colors/*
  debug/*
```

| Grupa | Przykłady | Właściciel sensu |
|---|---|---|
| `runtime/*` | shell modes, command palette, value field math, runtime feedback | zachowanie aplikacji |
| `layout/*` | panel_resize_rule, readable sizes, overflow_rule, compact_when_default | czytelność i responsywność |
| `events/*` | on_init, on_panel_open, on_panel_resize, on_hover, on_click, on_drag, on_value_changed, on_selection_changed | trigger model |
| `actions/*` | set_visible, set_layout_mode, set_panel_size, set_token_value, apply_color_transform, emit_command | command-backed behavior |
| `nodes/*` | node definitions, sockets, previews, allowed output contracts | wizualna edycja później |
| `colors/*` | color_swatch, palette_roles, target_by_color_role, transforms | role kolorów i procedural palette |
| `debug/*` | dry run, trace fields, validation views, preview targets | diagnostyka |

`nodes/*` musi pozostać wyraźnie oddzielone od nie-node runtime features. Node definition może opisywać widoczną reprezentację funkcji, ale nie przejmuje ownership runtime.

## Relacja do design tokens

Design tokens są warstwą wizualną. Figma variables mogą być zorganizowane jako:

```text
colors
type
layout
parts
node_graph
runtime_feedback
```

Figma jest visual mirror. Development source of truth powinien być w repo: JSON/TypeScript, walidowane schema i generowane eksporty. Nie robić z Figma runtime source of truth.

## Rekomendowana przyszła struktura repo

```text
src/core/database/
  tokens/
  features/
  nodes/
  schemas/
  generated/
scripts/database/
  generate/
  validate/
  export/
interface_core_documentation_00/
  architecture/
  decisions/
  roadmap/
  guides/
```

Ta struktura jest propozycją kierunku, nie obowiązkowym refactorem obecnego patcha.

## Relacja do Project model

| Element | Rola |
|---|---|
| Project model | zapisuje konkretny dokument/interfejs i jego obiekty |
| Project JSON | serializacja dokumentu użytkownika |
| feature_core | opisuje dostępne funkcje, reguły, akcje, konfiguracje engine |
| design tokens | opisują wizualne wartości i role |
| generated database output | może zasilać runtime, walidację i UI wyboru |

Trwałe mutacje Project nadal przechodzą przez command layer. Commands pozostają serializable.

## Relacja do node_graph

`node_graph` ma później wizualizować i edytować istniejące runtime/event/action/rule logic. Nie może stać się drugim źródłem prawdy ani osobnym silnikiem akcji.

Przydatne node concepts:

| Grupa | Node concepts |
|---|---|
| space/target | `screen_space`, `workspace_space`, `local_space`, `target_by_id`, `target_by_type`, `target_by_tag`, `target_by_role` |
| panel/layout | `panel_resize_rule`, `compact_when_default`, `rule_gate`, `compare`, `switch` |
| curve/value | `curve_parameter`, `trim_curve`, `map_range`, `float_curve` |
| preview/debug | `preview_gradient`, `preview_targets` |

Nie kopiować Blender Geometry Nodes 1:1. Pożyczać model pól, selekcji, curve factor 0-1, map range i preview, ale dostosować do UI engine.

## MVP order

Najpierw stabilny, strawny engine:

```text
dock shell
-> split / merge / resize panels
-> panel resize rules
-> compact default hiding
-> value field math
-> command palette
-> event/action runtime
-> debug trace / dry run
-> lightweight theme/color logic
```

Dopiero później:

```text
node_graph visual editor
-> path/curve graph
-> advanced procedural systems
```

## Panel rule concepts

| Concept | Cel |
|---|---|
| `panel_resize_rule` | decyduje jak panel reaguje na zmianę rozmiaru |
| `panel_readable_size` | minimalne/wygodne progi czytelności treści |
| `compact_when_default` | ukrywa pola z domyślną wartością |
| `collapse_when_small` | zwija sekcje przy małej szerokości/wysokości |
| `overflow_rule` | decyduje: scroll, wrap, truncate, hide, popover |
| `hide empty/default fields` | usuwa wizualny szum bez kasowania danych |
| `reveal on hover/edit/debug` | pokazuje ukryte dane tylko w odpowiednim trybie |

Czytelny layout ma pierwszeństwo przed gęstym wzrostem liczby funkcji.

## Color/procedural palette concepts

| Concept | Cel |
|---|---|
| `color_swatch` | pojedyncza próbka koloru |
| `swatch_stack` | zestaw próbek do wyprowadzenia palety |
| `palette_from_swatches` | generuje paletę z próbek |
| `palette_roles` | role typu background, border, accent, warning |
| `target_by_color_role` | wybiera elementy przez rolę koloru |
| `shift_saturation` | zmienia nasycenie zbioru kolorów |
| `shift_luminance` | zmienia jasność zbioru kolorów |
| `shift_hue` | przesuwa hue zbioru kolorów |
| `assign_color_by_role` | przypisuje kolor według roli |

Przykład: zmniejszenie saturacji wszystkich ról UI o 10% powinno działać przez targeting roli i transform, nie przez ręczną edycję każdego komponentu.

## Czego nie implementować jeszcze

- Nie robić broad repo reorganization tylko dlatego, że doc pokazuje przyszłą strukturę.
- Nie przenosić source of truth do Figma.
- Nie robić `node_graph` jako wymaganego właściciela event/action runtime.
- Nie tworzyć path/curve subsystem przed stabilnym runtime shell i panel rules.
- Nie eksponować debug/inspection jako default screen.
- Nie mieszać layout ownership: app shell docking, canvas object layout i node graph viewport pozostają osobne.
