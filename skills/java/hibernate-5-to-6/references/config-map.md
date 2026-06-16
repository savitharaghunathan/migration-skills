# Config Map — Hibernate 5 to 6

| old_property | new_property | default_changed | old_default | new_default | file_pattern | notes | source_section |
|---|---|---|---|---|---|---|---|
| javax.persistence.* | jakarta.persistence.* | | | | persistence.xml | All JPA settings renamed from javax.persistence to jakarta.persistence prefix; Hibernate temporarily supports both with deprecation warnings | Jakarta Persistence |
| hibernate.ejb.metamodel.population | hibernate.jpa.metamodel.population | | | | persistence.xml, hibernate.cfg.xml | Prefix changed from ejb to jpa | Configuration property renames |
| hibernate.ejb.cfgfile | hibernate.cfg_xml_file | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.xml_files | hibernate.orm_xml_files | | | | persistence.xml | | Configuration property renames |
| hibernate.hbmxml.files | hibernate.hbm_xml_files | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.loaded.classes | hibernate.loaded_classes | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.persistenceUnitName | hibernate.persistenceUnitName | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.discard_pc_on_close | hibernate.discard_pc_on_close | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.entitymanager_factory_name | hibernate.session_factory_name | | | | persistence.xml | Also renamed from entitymanager to session | Configuration property renames |
| hibernate.ejb.session_factory_observer | hibernate.session_factory_observer | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.identifier_generator_strategy_provider | hibernate.identifier_generator_strategy_provider | | | | persistence.xml | | Configuration property renames |
| hibernate.ejb.classcache.* | hibernate.classcache.* | | | | persistence.xml | Prefix changed | Configuration property renames |
| hibernate.ejb.collectioncache.* | hibernate.collectioncache.* | | | | persistence.xml | Prefix changed | Configuration property renames |
| hibernate.ejb.event.* | hibernate.event.* | | | | persistence.xml | Prefix changed | Configuration property renames |
| hibernate.hql.bulk_id_strategy | hibernate.query.mutation_strategy | | | | persistence.xml, hibernate.cfg.xml | Now refers to SqmMultiTableMutationStrategy classes | Query — Multi-table Mutation Queries |
| hibernate.classLoader.application | removed | | | | persistence.xml | Use `hibernate.classLoaders` instead | Previously Deprecated features |
| hibernate.classLoader.resources | removed | | | | persistence.xml | Use `hibernate.classLoaders` instead | Previously Deprecated features |
| hibernate.classLoader.hibernate | removed | | | | persistence.xml | Use `hibernate.classLoaders` instead | Previously Deprecated features |
| hibernate.classLoader.environment | removed | | | | persistence.xml | Use `hibernate.classLoaders` instead | Previously Deprecated features |
| hibernate.hbm2dll.create_namespaces | hibernate.hbm2ddl.create_namespaces | | | | persistence.xml | Typo fix (dll→ddl); also use `jakarta.persistence.create-database-schemas` | Previously Deprecated features |
| hibernate.multiTenancy | removed | | | | persistence.xml, hibernate.cfg.xml | Auto-inferred from MultiTenantConnectionProvider or @TenantId; `MultiTenancyStrategy` enum also removed | Multitenancy simplification |
| hibernate.id.db_structure_naming_strategy | hibernate.id.db_structure_naming_strategy | true | single (shared hibernate_sequence) | standard (per-entity sequence) | persistence.xml | Set to `single` or `legacy` for backward compatibility | Implicit Identifier Sequence and Table Name |
| hibernate.criteria.copy_tree | hibernate.criteria.copy_tree | true | true | false | persistence.xml | When false, criteria queries must not be mutated after createQuery(); set to true for backward compat | Query — SQM — Hibernate Criteria behavior change |
| hibernate.type.preferred_duration_jdbc_type | hibernate.type.preferred_duration_jdbc_type | | | INTERVAL_SECOND | persistence.xml | New; set to `BIGINT` for backward compat with 5.x Duration mapping | Duration mapping changes |
| hibernate.type.preferred_uuid_jdbc_type | hibernate.type.preferred_uuid_jdbc_type | | | UUID | persistence.xml | New; set to `BINARY` for backward compat with 5.x UUID mapping | UUID mapping changes |
| hibernate.type.preferred_instant_jdbc_type | hibernate.type.preferred_instant_jdbc_type | | | TIMESTAMP_UTC | persistence.xml | New; set to `TIMESTAMP` for backward compat with 5.x Instant mapping | Instant mapping changes |
| hibernate.timezone.default_storage | hibernate.timezone.default_storage | true | NORMALIZE | DEFAULT | persistence.xml | 6.2 changed default; DEFAULT stores time zone if DB supports it; set to NORMALIZE for 5.x behavior | 6.2 — Timezone and offset storage |
| hibernate.cdi.extensions | hibernate.cdi.extensions | true | true (implicit) | false | persistence.xml | 6.2: CDI extensions no longer auto-resolved; set to true if using CDI-resolved Hibernate extensions | 6.2 — Changes to CDI handling |
| hibernate.enhancer.enableLazyInitialization | hibernate.enhancer.enableLazyInitialization | true | false | true | persistence.xml, pom.xml | 6.2: default changed; deprecated for removal | 6.2 — Change enhancement defaults |
| hibernate.enhancer.enableDirtyTracking | hibernate.enhancer.enableDirtyTracking | true | false | true | persistence.xml, pom.xml | 6.2: default changed; deprecated for removal | 6.2 — Change enhancement defaults |
| hibernate.bytecode.use_reflection_optimizer | hibernate.bytecode.use_reflection_optimizer | true | false | true | persistence.xml | 6.2: default changed; deprecated for removal | 6.2 — Change enhancement defaults |
| hibernate.type.wrapper_array_handling | hibernate.type.wrapper_array_handling | | | DISALLOW | persistence.xml | 6.2: new property; set to `legacy` for backward compat with Byte[]/Character[] as VARBINARY/VARCHAR | 6.2 — Byte[]/Character[] mapping changes |
