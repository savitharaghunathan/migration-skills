# Dependency Map — JDK 21 to 25

| old_artifact | new_artifact | version_constraint | action | notes | source_section |
|---|---|---|---|---|---|
| jdk.random | removed | | remove | Module removed in JDK 23; random number generator implementations moved to java.base | Removed APIs, Tools, and Components (JDK 23) |
| jdk.crypto.ec | jdk.crypto.ec | | replace | Module deprecated for removal in JDK 22; elliptic curve crypto implementations being consolidated | Deprecated for Removal (JDK 22) |
| jdk.jsobject | removed | | remove | Module deprecated for removal in JDK 24; JavaScript–Java bridge no longer supported | Deprecated for Removal (JDK 24) |
