# API Map — Java EE 8 to Jakarta EE 10

| old_api | new_api | kind | action | before | after | notes | source_section |
|---|---|---|---|---|---|---|---|
| javax.activation.* | jakarta.activation.* | package | move_package | `import javax.activation.DataHandler;` | `import jakarta.activation.DataHandler;` | Jakarta Activation | Activation |
| javax.annotation.* | jakarta.annotation.* | package | move_package | `import javax.annotation.PostConstruct;` | `import jakarta.annotation.PostConstruct;` | Excludes `javax.annotation.processing.*` (Java SE) | Annotations |
| javax.annotation.security.* | jakarta.annotation.security.* | package | move_package | `import javax.annotation.security.RolesAllowed;` | `import jakarta.annotation.security.RolesAllowed;` | | Annotations |
| javax.annotation.sql.* | jakarta.annotation.sql.* | package | move_package | `import javax.annotation.sql.DataSourceDefinition;` | `import jakarta.annotation.sql.DataSourceDefinition;` | | Annotations |
| javax.batch.api.* | jakarta.batch.api.* | package | move_package | `import javax.batch.api.Batchlet;` | `import jakarta.batch.api.Batchlet;` | Includes chunk, chunk.listener, listener, partition subpackages | Batch |
| javax.batch.operations.* | jakarta.batch.operations.* | package | move_package | `import javax.batch.operations.JobOperator;` | `import jakarta.batch.operations.JobOperator;` | | Batch |
| javax.batch.runtime.* | jakarta.batch.runtime.* | package | move_package | `import javax.batch.runtime.BatchRuntime;` | `import jakarta.batch.runtime.BatchRuntime;` | Includes runtime.context subpackage | Batch |
| javax.decorator.* | jakarta.decorator.* | package | move_package | `import javax.decorator.Decorator;` | `import jakarta.decorator.Decorator;` | | CDI |
| javax.ejb.* | jakarta.ejb.* | package | move_package | `import javax.ejb.Stateless;` | `import jakarta.ejb.Stateless;` | Includes embeddable and spi subpackages | EJB |
| javax.el.* | jakarta.el.* | package | move_package | `import javax.el.ELContext;` | `import jakarta.el.ELContext;` | | Expression Language |
| javax.enterprise.concurrent.* | jakarta.enterprise.concurrent.* | package | move_package | `import javax.enterprise.concurrent.ManagedExecutorService;` | `import jakarta.enterprise.concurrent.ManagedExecutorService;` | | Concurrency |
| javax.enterprise.context.* | jakarta.enterprise.context.* | package | move_package | `import javax.enterprise.context.RequestScoped;` | `import jakarta.enterprise.context.RequestScoped;` | Includes control and spi subpackages | CDI |
| javax.enterprise.event.* | jakarta.enterprise.event.* | package | move_package | `import javax.enterprise.event.Observes;` | `import jakarta.enterprise.event.Observes;` | | CDI |
| javax.enterprise.inject.* | jakarta.enterprise.inject.* | package | move_package | `import javax.enterprise.inject.Produces;` | `import jakarta.enterprise.inject.Produces;` | Includes literal, se, spi, spi.configurator subpackages | CDI |
| javax.enterprise.util.* | jakarta.enterprise.util.* | package | move_package | `import javax.enterprise.util.AnnotationLiteral;` | `import jakarta.enterprise.util.AnnotationLiteral;` | | CDI |
| javax.faces.* | jakarta.faces.* | package | move_package | `import javax.faces.bean.ManagedBean;` | `import jakarta.faces.bean.ManagedBean;` | All subpackages: application, component, context, convert, el, event, flow, lifecycle, model, render, validator, view, webapp | Faces |
| javax.inject.* | jakarta.inject.* | package | move_package | `import javax.inject.Inject;` | `import jakarta.inject.Inject;` | | Dependency Injection |
| javax.interceptor.* | jakarta.interceptor.* | package | move_package | `import javax.interceptor.AroundInvoke;` | `import jakarta.interceptor.AroundInvoke;` | | Interceptors |
| javax.jms.* | jakarta.jms.* | package | move_package | `import javax.jms.ConnectionFactory;` | `import jakarta.jms.ConnectionFactory;` | | Messaging |
| javax.json.* | jakarta.json.* | package | move_package | `import javax.json.JsonObject;` | `import jakarta.json.JsonObject;` | Includes spi and stream subpackages | JSON Processing |
| javax.json.bind.* | jakarta.json.bind.* | package | move_package | `import javax.json.bind.Jsonb;` | `import jakarta.json.bind.Jsonb;` | Includes adapter, annotation, config, serializer, spi subpackages | JSON Binding |
| javax.jws.* | jakarta.jws.* | package | move_package | `import javax.jws.WebService;` | `import jakarta.jws.WebService;` | Includes jws.soap subpackage | Web Services Metadata |
| javax.mail.* | jakarta.mail.* | package | move_package | `import javax.mail.Session;` | `import jakarta.mail.Session;` | | Mail |
| javax.persistence.* | jakarta.persistence.* | package | move_package | `import javax.persistence.Entity;` | `import jakarta.persistence.Entity;` | Includes criteria, metamodel, spi subpackages | Persistence |
| javax.resource.* | jakarta.resource.* | package | move_package | `import javax.resource.cci.Connection;` | `import jakarta.resource.cci.Connection;` | Includes cci, spi, spi.endpoint, spi.security, spi.work subpackages | Connectors |
| javax.security.auth.message.* | jakarta.security.auth.message.* | package | move_package | `import javax.security.auth.message.AuthStatus;` | `import jakarta.security.auth.message.AuthStatus;` | Includes callback, config, module subpackages; NOT javax.security.auth (Java SE) | Authentication |
| javax.security.enterprise.* | jakarta.security.enterprise.* | package | move_package | `import javax.security.enterprise.SecurityContext;` | `import jakarta.security.enterprise.SecurityContext;` | | Security |
| javax.security.jacc.* | jakarta.security.jacc.* | package | move_package | `import javax.security.jacc.PolicyContext;` | `import jakarta.security.jacc.PolicyContext;` | | Authorization |
| javax.servlet.* | jakarta.servlet.* | package | move_package | `import javax.servlet.ServletException;` | `import jakarta.servlet.ServletException;` | Includes annotation, descriptor, http, jsp, jsp.el, jsp.tagext, resources subpackages | Servlet |
| javax.transaction.* | jakarta.transaction.* | package | move_package | `import javax.transaction.Transactional;` | `import jakarta.transaction.Transactional;` | Excludes `javax.transaction.xa.*` (Java SE) | Transactions |
| javax.validation.* | jakarta.validation.* | package | move_package | `import javax.validation.Valid;` | `import jakarta.validation.Valid;` | Includes bootstrap, constraints, constraintvalidation, executable, groups, metadata, spi, valueextraction subpackages | Validation |
| javax.websocket.* | jakarta.websocket.* | package | move_package | `import javax.websocket.OnMessage;` | `import jakarta.websocket.OnMessage;` | Includes server subpackage | WebSocket |
| javax.ws.rs.* | jakarta.ws.rs.* | package | move_package | `import javax.ws.rs.GET;` | `import jakarta.ws.rs.GET;` | Includes client, container, core, ext, sse subpackages | JAX-RS |
| javax.xml.bind.* | jakarta.xml.bind.* | package | move_package | `import javax.xml.bind.JAXBContext;` | `import jakarta.xml.bind.JAXBContext;` | | XML Binding |
| javax.xml.soap.* | jakarta.xml.soap.* | package | move_package | `import javax.xml.soap.SOAPMessage;` | `import jakarta.xml.soap.SOAPMessage;` | | SOAP |
| javax.xml.ws.* | jakarta.xml.ws.* | package | move_package | `import javax.xml.ws.WebServiceRef;` | `import jakarta.xml.ws.WebServiceRef;` | Includes handler, handler.soap, http, soap, spi, spi.http, wsaddressing subpackages | XML Web Services |
| javax.enterprise.deploy.* | removed | package | remove | `import javax.enterprise.deploy.spi.DeploymentManager;` | (removed — Jakarta Deployment dropped from platform) | Entire API removed from Jakarta EE | Deployment |
| javax.xml.registry.* | removed | package | remove | `import javax.xml.registry.ConnectionFactory;` | (removed — Jakarta XML Registries dropped from platform) | Entire API removed from Jakarta EE | XML Registries |
| javax.xml.rpc.* | removed | package | remove | `import javax.xml.rpc.Service;` | (removed — Jakarta XML RPC dropped from platform) | Entire API removed from Jakarta EE; migrate to JAX-WS | XML RPC |
