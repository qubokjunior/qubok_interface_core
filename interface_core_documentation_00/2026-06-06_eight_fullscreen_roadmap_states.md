# 2026-06-06 — eight fullscreen roadmap states

Osiem etapów reprezentujących prawdopodobny widok programu `qubok_interface_core` w fullscreen. To podział wizualny do dalszych grafik i dokumentacji, nie ścisły sprint plan kodu.

## Etap 01 — Foundation / model / primitive

Widoczny stan:

- pełny shell: top app bar, left tools, center canvas, right inspector/hierarchy, bottom shelf, status bar;
- canvas z prostymi primitive: rectangle, text rectangle, line, empty rectangle;
- selected rectangle z transform handles;
- hierarchy: Root > Rectangle_1, Rectangle_2, Text_1;
- inspector: ID, Type, X, Y, W, H, Rotation, Fill, Stroke, Radius;
- bottom shelf: Preview, Export, Validation, Command Log jako tabs.

Funkcje reprezentowane:

- Project JSON jako source of truth;
- primitive creation;
- basic transform/style;
- initial hierarchy;
- initial validation/status.

## Etap 02 — Canvas / primitives / inspector sync

Widoczny stan:

- Rectangle Tool aktywny;
- canvas z button draft, label, line separator, placeholder;
- aktywny object podświetlony cyan;
- box select overlay;
- transform handles;
- hierarchy row podświetlony na tym samym obiekcie;
- inspector pokazuje te same dane;
- status bar pokazuje active object, selected count, x/y/w/h, grid/snap.

Funkcje reprezentowane:

- canvas -> selection.set -> inspector -> hierarchy -> status sync;
- click select, shift multi-select, box select;
- transform przez command layer;
- grid i podstawowy snap.

## Etap 03 — Regions / bbox / interaction debug

Widoczny stan:

- centralny button z overlayami:
  - visual_bbox: cyan,
  - layout_bbox: yellow,
  - interaction_region: green;
- Debug Overlay: Interaction mode;
- inspector na Region tab;
- floating hover HUD;
- hierarchy z grupą Interaction_Regions;
- bottom shelf: Validation + Command Log.

Funkcje reprezentowane:

- rectangle renderuje shape;
- region obsługuje interakcję;
- linked_visual_id;
- region priority;
- cursor type;
- event bindings;
- pointer -> hitTest -> region target -> event -> command -> model update.

## Etap 04 — Spreadsheet / filters / parameter visibility

Widoczny stan:

- centralny spreadsheet panel dla selected/multi-selection;
- tabela: Object, Type, Parameter, Value, Source, Override, Visible, Locked, Warning;
- filter bar: search, type, tag, group, layer, custom, favorite, show hidden, show overridden, selected only;
- chips: interactive, button, Panel_Main, local override;
- panel Parameter Visibility z checkboxami sekcji;
- Mixed Inspector po prawej;
- bottom shelf: Actions, History, Validation.

Funkcje reprezentowane:

- filtrowanie złożonych obiektów i parametrów;
- ukrywanie noise;
- override visibility;
- sort/filter by type/tag/source;
- praca na wielu obiektach naraz.

## Etap 05 — Layers / hierarchy / tags / style sorting

Widoczny stan:

- duży tree panel:
  Root > Panel_Main > Header / Content / Footer / Interaction_Regions / Style_Tokens;
- rows z icons, visibility, lock, warning, region marker, selected state;
- Layer Filters: layer, tag, type, color, source, warning, favorite;
- Tags panel: primary, interactive, button, layout, debug, custom;
- sort by color/style source ze swatchami;
- canvas w tle z podświetlonymi elementami tego samego stylu.

Funkcje reprezentowane:

- hierarchy jako struktura danych;
- folders/subfolders;
- tag assignment;
- layer filters;
- style/color sorting;
- isolate/locate/visibility/lock.

## Etap 06 — Panel builder / component structure / states

Widoczny stan:

- canvas z budowanym panelem:
  Frame, Header, Content, Footer, Section_01, Row_01, Row_02, Resize handles, Drag region, Scroll region, Drop/content regions;
- ghost insert overlay: drop section here;
- inspector na Component / Panel tab;
- exposed parameters: width, height, title, header_height, padding, gap, radius, background, border, content_layout;
- states preview: Default, Hover, Pressed, Disabled, Focused;
- component card w bottom shelf: preview, name, category, tags, version.

Funkcje reprezentowane:

- panel jako group/component, nie pojedynczy rectangle;
- internal hierarchy;
- exposed parameters;
- component preview;
- save as component;
- validate component.

## Etap 07 — Actions / events / command layer / state graph

Widoczny stan:

- aktywny workspace: Logic / Events;
- left panel: Event Registry;
- actions panel: SetProperty, SetVisible, SetLocked, TransformObject, GroupSelected, ValidateProject, EmitCommand;
- central graph viewport z nodami:
  Event Input -> Target Resolver -> Condition -> Set Property -> Emit Command -> Debug Log;
- input/output ports na nodach;
- right inspector dla wybranego node;
- bottom shelf: Execution History, Command Preview, Watch.

Funkcje reprezentowane:

- event registry;
- action registry;
- target resolver;
- node graph z pan/zoom/multiselect;
- graph emits commands;
- no direct DOM mutation;
- validation graph output.

## Etap 08 — Docking / pinning / export / final MVP view

Widoczny stan:

- pełny fullscreen z docked/split panels;
- left hierarchy/layers;
- center canvas;
- right inspector + styles;
- bottom actions/history/export;
- floating pinned debug inspector;
- floating pinned component preview;
- splitters, dock headers, drag handles, pinned icons;
- dwa konteksty: pinned Panel_Main i active Button_Primary;
- bottom export: Project JSON, Component JSON, SVG, Validate, Debug Report;
- status: saved, selected count, grid, snap, validation clean, last command.

Funkcje reprezentowane:

- app shell docking;
- pinned panels;
- multi-view workflow;
- export/validation production view;
- rozdzielenie: app shell docking != canvas object layout != graph viewport layout.
