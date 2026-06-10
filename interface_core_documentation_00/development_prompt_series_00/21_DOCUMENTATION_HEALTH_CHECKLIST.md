# 21 — Documentation health checklist

status: active
version: v2.2
doc_type: documentation_policy
last_updated: 2026-06-10

## Cel

Checklist do użycia przy każdej większej zmianie dokumentacji `qubok_interface_core`. Ma zapobiegać narastaniu sprzecznych opisów, nieaktualnych promptów i zbyt szerokich sprintów.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## Checklist przed dodaniem nowego dokumentu

| Pytanie | Wymagane |
|---|---|
| Czy dokument ma status? | active / active_with_v2_2_amendment / policy / reference / archive |
| Czy dokument ma typ? | canonical / prompt / policy / glossary / matrix / archive |
| Czy dokument wskazuje wersję? | v2.2 albo inna jawna wersja |
| Czy dokument ma jeden główny cel? | tak |
| Czy dokument powiela istniejący plik? | jeśli tak, wskazać supersedes/replaces |
| Czy dokument jest do implementacji czy do referencji? | jasno określone |
| Czy jest AI-friendly? | krótkie sekcje, tabele, jednoznaczne reguły |
| Czy używa aktualnego nazewnictwa? | node_graph zamiast state_graph, qubok_interface_core zamiast nazw _00 |

## Checklist aktualizacji promptu sprintowego

Prompt sprintowy powinien zawierać:

- sprint number and one goal,
- assume previous sprint successful,
- global rules,
- naming v2.2 if relevant,
- current phase,
- maturity target,
- data owner,
- implement only,
- do not implement,
- allowed files/modules,
- forbidden files/modules,
- acceptance tests,
- response format.

## Checklist zgodności z v2.2

Każdy aktywny dokument implementacyjny powinien respektować:

- Project model as source of truth,
- core without React,
- persistent mutation through command layer,
- command history before heavy editing,
- selectors as shared read boundary,
- validation and tests before heavy UI growth,
- visual/layout/interaction separation,
- render adapter before renderer lock-in,
- events outside visual node graph,
- external libraries as adapters,
- node_graph naming,
- default UI only L3/L4.

## Red flags w dokumentacji

| Red flag | Co zrobić |
|---|---|
| Dokument mówi “dodaj wszystko” | podzielić na sprinty |
| Dokument miesza docking/canvas/node graph layout | dodać granice modeli |
| Dokument mówi o node graph przed event/action registry | oznaczyć jako superseded albo dopisać v2.2 amendment |
| Dokument używa `state_graph` w nowej treści | zmienić na `node_graph` / `nodeGraph` / `NodeGraph` |
| Dokument używa tymczasowych nazw projektu z `_00` | zmienić na `qubok_interface_core` / `interface_core` |
| Dokument pozwala UI mutować Project lokalnie | poprawić na command layer |
| Dokument nie ma acceptance tests | dopisać |
| Dokument zakłada external library jako model | poprawić na adapter policy |
| Dokument ma dużo opisów wizualnych bez data owner | dodać owner/command/view/validation |
| Dokument dotyczy debug i default UI naraz | rozdzielić normal mode i debug mode |

## Checklist po zmianie dokumentacji

1. Czy `00_CURRENT_SOURCE_OF_TRUTH.md` dalej jest aktualny?
2. Czy `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md` zna zmianę?
3. Czy `19_DOC_STATUS_AND_ARCHIVE_INDEX.md` zna nowy lub zmieniony plik?
4. Czy `20_AI_CONTEXT_MINIMAL.md` wymaga aktualizacji?
5. Czy prompt starter `14_NEXT_CHAT_STARTER_TEMPLATE.md` nadal wskazuje właściwe reguły?
6. Czy nowy dokument nie konfliktuje z roadmapą v2.2?
7. Czy stary dokument powinien dostać status `ACTIVE_WITH_V2_2_AMENDMENT`, `SUPERSEDED` albo `ARCHIVE`?
8. Czy dokument nie zwiększa scope kolejnego sprintu ponad jeden właściciel danych?

## Checklist dla AI-readability

Dobry dokument dla AI powinien mieć:

- krótki cel na początku,
- tabele dla hierarchii i zależności,
- listę zakazów,
- jednoznaczny status,
- jednoznaczny owner danych,
- brak nieopisanych skrótów,
- brak sprzecznych instrukcji,
- aktualne nazewnictwo v2.2,
- sekcję “do not implement” przy promptach,
- acceptance criteria możliwe do sprawdzenia.

## Maintenance cadence

Po każdym większym etapie developmentu sprawdzić:

- czy roadmapa nadal pasuje,
- czy maturity levels funkcji się zmieniły,
- czy jakiś plik jest superseded,
- czy nowy sprint powinien być canonical,
- czy AI minimal context nadal mieści się w krótkim prompt context.