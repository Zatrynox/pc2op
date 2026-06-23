# Project Sensibo Climate Control v2.0

Datos del alumno
- Nombre Victor Jhosef Laura Acosta
- Código u202418655
- NRC 11990
- Profesor Juan Anthonio Flores Moroco
- Proyecto `pc211990u202418655`
- Package raíz `com.sensibo.platform.u202418655`
- Puerto 8096
- Esquema BD `sensibo`

---

## Configuración inicial

### Base de datos

Abrir el `pgAdmin`, ingrese la contraseña `12345678` y crear la base de datos `sensibo`.

Ejecutar el siguiente SQL para crear el esquema

```sql
CREATE SCHEMA sensibo;
```

---

## Creación del proyecto (Spring Initializr)

Cargar el navegador y pegar el siguiente enlace

```
httpsstart.spring.io#!type=maven-project&language=java&platformVersion=4.0.6&packaging=jar&configurationFileFormat=properties&jvmVersion=25&groupId=com.sensibo.platform&artifactId=pc211990u202418655&packageName=com.sensibo.platform.u202418655&dependencies=data-jpa,validation,web,devtools,postgresql,lombok,springdoc-openapi
```

Generar el proyecto Spring. Guardarlo en la carpeta `IdeaProjects1ASI0729` y luego abrirlo en `IntelliJ IDEA`.

---

### Configuración del SDK

Hacer click en `File` → `Project Structure`. Seleccionar `Project` de `Project Settings` y seleccionar la versión `25` de Java para el `SDK`. Si no está instalado, descárguelo. Luego Seleccionar `SDKs` de `Platform Settings` y seleccionar la versión antes seleccionada. Hacer click en `OK`.

---

### Archivo pom.xml

Abrir `pom.xml` y modificar la versión de Java en `properties`

```xml
properties
    java.version25java.version
properties
```

Agregar la siguiente dependencia después de `springdoc-openapi`

```xml
!-- httpsmvnrepository.comartifactio.github.encryptorcodepluralize --
dependency
    groupIdio.github.encryptorcodegroupId
    artifactIdpluralizeartifactId
    version1.0.0version
dependency
```

Verificar que el `pom.xml` completo quede así

```xml
xml version=1.0 encoding=UTF-8
project xmlns=httpmaven.apache.orgPOM4.0.0
         xmlnsxsi=httpwww.w3.org2001XMLSchema-instance
         xsischemaLocation=httpmaven.apache.orgPOM4.0.0
         httpsmaven.apache.orgxsdmaven-4.0.0.xsd
    modelVersion4.0.0modelVersion
    parent
        groupIdorg.springframework.bootgroupId
        artifactIdspring-boot-starter-parentartifactId
        version4.0.6version
        relativePath
    parent
    groupIdcom.sensibo.platformgroupId
    artifactIdpc211990u202418655artifactId
    version0.0.1-SNAPSHOTversion
    namepc211990u202418655name
    descriptionSensibo Climate Control - Device Registration RESTful APIdescription
    url
    licenses
        license
    licenses
    developers
        developer
    developers
    scm
        connection
        developerConnection
        tag
        url
    scm
    properties
        java.version25java.version
    properties
    dependencies
        dependency
            groupIdorg.springframework.bootgroupId
            artifactIdspring-boot-starter-data-jpaartifactId
        dependency
        dependency
            groupIdorg.springframework.bootgroupId
            artifactIdspring-boot-starter-validationartifactId
        dependency
        dependency
            groupIdorg.springframework.bootgroupId
            artifactIdspring-boot-starter-webartifactId
        dependency
        dependency
            groupIdorg.springframework.bootgroupId
            artifactIdspring-boot-devtoolsartifactId
            scoperuntimescope
            optionaltrueoptional
        dependency
        dependency
            groupIdorg.postgresqlgroupId
            artifactIdpostgresqlartifactId
            scoperuntimescope
        dependency
        dependency
            groupIdorg.projectlombokgroupId
            artifactIdlombokartifactId
            optionaltrueoptional
        dependency
        dependency
            groupIdorg.springdocgroupId
            artifactIdspringdoc-openapi-starter-webmvc-uiartifactId
            version3.0.2version
        dependency
        !-- httpsmvnrepository.comartifactio.github.encryptorcodepluralize --
        dependency
            groupIdio.github.encryptorcodegroupId
            artifactIdpluralizeartifactId
            version1.0.0version
        dependency
        dependency
            groupIdorg.springframework.bootgroupId
            artifactIdspring-boot-starter-testartifactId
            scopetestscope
        dependency
    dependencies

    build
        plugins
            plugin
                groupIdorg.springframework.bootgroupId
                artifactIdspring-boot-maven-pluginartifactId
                configuration
                    excludes
                        exclude
                            groupIdorg.projectlombokgroupId
                            artifactIdlombokartifactId
                        exclude
                    excludes
                configuration
            plugin
            plugin
                groupIdorg.apache.maven.pluginsgroupId
                artifactIdmaven-compiler-pluginartifactId
                executions
                    execution
                        iddefault-compileid
                        phasecompilephase
                        goalsgoalcompilegoalgoals
                        configuration
                            annotationProcessorPaths
                                path
                                    groupIdorg.projectlombokgroupId
                                    artifactIdlombokartifactId
                                path
                            annotationProcessorPaths
                        configuration
                    execution
                    execution
                        iddefault-testCompileid
                        phasetest-compilephase
                        goalsgoaltestCompilegoalgoals
                        configuration
                            annotationProcessorPaths
                                path
                                    groupIdorg.projectlombokgroupId
                                    artifactIdlombokartifactId
                                path
                            annotationProcessorPaths
                        configuration
                    execution
                executions
            plugin
        plugins
    build
project
```

---

### Proyecto Application

Abrir el archivo `Pc211990u202418655Application.java` ubicado en el package `com.sensibo.platform.u202418655` y reemplazar con

```java
package com.sensibo.platform.u202418655;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;


  Main application class for the Sensibo Climate Control platform.
 
  pEnables JPA auditing for automatic management of entity creation
  and last-updated timestamps across all aggregate roots.p
 
  @author Victor Jhosef Laura Acosta
 
@SpringBootApplication
@EnableJpaAuditing
public class Pc211990u202418655Application {

    
      Entry point of the Spring Boot application.
     
      @param args command-line arguments
     
    public static void main(String[] args) {
        SpringApplication.run(Pc211990u202418655Application.class, args);
    }
}
```

---

### Archivo application.properties

Abrir el archivo `application.properties` en `srcmainresources` y reemplazar con

```ini
spring.application.name=pc211990u202418655

# Spring DataSource Configuration
spring.datasource.url jdbcpostgresqllocalhost5432sensibocurrentSchema=sensibo
spring.datasource.username postgres
spring.datasource.password 12345678
spring.datasource.driver-class-name org.postgresql.Driver

# Spring Data JPA Configuration
spring.jpa.database postgresql
spring.jpa.show-sql true

# Spring Data JPA Hibernate Configuration
spring.jpa.hibernate.ddl-auto update
spring.jpa.open-in-view=true
spring.jpa.properties.hibernate.format_sql true
spring.jpa.properties.hibernate.dialect org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.naming.physical-strategy=com.sensibo.platform.u202418655.shared.infrastructure.persistence.jpa.configuration.strategy.SnakeCaseWithPluralizedTablePhysicalNamingStrategy

server.port 8096

documentation.application.description=Sensibo Climate Control - Device Registration RESTful API
documentation.application.version=@project.version@

spring.messages.basename=messages
spring.messages.encoding=UTF-8
```

---

### Archivo messages.properties (i18n - Inglés)

Crear `srcmainresourcesmessages.properties`

```properties
# Validation messages
validation.field.prefix=Field
validation.request.failed=Request validation failed
validation.request.argument=request-argument

# Error messages
error.validation.message=Validation failed {0}
error.business-rule.message=Business rule violation {0}
error.unexpected.message=An unexpected error occurred
error.not-found.message={0} not found
error.conflict.message=Conflict {0}
error.generic.message=An error occurred
```

---

### Archivo messages_es.properties (i18n - Español)

Crear `srcmainresourcesmessages_es.properties`

```properties
# Validation messages
validation.field.prefix=Campo
validation.request.failed=Falló la validación de la solicitud
validation.request.argument=argumento-de-solicitud

# Error messages
error.validation.message=Error de validación {0}
error.business-rule.message=Violación de regla de negocio {0}
error.unexpected.message=Ocurrió un error inesperado
error.not-found.message={0} no encontrado
error.conflict.message=Conflicto {0}
error.generic.message=Ocurrió un error
```

---

### README.md

Crear `README.md` en la raíz del proyecto

```markdown
# Sensibo Climate Control - Device Registration API

## Description
Sensibo Climate Control Device Registration API is a RESTful web service built with Spring Boot
that provides backend support for registering new Sensibo devices in the platform.
The API allows registering devices such as Smart AC Controllers, Room Sensors, Air Quality Monitors,
and DoorWindow Sensors, enforcing all business rules including unique serial number and MAC address
validation.

## Technologies
- Java 25
- Spring Boot 4.0.6
- Spring Data JPA
- PostgreSQL
- Lombok
- OpenAPI (Swagger UI)

## Author
Victor Jhosef Laura Acosta

## License
Apache 2.0
```

---

## Shared Package - Estructura de carpetas

Crear toda la siguiente estructura bajo `srcmainjavacomsensiboplatformu202418655shared`

