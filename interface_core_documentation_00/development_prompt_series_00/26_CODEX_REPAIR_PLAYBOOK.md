# 26 — Codex repair playbook

status: active
version: v2.1
doc_type: codex_repair_policy
last_updated: 2026-06-10

## Cel

Ten dokument opisuje standardowy flow naprawy, gdy sprint, build, testy albo typecheck nie przechodzą. Ma zapobiegać temu, że Codex po błędzie przechodzi do kolejnego etapu lub robi szeroki refactor zamiast naprawić blokadę.

## Zasada nadrzędna

Jeżeli sprint nie przechodzi build/test/typecheck/manual acceptance, nie idź dalej w roadmapie. Zrób repair prompt dla aktualnego sprintu.

## Repair route

Dla błędu używaj Route B z `25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md`:

```text
Read:
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
- current sprint file
- failing log/code context only
```

Nie czytać całej dokumentacji.

## Repair prompt template

```text
REPAIR SPRINT: [current sprint name]

ASSUME CURRENT SPRINT FAILED
- Build/test/typecheck failed with the attached log.
- Repair only the current sprint.
- Do not expand scope.
- Do not implement next roadmap stage.

READ ONLY
- 25_CODEX_DOCUMENTATION_NAVIGATION_MAP.md
- 00_CURRENT_SOURCE_OF_TRUTH.md
- 01_PROMPT_PROTOCOL_AND_SCOPE_GATES.md
- [current sprint file]
- [failing log]
- [directly affected source files]

REPAIR GOAL
- Fix the smallest set of files needed to pass build/test/typecheck.
- Preserve v2.1 invariants.
- Do not refactor unrelated modules.

ACCEPTANCE
- Build/typecheck/test command passes, or missing command is explicitly documented.
- Original failing error no longer appears.
- No unrelated feature work was added.
- Response lists changed files, root cause, fix, test result and remaining risk.
```

## Failure classes

| Failure type | Likely cause | Repair strategy |
|---|---|---|
| TypeScript import error | renamed/missing export, wrong module boundary | fix export/import, do not move architecture unless required |
| Type mismatch | model drift, command payload mismatch | update type and caller together, add/adjust test |
| Build error from missing file | planned module not created or wrong path | create minimal module or correct import path |
| Runtime crash in panel | UI reads missing selector/data | add selector fallback or validation, avoid local mutation |
| Selection drift | canvas/inspector/hierarchy use different state | route through Project.selection and selectors |
| Validation failure | broken parent/child/region/component link | repair data relation, not just hide error |
| Test failure | behavior changed | decide if test or implementation is wrong, update one deliberately |
| External library issue | adapter boundary missing | keep library behind adapter, do not make it source of truth |
| Performance regression | excessive rerender/hit-test/logging | throttle, memoize, reduce debug output |

## Forbidden during repair

- Do not start the next sprint.
- Do not add state graph UI while repairing core/canvas.
- Do not add docking while repairing canvas/inspector.
- Do not replace architecture with external library state.
- Do not silence TypeScript with broad `any` unless marked temporary and isolated.
- Do not remove validation to make errors disappear.
- Do not remove tests without explaining why the test was invalid.
- Do not rewrite unrelated files for style cleanup.

## Minimal repair process

1. Identify the first failing command.
2. Identify the first failing file/module.
3. Classify the failure type.
4. Patch the smallest owner boundary.
5. Run the same failing command again.
6. If it passes, run the normal build/test command set.
7. Report root cause, changed files and remaining risks.

## Required repair response format

```text
Root cause
- ...

Co naprawiono
- ...

Zmienione pliki
- ...

Test / build result
- command: ...
- result: pass/fail/not available

Czego celowo nie ruszałem
- ...

Ryzyka / pending
- ...
```

## When to stop

Stop after the current sprint passes its acceptance. Do not continue to the next roadmap stage in the same repair task.