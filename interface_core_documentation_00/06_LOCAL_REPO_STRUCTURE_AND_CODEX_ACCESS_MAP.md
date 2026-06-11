# QUBOK_INTERFACE_CORE — LOCAL REPO STRUCTURE AND CODEX ACCESS MAP

Version: 2026-06-11
Status: current Codex navigation guide for the local PC project structure

## 1. Observed local root

Observed path from local screenshot:

```text
I:\Art\_AI\app_development\qubok_interface_core
```

Visible root entries include:

```text
._qubok_backups
._qubok_build_logs
_diagnostics
dist
docs
node_modules
src
.gitignore
index
package
package-lock
README
README.md.bak_*
SPRINT_13_5_*_NOTES.md
```

The repository root is currently a mixture of implementation files, generated folders, backup/debug folders and historical sprint notes.

## 2. Interpretation

This structure is normal for a fast prototype, but it is not ideal for Codex context loading.

| Root item type | Meaning | Codex rule |
|---|---|---|
| `src/` | actual implementation | inspect for code work |
| `package.json`, `package-lock.json` | build/dependency entry | inspect for build/test commands |
| `README.md` | public project entry | must point to canonical docs |
| `interface_core_documentation_00/` | canonical working docs on GitHub | read first |
| `SPRINT_*` notes | historical local task notes | read only if relevant to the current task |
| `_diagnostics` | local diagnostic output | useful evidence, not source of truth |
| `_qubok_build_logs` | local build logs | inspect only for build/error tasks |
| `_qubok_backups` / `README.md.bak_*` | rollback/history | do not delete without explicit user request |
| `dist` | generated build output | do not inspect unless release/build issue |
| `node_modules` | dependency folder | never use as context except dependency debugging |

## 3. Recommended cleanup policy

Do not delete local files automatically. Instead, document and gate cleanup.

Safe cleanup direction, only when explicitly requested:

| Move from root | Move to | Reason |
|---|---|---|
| `SPRINT_*.md` | `docs/history/sprints/` or `_diagnostics/sprint_notes/` | reduce root noise |
| `README.md.bak_*` | `_qubok_backups/readme/` | keep rollback but clean root |
| old logs | `_qubok_build_logs/archive/` | preserve evidence |
| generated diagnostics | `_diagnostics/` | keep separate from canon |

Never move/delete:

- `src/`,
- `package.json`,
- `package-lock.json`,
- `.gitignore`,
- active README,
- active documentation folder,
- user backups without approval.

## 4. Codex read order

For every implementation task, Codex should read context in this order:

1. `README.md`
2. `interface_core_documentation_00/00_INDEX.md`
3. `interface_core_documentation_00/00_CURRENT_SOURCE_OF_TRUTH.md`
4. `interface_core_documentation_00/02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md`
5. `interface_core_documentation_00/03_FEATURE_MATURITY_MATRIX.md`
6. `interface_core_documentation_00/04_CODEX_DEVELOPMENT_PROTOCOL.md`
7. `interface_core_documentation_00/05_CURRENT_IMPLEMENTATION_STABILIZATION_MAP.md`
8. the relevant files under `src/`
9. root `SPRINT_*` notes only if they match the current feature or error
10. logs only if the task is build/runtime diagnosis

## 5. Codex should not infer from root clutter

Root-level sprint files are not canonical unless explicitly referenced by the current task. If a root sprint note conflicts with canonical docs, follow the docs precedence:

```text
00_CURRENT_SOURCE_OF_TRUTH.md
-> 01_TERMINOLOGY_CANON_NODE_GRAPH.md
-> 02_ROADMAP_PHASE_TO_SCREEN_STATE_MAP.md
-> 03_FEATURE_MATURITY_MATRIX.md
-> 04_CODEX_DEVELOPMENT_PROTOCOL.md
-> 05_CURRENT_IMPLEMENTATION_STABILIZATION_MAP.md
-> 06_LOCAL_REPO_STRUCTURE_AND_CODEX_ACCESS_MAP.md
-> references 10-14
-> older notes/prompts
```

## 6. Working path naming

Canonical current working path:

```text
I:\Art\_AI\app_development\qubok_interface_core
```

Do not introduce `_00` in new documentation or prompts unless referring to older archived notes that used that name.

## 7. Current root-noise conclusion

The current PC structure suggests that the next work should begin with a safety/readability pass, not a new feature sprint.

Recommended first Codex task if build status is unknown:

```text
S0 Safety baseline / repo cleanliness
- inspect package scripts
- run build
- report root files grouped by role
- do not delete anything
- propose a cleanup plan
```

Recommended first Codex task if build status is known and safe:

```text
S1 Selection Operation Target Contract
- inspect canvas selection, hierarchy selection, delete and drag code
- implement stable operation target resolver
- add manual test checklist
```

## 8. File access map by task type

| Task type | Inspect first | Avoid |
|---|---|---|
| build error | `package.json`, latest build log, TypeScript error files | broad refactor |
| selection bug | canvas selection code, hierarchy selection code, delete/drag command path | UI restyling |
| drag freeze | pointer handlers, validation calls, command log, autosave/localStorage | graph work |
| multi-canvas performance | docking content resolver, canvas mount points, viewport components | rewriting object model |
| core boundary | `src/core` imports and browser API usage | moving UI into core |
| default UI cleanup | shell, panels, bottom shelf, debug visibility | hiding runtime bugs |
| node_graph naming | grep legacy graph names | changing semantic state terms |
| docs cleanup | README, 00_INDEX, canonical docs | editing old notes as source of truth |

## 9. Expected Codex output format

Codex should respond after each implementation step with:

```text
Done:
- changed files
- created files
- important decisions

How to test:
- build command
- manual UI checks
- expected result

Not touched:
- explicit out-of-scope items

Next:
- one logical next task
```
