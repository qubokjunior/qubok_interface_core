# QUBOK_INTERFACE_CORE — Atlas update: wartości, referencje, reguły, instancje

Status: documentation update
Source: seria wygenerowanych atlasów PNG 01/08–08/08 oraz bieżący kontekst projektu
Date: 2026-06-07

## 1. Cel aktualizacji

Ten dokument dopisuje do `interface_core_documentation_00` wnioski wynikające z nowej serii grafik atlasowych dotyczących:

- linkowania wartości parametrów,
- referowania i przypisywania obiektów,
- rules authoring / rules assignment,
- polityk rozmiaru panel × grupa,
- `instance on points`,
- orientacji / rotacji instancji,
- workflow budowania funkcjonalnego elementu przez instancjonowanie,
- trybów podglądu i filtrów: Default, Debug, Rules, State Machine, Link Elements & References.

Aktualizacja nie zastępuje obecnego fundamentu `primitive -> bbox -> region -> group -> component`; rozszerza go o warstwę parametrów, zależności, reguł i generatywnych helperów UI.

## 2. Syntetyczne podsumowanie atlasów 01/08–08/08

| Atlas | Temat | Najważniejsza decyzja projektowa |
|---|---|---|
| 01/08 | Value linking between parameters | Parametry mogą być źródłami i celami zależności; zmiana źródła propaguje się do celów przez jawny system linków i walidację cykli. |
| 02/08 | Object references / assignment | Obiekty mogą jawnie referować inne obiekty jako źródła geometrii, osie, bboxy, punkty min/max, targety wartości i obiekty uchwytów. |
| 03/08 | Rules authoring / display / assignment | Reguły są osobnym systemem definiującym ograniczenia, warunki, priorytety i zachowanie komponentów. |
| 04/08 | Panel × group size policy modes | Panel może respektować min/max dziecka, ignorować je i wyrównywać dziecko, albo narzucać własny rozmiar i wymuszać adaptację. |
| 05/08 | Instance on points — fundamentals | Uchwyty i helpery mogą być generowane jako instancje na punktach / segmentach z parametrami factor, offset, segment length i direction. |
| 06/08 | Instance orientation variants | Instancje mają tryby orientacji: toward center, tangent, normal, fixed world, mirrored, context-aware, overrides. |
| 07/08 | Workflow builder | Budowa funkcjonalnego elementu powinna mieć jawny pipeline: source geometry -> point stream -> instance object -> offset/factor -> rotation -> rules -> validation/preview. |
| 08/08 | Preview modes and filters | Ten sam element musi mieć rozdzielone widoki: Default, Debug, Rules, State Machine, Link Elements & References, z filtrami overlay. |

## 3. Porównanie z aktualnym opisem w GitHub

Obecny opis GitHub poprawnie definiuje:

- `Project JSON / data model = source of truth`,
- rozdzielenie canvas / inspector / hierarchy / export / debug jako widoków modelu,
- pipeline `primitive -> bbox -> transform/style -> region/layout -> group/panel -> exposed parameters -> library asset -> reusable UI component -> event/action/state behavior`,
- rozdzielenie `visual_bbox`, `layout_bbox`, `interaction_region`,
- zasadę, że rectangle renderuje shape, a region obsługuje interakcję,
- potrzebę rozdzielenia app shell docking, canvas object layout i graph viewport layout.

Nowa seria atlasów doprecyzowuje brakujące warstwy, które wcześniej były obecne tylko pośrednio:

1. `parameter_data` musi być pełnoprawną częścią `InterfaceObject`, a nie tylko ogólnym miejscem na exposed parameters.
2. `linked references` muszą być osobnym systemem nad modelem: z typami linków, targetami, kierunkiem propagacji, priorytetem, blokadami i walidacją cykli.
3. `reference assignment` musi obejmować obiekty geometryczne: bbox, axis, min/max endpoint, center, source path, target property, handle object.
4. `rules` muszą być osobnym modelem: rule set, condition, range/value, priority, scope, inheritance, conflict resolution, runtime evaluation.
5. Panel/group layout potrzebuje jawnych `size policy modes`, zamiast jednego ogólnego layout mode.
6. `instance on points` jest ważnym advanced/composable subsystemem dla uchwytów, helperów, overlayów, resize handles, guide markers i debug visuals.
7. `preview_mode` musi być nazwanym systemem, a nie pojedynczym debug overlay.

