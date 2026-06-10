# 27 — Project commands and tests guide

status: active
version: v2.2
doc_type: codex_commands_tests
last_updated: 2026-06-10

## Cel

Ten dokument ma ograniczyć zgadywanie komend przez Codex. Zawiera bezpieczną procedurę wykrywania lokalizacji projektu i uruchamiania build/test/typecheck tylko tam, gdzie istnieje `package.json`.

## Naming v2.2

- Project: `qubok_interface_core` / `interface_core`.
- Local PC path: `I:\Art\_AI\app_development\qubok_interface_core`.
- Graph system: `node_graph`, `nodeGraph`, `NodeGraph`, `node graph`.
- Stare nazewnictwo `state_graph` jest superseded.

## Ważna uwaga

W momencie tworzenia tego dokumentu `package.json` nie został znaleziony w root repo przez GitHub fetch/search. Dlatego Codex nie powinien zakładać, że root repo jest katalogiem aplikacji Node/Vite. Najpierw należy znaleźć właściwy katalog projektu.

Preferowana lokalna ścieżka robocza użytkownika:

```text
I:\Art\_AI\app_development\qubok_interface_core
```

## Project root discovery

Na Windows PowerShell:

```powershell
Get-ChildItem -Path . -Recurse -Filter package.json -ErrorAction SilentlyContinue | Select-Object -ExpandProperty FullName
```

Na shell bash:

```bash
find . -name package.json -not -path '*/node_modules/*'
```

Jeżeli znaleziono wiele `package.json`, wybierz ten, który zawiera właściwy app package dla `qubok_interface_core`, a nie dependency/example/cache.

## Read scripts before running commands

Po znalezieniu `package.json`, najpierw sprawdź scripts:

PowerShell:

```powershell
Get-Content .\package.json | Select-String '"scripts"' -Context 0,30
```

Bash:

```bash
cat package.json
```

Nie zgaduj komend testowych, jeśli nie ma ich w scripts.

## Standard command order

| Step | Command | Required? | Notes |
|---:|---|---|---|
| 1 | package manager install command | only if dependencies missing | prefer existing lockfile |
| 2 | `npm run build` | yes if script exists | primary acceptance for current prototype |
| 3 | `npm run test` | yes if script exists | headless tests |
| 4 | `npm run typecheck` | if script exists | often covered by build if Vite/tsc build |
| 5 | `npm run lint` | if script exists | do not add lint fixes outside scope |
| 6 | manual smoke test | if user can run app | describe exactly what to check |

## Build acceptance template

```text
Build / test
- package directory: [path]
- install needed: yes/no
- build command: [command or not available]
- build result: pass/fail/not run
- test command: [command or not available]
- test result: pass/fail/not run
- manual smoke test: [steps]
```

## Manual smoke tests by roadmap stage

| Stage | Manual smoke test |
|---|---|
| Foundation | docs/files exist, no contradictory status, current source of truth points to current sprint |
| Shell | app opens, zones visible, default UI not debug-heavy |
| Core model / commands | create Project headlessly, create/patch/select object through commands |
| Command history | object edit is logged; undo/redo contract works or is explicitly pending |
| Selectors | selected/active/root/children selectors return consistent data |
| Validation | broken parent/region cases produce useful errors/warnings |
| Render adapter | Project maps to render descriptors without React dependency |
| Canvas | primitive objects render from Project/render model |
| Primitive creation | create rectangle/text/region through tool/command and inspect Project |
| Selection / transform | click/box select, move/resize selected object, no viewport-object confusion |
| Inspector / hierarchy | inspector and hierarchy show same active/selected object as canvas |
| Region debug | visual/layout/interaction overlays are distinguishable and not default-heavy |
| Component proof | Panel_Monitor exists as Project data and validates |
| Save/export | Project JSON, Component JSON and SVG export without broken links |
| UI cleanup | default screen is compact, readable, no debug dashboard look |
| Component library | save/insert component creates fresh valid IDs |
| Layout/panel builder | snap/align/distribute affects selected objects through commands |
| Docking shell | split/merge/resize app panels without changing InterfaceObject transforms |
| Events/actions | event assignment can output command without visual node graph |
| Node graph | node graph viewport pan/zoom/selection works; node graph outputs commands |
| Advanced | feature is gated by maturity and hidden unless L3/L4 |

## Repair command rule

When a command fails:

1. Save the failing command and first relevant error.
2. Use `26_CODEX_REPAIR_PLAYBOOK.md`.
3. Repair only current sprint.
4. Re-run the same command.
5. Do not proceed to the next roadmap stage until pass.

## Do not do

- Do not run random commands from memory.
- Do not assume root repo has `package.json`.
- Do not install a new package manager unless project already uses it.
- Do not add a test framework without current sprint explicitly allowing it.
- Do not fix unrelated lint/style issues during a feature sprint.
- Do not delete tests to pass build.

## Recommended future update

After the real project directory and scripts are confirmed, update this file with exact commands, for example:

```text
package directory: [actual path]
build: npm run build
test: npm run test
dev: npm run dev
```