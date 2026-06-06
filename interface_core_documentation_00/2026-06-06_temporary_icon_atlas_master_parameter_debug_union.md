# QUBOK_INTERFACE_CORE — temporary icon atlas, master parameter panel, library filters, debug bbox union

Data: 2026-06-06
Status: decision addendum / architecture update
Zakres: dokumentacja projektu `qubok_interface_core`, bez zmian w runtime code.

## 1. Cel aktualizacji

Ten dokument dopisuje do aktualnej dokumentacji cztery nowe decyzje systemowe:

1. `temporary_icon_atlas` — tymczasowa biblioteka ikon fallbackowych dla funkcji, narzędzi i komponentów, które nie mają jeszcze właściwej ikony.
2. `master_parameter_panel` — nadrzędny panel parametrów pozwalający przeglądać wszystkie parametry projektu, nie tylko aktywny selection.
3. `library_filter_views` — filtrowanie, sortowanie, wyszukiwanie i zapisane widoki bibliotek zależne od kontekstu workspace.
4. `debug_bbox_union_overlay` — debugowy system łączenia bboxów podobnych/stykających się elementów w jeden wspólny obrys.

Te funkcje wzmacniają istniejące zasady:

- `Project JSON / data model` pozostaje source of truth.
- Canvas, inspector, hierarchy, spreadsheet, library, debug i state graph są widokami albo narzędziami nad modelem.
- Zmiany i przypisania muszą przechodzić przez command/model layer.
- Debug overlay nie może destrukcyjnie zmieniać obiektów źródłowych.

---

## 2. Temporary icon atlas

### 2.1. Problem

W czasie developmentu często pojawi się funkcjonalność, narzędzie, grupa, node, region, preset albo panel, który powinien posiadać ikonę, ale nie ma jeszcze zaprojektowanej specyficznej ikony.

Bez systemu fallbackowego UI zacznie używać przypadkowych placeholderów, pustych miejsc albo niekonsekwentnych etykiet tekstowych.

### 2.2. Decyzja

System posiada `temporary_icon_atlas` — bibliotekę tymczasowych ikon opartych na cyfrach.

Biblioteka zawiera:

- glyphy pojedynczych cyfr `0-9`,
- fallback icons dwucyfrowe `00-99`.

Do reprezentowania brakujących ikon funkcji/narzędzi/komponentów używane są wyłącznie ikony dwucyfrowe `00-99`.

Pojedyncze cyfry `0-9` mogą istnieć w atlasie jako glyphy pomocnicze, ale nie są używane jako tymczasowe ikony przypisywane do brakujących funkcji.

### 2.3. Reguła przypisywania

Jeżeli element UI potrzebuje ikony, a nie istnieje jeszcze specyficzna ikona:

1. System sprawdza `icon_registry`.
2. System znajduje pierwszy wolny slot `00-99`.
3. Slot zostaje przypisany do elementu.
4. Slot otrzymuje status `occupied`.
5. Mapping zostaje zapisany w project/library metadata.
6. Po stworzeniu właściwej ikony system może oznaczyć fallback jako `resolved`, ale zachowuje historię mappingu.

Przykład:

| Temporary icon | Assigned domain | Example target | Status |
|---|---|---|---|
| `00` | generic/missing | first missing icon case | occupied |
| `01` | logic/state graph | missing state graph tool icon | occupied |
| `02` | layout | missing layout command icon | occupied |
| `03` | region | missing region/drop icon | occupied |
| `04` | export | missing export action icon | occupied |

### 2.4. Kolor temporary icons

Każda tymczasowa ikona może mieć dowolny kolor. Domyślny kolor powinien wynikać z domeny albo typu elementu.

Rekomendowane domeny:

| Domain | Color token |
|---|---|
| core/model | `theme.accent_core` |
| selection/edit | `theme.accent_selection` |
| region | `theme.accent_region` |
| layout | `theme.accent_layout` |
| component/library | `theme.accent_component` |
| logic/state graph | `theme.accent_graph` |
| debug | `theme.accent_debug` |
| validation/error | `theme.accent_error` |
| disabled/future | `theme.accent_disabled` |

Kolor nie jest częścią samego numeru. Ten sam glyph `03` może być renderowany kolorem domeny, jeżeli mapping przypisuje go do danej kategorii.

### 2.5. Minimalny model danych

```ts
export type IconSource = "specific" | "temporary" | "none";

export type TemporaryIconStatus = "free" | "occupied" | "resolved" | "deprecated";

export interface TemporaryIconSlot {
  id: string; // "00".."99"
  glyph: string; // rendered two-digit label
  status: TemporaryIconStatus;
  assigned_to_id?: string;
  assigned_to_type?: string;
  assigned_to_name?: string;
  domain?: string;
  color_token?: string;
  created_at?: string;
  resolved_icon_id?: string;
}

export interface IconRegistryEntry {
  target_id: string;
  target_type: string;
  icon_source: IconSource;
  icon_id?: string;
  temporary_icon_id?: string;
  domain?: string;
  color_token?: string;
  assignment_status: "unassigned" | "temporary" | "specific" | "resolved";
}

export interface TemporaryIconAtlas {
  single_digit_glyphs: string[]; // ["0", ... "9"]
  fallback_slots: Record<string, TemporaryIconSlot>; // "00".."99"
}
```