```
shared
├── application
│   └── result
│       ├── Result.java
│       └── ApplicationError.java
├── domain
│   └── model
│       └── aggregates
│           └── AbstractDomainAggregateRoot.java
├── infrastructure
│   ├── documentation
│   │   └── openapi
│   │       └── configuration
│   │           └── OpenApiConfiguration.java
│   ├── i18n
│   │   └── configuration
│   │       └── LocaleConfiguration.java
│   └── persistence
│       └── jpa
│           ├── configuration
│           │   └── strategy
│           │       └── SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java
│           └── entities
│               └── AuditableAbstractPersistenceEntity.java
└── interfaces
    └── rest
        ├── GlobalExceptionHandler.java
        ├── resources
        │   ├── ErrorResource.java
        │   └── MessageResource.java
        └── transform
            ├── ErrorResponseAssembler.java
            └── ResponseEntityAssembler.java
```

---

## Shared Package - Archivos

### 01. AbstractDomainAggregateRoot.java

Ruta `shareddomainmodelaggregatesAbstractDomainAggregateRoot.java`

Package `com.sensibo.platform.u202418655.shared.domain.model.aggregates`

```java
package com.sensibo.platform.u202418655.shared.domain.model.aggregates;

import org.springframework.data.domain.AbstractAggregateRoot;

import java.util.Collection;


  Base class for all domain aggregate roots.
 
  pExtends Spring Data Commons' {@link AbstractAggregateRoot} to gain
  domain event registration support without pulling in any JPA or
  persistence concern. Identity and auditing are intentionally left
  to the infrastructure layer.p
 
  pAll bounded-context domain aggregate roots should extend this class.p
 
  @param T the concrete aggregate root type
  @author Victor Jhosef Laura Acosta
 
public abstract class AbstractDomainAggregateRootT extends AbstractDomainAggregateRootT
        extends AbstractAggregateRootT {

    
      Registers a domain event to be published after this aggregate is saved.
     
      @param event the domain event to register
     
    protected void registerDomainEvent(Object event) {
        super.registerEvent(event);
    }

    
      Returns all domain events registered on this aggregate since the last publication.
     
      @return the registered domain events
     
    @Override
    public CollectionObject domainEvents() {
        return super.domainEvents();
    }

    
      Clears all registered domain events.
     
    @Override
    public void clearDomainEvents() {
        super.clearDomainEvents();
    }
}
```

---

### 02. Result.java

Ruta `sharedapplicationresultResult.java`

Package `com.sensibo.platform.u202418655.shared.application.result`

```java
package com.sensibo.platform.u202418655.shared.application.result;

import java.util.Optional;
import java.util.function.Function;


  Represents the result of a command or operation that can either succeed
  with a value or fail with an error.
 
  pThis type encodes the possibility of failure in the type system,
  making error cases explicit and composable.p
 
  @param T The type of the successful result value
  @param E The type of the errorfailure information
  @author Victor Jhosef Laura Acosta
 
public sealed interface ResultT, E {

    
      Represents a successful result containing a value.
     
    record SuccessT, E(T value) implements ResultT, E {}

    
      Represents a failed result containing error information.
     
    record FailureT, E(E error) implements ResultT, E {}

    
      Creates a successful result with the given value.
     
    static T, E ResultT, E success(T value) {
        return new Success(value);
    }

    
      Creates a failed result with the given error.
     
    static T, E ResultT, E failure(E error) {
        return new Failure(error);
    }

    
      Returns true if this result is a success.
     
    default boolean isSuccess() {
        return this instanceof Success;
    }

    
      Returns true if this result is a failure.
     
    default boolean isFailure() {
        return this instanceof Failure;
    }

    
      Returns an Optional containing the value if success, otherwise empty.
     
    default OptionalT toOptional() {
        return switch (this) {
            case SuccessT, E s - Optional.of(s.value);
            case FailureT, E f - Optional.empty();
        };
    }

    
      Extracts the value if successful, or returns the given default if failed.
     
    default T getOrElse(T defaultValue) {
        return switch (this) {
            case SuccessT, E s - s.value;
            case FailureT, E f - defaultValue;
        };
    }

    
      Applies a function to the error if this is a failure.
     
    default E2 ResultT, E2 mapError(FunctionE, E2 f) {
        return switch (this) {
            case SuccessT, E s - Result.success(s.value);
            case FailureT, E failure - Result.failure(f.apply(failure.error));
        };
    }

    
      Applies a function to the value if this is a success, producing a new Result.
     
    default T2 ResultT2, E flatMap(FunctionT, ResultT2, E f) {
        return switch (this) {
            case SuccessT, E s - f.apply(s.value);
            case FailureT, E failure - Result.failure(failure.error);
        };
    }

    
      Applies a function to the value if this is a success.
     
    default T2 ResultT2, E map(FunctionT, T2 f) {
        return switch (this) {
            case SuccessT, E s - Result.success(f.apply(s.value));
            case FailureT, E failure - Result.failure(failure.error);
        };
    }

    
      Applies a function to the error if this is a failure, allowing fallback recovery.
     
    default ResultT, E recover(FunctionE, ResultT, E f) {
        return switch (this) {
            case SuccessT, E s - this;
            case FailureT, E failure - f.apply(failure.error);
        };
    }
}
```

---

### 03. ApplicationError.java

Ruta `sharedapplicationresultApplicationError.java`

Package `com.sensibo.platform.u202418655.shared.application.result`

```java
package com.sensibo.platform.u202418655.shared.application.result;


  Represents an error that occurred in the application layer.
 
  pDesigned to be easily mapped to HTTP responses with structured
  error information.p
 
  @param code     A machine-readable error code (e.g., DEVICE_CONFLICT)
  @param message  A human-readable error message
  @param details  Optional additional context about the error
  @author Victor Jhosef Laura Acosta
 
public record ApplicationError(String code, String message, String details) {

    
      Creates an ApplicationError with code and message only.
     
    public ApplicationError(String code, String message) {
        this(code, message, null);
    }

    
      Validation error input data is invalid or violates constraints.
     
    public static ApplicationError validationError(String fieldOrConcept, String reason) {
        return new ApplicationError(VALIDATION_ERROR,
                Validation failed %s.formatted(fieldOrConcept), reason);
    }

    
      Not found error the requested resource does not exist.
     
    public static ApplicationError notFound(String resourceType, String identifier) {
        return new ApplicationError(%s_NOT_FOUND.formatted(resourceType.toUpperCase()),
                %s not found %s.formatted(resourceType, identifier), null);
    }

    
      Business rule violation error operation violates domain constraints.
     
    public static ApplicationError businessRuleViolation(String rule, String reason) {
        return new ApplicationError(BUSINESS_RULE_VIOLATION,
                Business rule violation %s.formatted(rule), reason);
    }

    
      Conflict error operation cannot be completed due to conflicting state.
     
    public static ApplicationError conflict(String resource, String reason) {
        return new ApplicationError(%s_CONFLICT.formatted(resource.toUpperCase()),
                Conflict with %s.formatted(resource), reason);
    }

    
      Unexpected error something went wrong that shouldn't have.
     
    public static ApplicationError unexpected(String context, String reason) {
        return new ApplicationError(UNEXPECTED_ERROR,
                Unexpected error in %s.formatted(context), reason);
    }
}
```

---

### 04. SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java

Ruta `sharedinfrastructurepersistencejpaconfigurationstrategySnakeCaseWithPluralizedTablePhysicalNamingStrategy.java`

Package `com.sensibo.platform.u202418655.shared.infrastructure.persistence.jpa.configuration.strategy`

```java
package com.sensibo.platform.u202418655.shared.infrastructure.persistence.jpa.configuration.strategy;

import org.hibernate.boot.model.naming.Identifier;
import org.hibernate.boot.model.naming.PhysicalNamingStrategy;
import org.hibernate.engine.jdbc.env.spi.JdbcEnvironment;

import static io.github.encryptorcode.pluralize.Pluralize.pluralize;


  Snake Case With Pluralized Table Physical Naming Strategy.
 
  p{@link PhysicalNamingStrategy} implementation that converts entity
  names to snake_case and table names to pluralized snake_case.p
 
  @since 1.0.0
  @author Victor Jhosef Laura Acosta
 
public class SnakeCaseWithPluralizedTablePhysicalNamingStrategy implements PhysicalNamingStrategy {

    @Override
    public Identifier toPhysicalCatalogName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
        return null;
    }

    @Override
    public Identifier toPhysicalSchemaName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
        return this.toSnakeCase(identifier);
    }

    @Override
    public Identifier toPhysicalTableName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
        return this.toSnakeCase(this.toPlural(identifier));
    }

    @Override
    public Identifier toPhysicalSequenceName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
        return this.toSnakeCase(identifier);
    }

    @Override
    public Identifier toPhysicalColumnName(Identifier identifier, JdbcEnvironment jdbcEnvironment) {
        return this.toSnakeCase(identifier);
    }

    
      Converts an identifier to snake_case.
     
    private Identifier toSnakeCase(final Identifier identifier) {
        if (identifier == null) return null;
        final String regex = ([a-z])([A-Z]);
        final String replacement = $1_$2;
        final String newName = identifier.getText()
                .replaceAll(regex, replacement)
                .toLowerCase();
        return Identifier.toIdentifier(newName);
    }

    
      Converts an identifier to its plural form.
     
    private Identifier toPlural(final Identifier identifier) {
        return Identifier.toIdentifier(pluralize(identifier.getText()));
    }
}
```

---

### 05. AuditableAbstractPersistenceEntity.java

Ruta `sharedinfrastructurepersistencejpaentitiesAuditableAbstractPersistenceEntity.java`

Package `com.sensibo.platform.u202418655.shared.infrastructure.persistence.jpa.entities`

