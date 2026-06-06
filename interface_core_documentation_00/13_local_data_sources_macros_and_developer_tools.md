# 13 — Local data sources, macros and developer tools

Status: draft 00
Scope: local data source integration, user macros, local automations, debug/developer utilities, versioning, archive/recovery
Mode: private / local-first / semi-offline

## 1. Purpose

This document defines the local-only support layer around `qubok_interface_core`: local data sources, user macros, scheduled local actions, debug logs, validation tools, local versioning, snapshots and recovery.

This is not a cloud/team/server workflow. The system is designed for private/internal/semi-offline work.

Core rule:

```text
Local sources and macros may drive UI parameters, but they must do so through the same binding, action and command systems as everything else.
```

## 2. Scope boundaries

Included:

```text
local JSON / CSV / TXT / SVG / bitmap metadata sources
local file/folder references
manual refresh and local watch mode
schema detection and field mapping
local macro recorder and action lists
local event hooks
local scheduled tasks
local debug log and activity history
local snapshots and project archives
local migrations and validation reports
```

Excluded:

```text
cloud data sources
team dashboards
online collaboration
remote scheduling
server-based automation
remote telemetry
shared cloud libraries
external account permissions
```

## 3. Local data source types

| Source type | Typical use |
|---|---|
| JSON | structured project/UI data, component manifests, config |
| CSV | tables, chart data, lists, numeric values |
| TXT | notes, simple line lists, raw text templates |
| SVG | vector shapes/icons imported as primitive/path assets |
| image metadata | color palette extraction, dimensions, filename/EXIF-like metadata where available |
| folder | local asset browser, icon/image/font folders |
| local cache | derived preview/index/cache data |
| local loopback | optional same-machine tool endpoint, no internet dependency |

## 4. Local data source model

Recommended fields:

```text
source_id
name
source_type
local_path or local_endpoint
enabled
refresh_mode
schema_status
schema_definition
field_mappings
cache_policy
last_loaded_at
last_error
validation_status
tags
```

A local data source is not a UI element by itself. It feeds parameters or component lists through mappings/bindings.

## 5. Source schema detection

Source loading flow:

```text
choose file/folder/source
-> detect source type
-> read locally
-> detect schema/columns/fields
-> show preview table/tree
-> user maps fields to UI parameters
-> save mapping into project
-> validate mapping
```

Example CSV schema:

```text
columns: date, product, category, sales, count, region
inferred types: date, string, string, number, integer, string
```

Example mapping:

```text
sales -> ChartBar.height
product -> ChartLabel.text
category -> color palette index
```

## 6. Refresh and cache policies

| Policy | Meaning |
|---|---|
| manual_refresh | user refreshes explicitly |
| watch_file | local file watcher refreshes when file changes |
| cache_until_dirty | cache data until path timestamp/hash changes |
| refresh_on_project_open | load when project opens |
| disabled | source remains linked but inactive |

Do not require remote services. Watch mode should only observe local files/folders.

## 7. Data mapping pipeline

```text
Source
-> Schema Detection
-> Transform / Filter / Sort / Map
-> Binding
-> UI Object Parameters
-> Command / Computed Runtime Output
-> Render / Validation / Debug
```

Examples:

```text
CSV.sales -> map_range(0..max_sales, 0..chart_height) -> Bar.height
JSON.panel.title -> Panel_Title_Text.string
SVG.icon_path -> Icon_Component.path_data
Folder.assets -> ComponentLibrary cards
TXT.lines -> Dropdown item list
```

## 8. Local data binding validation

Checks:

```text
source path exists
file type supported
schema readable
mapped field exists
type conversion valid
value range valid
target parameter exists
target writable or binding-output allowed
cache not stale unless accepted
no remote dependency required
```

Failures should produce warnings/placeholders, not crashes.

## 9. Macro system overview

Macros are recorded or manually assembled sequences of actions/commands.

Macro use cases:

```text
create repeated panel layout
apply same preset to selected controls
export selected group
run validation and save snapshot
create component from selection
update local data source and refresh chart
set debug overlay state
```

Macro is not arbitrary code in MVP. It is a sequence of known actions/commands.

## 10. Macro model

Recommended fields:

```text
macro_id
name
description
enabled
trigger optional
actions[]
parameters
scope
requires_selection
safety_mode
dry_run_supported
created_at
updated_at
last_run_at
run_count
validation_status
```

Action item:

```text
step_id
action_id
parameters
target_policy
condition optional
on_error policy
notes
```

## 11. Macro triggers / event hooks

Allowed local triggers:

| Trigger | Example |
|---|---|
| manual_run | user clicks Run Macro |
| shortcut | local keyboard shortcut |
| on_save | after project save |
| on_project_open | when project opens |
| on_selection_change | when active object changes |
| on_region_enter | local UI region hover/enter |
| on_panel_open | when panel/workspace opens |
| delay_elapsed | local timer after trigger |
| interval_tick | local scheduled task |
| validation_error_added | after validation detects issue |

Trigger scope should be explicit:

```text
project
workspace
selected object
selected panel
local source
```

## 12. Macro safety modes

| Mode | Meaning |
|---|---|
| dry_run | simulate and preview commands without mutation |
| confirm_destructive | ask before delete/overwrite/import/migration |
| undo_integrated | every command enters undo/redo stack |
| read_only | observe/report only |
| selection_locked | macro cannot modify outside selection |
| project_locked | macro cannot persist changes, preview only |

Destructive actions:

```text
delete object
overwrite preset
migrate project
import over existing asset
clear library
remove snapshot
```

should require confirmation or dry-run preview.

## 13. Macro editor UI

Macro editor should show:

```text
macro name
trigger
scope
step list
action id
parameters
target resolver
condition
validation status
run / dry run / save
last log
```

The visual style can resemble a compact spreadsheet/action list.

Each row:

```text
enabled | step | action | target | parameters | condition | status | menu
```

## 14. Local scheduled tasks

Scheduled tasks are macros with time triggers.

Examples:

```text
every 5 min -> create autosave snapshot
daily 02:00 -> clean cache/logs
on close -> backup presets
manual -> export selected PNG/SVG
```

Schedule must remain local. No server scheduler.

## 15. Developer tools

Developer/debug tools are local utility panels, not collaboration tools.

Useful panels:

```text
Validation Panel
Command Log
Binding Debug Table
Event/Action Trace
Routing Debug Overlay
Performance Overlay
Migration Report
Archive/Backup Manager
Local Source Inspector
Preset/Component Dependency Inspector
```

## 16. Validation panel

Should show:

```text
project status
errors
warnings
broken references
invalid bindings
missing local files
component version mismatch
unsupported export objects
state graph validation issues
routing errors
```

Each issue should include:

```text
severity
object/source id
message
source path
suggested action
can_auto_fix boolean
```

## 17. Command log and activity history

Command log fields:

```text
time
source user/macro/event/graph
command type
affected objects
payload summary
validation result
dirty flags
undoable
status
```

Activity history may also include:

```text
project opened
project saved
snapshot created
source refreshed
macro run
export completed
migration run
```

## 18. Local versioning

Local versioning should support:

```text
manual snapshots
named milestones
before/after migration states
comparison of project versions
restore snapshot
archive project
```

It should not require Git, but should be compatible with storing files in a Git repository if the user chooses.

Version record:

```text
version_id
label
created_at
reason
project_schema_version
file_hash
validation_summary
notes
```

## 19. Migration tools

Migration flow:

```text
open old project
validate old schema
create pre-migration snapshot
run migration steps
validate migrated project
show changed fields
allow save as new file or restore snapshot
```

Migration report:

```text
from_version
to_version
changed_objects
changed_fields
warnings
errors
auto_fixed
manual_actions_required
```

## 20. Archive/recovery manager

Archive manager should support:

```text
create archive
inspect archive
restore from archive
verify archive completeness
export logs/reports
clean old autosaves
```

Archive completeness checks:

```text
project file included
assets included or intentionally external
component assets included
themes/presets included
local data mappings included
validation report included
```

## 21. Error cases

| Error | UI response |
|---|---|
| local file missing | warning + relink option |
| bad data format | schema preview error |
| mapping mismatch | field marked invalid |
| macro target missing | step blocked |
| destructive macro | confirmation required |
| migration failure | restore snapshot option |
| cache mismatch | refresh/rebuild cache option |
| invalid component version | migration/duplicate-as-new option |

## 22. Roadmap placement

| Feature | Maturity start | Target |
|---|---|---|
| command log | L2 | early debug |
| validation panel | L2/L3 | MVP support |
| local snapshots | L2/L3 | after persistence |
| local source inspector | L2 | post-MVP data tools |
| macro action list | L2 | after action registry |
| macro recorder | L2/L3 | later user workflow |
| scheduled local tasks | L2 | optional utility |
| archive manager | L2/L3 | developer/recovery tools |
| migration reports | L1/L2 | schema evolution |

## 23. Acceptance rule

The local source/macro/developer layer is valid when:

```text
it never requires cloud/server/team infrastructure
local files can feed UI through mappings and bindings
macros are command/action sequences, not arbitrary unsafe code
all destructive actions are previewable or confirmable
logs and validation make failures explainable
archives and snapshots can recover project state
```

## 24. Final decision

Local data sources, macros and developer tools should be treated as private workflow accelerators around the same model/command/binding core. They must not introduce a second automation system or any server assumption.
