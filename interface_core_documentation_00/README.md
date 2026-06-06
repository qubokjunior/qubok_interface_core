# interface_core_documentation_00

Folder roboczy dla skondensowanej dokumentacji `qubok_interface_core`: notatki, decyzje, opisy etapów roadmapy, aktualizacje po grafikach i prompty do generowania kolejnych plansz.

## Aktualne pliki

| Plik | Cel |
|---|---|
| `2026-06-06_visual_delta_after_infographics.md` | Skondensowana aktualizacja wniosków po serii grafik: nowe elementy UI, funkcje, panele, ratio fullscreen, filtrowanie, pinning, docking, state graph. |
| `2026-06-06_eight_fullscreen_roadmap_states.md` | Osiem etapów widoku programu w fullscreen, od fundamentów po finalniejszy MVP z dockingiem, pinningiem i exportem. |
| `2026-06-06_prompt_generate_8_fullscreen_infographics_png.md` | Gotowy prompt do kolejnego czatu generującego 8 osobnych PNG. |
| `2026-06-06_temporary_icon_atlas_master_parameter_debug_union.md` | Addendum architektoniczne: temporary icon atlas `00-99`, master parameter panel, context-aware library filters/saved views oraz debug bbox union overlay dla łączenia obrysów podobnych/stykających się paneli. |

## Decyzje nadrzędne

- `Project JSON / data model` jest źródłem prawdy.
- Canvas, inspector, hierarchy, spreadsheet, export, debug, component library, event registry i state graph są widokami albo narzędziami nad modelem.
- Zmiany przechodzą przez command layer.
- Rectangle renderuje shape; region obsługuje hit/hover/drag/drop/resize/snap/scroll/content/layout.
- Panel to struktura: frame, header, content, footer, sections, rows, regions, exposed parameters.
- State graph emituje commands i działa jako osobny workspace, nie jako część defaultowego MVP.
- App shell docking, canvas object layout i graph viewport layout to trzy oddzielne systemy.
- Default UI ma być compact, dark, docked, techniczny i realnie używalny w fullscreen.
- Temporary icon atlas używa dwucyfrowych fallback icons `00-99`; pojedyncze cyfry `0-9` są glyphami pomocniczymi, nie fallback assignment icons.
- Każde przypisanie temporary icon rezerwuje slot jako `occupied`; późniejsze zastąpienie właściwą ikoną oznacza mapping jako `resolved`, bez utraty historii.
- Master parameter panel jest globalnym audytem parametrów projektu; zwykły inspector pozostaje panelem aktywnego selection.
- Biblioteki muszą wspierać `search`, `filter by`, `sort by`, tags, categories, saved filter views i context-aware workspace views.
- Debug bbox union overlay może łączyć obrysy podobnych/stykających się paneli w jeden computed union outline, ale nie modyfikuje transformów, geometrii, hierarchy ani exportu.
