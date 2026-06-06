# 2026-06-06 — prompt generate 8 fullscreen infographics PNG

Gotowy prompt do wklejenia w kolejnym czacie generującym 8 osobnych grafik PNG.

```text
PROMPT DO WYGENEROWANIA 8 INFOGRAFIK PNG — QUBOK_INTERFACE_CORE / FULLSCREEN ROADMAP UI STATES

ZADANIE
Wygeneruj 8 osobnych plików graficznych PNG. Nie twórz jednej zbiorczej grafiki, kolażu ani planszy 2x4. Każdy plik ma być osobną infografiką w proporcji 16:9, przedstawiającą pełny widok programu na monitorze w trybie fullscreen. Nazwij pliki dokładnie:

01_foundation_fullscreen_interface_shell.png
02_canvas_primitives_inspector_sync.png
03_regions_bbox_interaction_debug.png
04_spreadsheet_filters_parameter_visibility.png
05_layers_hierarchy_tags_sorting.png
06_panel_builder_component_states.png
07_actions_events_command_state_graph.png
08_docking_pinning_export_final_mvp.png

CEL SERII
Osiem plików ma razem pokazać rozwój i funkcjonalność qubok_interface_core jako parametrycznego silnika interfejsu. Nie pokazuj przyszłych aplikacji, map, danych finansowych, projektów pobocznych, audio, słowników, particle tools, texture tools ani żadnych niepowiązanych treści. Wszystko ma dotyczyć samego tworzenia, edycji, organizacji, debugowania, filtrowania, składania i eksportowania interfejsu.

NAJWAŻNIEJSZY KONTEKST
qubok_interface_core nie jest UI kitem i nie jest pojedynczą aplikacją użytkową. To parametryczny engine UI, gdzie każdy element interfejsu jest obiektem danych: id, type, transform, visual_bbox, layout_bbox, interaction_region, style, hierarchy, state, exposed parameters, validation status, event bindings, component metadata.

Główna ścieżka:
primitive -> bbox -> transform/style -> region/layout -> group/panel -> exposed parameters -> library asset -> reusable UI component -> event/action/state behavior.

Project JSON / data model = source of truth.
Canvas, inspector, hierarchy, spreadsheet, export, debug, component library, event registry i state graph są widokami lub narzędziami nad modelem.
Zmiany idą przez command layer.
Rectangle renderuje shape.
Region obsługuje hit/hover/drag/drop/resize/snap/scroll/content/layout.
State graph emituje commands, nie mutuje bezpośrednio DOM/canvas.
Docking shell, canvas object layout i graph viewport layout to trzy oddzielne systemy.

WSPÓLNY STYL WSZYSTKICH 8 PNG
- dark compact technical UI,
- styl między Blender / Photoshop / technical dashboard,
- widok programu fullscreen na monitorze, 16:9,
- cienkie linie, małe panele, gęsta informacja, techniczny układ,
- radius 2–4 px,
- border 1 px,
- bez ciężkich glow, bevel, soft shadows, wielkich pustych przestrzeni,
- nie rób chunky/bulky interface,
- panele mogą być docked, split, anchored, floating i pinned,
- dużo drobnych realnych elementów UI: tabs, fields, sliders, toggles, row lists, filters, dropdowns, checkboxes, tree rows, command rows, warning chips,
- tekst po polsku, ale techniczne krótkie etykiety mogą być po angielsku,
- semantyczne kolory:
  cyan/blue = selection / active / accent,
  green = valid / region / snap OK / enabled,
  yellow/orange = layout / guides / warning / measurement,
  red = error / invalid / destructive,
  purple = graph / logic / state machine,
  grey = disabled / hidden / future / inactive.

GLOBALNY UKŁAD PROGRAMU, KTÓRY MA BYĆ WIDOCZNY W WIĘKSZOŚCI PLANSZ
TopAppBar: project name, workspace switch, save state, undo/redo, zoom, preview toggle, debug overlay, preferences, window controls.
LeftToolPanel: select, rectangle, text, line, region, empty, group/component, layout; primitive library; recent tools.
CenterCanvas: subtle grid, selected outline, transform handles, optional rulers, guides, region overlays only when debug/context requires, sample interface component/panel on canvas.
RightSidebar: Inspector with tabs: Transform, Geometry, Style, Text, Region, Layout, Component, Events, Validation. Hierarchy/Layers tree. Component cards or parameter groups when relevant.
BottomShelf: Preview, Export, Components, Validation, Command Log, Logic Debug. Logic Debug collapsed except in plansza 07.
StatusBar: saved/dirty, selected count, active object id/name/type, x/y/w/h/r, zoom, grid size, snap state, validation clean/warning/error, last command.

WAŻNE OGRANICZENIA WIZUALNE
- Nie pokazuj tylko dekoracyjnych prostokątów. Każdy element ma wyglądać jak realny panel programu.
- Jeśli pokazujesz state graph, nody muszą mieć input/output ports, labels, edges, event names, condition labels i command outputs.
- Jeśli pokazujesz panel, nie pokazuj go jako jednego rectangle. Panel ma mieć frame, header, content, footer, sections, rows, resize/drag/scroll/drop regions.
- Jeśli pokazujesz hierarchy, ma mieć foldery/subfoldery, expand arrows, visibility, lock, warning, region markers, selected row.
- Jeśli pokazujesz spreadsheet, ma mieć kolumny, sort, filter chips, status, selected rows, hidden/visible toggles.
- Debug ma być dostępny, ale nie ma dominować defaultowego widoku.
- Zachowaj czytelność. Drobne teksty mają być uporządkowane, nie losowo rozrzucone.

PLANSZA 01 — foundation_fullscreen_interface_shell
Temat: bazowy fullscreen shell programu i fundamenty modelu.
Pokaż: pełny układ programu; canvas z rectangle, text_rectangle, empty_rectangle, line; zaznaczony rectangle z handles; inspector z ID, Type, X, Y, W, H, Rotation, Fill, Stroke, Radius; hierarchy Root > Rectangle_1, Rectangle_2, Text_1; bottom shelf Preview / Export / Validation / Command Log; callout Project JSON = source of truth; pipeline primitive -> bbox -> style -> region -> group -> component.
Cel: startowy interface jest realnym narzędziem, nie pustym debug screenem.

PLANSZA 02 — canvas_primitives_inspector_sync
Temat: canvas, primitive creation, selection, transform, sync canvas-inspector-hierarchy.
Pokaż: Rectangle Tool aktywny; canvas z button draft, label, line separator, placeholder; aktywny object cyan outline; box select overlay; transform handles; hierarchy selected row; inspector z tym samym obiektem; status bar z active object, selected count, x/y/w/h, grid/snap; flow: click object -> selection.set -> inspector update -> hierarchy row highlight -> status update.
Cel: jedna prawda selection i synchronizacja widoków.

PLANSZA 03 — regions_bbox_interaction_debug
Temat: visual_bbox, layout_bbox, interaction_region, region debug, hover inspect.
Pokaż: centralny button z overlayami visual_bbox cyan, layout_bbox yellow, interaction_region green; debug overlay Interaction mode; inspector Region tab; floating hover HUD; hierarchy z Interaction_Regions; bottom shelf Validation + Command Log; event flow pointer -> hitTest -> region target -> event -> command -> model update.
Cel: rectangle wygląda, region reaguje.

PLANSZA 04 — spreadsheet_filters_parameter_visibility
Temat: spreadsheet parametrów, filtrowanie, ukrywanie noisu, widok danych wybranych elementów.
Pokaż: centralny spreadsheet panel; tabela Object, Type, Parameter, Value, Source, Override, Visible, Locked, Warning; filter bar search/type/tag/group/layer/custom/favorite/show hidden/show overridden/selected only; chips interactive/button/Panel_Main/local override; Parameter Visibility z checkboxami Transform, Style, Text, Region, Layout, Events, Advanced, Hidden defaults; Mixed Inspector; bottom shelf Actions / History / Validation.
Cel: filtrowanie i widoczność parametrów jako core workflow.

PLANSZA 05 — layers_hierarchy_tags_sorting
Temat: layers, folders, subfolders, hierarchy, tags, sort/filter by style/color.
Pokaż: duży tree panel Root > Panel_Main > Header / Content / Footer / Interaction_Regions / Style_Tokens; rows z icons, visibility, lock, warning, region marker, selected state; Layer Filters: layer, tag, type, color, source, warning, favorite; Tags panel primary, interactive, button, layout, debug, custom; sort by color/style source ze swatchami; canvas z podświetlonymi elementami tego samego stylu.
Cel: hierarchy jako struktura danych i narzędzie szybkiej nawigacji.

PLANSZA 06 — panel_builder_component_states
Temat: panel builder, component structure, exposed parameters, style states.
Pokaż: canvas z budowanym panelem Frame, Header, Content, Footer, Section_01, Row_01, Row_02, Resize handles, Drag region, Scroll region, Drop/content regions; ghost insert overlay drop section here; inspector Component / Panel tab; exposed parameters width, height, title, header_height, padding, gap, radius, background, border, content_layout; states preview Default, Hover, Pressed, Disabled, Focused; component card: preview, name, category, tags, version.
Cel: panel jako group/component z internal hierarchy i exposed parameters.

PLANSZA 07 — actions_events_command_state_graph
Temat: actions, events, command layer, state machine jako osobny logic workspace.
Pokaż: workspace Logic / Events; Event Registry; Actions panel: SetProperty, SetVisible, SetLocked, TransformObject, GroupSelected, ValidateProject, EmitCommand; central graph viewport z nodami Event Input -> Target Resolver -> Condition -> Set Property -> Emit Command -> Debug Log; input/output ports; node inspector; bottom shelf Execution History / Command Preview / Watch; warning: State graph outputs commands — no direct DOM mutation.
Cel: logika jako pełny workspace, nie defaultowy clutter.

PLANSZA 08 — docking_pinning_export_final_mvp
Temat: finalniejszy MVP: docking, pinned panels, multi-view, export, validation.
Pokaż: pełny fullscreen z docked/split panels; left hierarchy/layers; center canvas; right inspector + styles; bottom actions/history/export; floating pinned debug inspector; floating pinned component preview; splitters, dock headers, drag handles, pinned icons; pinned Panel_Main oraz active Button_Primary; export Project JSON, Component JSON, SVG, Validate, Debug Report; status saved, selected count, grid, snap, validation clean, last command; diagram app shell docking != canvas object layout != graph viewport layout.
Cel: produkcyjny widok MVP z dockowaniem, przypinaniem, walidacją, exportem i multi-view workflow.

DODATKOWE WYMAGANIA JAKOŚCI
- Każdy plik powinien wyglądać jak screenshot realnego programu plus techniczna infografika, nie abstrakcyjny poster.
- Zachowaj spójny język wizualny między 8 plikami.
- Każdy panel powinien mieć sensowną zawartość odpowiadającą tematowi.
- Unikaj marketingowych haseł; używaj krótkich technicznych labeli.
- Nie zostawiaj dużych pustych obszarów.
- Nie powtarzaj dokładnie tego samego układu w każdym pliku; zachowaj wspólny shell, ale zmieniaj aktywny workspace i widoczne panele zgodnie z etapem.
- Wygeneruj i dostarcz / wyświetl 8 osobnych plików PNG nazwanych 01–08 zgodnie z listą.
KONIEC PROMPTU
```
