# Dependency Map — Apache HttpClient 4.x to 5.x

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| org.apache.httpcomponents:httpclient | org.apache.httpcomponents.client5:httpclient5 | >= 5.0 | replace | Package namespace changes from `org.apache.http` to `org.apache.hc.client5` and `org.apache.hc.core5` | Migration steps |
| org.apache.httpcomponents:httpcore | org.apache.httpcomponents.core5:httpcore5 | >= 5.0 | replace | Core classes move to `org.apache.hc.core5` package | Migration steps |
| org.apache.httpcomponents:httpmime | org.apache.httpcomponents.client5:httpclient5 | >= 5.0 | merge | MIME support merged into httpclient5 | Migration steps |
| org.apache.httpcomponents:httpclient-cache | org.apache.httpcomponents.client5:httpclient5-cache | >= 5.0 | replace | Cache module coordinates changed | Migration steps |
| org.apache.httpcomponents:fluent-hc | org.apache.httpcomponents.client5:httpclient5-fluent | >= 5.0 | replace | Fluent API module coordinates changed | Migration steps |
| org.apache.httpcomponents:httpclient-osgi | removed | | remove | OSGi bundle no longer separately packaged | Migration steps |
| org.apache.httpcomponents:httpclient-win | removed | | remove | Windows auth module no longer separately packaged | Migration steps |