## 4. Nowe pojęcia kanoniczne

### 4.1 Parameter link

`ParameterLink` opisuje zależność między wartością źródłową i jedną albo wieloma wartościami docelowymi.

Minimalne pola:

- `id`,
- `source_object_id`,
- `source_parameter_path`,
- `target_object_id`,
- `target_parameter_path`,
- `expression`,
- `mode`: `live | deferred | manual`,
- `direction`: `one_way | two_way`,
- `lock_target_manual_edit`,
- `priority`,
- `enabled`,
- `validation_status`.

Przykład:

```text
Panel.width -> Label.offset = width * 0.05
Panel.width -> Knob.range = width * 0.90
Panel.width -> Fill.length = width * 0.90
```

Reguła: link nie mutuje UI bezpośrednio. Link generuje kontrolowany update modelu przez command layer albo przez runtime propagation engine zgodny z command/validation path.

### 4.2 Object reference assignment

`ObjectReference` opisuje jawne przypisanie jednego obiektu jako referencji dla innego obiektu.

Minimalne typy referencji:

- `source_object`,
- `target_object`,
- `bbox_reference`,
- `axis_reference`,
- `min_endpoint`,
- `max_endpoint`,
- `center_reference`,
- `path_reference`,
- `region_reference`,
- `handle_object`,
- `target_property`.

Przykład slidera:

```text
Slider_Knob.handle -> Slider_System.value
Slider_Knob.axis -> Slider_Axis_BBox.axis_x
Slider_Knob.min_endpoint -> Slider_Min_Point.position.x
Slider_Knob.max_endpoint -> Slider_Max_Point.position.x
```

Reguła: referencje muszą być walidowane pod kątem istnienia obiektów, zgodności typu, zakresu, osi i cykli zależności.

### 4.3 Rule set

`RuleSet` definiuje ograniczenia i zachowania komponentu, grupy, panelu albo instancji.

Minimalne pola rule:

- `id`,
- `name`,
- `type`: `size | layout | semantic | aesthetic | interaction | state | behavior | export`,
- `condition`,
- `range_or_value`,
- `priority`,
- `scope`,
- `inheritance_mode`,
- `enabled`,
- `fallback`,
- `validation_status`.

Przykład rules dla slider group:

```text
MIN_WIDTH_20: width >= 20 px
MAX_WIDTH_100: width <= 100 px
INTERACTION_HIT_AREA: thumb >= 12 px
SEMANTIC_READABILITY: contrast >= 4.5
BOUNDARY_CLAMP: value out of range -> clamp(min,max)
```

Reguła: wyższy priorytet ma pierwszeństwo, ale konflikt reguł musi pojawić się w validation/debug, nie jako cicha zmiana layoutu.

### 4.4 Panel × group size policy

Panel powinien mieć jawny sposób traktowania reguł rozmiaru dzieci.

Tryby:

| Tryb | Zachowanie |
|---|---|
| `respect_child_min_max` | Panel respektuje min/max dziecka i własny zakres osi. Dziecko nie przekracza własnego limitu. |
| `ignore_child_min_max_align` | Panel ignoruje min/max dziecka; dziecko rośnie do własnego maksimum, potem jest wyrównywane left/center/right. |
| `force_panel_size` | Panel narzuca szerokość; dziecko adaptuje się strategią: shrink proportionally, prioritize readability, wrap if possible, clip/ellipsis. |

To powinno wejść do `layout_data` albo `panel_data`, nie do stylu wizualnego.

### 4.5 Instance on points

`InstanceOnPoints` to subsystem generujący obiekty pomocnicze na punktach, segmentach, krzywych lub bboxach.

Minimalny pipeline:

```text
source geometry
-> point stream / segment stream
-> instance object
-> factor / offset
-> orientation rule
-> bindings / rules
-> validation / preview
```

