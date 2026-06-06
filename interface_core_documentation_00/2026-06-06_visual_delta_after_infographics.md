# 2026-06-06 — visual delta after infographics

Skondensowana aktualizacja dokumentacji po serii wygenerowanych grafik i omówionych wnioskach. Dokument opisuje elementy nowe, doprecyzowane wizualnie oraz te, które powinny zostać uwzględnione w dalszej roadmapie i promptach graficznych.

## 1. Główna aktualizacja kierunku

`qubok_interface_core` został doprecyzowany jako fullscreen, dockowalny `interface creator`, nie jako luźny zestaw mockupów ani UI kit.

Najważniejsze przesunięcie:

- widok ma reprezentować realny program na monitorze w fullscreen;
- każdy etap roadmapy powinien mieć prawdopodobny stan interface, a nie tylko listę funkcji;
- panele mają być gęste, docked/split/floating/anchored/pinned, a nie bulky;
- state graph musi wyglądać jak pełny graph viewport z portami input/output, nie jak dekoracyjny schemat;
- spreadsheet, filters, hierarchy, layer tree, command history i pinned inspectors są częścią workflow, nie ozdobą.

## 2. Bazowy fullscreen layout

| Strefa | Funkcja | Doprecyzowanie |
|---|---|---|
| TopAppBar | globalny kontekst | project/file, workspace switch, save state, undo/redo, zoom, debug overlay, preferences, window controls |
| LeftToolPanel | narzędzia i primitive | select, rectangle, text, line, region, empty, group/component, layout, primitive library, recent tools |
| CenterCanvas | główna praca | grid, selection outline, transform handles, snap guides, box select, sample component/panel |
| RightSidebar | parametry aktywnego celu | inspector tabs: Transform, Geometry, Style, Text, Region, Layout, Component, Events, Validation |
| Hierarchy/Layers | struktura danych | tree, folders, subfolders, visibility, lock, warning, region marker, selected row, tags, filters |
| BottomShelf | operacje pomocnicze | Preview, Export, Components, Validation, Command Log, Logic Debug collapsed |
| StatusBar | stan sesji | saved/dirty, selected count, active object, x/y/w/h/r, zoom, grid, snap, validation, last command |

## 3. Elementy nowe lub mocniej doprecyzowane

| Obszar | Aktualizacja |
|---|---|
| Spreadsheet | Panel danych dla zaznaczonych elementów: object, type, parameter, value, source, override, visible, locked, warning. |
| Filtering | Wspólny wzorzec filtrowania: type, tag, group, layer, custom, favorite, search, show hidden, show overridden, selected only. |
| Parameter noise control | Możliwość ukrywania sekcji parametrów, kolumn spreadsheetu, future/advanced fields i nieistotnych grup w danym trybie. |
| Hierarchy / layers | Tree danych, nie render-order list: folders, subfolders, object icons, lock, visibility, warning, region marker, selected row. |
| Tags / types | Tagi jako element modelu i filtrowania: interactive, primary, button, panel, layout, style, debug, custom. |
| Sort by style/color | Sortowanie i filtrowanie według fill/stroke/source/override, pomocne podczas definiowania stylu renderu. |
| Pinned panels | Przypinanie inspectora, preview lub debug panelu dla jednego elementu; zmiana focusu otwiera nowy kontekst bez zamykania pinned view. |
| Actions/history | Actions list, events list, command history, execution history, actions_global spreadsheet. |
| State graph | Osobny graph workspace; nody mają input/output ports, edges, event names, conditions, command outputs, watch/debug. |
| Docking | Docked, split, floating, anchored, pinned panels; dock shell nie zmienia object layoutu na canvasie. |
| Preview/export | Component preview, export Project JSON, Component JSON, SVG, debug report, validate before export. |
| Validation | Validation chips, row warnings, export warnings, broken region links, invalid hierarchy, clean/warning/error status. |
| Debug overlays | Normal, Selected Debug, Layout, Interaction, Validation, All Debug; debug dostępny, ale nie defaultowy chaos. |

## 4. Zasady wizualne potwierdzone przez grafiki

- Interface ma być compact/dense, dark, techniczny.
- Panele nie powinny wypełniać przestrzeni na siłę.
- Duże puste obszary są antywzorcem.
- Elementy nie mogą na siebie nachodzić.
- Każda plansza ma pokazywać realny stan programu w danym etapie.
- Graphy i nody muszą mieć prawdziwe porty i czytelną topologię.
- Panel builder ma pokazywać strukturę panelu: header/content/footer/sections/rows/regions.
- Default UI nie może wyglądać jak raw debug/API registry.
- Debug i logic są dostępne, ale zwinięte albo w osobnym workspace.

## 5. Architektoniczne doprecyzowania z wizualizacji

| Warstwa | Własność | Zakaz |
|---|---|---|
| Core model | Project, objects_by_id, root_children, selection, viewport, validation, library assets | bez Reacta, bez bezpośredniego UI state |
| Command layer | object.create, object.patch, transform.update, group.create, layout.align, export, validate | inspector/canvas nie obchodzą command layer |
| Canvas | render modelu, grid, handles, overlays, selection | nie jest source of truth |
| Inspector | schema-driven edit aktywnego celu | nie trzyma trwałej kopii danych |
| Hierarchy | parent/children/layers/tags/regions | nie jest tylko listą render order |
| Region system | hit/hover/drag/drop/resize/snap/scroll/content/layout | nie jest ukryty w rectangle |
| Docking shell | split/merge/resize paneli aplikacji | nie transformuje obiektów na canvasie |
| State graph | event -> target resolver -> command | nie mutuje DOM/canvas bezpośrednio |

## 6. Wnioski do dalszej dokumentacji

1. Każdy etap roadmapy powinien mieć przypisany `default visible UI state`.
2. Każda większa funkcja musi mieć miejsce: panel, tab, shelf, workspace, overlay albo command palette.
3. Spreadsheet i filtering powinny być traktowane jako podstawowe narzędzia nawigacji po złożoności.
4. Pinning paneli jest krytyczny dla porównywania i pracy na wielu elementach.
5. Logic/state graph musi wejść dopiero jako pełny workspace z viewportem, nie jako ciasny debug panel.
6. Panel builder powinien operować na strukturze danych panelu, nie na pojedynczym prostokącie.
7. Export i validation powinny być widoczne w bottom shelf od MVP.
8. Docking należy dokumentować jako app shell layout, oddzielony od canvas object layout i graph viewport layout.