```java
package com.sensibo.platform.u202418655.shared.infrastructure.persistence.jpa.entities;

import jakarta.persistence.;
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import java.util.Date;


  Base JPA persistence entity for all persistence entities that require auditing.
 
  pProvides {@code id}, {@code createdAt}, and {@code updatedAt} fields.
  This class intentionally lives in the infrastructure layer to keep JPA
  and Spring Data auditing concerns out of the domain model.p
 
  @author Victor Jhosef Laura Acosta
 
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class AuditableAbstractPersistenceEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @CreatedDate
    @Column(nullable = false, updatable = false)
    private Date createdAt;

    @LastModifiedDate
    @Column(nullable = false)
    private Date updatedAt;

    
      Sets the id for reconstruction from an existing domain object.
     
      @param id the persistence identity to assign
     
    public void setId(Long id) {
        this.id = id;
    }
}
```

---

### 06. OpenApiConfiguration.java

Ruta `sharedinfrastructuredocumentationopenapiconfigurationOpenApiConfiguration.java`

Package `com.sensibo.platform.u202418655.shared.infrastructure.documentation.openapi.configuration`

```java
package com.sensibo.platform.u202418655.shared.infrastructure.documentation.openapi.configuration;

import io.swagger.v3.oas.models.ExternalDocumentation;
import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Contact;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.License;
import io.swagger.v3.oas.models.servers.Server;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import java.util.List;


  Configures the OpenAPI specification exposed by the platform.
 
  @author Victor Jhosef Laura Acosta
 
@Configuration
public class OpenApiConfiguration {

    @Value(${spring.application.name})
    String applicationName;

    @Value(${documentation.application.description})
    String applicationDescription;

    @Value(${documentation.application.version})
    String applicationVersion;

    
      Builds the OpenAPI document used by Swagger UI and client generation tools.
     
      @return configured OpenAPI descriptor
     
    @Bean
    public OpenAPI sensiboPlatformOpenApi() {
        var openApi = new OpenAPI();
        openApi
                .info(new Info()
                        .title(this.applicationName)
                        .description(this.applicationDescription)
                        .version(this.applicationVersion)
                        .contact(new Contact()
                                .name(Sensibo Climate Control)
                                .email(support@sensibo.com)
                                .url(httpssensibo.comsupport))
                        .license(new License()
                                .name(Apache 2.0)
                                .url(httpswww.apache.orglicensesLICENSE-2.0.html)))
                .externalDocs(new ExternalDocumentation()
                        .description(Sensibo Climate Control Platform Wiki Documentation)
                        .url(httpssensibo.wiki.github.iodocs));

        openApi.servers(List.of(
                new Server().url(httplocalhost8096)
                        .description(Local Development Environment),
                new Server().url(httpsstaging-api.sensibo.com)
                        .description(Staging Environment),
                new Server().url(httpsapi.sensibo.com)
                        .description(Production Environment)
        ));
        return openApi;
    }
}
```

---

### 07. LocaleConfiguration.java

Ruta `sharedinfrastructurei18nconfigurationLocaleConfiguration.java`

Package `com.sensibo.platform.u202418655.shared.infrastructure.i18n.configuration`

```java
package com.sensibo.platform.u202418655.shared.infrastructure.i18n.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver;

import java.util.List;
import java.util.Locale;


  Configures locale resolution for REST requests.
 
  pUses the Accept-Language header to determine the response language.
  Supports English (default) and Spanish.p
 
  @author Victor Jhosef Laura Acosta
 
@Configuration
public class LocaleConfiguration {

    
      Creates a LocaleResolver that uses the Accept-Language header.
     
      @return the locale resolver instance
     
    @Bean
    public LocaleResolver localeResolver() {
        var resolver = new AcceptHeaderLocaleResolver();
        resolver.setDefaultLocale(Locale.ENGLISH);
        resolver.setSupportedLocales(List.of(Locale.ENGLISH, Locale.forLanguageTag(es)));
        return resolver;
    }
}
```

---

### 08. ErrorResource.java

Ruta `sharedinterfacesrestresourcesErrorResource.java`

Package `com.sensibo.platform.u202418655.shared.interfaces.rest.resources`

```java
package com.sensibo.platform.u202418655.shared.interfaces.rest.resources;

import com.fasterxml.jackson.annotation.JsonInclude;


  Standard error response resource returned from REST endpoints.
 
  @param code    A machine-readable error code
  @param message A human-readable error message
  @param details Optional additional context about the error
  @author Victor Jhosef Laura Acosta
 
@JsonInclude(JsonInclude.Include.NON_NULL)
public record ErrorResource(String code, String message, String details) {

    
      Creates an ErrorResource from code and message only (no details).
     
    public ErrorResource(String code, String message) {
        this(code, message, null);
    }
}
```

---

### 09. MessageResource.java

Ruta `sharedinterfacesrestresourcesMessageResource.java`

Package `com.sensibo.platform.u202418655.shared.interfaces.rest.resources`

```java
package com.sensibo.platform.u202418655.shared.interfaces.rest.resources;


  Resource used for simple success or informational REST responses.
 
  @param message A informational message
  @author Victor Jhosef Laura Acosta
 
public record MessageResource(String message) {}
```

---

### 10. ErrorResponseAssembler.java

Ruta `sharedinterfacesresttransformErrorResponseAssembler.java`

Package `com.sensibo.platform.u202418655.shared.interfaces.rest.transform`

```java
package com.sensibo.platform.u202418655.shared.interfaces.rest.transform;

import com.sensibo.platform.u202418655.shared.application.result.ApplicationError;
import com.sensibo.platform.u202418655.shared.interfaces.rest.resources.ErrorResource;
import org.springframework.http.HttpStatus;
import org.springframework.http.HttpStatusCode;
import org.springframework.http.ResponseEntity;


  Assembler for converting application errors to HTTP responses.
 
  @author Victor Jhosef Laura Acosta
 
public final class ErrorResponseAssembler {

    private ErrorResponseAssembler() {}

    
      Maps an ApplicationError to an appropriate HTTP ResponseEntity.
     
      @param error the ApplicationError to map
      @return a ResponseEntity with the appropriate HTTP status and error resource
     
    public static ResponseEntityErrorResource toErrorResponseFromApplicationError(ApplicationError error) {
        HttpStatusCode status = toStatusFromErrorCode(error.code());
        return new ResponseEntity(
                new ErrorResource(error.code(), error.message(), error.details()), status);
    }

    
      Determines the appropriate HTTP status code for a given error code.
     
      @param errorCode the error code string
      @return the corresponding HttpStatus
     
    public static HttpStatusCode toStatusFromErrorCode(String errorCode) {
        return switch (errorCode) {
            case VALIDATION_ERROR - HttpStatus.BAD_REQUEST;
            case String s when s.endsWith(_NOT_FOUND) - HttpStatus.NOT_FOUND;
            case BUSINESS_RULE_VIOLATION - HttpStatusCode.valueOf(422);
            case String s when s.endsWith(_CONFLICT) - HttpStatus.CONFLICT;
            case UNEXPECTED_ERROR - HttpStatus.INTERNAL_SERVER_ERROR;
            default - HttpStatus.INTERNAL_SERVER_ERROR;
        };
    }
}
```

---

### 11. ResponseEntityAssembler.java

Ruta `sharedinterfacesresttransformResponseEntityAssembler.java`

Package `com.sensibo.platform.u202418655.shared.interfaces.rest.transform`

```java
package com.sensibo.platform.u202418655.shared.interfaces.rest.transform;

import com.sensibo.platform.u202418655.shared.application.result.ApplicationError;
import com.sensibo.platform.u202418655.shared.application.result.Result;
import org.springframework.http.HttpStatusCode;
import org.springframework.http.ResponseEntity;

import java.util.function.Function;


  Assembler that translates application Result values into HTTP responses.
 
  @author Victor Jhosef Laura Acosta
 
public final class ResponseEntityAssembler {

    private ResponseEntityAssembler() {}

    
      Converts a Result into an HTTP response using the provided assembler.
     
      @param result                    the application result
      @param successResourceAssembler  function that maps success value to response resource
      @param successStatus             HTTP status to use for successful responses
      @param T                       success value type
      @param R                       success response resource type
      @return response entity for success or failure
     
    public static T, R ResponseEntity toResponseEntityFromResult(
            ResultT, ApplicationError result,
            FunctionT, R successResourceAssembler,
            HttpStatusCode successStatus
    ) {
        return switch (result) {
            case Result.SuccessT, ApplicationError success -
                    new ResponseEntity(successResourceAssembler.apply(success.value()), successStatus);
            case Result.FailureT, ApplicationError failure -
                    ErrorResponseAssembler.toErrorResponseFromApplicationError(failure.error());
        };
    }
}
```

---

### 12. GlobalExceptionHandler.java

Ruta `sharedinterfacesrestGlobalExceptionHandler.java`

Package `com.sensibo.platform.u202418655.shared.interfaces.rest`

```java
package com.sensibo.platform.u202418655.shared.interfaces.rest;

import com.sensibo.platform.u202418655.shared.application.result.ApplicationError;
import com.sensibo.platform.u202418655.shared.interfaces.rest.transform.ErrorResponseAssembler;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;


  Global exception handler for REST API.
 
  pProvides centralized exception handling ensuring all unhandled
  exceptions are translated to consistent HTTP responses.p
 
  @author Victor Jhosef Laura Acosta
 
@RestControllerAdvice
public class GlobalExceptionHandler {

    
      Handles validation exceptions from Spring's request body validation.
     
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity handleMethodArgumentNotValid(MethodArgumentNotValidException ex) {
        var fieldErrors = ex.getBindingResult().getFieldErrors();
        var errorDetails = fieldErrors.isEmpty()
                 Request validation failed
                 fieldErrors.stream()
                        .map(error - Field %s %s.formatted(
                                error.getField(), error.getDefaultMessage()))
                        .reduce((a, b) - a + ;  + b)
                        .orElse(Request validation failed);

        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.validationError(request-body, errorDetails));
    }

    
      Handles invalid request arguments such as malformed values.
     
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity handleIllegalArgumentException(IllegalArgumentException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.validationError(request-argument,
                        ex.getMessage() != null  ex.getMessage()  Invalid request argument));
    }

    
      Handles unexpected runtime exceptions.
     
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity handleRuntimeException(RuntimeException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.unexpected(global-exception-handler,
                        ex.getMessage() != null  ex.getMessage()  An unexpected error occurred));
    }

    
      Handles all other exceptions not matched by specific handlers.
     
    @ExceptionHandler(Exception.class)
    public ResponseEntity handleException(Exception ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.unexpected(global-exception-handler,
                        ex.getMessage() != null  ex.getMessage()  An unexpected error occurred));
    }
}
```

