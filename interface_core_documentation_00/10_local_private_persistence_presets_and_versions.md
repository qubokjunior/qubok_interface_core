# 10 — Local/private persistence, presets and versions

Status: draft 00
Scope: local project persistence, snapshots, presets, themes, override chains, versioning and archive rules
Mode: private / local-first / semi-offline

## 1. Purpose

This document defines how `qubok_interface_core` should store projects, settings, presets, snapshots and reusable assets in a private/semi-offline workflow.

The system is intended for local/internal use. It must not assume cloud accounts, remote collaboration, team permissions, public servers or online libraries.

Core rule:

```text
Everything important must be recoverable from local files.
Everything persistent must be represented as data, not hidden UI state.
```

## 2. Scope boundaries

Included:

```text
local project files
local component library
local presets
theme tokens
workspace settings
local snapshots
manual export/import
migration records
local recovery and validation
```

Excluded from this layer:

```text
cloud sync
online collaboration
shared server workspaces
team permissions
remote asset marketplace
analytics/telemetry
account-based storage
```

Allowed later, only as optional connectors or import/export utilities:

```text
open local folder
import package from file
export archive
local loopback endpoint on 127.0.0.1 for same-machine tools
```

## 3. File categories

| File / folder | Meaning | Persistence level |
|---|---|---|
| `.uiq` | main project file | required |
| `.qcomponent` | exported component/group/panel asset | optional reusable asset |
| `.qpreset` | parameter/style/layout/action preset | optional reusable asset |
| `.qtheme` | theme token set | optional reusable asset |
| `.qsnapshot` | project snapshot/recovery point | safety/recovery |
| `.qarchive` | portable project archive package | backup/transfer |
| `.qmap` | local data source mapping | local data binding |
| `/assets/` | local images, SVG, fonts, icons, data files | referenced local files |
| `/snapshots/` | autosave/manual snapshots | recovery |
| `/exports/` | SVG/PNG/JSON/export outputs | output only |
| `/logs/` | local debug/activity logs | diagnostic |

File extensions are implementation proposals. The internal format may remain JSON-like during early development.

## 4. Project file `.uiq`

The main project file should store the full persistent model.

Recommended fields:

```text
project_meta
schema_version
objects_by_id
root_children
selection optional / editor session only
viewport optional / editor session only
workspace_layout optional
settings_reference
theme_tokens
library_assets local references
component_instances
event_assignments
state_graphs
local_data_sources
validation_state latest compact
migration_history
```

Important distinction:

| Data | Stored in `.uiq`? | Reason |
|---|---:|---|
| objects, hierarchy, transforms | yes | project identity |
| region links and event bindings | yes | behavior identity |
| component instance overrides | yes | project identity |
| theme tokens used by project | yes or referenced | deterministic render |
| selected object | optional | editor convenience |
| viewport camera | optional | workflow convenience |
| command log | optional / compact | debug/history, may grow large |
| runtime hover state | no | transient |
| animation frame offset | no, unless recorded replay | runtime/debug |
| spatial index/cache | no | derived |

## 5. Settings and preferences

Settings should be split into two groups.

| Settings type | Stored where | Examples |
|---|---|---|
| global user preferences | local app settings | UI density, default theme, shortcuts, panel layout preference |
| project settings | `.uiq` | project theme, grid size, default export mode, included presets |

Global settings must not silently rewrite old projects. If user wants to apply new defaults to an existing project, that should be an explicit command:

```text
preferences.apply_to_project
preferences.apply_to_selected
preferences.apply_to_new_objects_only
```

## 6. Preset types

Presets are reusable parameter sets. They should not always imply a new component structure.

| Preset type | Affects | Structural change? |
|---|---|---:|
| style preset | fill/stroke/opacity/radius/text style | no |
| layout preset | margin/padding/gap/align/size policy | no unless slots change |
| component preset | exposed parameters of one component structure | no |
| theme preset | semantic tokens | no |
| event/action preset | event assignment/action config | no object count change |
| graph preset | reusable state graph/action chain | usually no visual structure change |
| panel template | panel/group structure | yes |
| structural variant | child count/roles/slots differ | yes |

Core distinction:

```text
Changing parameters of existing objects = preset/theme variation.
Changing number of objects, hierarchy, slots, regions or roles = new component/template variant.
```

## 7. Theme variation versus structure variant

### Theme / preset variation

Examples:

```text
button fill changes
theme accent changes
slider track radius changes
panel background switches to warning style
text size changes
hover color changes
```

This keeps:

```text
same object count
same roles
same slots
same region IDs/roles
same exposed parameter schema
```

### Structural variant

Examples:

```text
button gains icon slot
slider becomes minMax slider with second handle
panel adds footer section
dropdown adds search input
component changes region count
component exposes new required slot
```

This creates a new template/component variant and should update structure signature.

## 8. Override/source chain for persisted values

Values may be resolved from multiple sources. Persistence must preserve enough data to reconstruct the final value.

Source modes:

```text
default
local
theme_token
preset
inherited
dictated
component_parameter
linked_reference
graph_output
calculated
runtime_debug
```

Recommended stored row for a parameter override:

