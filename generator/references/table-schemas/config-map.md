# Config Map Schema

Each row represents a configuration property that was renamed, removed, or had its default value changed.

## Columns

| Column | Required | Description |
|--------|----------|-------------|
| old_property | yes | The property name in the source version |
| new_property | yes | Replacement property name, or `removed` if no replacement exists |
| default_changed | no | `true` if the default value changed (even if the property name stayed the same) |
| old_default | no | Previous default value |
| new_default | no | New default value |
| file_pattern | no | Which files contain this (e.g., `application.yml`, `application.properties`, `appsettings.json`) |
| notes | no | Context |
| source_section | yes | Guide section heading |

## Example Rows

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| spring.resources.static-locations | spring.web.resources.static-locations | | | | application.properties | Prefix changed from spring.resources to spring.web.resources | Static Resources |
| server.servlet.session.timeout | server.servlet.session.timeout | true | 30m | 15m | application.yml | Default reduced; explicit config needed to keep old behavior | Session Management |
| management.endpoints.web.exposure.include | removed | | | | application.properties | All actuator endpoints exposed by default in v4 | Actuator Changes |