---

## Registry Bounded Context - Estructura de carpetas

Crear la siguiente estructura bajo `srcmainjavacomsensiboplatformu202418655registry`

```
registry
├── domain
│   ├── model
│   │   ├── aggregates
│   │   │   └── Device.java
│   │   ├── commands
│   │   │   └── CreateDeviceCommand.java
│   │   ├── queries
│   │   │   └── GetDeviceById.java
│   │   └── valueobjects
│   │       ├── DeviceTypes.java
│   │       ├── MacAddress.java
│   │       ├── SensiboUserId.java
│   │       ├── SerialNumber.java
│   │       └── Version.java
│   └── repositories
│       └── DeviceRepository.java
├── application
│   ├── commandservices
│   │   └── DeviceCommandService.java
│   ├── queryservices
│   │   └── DeviceQueryService.java
│   └── internal
│       ├── commandservices
│       │   └── DeviceCommandServiceImpl.java
│       └── queryservices
│           └── DeviceQueryServiceImpl.java
├── infrastructure
│   └── persistence
│       └── jpa
│           ├── adapters
│           │   └── DeviceRepositoryImpl.java
│           ├── assemblers
│           │   └── DevicePersistenceAssembler.java
│           ├── converters
│           │   ├── MacAddressPersistenceConverter.java
│           │   ├── SensiboUserIdPersistenceConverter.java
│           │   ├── SerialNumberPersistenceConverter.java
│           │   └── VersionPersistenceConverter.java
│           ├── entities
│           │   └── DevicePersistenceEntity.java
│           └── repositories
│               └── DevicePersistenceRepository.java
└── interfaces
    └── rest
        ├── DevicesController.java
        ├── resources
        │   ├── CreateDeviceResource.java
        │   └── DeviceResource.java
        └── transform
            ├── CreateDeviceCommandFromResourceAssembler.java
            └── DeviceResourceFromEntityAssembler.java
```

---

## Registry - Domain Layer (Value Objects)

### 13. DeviceTypes.java

Ruta `registrydomainmodelvalueobjectsDeviceTypes.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.valueobjects`

```java
package com.sensibo.platform.u202418655.registry.domain.model.valueobjects;

import java.util.Arrays;


  Enumeration representing the types of Sensibo devices.
 
  pDefines the available device types with their numeric identifiersp
  ul
    li1 - SMART_AC_CONTROLLERli
    li2 - ROOM_SENSORli
    li3 - AIR_QUALITY_MONITORli
    li4 - DOOR_WINDOW_SENSORli
  ul
 
  @author Victor Jhosef Laura Acosta
 
public enum DeviceTypes {
    SMART_AC_CONTROLLER(1),
    ROOM_SENSOR(2),
    AIR_QUALITY_MONITOR(3),
    DOOR_WINDOW_SENSOR(4);

    private final int id;

    DeviceTypes(int id) {
        this.id = id;
    }

    
      Returns the numeric identifier of this device type.
     
      @return the numeric id
     
    public int getId() {
        return id;
    }

    
      Returns the DeviceTypes from a given numeric id.
     
      @param id the numeric id
      @return the matching DeviceTypes
      @throws IllegalArgumentException if no type matches
     
    public static DeviceTypes fromValue(int id) {
        return Arrays.stream(DeviceTypes.values())
                .filter(dt - dt.id == id)
                .findFirst()
                .orElseThrow(() -
                        new IllegalArgumentException([DeviceTypes] Invalid value  + id));
    }

    
      Returns the DeviceTypes from a given name (case-insensitive).
     
      @param name the name of the device type
      @return the matching DeviceTypes
      @throws IllegalArgumentException if no type matches
     
    public static DeviceTypes fromString(String name) {
        return Arrays.stream(DeviceTypes.values())
                .filter(dt - dt.name().equalsIgnoreCase(name))
                .findFirst()
                .orElseThrow(() -
                        new IllegalArgumentException([DeviceTypes] Invalid string  + name));
    }
}
```

---

### 14. MacAddress.java

Ruta `registrydomainmodelvalueobjectsMacAddress.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.valueobjects`

```java
package com.sensibo.platform.u202418655.registry.domain.model.valueobjects;

import java.util.Objects;


  Value object representing a MAC address.
 
  pValidates that the MAC address follows the standard hexadecimal format
  {@code AABBCCDDEEFF}.p
 
  @param value the MAC address string
  @author Victor Jhosef Laura Acosta
 
public record MacAddress(String value) {

    private static final String MAC_REGEX = ^([0-9A-Fa-f]{2}){5}[0-9A-Fa-f]{2}$;

    
      Creates a MacAddress with validation.
     
      @throws IllegalArgumentException if value is null, blank, or invalid format
     
    public MacAddress {
        if (Objects.isNull(value)  value.isBlank())
            throw new IllegalArgumentException(
                    [MacAddress] value cannot be null or blank);
        if (!value.matches(MAC_REGEX))
            throw new IllegalArgumentException(
                    [MacAddress] Invalid MAC address format. Expected AABBCCDDEEFF);
    }

    
      Default constructor required for JPA.
     
    public MacAddress() {
        this(null);
    }
}
```

---

### 15. SensiboUserId.java

Ruta `registrydomainmodelvalueobjectsSensiboUserId.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.valueobjects`

```java
package com.sensibo.platform.u202418655.registry.domain.model.valueobjects;

import java.util.Objects;


  Value object representing a Sensibo user identifier.
 
  pContains a Long identifier that must not be null and must be greater than zero.p
 
  @param userId the user identifier value
  @author Victor Jhosef Laura Acosta
 
public record SensiboUserId(Long userId) {

    
      Creates a SensiboUserId with validation.
     
      @throws IllegalArgumentException if value is null or less than or equal to zero
     
    public SensiboUserId {
        if (Objects.isNull(userId))
            throw new IllegalArgumentException([SensiboUserId] value cannot be null);
        if (userId = 0)
            throw new IllegalArgumentException([SensiboUserId] value must be greater than zero);
    }

    
      Default constructor required for JPA.
     
    public SensiboUserId() {
        this(null);
    }
}
```

---

### 16. SerialNumber.java

Ruta `registrydomainmodelvalueobjectsSerialNumber.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.valueobjects`

```java
package com.sensibo.platform.u202418655.registry.domain.model.valueobjects;

import java.util.Objects;
import java.util.UUID;


  Value object representing a device serial number.
 
  pEncapsulates a UUID-based serial number. When no value is provided,
  a random UUID is auto-generated.p
 
  @param value the serial number as a UUID string
  @author Victor Jhosef Laura Acosta
 
public record SerialNumber(String value) {

    
      Creates a SerialNumber with validation.
     
      @throws IllegalArgumentException if value is null, blank, not 36 chars, or not a valid UUID
     
    public SerialNumber {
        if (Objects.isNull(value)  value.isBlank())
            throw new IllegalArgumentException([SerialNumber] value cannot be null or blank);
        if (value.length() != 36)
            throw new IllegalArgumentException([SerialNumber] must be 36 characters long);
        if (!value.matches([a-f0-9]{8}-([a-f0-9]{4}-){3}[a-f0-9]{12}))
            throw new IllegalArgumentException([SerialNumber] must be a valid UUID);
    }

    
      Auto-generates a random UUID serial number.
     
    public SerialNumber() {
        this(UUID.randomUUID().toString());
    }
}
```

---

### 17. Version.java

Ruta `registrydomainmodelvalueobjectsVersion.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.valueobjects`

```java
package com.sensibo.platform.u202418655.registry.domain.model.valueobjects;

import java.util.Objects;


  Value object representing a firmware version.
 
  pValidates that the version follows the {@code X.Y.Z} format.p
 
  @param value the version string
  @author Victor Jhosef Laura Acosta
 
public record Version(String value) {

    private static final String VERSION_REGEX = ^d+.d+.d+$;

    
      Creates a Version with validation.
     
      @throws IllegalArgumentException if value is null, blank, or not in X.Y.Z format
     
    public Version {
        if (Objects.isNull(value)  value.isBlank())
            throw new IllegalArgumentException([Version] value cannot be null or blank);
        if (!value.matches(VERSION_REGEX))
            throw new IllegalArgumentException([Version] Invalid version format. Expected X.Y.Z);
    }

    
      Default constructor required for JPA.
     
    public Version() {
        this(null);
    }
}
```

---

## Registry - Domain Layer (Aggregate Root)

### 18. Device.java

Ruta `registrydomainmodelaggregatesDevice.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.aggregates`

