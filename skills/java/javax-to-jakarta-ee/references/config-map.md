# Config Map — Java EE 8 to Jakarta EE 10

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| javax.persistence.jdbc.url | jakarta.persistence.jdbc.url | | | | persistence.xml | JPA JDBC connection URL | Persistence properties |
| javax.persistence.jdbc.user | jakarta.persistence.jdbc.user | | | | persistence.xml | JPA JDBC username | Persistence properties |
| javax.persistence.jdbc.password | jakarta.persistence.jdbc.password | | | | persistence.xml | JPA JDBC password | Persistence properties |
| javax.persistence.jdbc.driver | jakarta.persistence.jdbc.driver | | | | persistence.xml | JPA JDBC driver class | Persistence properties |
| javax.persistence.provider | jakarta.persistence.provider | | | | persistence.xml | JPA provider class name | Persistence properties |
| javax.persistence.transactionType | jakarta.persistence.transactionType | | | | persistence.xml | JTA or RESOURCE_LOCAL | Persistence properties |
| javax.persistence.jtaDataSource | jakarta.persistence.jtaDataSource | | | | persistence.xml | JTA datasource JNDI name | Persistence properties |
| javax.persistence.nonJtaDataSource | jakarta.persistence.nonJtaDataSource | | | | persistence.xml | Non-JTA datasource JNDI name | Persistence properties |
| javax.persistence.schema-generation.database.action | jakarta.persistence.schema-generation.database.action | | | | persistence.xml | Schema generation action (none, create, drop, drop-and-create) | Persistence properties |
| javax.persistence.schema-generation.scripts.action | jakarta.persistence.schema-generation.scripts.action | | | | persistence.xml | Script generation action | Persistence properties |
| javax.persistence.schema-generation.create-source | jakarta.persistence.schema-generation.create-source | | | | persistence.xml | Schema creation source | Persistence properties |
| javax.persistence.schema-generation.drop-source | jakarta.persistence.schema-generation.drop-source | | | | persistence.xml | Schema drop source | Persistence properties |
| javax.persistence.schema-generation.create-database-schemas | jakarta.persistence.schema-generation.create-database-schemas | | | | persistence.xml | Auto-create database schemas | Persistence properties |
| javax.persistence.schema-generation.scripts.create-target | jakarta.persistence.schema-generation.scripts.create-target | | | | persistence.xml | Create script output target | Persistence properties |
| javax.persistence.schema-generation.scripts.drop-target | jakarta.persistence.schema-generation.scripts.drop-target | | | | persistence.xml | Drop script output target | Persistence properties |
| javax.persistence.sql-load-script-source | jakarta.persistence.sql-load-script-source | | | | persistence.xml | SQL load script source | Persistence properties |
| javax.persistence.validation.mode | jakarta.persistence.validation.mode | | | | persistence.xml | Bean validation mode (AUTO, CALLBACK, NONE) | Persistence properties |
| javax.persistence.validation.group.pre-persist | jakarta.persistence.validation.group.pre-persist | | | | persistence.xml | Validation groups for pre-persist | Persistence properties |
| javax.persistence.validation.group.pre-update | jakarta.persistence.validation.group.pre-update | | | | persistence.xml | Validation groups for pre-update | Persistence properties |
| javax.persistence.validation.group.pre-remove | jakarta.persistence.validation.group.pre-remove | | | | persistence.xml | Validation groups for pre-remove | Persistence properties |
| javax.persistence.lock.timeout | jakarta.persistence.lock.timeout | | | | persistence.xml | Lock timeout in milliseconds | Persistence properties |
| javax.persistence.query.timeout | jakarta.persistence.query.timeout | | | | persistence.xml | Query timeout in milliseconds | Persistence properties |
| javax.persistence.sharedCache.mode | jakarta.persistence.sharedCache.mode | | | | persistence.xml | Shared cache mode (ALL, NONE, ENABLE_SELECTIVE, DISABLE_SELECTIVE) | Persistence properties |
