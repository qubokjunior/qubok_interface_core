# 21 — Documentation health checklist

status: active
version: v2.1
doc_type: documentation_policy
last_updated: 2026-06-10

## Cel

Checklist do użycia przy każdej większej zmianie dokumentacji `qubok_interface_core`. Ma zapobiegać narastaniu sprzecznych opisów, nieaktualnych promptów i zbyt szerokich sprintów.

## Checklist przed dodaniem nowego dokumentu

| Pytanie | Wymagane |
|---|---|
| Czy dokument ma status? | active / active_with_v2_1_amendment / policy / reference / archive |
| Czy dokument ma typ? | canonical / prompt / policy / glossary / matrix / archive |
| Czy dokument wskazuje wersję? | v2.1 albo inna jawna wersja |
| Czy dokument ma jeden główny cel? | tak |
| Czy dokument powiela istniejący plik? | jeśli tak, wskazać supersedes/replaces |
| Czy dokument jest do implementacji czy do referencji? | jasno określone |
| Czy jest AI-friendly? | krótkie sekcje, tabele, jednoznaczne reguły |

## Checklist aktualizacji promptu sprintowego

Prompt sprintowy powinien zawierać:

- sprint number and one goal,
- assume previous sprint successful,
- global rules,
- current phase,
- maturity target,
- data owner,
- implement only,
- do not implement,
- allowed files/modules,
- forbidden files/modules,
- acceptance tests,
- response format.

## Checklist zgodności z v2.1

Każdy aktywny dokument implementacyjny powinien respektować:

- Project model as source of truth,
- core without React,
- persistent mutation through command layer,
- command history before heavy editing,
- selectors as shared read boundary,
- validation and tests before heavy UI growth,
- visual/layout/interaction separation,
- render adapter before renderer lock-in,
- events outside visual state graph,
- external libraries as adapters,
- default UI only L3/L4.

## Red flags w dokumentacji

| Red flag | Co zrobić |
|---|---|
| Dokument mówi “dodaj wszystko” | podzielić na sprinty |
| Dokument miesza docking/canvas/graph layout | dodać granice modeli |
| Dokument mówi o state graph przed event/action registry | oznaczyć jako superseded albo dopisać v2.1 amendment |
| Dokument pozwala UI mutować Project lokalnie | poprawić na command layer |
| Dokument nie ma acceptance tests | dopisać |
| Dokument zakłada external library jako model | poprawić na adapter policy |
| Dokument ma dużo opisów wizualnych bez data owner | dodać owner/command/view/validation |
| Dokument dotyczy debug i default UI naraz | rozdzielić normal mode i debug mode |

## Checklist po zmianie dokumentacji

1. Czy `00_CURRENT_SOURCE_OF_TRUTH.md` dalej jest aktualny?
2. Czy `19_DOC_STATUS_AND_ARCHIVE_INDEX.md` zna nowy plik?
3. Czy `20_AI_CONTEXT_MINIMAL.md` wymaga aktualizacji?
4. Czy prompt starter `14_NEXT_CHAT_STARTER_TEMPLATE.md` nadal wskazuje właściwe reguły?
5. Czy nowy dokument nie konfliktuje z roadmapą v2.1?
6. Czy stary dokument powinien dostać status `ACTIVE_WITH_V2_1_AMENDMENT`, `SUPERSEDED` albo `ARCHIVE`?
7. Czy dokument nie zwiększa scope kolejnego sprintu ponad jeden właściciel danych?

## Checklist dla AI-readability

Dobry dokument dla AI powinien mieć:

- krótki cel na początku,
- tabele dla hierarchii i zależności,
- listę zakazów,
- jednoznaczny status,
- jednoznaczny owner danych,
- brak nieopisanych skrótów,
- brak sprzecznych instrukcji,
- sekcję “do not implement” przy promptach,
- acceptance criteria możliwe do sprawdzenia.

## Maintenance cadence

Po każdym większym etapie developmentu sprawdzić:

- czy roadmapa nadal pasuje,
- czy maturity levels funkcji się zmieniły,
- czy jakiś plik jest superseded,
- czy nowy sprint powinien być canonical,
- czy AI minimal context nadal mieści się w krótkim prompt context.