```java
package com.sensibo.platform.u202418655.registry.domain.model.aggregates;

import com.sensibo.platform.u202418655.registry.domain.model.commands.CreateDeviceCommand;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.;
import com.sensibo.platform.u202418655.shared.domain.model.aggregates.AbstractDomainAggregateRoot;
import lombok.Getter;
import lombok.Setter;

import java.time.LocalDate;


  Aggregate root representing a Sensibo device.
 
  pA Device represents a physical Sensibo device registered in the platform,
  such as an AC controller, room sensor, air quality monitor, or doorwindow sensor.p
 
  @author Victor Jhosef Laura Acosta
 
@Getter
@Setter
public class Device extends AbstractDomainAggregateRootDevice {

    private Long id;
    private SensiboUserId userId;
    private DeviceTypes deviceType;
    private String modelName;
    private SerialNumber serialNumber;
    private MacAddress macAddress;
    private Version firmwareVersion;
    private LocalDate installationDate;
    private String roomLocation;

    
      Default constructor required by JPA.
     
    public Device() {}

    
      Creates a new Device from the given command.
     
      @param command the create device command
     
    public Device(CreateDeviceCommand command) {
        this.userId = new SensiboUserId(command.userId());
        this.deviceType = DeviceTypes.fromString(command.deviceType());
        this.modelName = command.modelName();
        this.serialNumber = new SerialNumber(command.serialNumber());
        this.macAddress = new MacAddress(command.macAddress());
        this.firmwareVersion = new Version(command.firmwareVersion());
        this.installationDate = command.installationDate();
        this.roomLocation = command.roomLocation();
    }
}
```

---

## Registry - Domain Layer (Commands)

### 19. CreateDeviceCommand.java

Ruta `registrydomainmodelcommandsCreateDeviceCommand.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.commands`

```java
package com.sensibo.platform.u202418655.registry.domain.model.commands;

import java.time.LocalDate;


  Command to create a new device.
 
  pEncapsulates all data required to create a Device aggregate.
  Validation of mandatory fields is performed in the constructor.p
 
  @param userId           the Sensibo user identifier
  @param deviceType       the device type name
  @param modelName        the model name
  @param serialNumber     the serial number (UUID, auto-generated)
  @param macAddress       the MAC address
  @param firmwareVersion  the firmware version
  @param installationDate the installation date
  @param roomLocation     the room location
  @author Victor Jhosef Laura Acosta
 
public record CreateDeviceCommand(
        Long userId, String deviceType, String modelName, String serialNumber,
        String macAddress, String firmwareVersion, LocalDate installationDate,
        String roomLocation) {

    
      Validates that all required fields are present.
     
      @throws IllegalArgumentException if any required field is null
     
    public CreateDeviceCommand {
        if (userId == null)
            throw new IllegalArgumentException(userId cannot be null);
        if (deviceType == null)
            throw new IllegalArgumentException(deviceType cannot be null);
        if (modelName == null)
            throw new IllegalArgumentException(modelName cannot be null);
        if (serialNumber == null)
            throw new IllegalArgumentException(serialNumber cannot be null);
        if (firmwareVersion == null)
            throw new IllegalArgumentException(firmwareVersion cannot be null);
        if (installationDate == null)
            throw new IllegalArgumentException(installationDate cannot be null);
        if (roomLocation == null)
            throw new IllegalArgumentException(roomLocation cannot be null);
    }
}
```

---

## Registry - Domain Layer (Queries)

### 20. GetDeviceById.java

Ruta `registrydomainmodelqueriesGetDeviceById.java`

Package `com.sensibo.platform.u202418655.registry.domain.model.queries`

```java
package com.sensibo.platform.u202418655.registry.domain.model.queries;


  Query to retrieve a device by its identifier.
 
  @param deviceId the unique identifier of the device
  @author Victor Jhosef Laura Acosta
 
public record GetDeviceById(Long deviceId) {

    
      Validates the query parameters.
     
      @throws IllegalArgumentException if deviceId is null or negative
     
    public GetDeviceById {
        if (deviceId == null)
            throw new IllegalArgumentException(deviceId cannot be null);
        if (deviceId  0)
            throw new IllegalArgumentException(deviceId cannot be negative);
    }
}
```

---

## Registry - Domain Layer (Repository)

### 21. DeviceRepository.java

Ruta `registrydomainrepositoriesDeviceRepository.java`

Package `com.sensibo.platform.u202418655.registry.domain.repositories`

```java
package com.sensibo.platform.u202418655.registry.domain.repositories;

import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.MacAddress;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SerialNumber;

import java.util.Optional;


  Repository interface for Device aggregate root.
 
  pDefines the contract for persisting and retrieving devices.
  Implementations are provided in the infrastructure layer.p
 
  @author Victor Jhosef Laura Acosta
 
public interface DeviceRepository {

    
      Saves a device.
     
      @param device the device to save
      @return the saved device
     
    Device save(Device device);

    
      Finds a device by its identifier.
     
      @param deviceId the device identifier
      @return an Optional containing the device if found
     
    OptionalDevice findById(Long deviceId);

    
      Checks if a device already exists with the given serial number and MAC address.
     
      @param serialNumber the serial number
      @param macAddress   the MAC address
      @return true if a device with that combination exists
     
    boolean existsBySerialNumberAndMacAddress(SerialNumber serialNumber, MacAddress macAddress);
}
```

---

## Registry - Application Layer (Service Interfaces)

### 22. DeviceCommandService.java

Ruta `registryapplicationcommandservicesDeviceCommandService.java`

Package `com.sensibo.platform.u202418655.registry.application.commandservices`

```java
package com.sensibo.platform.u202418655.registry.application.commandservices;

import com.sensibo.platform.u202418655.registry.domain.model.commands.CreateDeviceCommand;
import com.sensibo.platform.u202418655.shared.application.result.ApplicationError;
import com.sensibo.platform.u202418655.shared.application.result.Result;


  Service interface for handling device commands.
 
  @author Victor Jhosef Laura Acosta
 
public interface DeviceCommandService {

    
      Handles the creation of a new device.
     
      @param command the create device command
      @return a Result containing the ID of the created device on success, or an error on failure
     
    ResultLong, ApplicationError handle(CreateDeviceCommand command);
}
```

---

### 23. DeviceQueryService.java

Ruta `registryapplicationqueryservicesDeviceQueryService.java`

Package `com.sensibo.platform.u202418655.registry.application.queryservices`

```java
package com.sensibo.platform.u202418655.registry.application.queryservices;

import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.domain.model.queries.GetDeviceById;

import java.util.Optional;


  Service interface for handling device queries.
 
  @author Victor Jhosef Laura Acosta
 
public interface DeviceQueryService {

    
      Handles the retrieval of a device by its identifier.
     
      @param query the get device by id query
      @return an Optional containing the device if found
     
    OptionalDevice handle(GetDeviceById query);
}
```

---

## Registry - Application Layer (Service Implementations)

### 24. DeviceCommandServiceImpl.java

Ruta `registryapplicationinternalcommandservicesDeviceCommandServiceImpl.java`

Package `com.sensibo.platform.u202418655.registry.application.internal.commandservices`

```java
package com.sensibo.platform.u202418655.registry.application.internal.commandservices;

import com.sensibo.platform.u202418655.registry.application.commandservices.DeviceCommandService;
import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.domain.model.commands.CreateDeviceCommand;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.MacAddress;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SerialNumber;
import com.sensibo.platform.u202418655.registry.domain.repositories.DeviceRepository;
import com.sensibo.platform.u202418655.shared.application.result.ApplicationError;
import com.sensibo.platform.u202418655.shared.application.result.Result;
import org.springframework.stereotype.Service;


  Implementation of {@link DeviceCommandService}.
 
  pBusiness rules enforcedp
  ul
    liNo duplicate serialNumber + macAddress is allowed.li
  ul
 
  @author Victor Jhosef Laura Acosta
 
@Service
public class DeviceCommandServiceImpl implements DeviceCommandService {

    private final DeviceRepository deviceRepository;

    
      Constructor for dependency injection.
     
      @param deviceRepository the device repository
     
    public DeviceCommandServiceImpl(DeviceRepository deviceRepository) {
        this.deviceRepository = deviceRepository;
    }

    
      Creates a new device after validating all business rules.
     
      @param command the create device command
      @return a Result containing the device ID on success, or an error on failure
     
    @Override
    public ResultLong, ApplicationError handle(CreateDeviceCommand command) {

         BUSINESS RULE No duplicate serialNumber + macAddress
        if (this.deviceRepository.existsBySerialNumberAndMacAddress(
                new SerialNumber(command.serialNumber()),
                new MacAddress(command.macAddress()))) {
            return Result.failure(ApplicationError.conflict(Device,
                    A device with the same serialNumber and macAddress already exists));
        }

        var device = new Device(command);
        try {
            device = deviceRepository.save(device);
        } catch (Exception e) {
            return Result.failure(
                    ApplicationError.unexpected(create-device, e.getMessage()));
        }
        return Result.success(device.getId());
    }
}
```

---

### 25. DeviceQueryServiceImpl.java

Ruta `registryapplicationinternalqueryservicesDeviceQueryServiceImpl.java`

Package `com.sensibo.platform.u202418655.registry.application.internal.queryservices`

```java
package com.sensibo.platform.u202418655.registry.application.internal.queryservices;

import com.sensibo.platform.u202418655.registry.application.queryservices.DeviceQueryService;
import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.domain.model.queries.GetDeviceById;
import com.sensibo.platform.u202418655.registry.domain.repositories.DeviceRepository;
import org.springframework.stereotype.Service;

import java.util.Optional;


  Implementation of {@link DeviceQueryService}.
 
  @author Victor Jhosef Laura Acosta
 
@Service
public class DeviceQueryServiceImpl implements DeviceQueryService {

    private final DeviceRepository deviceRepository;

    
      Constructor for dependency injection.
     
      @param deviceRepository the device repository
     
    public DeviceQueryServiceImpl(DeviceRepository deviceRepository) {
        this.deviceRepository = deviceRepository;
    }

    
      Retrieves a device by its identifier.
     
      @param query the get device by id query
      @return an Optional containing the device if found
     
    @Override
    public OptionalDevice handle(GetDeviceById query) {
        return this.deviceRepository.findById(query.deviceId());
    }
}
```

---

## Registry - Infrastructure Layer (JPA Converters)

