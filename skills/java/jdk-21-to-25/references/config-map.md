# Config Map — JDK 21 to 25

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| java.locale.providers=JRE | java.locale.providers=CLDR | | JRE | CLDR | JVM system property | Legacy JRE locale data removed in JDK 23; migrate to CLDR locale data | Removed APIs (JDK 23) |
| java.locale.providers=COMPAT | java.locale.providers=CLDR | | COMPAT | CLDR | JVM system property | COMPAT is an alias for JRE; also removed in JDK 23 | Removed APIs (JDK 23) |
| java.naming.rmi.security.manager | removed | | | | jndi.properties | JNDI environment property removed in JDK 24 | Removed APIs (JDK 24) |
| java.security.policy | removed | | | | JVM system property | Ignored since JDK 24; Security Manager permanently disabled (JEP 486) | Permanently Disable the Security Manager (JDK 24) |
| java.security.manager | removed | | | | JVM system property | Ignored since JDK 24; Security Manager permanently disabled (JEP 486) | Permanently Disable the Security Manager (JDK 24) |
| java.locale.useOldISOCodes | removed | | | | JVM system property | System property deprecated for removal in JDK 25 | Deprecated for Removal (JDK 25) |
| jdk.tls.server.newSessionTicket | jdk.tls.server.newSessionTicket | true | (not configurable) | (configurable) | JVM system property | New system property in JDK 24 to configure TLS 1.3 session ticket count | Security Updates (JDK 24) |