Przykład użycia:

- resize handles na rogach bbox,
- edge midpoint handles,
- grab handles,
- helper markers,
- measurement guides,
- alignment hints,
- state/debug indicators.

Minimalne pola:

- `source_object_id`,
- `source_geometry_type`: `path | bbox | region | curve | point_list | segment_list`,
- `point_source_mode`,
- `segment_source_mode`,
- `instance_object_id`,
- `factor`,
- `offset_along_segment`,
- `offset_normal`,
- `offset_tangent`,
- `orientation_mode`,
- `target_center_reference`,
- `filters`,
- `rules`,
- `preview_mode`,
- `validation_status`.

### 4.6 Orientation modes for instances

Tryby orientacji instancji:

| Tryb | Sens | Zastosowanie |
|---|---|---|
| `facing_center` | obrót w stronę środka obiektu/grupy | uchwyty skalowania, strzałki do środka, wskaźniki wewnętrzne |
| `facing_outward` | obrót na zewnątrz od środka | etykiety zewnętrzne, offset guides |
| `tangent` | wzdłuż segmentu | przepływ, guide lines, animowane obramowania |
| `normal` | prostopadle do segmentu | odstępy, labels, snap hints |
| `fixed_world` | stała orientacja globalna | ikony UI, markery statusu |
| `mirrored_auto` | automatyczne odbicie względem kierunku | symetryczne handles |
| `context_aware` | reguły decydują o rotacji | wyjątki i złożone przypadki UI |
| `override_per_point` | nadpisanie punktu/segmentu | niestandardowe narożniki / specjalne hit handles |

### 4.7 Preview mode system

`preview_mode` powinien być systemowym trybem widoku, nie tylko flagą debug.

Tryby:

- `default`: produkcyjny wygląd elementu,
- `debug`: bboxy, handles, ID, diagnostyka,
- `rules`: constraints, min/max, aktywne reguły,
- `state_machine`: stany, przejścia, zachowanie,
- `link_elements_references`: linki, referencje, dependency map.

Każdy tryb może mieć filtry overlay:

- bounds,
- handles,
- ids/names,
- diagnostics,
- rules/min/max,
- states/transitions,
- links/references,
- dependencies,
- opacity.

## 5. Aktualizacja modelu danych

### 5.1 Project

Dodać albo doprecyzować pola:

```text
parameter_links
object_references
rule_sets
instance_generators
preview_presets
```

### 5.2 InterfaceObject

Doprecyzować pola:

```text
parameter_data
reference_data
rule_assignments
instance_source_data
instance_output_data
preview_data
```

### 5.3 LayoutData / PanelData

Dodać:

```text
size_policy_mode
child_min_max_policy
alignment_when_child_clamped
child_adaptation_strategy
overflow_policy
readability_policy
```

### 5.4 Validation

Nowe klasy walidacji:

- broken parameter link,
- circular parameter dependency,
- invalid expression,
- missing referenced object,
- invalid reference type,
- invalid axis or endpoint assignment,
- conflicting rules,
- unreachable fallback rule,
- unsafe size policy,
- generated instance count mismatch,
- duplicate generated instance position,
- invalid orientation mode,
- preview overlay conflict.

## 6. Aktualizacja workspaces i paneli

### 6.1 Nowe / rozszerzone workspace

| Workspace | Rola | Maturity |
|---|---|---|
| Link Elements & References | edycja parameter links i object references | L2 -> L4 |
| Rules | rule sets, conditions, priorities, scope, inheritance | L2 -> L4 |
| Instance on Points | generator handles/helpers na punktach i segmentach | L1/L2 -> L5 |
| Preview Modes | kontrola overlay i porównanie trybów widoku | L2 -> L4 |

### 6.2 Inspector tabs

Dodać / doprecyzować tabs:

- `Parameters`,
- `Links`,
- `References`,
- `Rules`,
- `Instances`,
- `Preview`.

### 6.3 Bottom shelf

Dodać panele:

- `Dependency Graph`,
- `Reference Map`,
- `Rule Evaluation History`,
- `Instance List`,
- `Preview Mode Comparison`.