### 26. MacAddressPersistenceConverter.java

Ruta `registryinfrastructurepersistencejpaconvertersMacAddressPersistenceConverter.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters;

import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.MacAddress;
import jakarta.persistence.AttributeConverter;
import jakarta.persistence.Converter;


  JPA attribute converter for {@link MacAddress}.
 
  pConverts between MacAddress (domain) and String (database column).p
 
  @author Victor Jhosef Laura Acosta
 
@Converter(autoApply = true)
public class MacAddressPersistenceConverter implements AttributeConverterMacAddress, String {

    @Override
    public String convertToDatabaseColumn(MacAddress attribute) {
        return attribute == null  null  attribute.value();
    }

    @Override
    public MacAddress convertToEntityAttribute(String dbData) {
        return dbData == null  null  new MacAddress(dbData);
    }
}
```

---

### 27. SensiboUserIdPersistenceConverter.java

Ruta `registryinfrastructurepersistencejpaconvertersSensiboUserIdPersistenceConverter.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters;

import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SensiboUserId;
import jakarta.persistence.AttributeConverter;
import jakarta.persistence.Converter;


  JPA attribute converter for {@link SensiboUserId}.
 
  pConverts between SensiboUserId (domain) and Long (database column).p
 
  @author Victor Jhosef Laura Acosta
 
@Converter(autoApply = true)
public class SensiboUserIdPersistenceConverter implements AttributeConverterSensiboUserId, Long {

    @Override
    public Long convertToDatabaseColumn(SensiboUserId attribute) {
        return attribute == null  null  attribute.userId();
    }

    @Override
    public SensiboUserId convertToEntityAttribute(Long dbData) {
        return dbData == null  null  new SensiboUserId(dbData);
    }
}
```

---

### 28. SerialNumberPersistenceConverter.java

Ruta `registryinfrastructurepersistencejpaconvertersSerialNumberPersistenceConverter.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters;

import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SerialNumber;
import jakarta.persistence.AttributeConverter;
import jakarta.persistence.Converter;


  JPA attribute converter for {@link SerialNumber}.
 
  pConverts between SerialNumber (domain) and String (database column).p
 
  @author Victor Jhosef Laura Acosta
 
@Converter(autoApply = true)
public class SerialNumberPersistenceConverter implements AttributeConverterSerialNumber, String {

    @Override
    public String convertToDatabaseColumn(SerialNumber attribute) {
        return attribute == null  null  attribute.value();
    }

    @Override
    public SerialNumber convertToEntityAttribute(String dbData) {
        return dbData == null  null  new SerialNumber(dbData);
    }
}
```

---

### 29. VersionPersistenceConverter.java

Ruta `registryinfrastructurepersistencejpaconvertersVersionPersistenceConverter.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters;

import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.Version;
import jakarta.persistence.AttributeConverter;
import jakarta.persistence.Converter;


  JPA attribute converter for {@link Version}.
 
  pConverts between Version (domain) and String (database column).p
 
  @author Victor Jhosef Laura Acosta
 
@Converter(autoApply = true)
public class VersionPersistenceConverter implements AttributeConverterVersion, String {

    @Override
    public String convertToDatabaseColumn(Version attribute) {
        return attribute == null  null  attribute.value();
    }

    @Override
    public Version convertToEntityAttribute(String dbData) {
        return dbData == null  null  new Version(dbData);
    }
}
```

---

## Registry - Infrastructure Layer (JPA Entity)

### 30. DevicePersistenceEntity.java

Ruta `registryinfrastructurepersistencejpaentitiesDevicePersistenceEntity.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.entities`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.entities;

import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.;
import com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.converters.;
import com.sensibo.platform.u202418655.shared.infrastructure.persistence.jpa.entities.AuditableAbstractPersistenceEntity;
import jakarta.persistence.;
import lombok.Getter;
import lombok.Setter;

import java.time.LocalDate;


  JPA persistence entity for Device.
 
  pMaps the Device aggregate root to the database table.p
 
  @author Victor Jhosef Laura Acosta
 
@Entity
@Table(name = devices, schema = sensibo)
@Getter
@Setter
public class DevicePersistenceEntity extends AuditableAbstractPersistenceEntity {

    @Convert(converter = SensiboUserIdPersistenceConverter.class)
    @Column(name = user_id, nullable = false)
    private SensiboUserId userId;

    @Enumerated(EnumType.ORDINAL)
    @Column(name = device_type, nullable = false)
    private DeviceTypes deviceType;

    @Column(name = model_name, length = 50, nullable = false)
    private String modelName;

    @Convert(converter = SerialNumberPersistenceConverter.class)
    @Column(name = serial_number, nullable = false, unique = true)
    private SerialNumber serialNumber;

    @Convert(converter = MacAddressPersistenceConverter.class)
    @Column(name = mac_address, nullable = false)
    private MacAddress macAddress;

    @Convert(converter = VersionPersistenceConverter.class)
    @Column(name = firmware_version, nullable = false)
    private Version firmwareVersion;

    @Temporal(TemporalType.DATE)
    @Column(name = installation_date, nullable = false)
    private LocalDate installationDate;

    @Column(name = room_location, nullable = false)
    private String roomLocation;
}
```

---

## Registry - Infrastructure Layer (JPA Repository)

### 31. DevicePersistenceRepository.java

Ruta `registryinfrastructurepersistencejparepositoriesDevicePersistenceRepository.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.repositories`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.repositories;

import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.MacAddress;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SerialNumber;
import com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.entities.DevicePersistenceEntity;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;


  Spring Data JPA repository for DevicePersistenceEntity.
 
  @author Victor Jhosef Laura Acosta
 
@Repository
public interface DevicePersistenceRepository extends JpaRepositoryDevicePersistenceEntity, Long {

    
      Checks if a device exists with the given serial number and MAC address.
     
      @param serialNumber the serial number
      @param macAddress   the MAC address
      @return true if a matching device exists
     
    boolean existsBySerialNumberAndMacAddress(SerialNumber serialNumber, MacAddress macAddress);
}
```

---

## Registry - Infrastructure Layer (Assembler)

### 32. DevicePersistenceAssembler.java

Ruta `registryinfrastructurepersistencejpaassemblersDevicePersistenceAssembler.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.assemblers`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.assemblers;

import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.entities.DevicePersistenceEntity;


  Assembler for converting between Device (domain) and DevicePersistenceEntity (JPA).
 
  @author Victor Jhosef Laura Acosta
 
public final class DevicePersistenceAssembler {

    private DevicePersistenceAssembler() {}

    
      Converts a domain Device to a JPA persistence entity.
     
      @param device the domain aggregate
      @return the JPA persistence entity
     
    public static DevicePersistenceEntity toPersistenceFromDomain(Device device) {
        DevicePersistenceEntity entity = new DevicePersistenceEntity();
        entity.setId(device.getId());
        entity.setUserId(device.getUserId());
        entity.setDeviceType(device.getDeviceType());
        entity.setModelName(device.getModelName());
        entity.setSerialNumber(device.getSerialNumber());
        entity.setMacAddress(device.getMacAddress());
        entity.setFirmwareVersion(device.getFirmwareVersion());
        entity.setInstallationDate(device.getInstallationDate());
        entity.setRoomLocation(device.getRoomLocation());
        return entity;
    }

    
      Converts a JPA persistence entity to a domain Device.
     
      @param entity the JPA persistence entity
      @return the domain aggregate
     
    public static Device toDomainFromPersistence(DevicePersistenceEntity entity) {
        Device device = new Device();
        device.setId(entity.getId());
        device.setUserId(entity.getUserId());
        device.setDeviceType(entity.getDeviceType());
        device.setModelName(entity.getModelName());
        device.setSerialNumber(entity.getSerialNumber());
        device.setMacAddress(entity.getMacAddress());
        device.setFirmwareVersion(entity.getFirmwareVersion());
        device.setInstallationDate(entity.getInstallationDate());
        device.setRoomLocation(entity.getRoomLocation());
        return device;
    }
}
```

---

## Registry - Infrastructure Layer (Adapter)

### 33. DeviceRepositoryImpl.java

Ruta `registryinfrastructurepersistencejpaadaptersDeviceRepositoryImpl.java`

Package `com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.adapters`

```java
package com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.adapters;

import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.MacAddress;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SerialNumber;
import com.sensibo.platform.u202418655.registry.domain.repositories.DeviceRepository;
import com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.assemblers.DevicePersistenceAssembler;
import com.sensibo.platform.u202418655.registry.infrastructure.persistence.jpa.repositories.DevicePersistenceRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;


  Implementation of {@link DeviceRepository} using JPA.
 
  pActs as an adapter between the domain repository interface and
  Spring Data JPA persistence.p
 
  @author Victor Jhosef Laura Acosta
 
@Repository
public class DeviceRepositoryImpl implements DeviceRepository {

    private final DevicePersistenceRepository devicePersistenceRepository;

    
      Constructor for dependency injection.
     
      @param devicePersistenceRepository the JPA persistence repository
     
    public DeviceRepositoryImpl(DevicePersistenceRepository devicePersistenceRepository) {
        this.devicePersistenceRepository = devicePersistenceRepository;
    }

    
      Saves a device by converting to persistence entity, saving, and converting back.
     
    @Override
    public Device save(Device device) {
        var saved = this.devicePersistenceRepository
                .save(DevicePersistenceAssembler.toPersistenceFromDomain(device));
        return DevicePersistenceAssembler.toDomainFromPersistence(saved);
    }

    
      Finds a device by its identifier.
     
    @Override
    public OptionalDevice findById(Long deviceId) {
        return this.devicePersistenceRepository.findById(deviceId)
                .map(DevicePersistenceAssemblertoDomainFromPersistence);
    }

    
      Checks if a device exists with the given serial number and MAC address.
     