### 2.6. Commands

Rekomendowane komendy:

- `icon.assignTemporary`
- `icon.releaseTemporary`
- `icon.resolveTemporary`
- `icon.assignSpecific`
- `icon.patchDomainColor`
- `icon.validateRegistry`

Każda z nich powinna aktualizować model, a nie tylko stan UI.

---

## 3. Master parameter panel

### 3.1. Problem

Standardowy inspector pokazuje parametry aktywnego obiektu albo selection. Przy dużym systemie to nie wystarcza, ponieważ parametry istnieją na wielu poziomach:

- project,
- theme,
- defaults,
- objects,
- components,
- regions,
- layout,
- debug,
- state graph,
- icon registry,
- saved library views.

### 3.2. Decyzja

System powinien posiadać `master_parameter_panel` — nadrzędny panel, który może pokazać wszystkie parametry systemu, pogrupowane według domen.

To nie zastępuje inspectora. Inspector jest szybkim panelem aktywnego celu. Master panel jest narzędziem globalnej kontroli, audytu i masowego filtrowania parametrów.

### 3.3. Grupy master panelu

| Group | Purpose |
|---|---|
| Project | metadata, version, viewport, save state, validation state |
| Theme tokens | global colors, density, radius, typography, spacing |
| Object defaults | default rectangle, text, line, region, panel, component |
| Objects | all interface objects, filtered by type/tag/state |
| Components | assets, exposed parameters, preview, categories |
| Regions | hit, hover, drag, drop, resize, snap, scroll, content, layout |
| Layout | grid, snap, align, distribute, padding, gap, layout bbox |
| Debug overlays | bbox modes, labels, validation overlays, union overlay |
| State graph | graphs, nodes, events, actions, target resolver, presets |
| Icon registry | specific icons, temporary icons, occupied slots, resolved slots |
| Library views | saved filters, sorting presets, workspace-scoped views |

### 3.4. Minimalny model

```ts
export interface MasterParameterView {
  id: string;
  name: string;
  scope: "project" | "objects" | "components" | "regions" | "layout" | "debug" | "logic" | "icons" | "library";
  filter_query?: string;
  filter_tags?: string[];
  filter_types?: string[];
  sort_by?: string;
  sort_direction?: "asc" | "desc";
  visible_columns?: string[];
  grouped_by?: string;
  workspace_context?: string;
}
```

---

## 4. Library filter / sort / search / saved views

### 4.1. Decyzja

Biblioteki w `qubok_interface_core` nie mogą być tylko listą assetów. Muszą posiadać:

- `search`,
- `filter by`,
- `sort by`,
- `tags`,
- `categories`,
- `domain`,
- `maturity`,
- `saved filter views`,
- context-aware default view.

### 4.2. Context-aware behavior

Workspace może automatycznie zawęzić bibliotekę.

Przykład: w `Logic / Events Workspace` system może aktywować widok:

```text
Library View: State Graph Defaults
Filters:
- type: state_graph OR graph_node_template OR event_assignment OR action_preset
- domain: logic
- maturity: L2/L3/L4
Sort:
- usage_count desc
- name asc
```

Przykład: w `Panel Builder Workspace` system może aktywować widok:

```text
Library View: Panel Composition
Filters:
- type: panel_template OR component OR region_preset OR layout_preset
- domain: component/layout/region
Sort:
- category asc
- name asc
```

### 4.3. Minimalny model

```ts
export interface LibraryFilterView {
  id: string;
  name: string;
  workspace_context?: string;
  query?: string;
  types?: string[];
  domains?: string[];
  tags?: string[];
  maturity_levels?: string[];
  include_temporary?: boolean;
  include_deprecated?: boolean;
  sort_by: string;
  sort_direction: "asc" | "desc";
  created_at?: string;
  updated_at?: string;
}
```

---

## 5. Shape booleans and debug bbox union

### 5.1. Problem

Shapes mają docelowo obsługiwać boolean operations, ale w debug mode pojawia się podobny wizualnie przypadek: wiele bboxów albo paneli tego samego typu styka się i powinno wyglądać jak jeden wspólny obrys.

To są dwa różne systemy i nie wolno ich mieszać.

### 5.2. Decyzja

Rozdzielamy:

| System | Purpose | Mutates source geometry? |
|---|---|---|
| Shape boolean | real geometry/path boolean: union, subtract, intersect, exclude | optional, only through explicit command |
| Debug bbox union | merged overlay outline for related/touching bboxes | no |
| Panel cluster union | visual grouping of anchored/touching panels | no |
| Layout union | shared layout/debug area visualization | no |

Dla opisanego przypadku MVP/post-MVP potrzebuje głównie `debug_bbox_union_overlay`, nie pełnego destrukcyjnego shape boolean.

### 5.3. Debug bbox union behavior

Jeżeli debug mode albo filtr debugowy jest aktywny, system może:

1. zebrać obiekty widoczne w danym overlayu,
2. pogrupować je według typu, tagu, domeny, parenta, anchora albo saved filter,
3. sprawdzić, czy ich bboxy się stykają, nachodzą albo są w progu dystansu,
4. policzyć jeden `computed_union_bbox` albo outline union,
5. wyrenderować jeden zbiorczy obrys,
6. zachować listę `source_object_ids`,
7. nie modyfikować transformów ani geometrii obiektów źródłowych.

### 5.4. Merge rules

| Rule | Meaning |
|---|---|
| `merge_by_type` | merge objects/panels of same object type |
| `merge_by_subtype` | merge by subtype, for example inspector section, graph panel, library panel |
| `merge_by_tag` | merge by debug/layout/component tag |
| `merge_by_domain` | merge by semantic domain |
| `merge_by_parent` | merge children under same parent/group |
| `merge_by_anchor` | merge anchored/pinned objects |
| `merge_touching_only` | merge only touching/overlapping bboxes |
| `merge_distance_px` | optional tolerance for small gaps, for example 0-4 px |

### 5.5. Minimalny model

```ts
export type DebugBboxMergeMode = "none" | "touching" | "overlapping" | "distance_threshold" | "same_anchor" | "same_parent" | "same_filter_group";

export interface DebugBboxUnionSettings {
  enabled: boolean;
  merge_mode: DebugBboxMergeMode;
  merge_by_type: boolean;
  merge_by_subtype: boolean;
  merge_by_tag: boolean;
  merge_by_domain: boolean;
  merge_by_parent: boolean;
  merge_by_anchor: boolean;
  merge_touching_only: boolean;
  merge_distance_px: number;
}

export interface DebugBboxOverlayGroup {
  id: string;
  source_object_ids: string[];
  merge_key: string;
  computed_union_bbox: {
    x: number;
    y: number;
    width: number;
    height: number;
  };
  boolean_mode: "union";
  debug_color_token?: string;
}
```

### 5.6. ASCII behavior diagram

```text
Before debug union:

+---------+ +---------+ +---------+
| Panel A | | Panel B | | Panel C |
+---------+ +---------+ +---------+

same type/tag + touching or within threshold
                |
                v
After debug union overlay:

+-------------------------------+
| Panel A   Panel B   Panel C   |
+-------------------------------+

source objects remain separate
```

---

## 6. Roadmap placement

| Phase | Update |
|---|---|
| Phase A — Foundation | add temporary icon policy and registry rules |
| Phase C — Core model | add `icon_registry`, `temporary_icon_atlas`, `library_filter_views`, `master_parameter_views` |
| Phase G — BBox / Region / Debug | add `debug_bbox_union_overlay` as non-destructive debug layer |
| Phase I — Default MVP cleanup | show compact icon registry status only, not full registry UI |
| Phase J — Component Library MVP | add filter/sort/search/saved views |
| Phase K — Panel Builder | add anchored panel cluster overlays and layout/debug union views |
| Phase O — Advanced Shape / Procedural | add full real shape boolean operations for geometry/path editing |

---

## 7. Implementation guardrails

1. Temporary icons must be data-driven and stored in registry, not hardcoded as random labels in React.
2. Two-digit icons `00-99` are fallback assignment icons; single digits `0-9` are not used as fallback assignments.
3. Temporary icon assignment must be deterministic: first free slot unless user overrides.
4. Master parameter panel must read project/model state, not duplicate committed data.
5. Library saved views must be serializable.
6. Context-aware library filtering must be workspace-scoped and reversible.
7. Debug bbox union must never modify source object transforms, geometry, hierarchy or export.
8. Real shape booleans are a separate advanced feature and require explicit command/action.
9. Debug overlay groups must preserve `source_object_ids` for hover inspection and selection trace.
10. Default UI must not be dominated by icon registry or master parameter table; expose compact status and open full view on demand.

---

## 8. Acceptance tests

### Temporary icon registry

1. Create or register a tool/component without specific icon.
2. Run temporary icon assignment.
3. Verify first free `00-99` slot is assigned.
4. Verify slot becomes `occupied`.
5. Verify icon color comes from domain token.
6. Assign specific icon later.
7. Verify temporary slot mapping is marked `resolved`, not silently lost.

### Master parameter panel

1. Open master panel.
2. Filter objects by type `region_rectangle`.
3. Sort by name.
4. Switch to icon registry group.
5. Filter occupied temporary icons.
6. Save current filter view.
7. Reopen workspace and verify saved view persists.

### Context library

1. Open Logic workspace.
2. Verify library view is auto-scoped to state graphs/events/actions.
3. Open Panel Builder workspace.
4. Verify library view changes to panel/component/layout assets.
5. Clear context filter and verify full library can still be searched.

### Debug bbox union

1. Create three panels of same subtype.
2. Move them until their bbox edges touch.
3. Enable debug mode with `merge_touching_only`.
4. Verify one merged outline appears.
5. Move one panel away beyond threshold.
6. Verify outline splits again.
7. Export SVG/project and verify source geometry was not merged by debug overlay.