## 7. Roadmap placement

Nowe funkcje nie powinny dominować MVP startowego. Proponowana klasyfikacja:

| Funkcja | Etap | Start maturity | Docelowo |
|---|---:|---|---|
| Parameter sources + schema | Phase C / core model | L1 | L4 |
| Parameter links | Phase M / headless events/actions lub Phase O advanced | L1/L2 | L5 |
| Object references | Phase G/K | L1/L2 | L4 |
| Rule sets | Phase K/M | L1/L2 | L5 |
| Panel size policy modes | Phase K | L2/L3 | L4 |
| Instance on points | Phase O | L1/L2 | L5 |
| Orientation modes | Phase O | L1/L2 | L5 |
| Preview modes and overlay filters | Phase G + Phase I | L2/L3 | L4 |

Minimalny MVP powinien jedynie zarezerwować pola i walidację. Pełne edytory `Rules`, `Link Elements & References` i `Instance on Points` powinny być workspace albo debug/polished tools po stabilizacji core.

## 8. Guardrails

1. Linki parametrów nie mogą tworzyć ukrytych mutacji poza command layer.
2. Targety linków muszą mieć możliwość blokady ręcznej edycji albo oznaczenia jako runtime-derived.
3. Referencje obiektów muszą być jawne i widoczne w Reference Map.
4. Rule engine nie może cicho łamać layoutu; konflikty muszą być raportowane.
5. Panel size policy musi być przypisane do layout/panel data, nie do CSS-only style.
6. Instance on points nie tworzy ręcznie edytowanych dzieci, dopóki użytkownik nie wybierze `bake / convert to objects`.
7. Preview mode zmienia widoczność overlay, nie dane projektowe.
8. Debug/Rules/State/Links overlays mogą być eksportowane do dokumentacji, ale nie powinny trafiać do normalnego SVG UI.

## 9. Kryteria akceptacji dla przyszłej implementacji

### Parameter linking proof

1. Utwórz komponent slider.
2. Ustaw `width` jako source parameter.
3. Powiąż `label_offset = width * 0.05`.
4. Powiąż `knob_range = width * 0.90`.
5. Zmień width.
6. Inspector, live preview, dependency graph i command log pokazują spójną zmianę.
7. Cykle typu `A -> B -> A` są blokowane.

### Reference assignment proof

1. Utwórz `Slider_Knob`.
2. Przypisz `Slider_Axis_BBox` jako axis reference.
3. Przypisz `Slider_Min_Point` i `Slider_Max_Point`.
4. Przypisz `Slider_System.value` jako target property.
5. Przeciągnięcie knob działa tylko w przypisanym zakresie.
6. Reference Map pokazuje pełny graf zależności.

### Rules proof

1. Utwórz `Slider_Group`.
2. Ustaw rules: min width 20 px, max width 100 px, min hit area 12 px.
3. Przypisz rules do grupy.
4. Zmień szerokość panelu.
5. Rule evaluation pokazuje pass/warning/error.
6. Konflikt rule set jest widoczny w validation.

### Instance on points proof

1. Wybierz bbox albo path jako source geometry.
2. Wygeneruj corner handles i edge midpoint handles.
3. Ustaw factor 0.5 dla midpoint.
4. Ustaw rotation mode `facing_center`.
5. Preview pokazuje uchwyty skierowane do środka.
6. Instance list pokazuje ID, segment, factor, offset, pozycję i rotację.
7. Validation wykrywa duplikaty pozycji i błędne orientacje.

## 10. Wniosek końcowy

Nowe atlasy przesuwają opis projektu z poziomu `edytor primitive + component proof` do pełniejszej wizji: `parametryczny system UI`, w którym elementy nie tylko mają transform/style/region, ale również:

- zależności wartości,
- jawne referencje obiektów,
- reguły zachowania,
- polityki adaptacji layoutu,
- generowane helpery/handles,
- wielowarstwowe tryby podglądu.

To nadal powinno być implementowane zgodnie z zasadą dojrzałości: najpierw model i walidacja, potem debug/workbench, dopiero później polished user workflow.