    @Override
    public boolean existsBySerialNumberAndMacAddress(SerialNumber serialNumber, MacAddress macAddress) {
        return this.devicePersistenceRepository
                .existsBySerialNumberAndMacAddress(serialNumber, macAddress);
    }
}
```

---

## Registry - Interfaces Layer (Resources)

### 34. CreateDeviceResource.java

Ruta `registryinterfacesrestresourcesCreateDeviceResource.java`

Package `com.sensibo.platform.u202418655.registry.interfaces.rest.resources`

```java
package com.sensibo.platform.u202418655.registry.interfaces.rest.resources;

import java.time.LocalDate;


  REST resource for creating a new device.
 
  pNote that id and serialNumber are not included here,
  as they are auto-generated when storing the information.p
 
  @param userId           the Sensibo user identifier
  @param deviceType       the device type name
  @param modelName        the model name
  @param macAddress       the MAC address (format AABBCCDDEEFF)
  @param firmwareVersion  the firmware version (format X.Y.Z)
  @param installationDate the installation date
  @param roomLocation     the room location
  @author Victor Jhosef Laura Acosta
 
public record CreateDeviceResource(
        Long userId, String deviceType, String modelName,
        String macAddress, String firmwareVersion, LocalDate installationDate,
        String roomLocation) {

    
      Validates the resource fields at the boundary.
     
      @throws IllegalArgumentException if any field violates constraints
     
    public CreateDeviceResource {
        if (userId == null  userId  0)
            throw new IllegalArgumentException(userId cannot be null or negative);
        if (deviceType == null  deviceType.isBlank())
            throw new IllegalArgumentException(deviceType cannot be null or blank);
        if (modelName == null  modelName.isBlank())
            throw new IllegalArgumentException(modelName cannot be null or blank);
        if (macAddress == null  macAddress.isBlank())
            throw new IllegalArgumentException(macAddress cannot be null or blank);
        if (firmwareVersion == null  firmwareVersion.isBlank())
            throw new IllegalArgumentException(firmwareVersion cannot be null or blank);
        if (installationDate == null)
            throw new IllegalArgumentException(installationDate cannot be null);
        if (roomLocation == null  roomLocation.isBlank())
            throw new IllegalArgumentException(roomLocation cannot be null or blank);
    }
}
```

---

### 35. DeviceResource.java

Ruta `registryinterfacesrestresourcesDeviceResource.java`

Package `com.sensibo.platform.u202418655.registry.interfaces.rest.resources`

```java
package com.sensibo.platform.u202418655.registry.interfaces.rest.resources;

import java.time.LocalDate;


  REST resource representing a device response.
 
  pIncludes id, serialNumber (as String), deviceType (as String),
  modelName, macAddress (as String), firmwareVersion (as String),
  installationDate, roomLocation.p
 
  @param id              the device identifier
  @param serialNumber    the serial number (UUID string)
  @param deviceType      the device type name
  @param modelName       the model name
  @param macAddress      the MAC address
  @param firmwareVersion the firmware version
  @param installationDate the installation date
  @param roomLocation    the room location
  @author Victor Jhosef Laura Acosta
 
public record DeviceResource(
        Long id, String serialNumber, String deviceType, String modelName,
        String macAddress, String firmwareVersion, LocalDate installationDate,
        String roomLocation) {
}
```

---

## Registry - Interfaces Layer (Assemblers)

### 36. CreateDeviceCommandFromResourceAssembler.java

Ruta `registryinterfacesresttransformCreateDeviceCommandFromResourceAssembler.java`

Package `com.sensibo.platform.u202418655.registry.interfaces.rest.transform`

```java
package com.sensibo.platform.u202418655.registry.interfaces.rest.transform;

import com.sensibo.platform.u202418655.registry.domain.model.commands.CreateDeviceCommand;
import com.sensibo.platform.u202418655.registry.domain.model.valueobjects.SerialNumber;
import com.sensibo.platform.u202418655.registry.interfaces.rest.resources.CreateDeviceResource;


  Assembler for converting a {@link CreateDeviceResource} to a
  {@link CreateDeviceCommand}.
 
  pThe serialNumber is auto-generated during assembly.p
 
  @author Victor Jhosef Laura Acosta
 
public final class CreateDeviceCommandFromResourceAssembler {

    private CreateDeviceCommandFromResourceAssembler() {}

    
      Converts a REST resource to a domain command, auto-generating the serialNumber.
     
      @param resource the REST resource from the request body
      @return the create device command
     
    public static CreateDeviceCommand toCommandFromResource(CreateDeviceResource resource) {
        return new CreateDeviceCommand(
                resource.userId(), resource.deviceType(), resource.modelName(),
                new SerialNumber().value(),  auto-generated UUID
                resource.macAddress(), resource.firmwareVersion(),
                resource.installationDate(), resource.roomLocation());
    }
}
```

---

### 37. DeviceResourceFromEntityAssembler.java

Ruta `registryinterfacesresttransformDeviceResourceFromEntityAssembler.java`

Package `com.sensibo.platform.u202418655.registry.interfaces.rest.transform`

```java
package com.sensibo.platform.u202418655.registry.interfaces.rest.transform;

import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.interfaces.rest.resources.DeviceResource;


  Assembler for converting a {@link Device} domain entity to a
  {@link DeviceResource}.
 
  @author Victor Jhosef Laura Acosta
 
public final class DeviceResourceFromEntityAssembler {

    private DeviceResourceFromEntityAssembler() {}

    
      Converts a domain Device to a REST resource.
     
      @param entity the domain aggregate
      @return the REST resource
     
    public static DeviceResource toResourceFromEntity(Device entity) {
        return new DeviceResource(
                entity.getId(),
                entity.getSerialNumber().value(),
                entity.getDeviceType().name(),
                entity.getModelName(),
                entity.getMacAddress().value(),
                entity.getFirmwareVersion().value(),
                entity.getInstallationDate(),
                entity.getRoomLocation());
    }
}
```

---

## Registry - Interfaces Layer (Controller con OpenAPI - Punto 12 de la rúbrica)

### 38. DevicesController.java

Ruta `registryinterfacesrestDevicesController.java`

Package `com.sensibo.platform.u202418655.registry.interfaces.rest`

```java
package com.sensibo.platform.u202418655.registry.interfaces.rest;

import com.sensibo.platform.u202418655.registry.application.commandservices.DeviceCommandService;
import com.sensibo.platform.u202418655.registry.application.queryservices.DeviceQueryService;
import com.sensibo.platform.u202418655.registry.domain.model.aggregates.Device;
import com.sensibo.platform.u202418655.registry.domain.model.queries.GetDeviceById;
import com.sensibo.platform.u202418655.registry.interfaces.rest.resources.CreateDeviceResource;
import com.sensibo.platform.u202418655.registry.interfaces.rest.resources.DeviceResource;
import com.sensibo.platform.u202418655.registry.interfaces.rest.transform.CreateDeviceCommandFromResourceAssembler;
import com.sensibo.platform.u202418655.registry.interfaces.rest.transform.DeviceResourceFromEntityAssembler;
import com.sensibo.platform.u202418655.shared.application.result.ApplicationError;
import com.sensibo.platform.u202418655.shared.application.result.Result;
import com.sensibo.platform.u202418655.shared.interfaces.rest.transform.ResponseEntityAssembler;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.;


  REST controller for device operations.
 
  pExposes endpoints for managing devices under the
  {@code apiv1devices} base path.p
 
  @author Victor Jhosef Laura Acosta
 
@RestController
@RequestMapping(value = apiv1devices)
@Tag(name = Devices, description = Operations related to device management in the Sensibo platform)
public class DevicesController {

    private final DeviceCommandService deviceCommandService;
    private final DeviceQueryService deviceQueryService;

    
      Constructor for dependency injection.
     
      @param deviceCommandService the command service
      @param deviceQueryService   the query service
     
    public DevicesController(
            DeviceCommandService deviceCommandService,
            DeviceQueryService deviceQueryService) {
        this.deviceCommandService = deviceCommandService;
        this.deviceQueryService = deviceQueryService;
    }

    
      Creates a new device.
     
      pThe id and serialNumber are auto-generated. The deviceType is provided
      as a String and must match a valid DeviceTypes name.p
     
      @param resource the request body containing device details
      @return HTTP 201 with the created device resource on success,
              or appropriate error status on failure
     