```json
{
  "parameter_path": "style.color_main",
  "source_mode": "local_override",
  "value": "#66cc66",
  "original_source": "theme.button.defaultFill",
  "changed_at": "2026-06-06T00:00:00Z",
  "changed_by": "inspector",
  "note": "user override over component preset"
}
```

## 9. Local library model

The local library should live inside or beside the project folder.

Recommended library asset metadata:

```text
asset_id
name
asset_type
category
tags
version
schema_version
created_at
updated_at
preview_ref
source_file
structure_signature
exposed_parameters
dependencies
used_times
referred_times
validation_status
```

Local library categories:

```text
Primitives
Controls
Panels
Templates
Icons
Curves
StateGraph
Actions
Presets
Themes
LocalDataMappings
```

## 10. Snapshot model

Snapshots are local safety points.

Snapshot triggers:

| Trigger | Suggested default |
|---|---|
| manual save | create optional pre-save snapshot |
| project open | optional recovery snapshot |
| before destructive action | snapshot if enabled |
| before migration | required snapshot |
| autosave interval | optional, local only |
| before import | recommended |

Snapshot fields:

```text
snapshot_id
project_id
created_at
reason
schema_version
project_hash
file_ref
notes
validation_summary
```

Snapshot policy:

```text
keep last N autosaves
keep manual snapshots until user deletes
keep migration snapshots permanently unless cleaned manually
compress old snapshots optionally
```

## 11. Autosave and manual save

Suggested flow:

```text
edit command
-> Project dirty flag
-> local autosave queue
-> validate lightweight
-> write temp file
-> atomic rename
-> update save status
-> optional snapshot rotation
```

Rules:

```text
autosave never requires network
autosave writes to local disk / browser storage / app storage depending on runtime
autosave failure must be visible in status/debug
manual save should be explicit and traceable
```

## 12. Versioning and migration

Each file should have:

```text
schema_version
app_version_created
app_version_updated
migration_history
```

Migration record:

```json
{
  "from_schema": "0.2.0",
  "to_schema": "0.3.0",
  "date": "2026-06-06T00:00:00Z",
  "migration_id": "region-data-normalization",
  "status": "success",
  "warnings": []
}
```

Before migration:

```text
create migration snapshot
validate original file
run migration
validate migrated file
show warnings
allow rollback to snapshot
```

## 13. Local archive package

A `.qarchive` should be a portable local package.

Suggested content:

```text
project.uiq
metadata.json
assets/
components/
presets/
themes/
data_sources/
snapshots optional
README.txt optional
validation_report.json
```

Archive use cases:

```text
backup
move project to another machine
freeze version before big refactor
send manually as file if user chooses
```

No background server assumptions.

## 14. UI panels for persistence and presets

### Project Save panel

Should show:

```text
file name
local path
schema version
last saved
dirty state
validation status
snapshot option
save button
save as button
export archive button
```

### Preset panel

Should show:

```text
preset name
preset type
category/tags
affected parameter paths
preview
source component/template
structure signature if relevant
save preset
apply preset
clear override
```

### Override inspector

Should show:

```text
parameter_path
original value
local value
resolved value
source mode
source chain
changed state
reset/revert
```

## 15. Validation for persistence

Checks:

```text
project_id exists
schema_version supported
all object ids unique
all parent/child links valid
all component asset references valid or marked missing
all preset references valid or fallback exists
all theme tokens resolvable
all local file paths valid or missing warning exists
all event/action assignments refer to existing registries
all state_graph edges/nodes valid if present
no remote/cloud-only dependency required for opening project
```

## 16. Failure and recovery cases

| Case | Behavior |
|---|---|
| project file missing | show open dialog / recent missing warning |
| asset missing | keep placeholder and warning, do not crash |
| preset missing | fallback to stored resolved value if available |
| theme missing | fallback to project embedded tokens or default theme |
| migration fails | restore pre-migration snapshot |
| autosave fails | keep in-memory state, show persistent error |
| corrupted project | try backup/snapshot, show recovery panel |
| unsupported schema | open read-only or require migration |

## 17. Roadmap placement

| Feature | Maturity start | Target |
|---|---|---|
| project JSON save/load | L1/L3 | MVP |
| local presets | L2/L3 | component/library phase |
| theme token persistence | L2/L3 | early UI polish |
| snapshots | L2/L3 | after save/load |
| archive package | L2 | post-MVP utility |
| migration records | L1/L2 | before schema changes become common |
| local library manager | L3 | component library phase |
| preset override inspector | L2/L3 | debug/inspector phase |

## 18. Acceptance rule

Persistence is valid when:

```text
project can be saved locally
project can be reopened without losing hierarchy, regions, bindings or presets
component assets instantiate with new IDs
snapshots can recover from broken edits
missing local assets degrade into warnings, not crashes
no cloud/server/team feature is required for normal use
```

## 19. Final decision

`qubok_interface_core` persistence should be local-first, explicit, inspectable and recoverable. Presets and themes should be parameter-layer systems. Templates and structural variants should be object-structure systems. Keeping those two categories separate prevents chaos in component reuse and debugging.