    @PostMapping
    @Operation(
            summary = Create a new device,
            description = Registers a new Sensibo device in the platform. 
                    + The deviceType must be a valid DeviceTypes name (SMART_AC_CONTROLLER, 
                    + ROOM_SENSOR, AIR_QUALITY_MONITOR, DOOR_WINDOW_SENSOR). 
                    + The serialNumber is auto-generated and the id is auto-generated 
                    + by the database.
    )
    @ApiResponses(value = {
            @ApiResponse(
                    responseCode = 201,
                    description = Device created successfully,
                    content = @Content(
                            schema = @Schema(implementation = DeviceResource.class))
            ),
            @ApiResponse(
                    responseCode = 400,
                    description = Invalid input data - validation error),
            @ApiResponse(
                    responseCode = 409,
                    description = Conflict - Device with same serialNumber 
                            + and macAddress already exists),
            @ApiResponse(
                    responseCode = 422,
                    description = Business rule violation)
    })
    public ResponseEntity createDevice(@RequestBody CreateDeviceResource resource) {
        var command = CreateDeviceCommandFromResourceAssembler
                .toCommandFromResource(resource);
        var result = this.deviceCommandService.handle(command)
                .flatMap(deviceId - this.deviceQueryService
                        .handle(new GetDeviceById(deviceId))
                        .ResultDevice, ApplicationErrormap(Resultsuccess)
                        .orElseGet(() - Result.failure(
                                ApplicationError.notFound(Device,
                                        deviceId.toString()))));

        return ResponseEntityAssembler.toResponseEntityFromResult(
                result,
                DeviceResourceFromEntityAssemblertoResourceFromEntity,
                HttpStatus.CREATED);
    }
}
```

---

## Resumen de Cumplimiento de la Rúbrica

 Punto  Requisito  Estado  Dónde 
---------------------------------
 1  Java 25, Spring Boot 3.5.6  4.0.6  ✅  pom.xml 
 2  Nombre `pc2NRCucodigo`  ✅  `pc211990u202418655` 
 3  PostgreSQL, esquema `sensibo`  ✅  `application.properties` + `@Table(schema = sensibo)` 
 4  Package raíz `com.sensibo.platform.ucodigo`  ✅  `com.sensibo.platform.u202418655` 
 5  Bounded context `registry`  ✅  Package `registry` 
 6  Jakarta Validation  @Pattern  ✅  Value Objects con validación propia 
 7  i18n ENES via Accept-Language  ✅  `LocaleConfiguration.java` + `messages.properties` 
 8  DDD, CQRS, layered architecture  ✅  domainapplicationinterfacesinfrastructure 
 9  SnakeCase + plural naming strategy  ✅  `SnakeCaseWithPluralizedTablePhysicalNamingStrategy` 
 10  Shared bounded context  ✅  `shared` package 
 11  JavaDoc en inglés con @author  ✅  Todos los archivos 
 12  OpenAPI con Swagger UI  ✅  `@Operation` + `@ApiResponses` en `DevicesController.java` 
 13  Puerto 8096  ✅  `application.properties` 
 14  URLs minúsculas y plural  ✅  `apiv1devices` 
 15  Lombok  ✅  `@Getter`, `@Setter` 
 16  Mensajes en inglés  ✅  `messages.properties` 
 17  Gestión de excepciones  ✅  `GlobalExceptionHandler.java` 
 18  README.md  ✅  `README.md` 
 19  ZIP `pc2NRCucodigo.zip`  ✅  `pc211990u202418655.zip` 
 20  Subir a actividad PC2  ✅  — 

---

## Reglas de Negocio Implementadas

 #  Regla  Implementación  Archivo 
----------------------------------
 1  No duplicado serialNumber + macAddress  `existsBySerialNumberAndMacAddress()`  `DeviceCommandServiceImpl.java30-34` 
 2  MAC Address formato `AABBCCDDEEFF`  Regex `^([0-9A-Fa-f]{2}){5}[0-9A-Fa-f]{2}$`  `MacAddress.java11` 
 3  Version formato `X.Y.Z`  Regex `^d+.d+.d+$`  `Version.java10` 
 4  SensiboUserId Long  0, no null  Validación en compact constructor  `SensiboUserId.java11-14` 
 5  DeviceTypes como Enum numérico  `@Enumerated(EnumType.ORDINAL)`  `DeviceTypes.java` + `DevicePersistenceEntity.java40` 
 6  SerialNumber UUID autogenerado  `UUID.randomUUID()`  `SerialNumber.java24` 
 7  Device auditable  `@EnableJpaAuditing` + `AuditableAbstractPersistenceEntity`  `Application.java` + shared 
 8  i18n ENES  `AcceptHeaderLocaleResolver` + ResourceBundle  `LocaleConfiguration.java` 
 9  modelName máx 50 caracteres  `@Column(length = 50)`  `DevicePersistenceEntity.java42` 
 10  Response incluye todos los campos  `DeviceResource` con 8 campos  `DeviceResource.java` 
 11  id autogenerado  `@GeneratedValue(IDENTITY)`  `AuditableAbstractPersistenceEntity.java25` 
 12  serialNumber no se ingresa  Se genera en el assembler  `CreateDeviceCommandFromResourceAssembler.java21` 
 13  deviceType como String (input)  `fromString()` en el Device constructor  `Device.java35` 
 14  HTTP 201 en éxito  `HttpStatus.CREATED`  `DevicesController.java91` 
 15  HTTP Status correcto en errores  Switch en `ErrorResponseAssembler`  `ErrorResponseAssembler.java32-40` 

---

## Verificación Final (Checklist)

- [ ] Puerto `httplocalhost8096`
- [ ] Swagger UI `httplocalhost8096swagger-uiindex.html`
- [ ] OpenAPI JSON `httplocalhost8096v3api-docs`
- [ ] BD PostgreSQL `sensibo` con esquema `sensibo`
- [ ] Endpoint `POST apiv1devices` → 201 Created
- [ ] Regla 1 mismo serialNumber + macAddress → 409 Conflict
- [ ] Regla 2 MAC inválida (`ZZYY...`) → 400 Bad Request
- [ ] Regla 3 Version inválida (`abc`) → 400 Bad Request
- [ ] Regla 4 userId = 0 → 400 Bad Request
- [ ] DeviceTypes válidos `SMART_AC_CONTROLLER`, `ROOM_SENSOR`, `AIR_QUALITY_MONITOR`, `DOOR_WINDOW_SENSOR`
- [ ] Convenciones snake_case, tablas plural, inglés, UpperCamelCase clases, lowerCamelCase métodosvariables
- [ ] JavaDoc `@author Victor Jhosef Laura Acosta` en todos los archivos
- [ ] OpenAPI `@Operation` + `@ApiResponses` visibles en Swagger UI
- [ ] i18n con header `Accept-Language es` → mensajes en español
- [ ] Nombre del ZIP `pc211990u202418655.zip`

---

## Estructura Final del Proyecto (38 archivos .java + 5 archivos de configuración)

```
pc211990u202418655
├── pom.xml
├── README.md
├── mvnw
├── mvnw.cmd
├── HELP.md
├── src
│   ├── main
│   │   ├── javacomsensiboplatformu202418655
│   │   │   ├── Pc211990u202418655Application.java
│   │   │   ├── shared
│   │   │   │   ├── applicationresult
│   │   │   │   │   ├── Result.java
│   │   │   │   │   └── ApplicationError.java
│   │   │   │   ├── domainmodelaggregates
│   │   │   │   │   └── AbstractDomainAggregateRoot.java
│   │   │   │   ├── infrastructure
│   │   │   │   │   ├── documentationopenapiconfiguration
│   │   │   │   │   │   └── OpenApiConfiguration.java
│   │   │   │   │   ├── i18nconfiguration
│   │   │   │   │   │   └── LocaleConfiguration.java
│   │   │   │   │   └── persistencejpa
│   │   │   │   │       ├── configurationstrategy
│   │   │   │   │       │   └── SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java
│   │   │   │   │       └── entities
│   │   │   │   │           └── AuditableAbstractPersistenceEntity.java
│   │   │   │   └── interfacesrest
│   │   │   │       ├── GlobalExceptionHandler.java
│   │   │   │       ├── resources
│   │   │   │       │   ├── ErrorResource.java
│   │   │   │       │   └── MessageResource.java
│   │   │   │       └── transform
│   │   │   │           ├── ErrorResponseAssembler.java
│   │   │   │           └── ResponseEntityAssembler.java
│   │   │   └── registry
│   │   │       ├── domain
│   │   │       │   ├── model
│   │   │       │   │   ├── aggregates
│   │   │       │   │   │   └── Device.java
│   │   │       │   │   ├── commands
│   │   │       │   │   │   └── CreateDeviceCommand.java
│   │   │       │   │   ├── queries
│   │   │       │   │   │   └── GetDeviceById.java
│   │   │       │   │   └── valueobjects
│   │   │       │   │       ├── DeviceTypes.java
│   │   │       │   │       ├── MacAddress.java
│   │   │       │   │       ├── SensiboUserId.java
│   │   │       │   │       ├── SerialNumber.java
│   │   │       │   │       └── Version.java
│   │   │       │   └── repositories
│   │   │       │       └── DeviceRepository.java
│   │   │       ├── application
│   │   │       │   ├── commandservices
│   │   │       │   │   └── DeviceCommandService.java
│   │   │       │   ├── queryservices
│   │   │       │   │   └── DeviceQueryService.java
│   │   │       │   └── internal
│   │   │       │       ├── commandservices
│   │   │       │       │   └── DeviceCommandServiceImpl.java
│   │   │       │       └── queryservices
│   │   │       │           └── DeviceQueryServiceImpl.java
│   │   │       ├── infrastructurepersistencejpa
│   │   │       │   ├── adapters
│   │   │       │   │   └── DeviceRepositoryImpl.java
│   │   │       │   ├── assemblers
│   │   │       │   │   └── DevicePersistenceAssembler.java
│   │   │       │   ├── converters
│   │   │       │   │   ├── MacAddressPersistenceConverter.java
│   │   │       │   │   ├── SensiboUserIdPersistenceConverter.java
│   │   │       │   │   ├── SerialNumberPersistenceConverter.java
│   │   │       │   │   └── VersionPersistenceConverter.java
│   │   │       │   ├── entities
│   │   │       │   │   └── DevicePersistenceEntity.java
│   │   │       │   └── repositories
│   │   │       │       └── DevicePersistenceRepository.java
│   │   │       └── interfacesrest
│   │   │           ├── DevicesController.java
│   │   │           ├── resources
│   │   │           │   ├── CreateDeviceResource.java
│   │   │           │   └── DeviceResource.java
│   │   │           └── transform
│   │   │               ├── CreateDeviceCommandFromResourceAssembler.java
│   │   │               └── DeviceResourceFromEntityAssembler.java
│   │   └── resources
│   │       ├── application.properties
│   │       ├── messages.properties
│   │       ├── messages_es.properties
│   │       ├── static
│   │       └── templates
│   └── testjavacomsensiboplatformu202418655
│       └── Pc211990u202418655ApplicationTests.java
```
