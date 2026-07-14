# PC2 - SENSIBO Guía Template Paso a Paso
## 1ASI0729 - Desarrollo de Aplicaciones Open Source

---

## ⚠️ CÓMO USAR ESTA GUÍA

- Todo lo que aparece como `«ESTO_EN_ANGULOS»` lo **reemplazas con el valor de tu examen**
- Todo lo que aparece en bloques de código **sin ángulos se copia y pega tal cual**
- Sigue los pasos **en orden estricto**
- Esta guía cubre el patrón de los Exámenes Finales: **2 bounded contexts + shared + ACL + eventos de dominio + data seeding**

---

## PASO 0 — Lee tu examen y llena esta tabla ANTES de empezar

| Variable | Ejemplo EB 202520 (RecipeVault) | Ejemplo EB 202610 (Whirlpool) | **TU EXAMEN** |
|---|---|---|---|
| **NRC** | 7385 | 20262 | _______ |
| **Código estudiante** | u202418655 | u202418655 | u202418655 |
| **Nombre proyecto (ZIP)** | eb7385u202418655 | eb20262u202418655 | eb«NRC»u202418655 |
| **Java version** | 25 | 26 | _______ |
| **Spring Boot version** | 3.5.8 | 4.1.0 | _______ |
| **BD tipo** | PostgreSQL | MySQL | _______ |
| **BD esquema** | nutrilog | whirlpool_os | _______ |
| **Package raíz** | app.recipevault.platform | com.whirlpool.care.platform | _______ |
| **BC1 nombre** | consumption-tracking | entitlement | _______ |
| **BC1 package** | consumptiontracking | entitlement | _______ |
| **BC1 aggregate** | ConsumptionRecord | Claim | _______ |
| **BC1 endpoint** | /api/v1/consumption-records | /api/v1/claims | _______ |
| **BC1 operación** | POST | POST | _______ |
| **BC2 nombre** | catalog-management | contracts | _______ |
| **BC2 package** | catalogmanagement | contracts | _______ |
| **BC2 aggregate** | Ingredient | Policy | _______ |
| **BC2 endpoint** | /api/v1/ingredients | /api/v1/policies | _______ |
| **BC2 operación** | GET | GET | _______ |
| **Value Object en Shared** | IngredientCode | Period | _______ |
| **Evento de integración** | ConsumptionRecordRegisteredEvent | ClaimCreatedEvent | _______ |
| **Enums del dominio** | MealType, DietaryTag | ClaimStatus | _______ |
| **Puerto servidor** | (ver enunciado) | (ver enunciado) | _______ |
| **¿Cuál BC hace data seeding?** | BC2 (Ingredient) | BC2 (Policy) | _______ |

---

## PASO 1 — Configuración de la Base de Datos

### Si tu examen usa PostgreSQL

Abrir `pgAdmin`, ingresar contraseña `12345678` y crear la base de datos:

```sql
CREATE DATABASE «BD_ESQUEMA»;
```

Luego ejecutar:

```sql
CREATE SCHEMA «BD_ESQUEMA»;
```

### Si tu examen usa MySQL

Abrir MySQL CLI o phpMyAdmin y ejecutar:

```sql
CREATE SCHEMA IF NOT EXISTS «BD_ESQUEMA»
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
```

---

## PASO 2 — Creación del proyecto (Spring Initializr)

Cargar el navegador y pegar la URL correspondiente:

### Si PostgreSQL:
```
https://start.spring.io/#!type=maven-project&language=java&platformVersion=«SPRING_BOOT_VERSION»&packaging=jar&configurationFileFormat=properties&jvmVersion=«JAVA_VERSION»&groupId=«PACKAGE_RAIZ»&artifactId=eb«NRC»u202418655&packageName=«PACKAGE_RAIZ».u202418655&dependencies=data-jpa,validation,web,devtools,postgresql,lombok,springdoc-openapi
```

### Si MySQL:
```
https://start.spring.io/#!type=maven-project&language=java&platformVersion=«SPRING_BOOT_VERSION»&packaging=jar&configurationFileFormat=properties&jvmVersion=«JAVA_VERSION»&groupId=«PACKAGE_RAIZ»&artifactId=eb«NRC»u202418655&packageName=«PACKAGE_RAIZ».u202418655&dependencies=data-jpa,validation,web,devtools,mysql,lombok,springdoc-openapi
```

> Generar el proyecto, guardarlo en `IdeaProjects/1ASI0729/` y abrirlo en IntelliJ IDEA.

---

## PASO 3 — Configuración del SDK

File → Project Structure → Project → SDK: seleccionar versión `«JAVA_VERSION»`. Si no está instalada, descargarla. Click en OK.

---

## PASO 4 — Archivo pom.xml

Abrir `pom.xml` y modificar la versión de Java en `<properties>`:

```xml
<properties>
    <java.version>«JAVA_VERSION»</java.version>
</properties>
```

Agregar la dependencia de pluralize después de springdoc-openapi:

```xml
<!-- https://mvnrepository.com/artifact/io.github.encryptorcode/pluralize -->
<dependency>
    <groupId>io.github.encryptorcode</groupId>
    <artifactId>pluralize</artifactId>
    <version>1.0.0</version>
</dependency>
```

> ✅ El resto del `pom.xml` queda como lo generó Spring Initializr.

---

## PASO 5 — Clase Application principal

Abrir `Eb«NRC»u202418655Application.java` en el package `«PACKAGE_RAIZ».u202418655` y reemplazar con:

```java
package «PACKAGE_RAIZ».u202418655;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

/**
 * Main application class for the «NOMBRE_CASO» platform.
 *
 * <p>Enables JPA auditing for automatic management of entity creation
 * and last-updated timestamps across all aggregate roots.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@SpringBootApplication
@EnableJpaAuditing
public class Eb«NRC»u202418655Application {

    /**
     * Entry point of the Spring Boot application.
     *
     * @param args command-line arguments
     */
    public static void main(String[] args) {
        SpringApplication.run(Eb«NRC»u202418655Application.class, args);
    }
}
```

---

## PASO 6 — Archivo application.properties

### Si PostgreSQL:

```ini
spring.application.name=eb«NRC»u202418655

# Spring DataSource Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/«BD_ESQUEMA»?currentSchema=«BD_ESQUEMA»
spring.datasource.username=postgres
spring.datasource.password=12345678
spring.datasource.driver-class-name=org.postgresql.Driver

# Spring Data JPA Configuration
spring.jpa.database=postgresql
spring.jpa.show-sql=true

# Spring Data JPA Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.open-in-view=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.PostgreSQLDialect
spring.jpa.hibernate.naming.physical-strategy=«PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.configuration.strategy.SnakeCaseWithPluralizedTablePhysicalNamingStrategy

server.port=«PUERTO»

documentation.application.description=«NOMBRE_CASO» RESTful API
documentation.application.version=@project.version@

spring.messages.basename=messages
spring.messages.encoding=UTF-8
```

### Si MySQL:

```ini
spring.application.name=eb«NRC»u202418655

# Spring DataSource Configuration
spring.datasource.url=jdbc:mysql://localhost:3306/«BD_ESQUEMA»
spring.datasource.username=root
spring.datasource.password=12345678
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# Spring Data JPA Configuration
spring.jpa.database=mysql
spring.jpa.show-sql=true

# Spring Data JPA Hibernate Configuration
spring.jpa.hibernate.ddl-auto=update
spring.jpa.open-in-view=true
spring.jpa.properties.hibernate.format_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQLDialect
spring.jpa.hibernate.naming.physical-strategy=«PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.configuration.strategy.SnakeCaseWithPluralizedTablePhysicalNamingStrategy

server.port=«PUERTO»

documentation.application.description=«NOMBRE_CASO» RESTful API
documentation.application.version=@project.version@

spring.messages.basename=messages
spring.messages.encoding=UTF-8
```

---

## PASO 7 — Archivos i18n

### Crear `src/main/resources/messages.properties`

```properties
# Validation messages
validation.field.prefix=Field
validation.request.failed=Request validation failed
validation.request.argument=request-argument

# Error messages
error.validation.message=Validation failed: {0}
error.business-rule.message=Business rule violation: {0}
error.unexpected.message=An unexpected error occurred
error.not-found.message={0} not found
error.conflict.message=Conflict: {0}
error.generic.message=An error occurred
```

### Crear `src/main/resources/messages_es.properties`

```properties
# Validation messages
validation.field.prefix=Campo
validation.request.failed=Falló la validación de la solicitud
validation.request.argument=argumento-de-solicitud

# Error messages
error.validation.message=Error de validación: {0}
error.business-rule.message=Violación de regla de negocio: {0}
error.unexpected.message=Ocurrió un error inesperado
error.not-found.message={0} no encontrado
error.conflict.message=Conflicto: {0}
error.generic.message=Ocurrió un error
```

---

## PASO 8 — README.md

Crear `README.md` en la raíz del proyecto:

```markdown
# «NOMBRE_CASO» API

## Description
RESTful API for the «NOMBRE_CASO» case study, implementing Domain-Driven Design,
CQRS, Anti-Corruption Layer, and domain events.
Built with Java «JAVA_VERSION» / Spring Boot «SPRING_BOOT_VERSION» / Spring Data JPA.

## Technologies
- Java «JAVA_VERSION»
- Spring Boot «SPRING_BOOT_VERSION»
- Spring Data JPA
- «BD_TIPO»
- Lombok
- OpenAPI (Swagger UI)

## Author
Victor Jhosef Laura Acosta

## License
Apache 2.0
```

---

## PASO 9 — Estructura de carpetas completa

Crear toda la siguiente estructura bajo `src/main/java/«PACKAGE_RAIZ»/u202418655/`:

```
u202418655/
├── shared/
│   ├── application/result/
│   │   ├── Result.java
│   │   └── ApplicationError.java
│   ├── domain/model/aggregates/
│   │   └── AbstractDomainAggregateRoot.java
│   ├── domain/model/valueobjects/
│   │   └── «SHARED_VALUE_OBJECT».java       ← El VO que pida tu examen en Shared
│   ├── infrastructure/documentation/openapi/configuration/
│   │   └── OpenApiConfiguration.java
│   ├── infrastructure/i18n/configuration/
│   │   └── LocaleConfiguration.java
│   ├── infrastructure/persistence/jpa/configuration/strategy/
│   │   └── SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java
│   └── infrastructure/persistence/jpa/entities/
│       └── AuditableAbstractPersistenceEntity.java
│   └── interfaces/rest/
│       ├── GlobalExceptionHandler.java
│       ├── resources/
│       │   ├── ErrorResource.java
│       │   └── MessageResource.java
│       └── transform/
│           ├── ErrorResponseAssembler.java
│           └── ResponseEntityAssembler.java
│
├── «BC1_PACKAGE»/                          ← BC1: «BC1_NOMBRE»
│   ├── domain/model/aggregates/
│   │   └── «BC1_AGGREGATE».java
│   ├── domain/model/valueobjects/
│   │   └── (value objects específicos de BC1)
│   ├── domain/model/commands/
│   │   └── Create«BC1_AGGREGATE»Command.java
│   ├── domain/model/queries/
│   │   └── GetAll«BC1_AGGREGATE»sQuery.java (si aplica GET)
│   ├── domain/model/events/                ← Eventos de integración
│   │   └── «DOMAIN_EVENT_NAME».java
│   ├── domain/repositories/
│   │   └── I«BC1_AGGREGATE»Repository.java
│   ├── application/commandservices/
│   │   └── I«BC1_AGGREGATE»CommandService.java
│   ├── application/queryservices/ (si aplica)
│   │   └── I«BC1_AGGREGATE»QueryService.java
│   ├── application/internal/commandservices/
│   │   └── «BC1_AGGREGATE»CommandServiceImpl.java
│   ├── application/internal/queryservices/ (si aplica)
│   │   └── «BC1_AGGREGATE»QueryServiceImpl.java
│   ├── application/internal/eventhandlers/  ← Si BC1 recibe eventos de BC2
│   │   └── «DOMAIN_EVENT_NAME»Handler.java
│   ├── infrastructure/acl/                  ← Anti-Corruption Layer
│   │   └── «BC2_AGGREGATE»ContextFacade.java
│   ├── infrastructure/persistence/jpa/adapters/
│   │   └── «BC1_AGGREGATE»RepositoryImpl.java
│   ├── infrastructure/persistence/jpa/assemblers/
│   │   └── «BC1_AGGREGATE»PersistenceAssembler.java
│   ├── infrastructure/persistence/jpa/converters/
│   │   └── (converters de value objects si aplica)
│   ├── infrastructure/persistence/jpa/entities/
│   │   └── «BC1_AGGREGATE»PersistenceEntity.java
│   └── infrastructure/persistence/jpa/repositories/
│       └── «BC1_AGGREGATE»PersistenceRepository.java
│   └── interfaces/rest/
│       ├── «BC1_AGGREGATE»sController.java
│       ├── resources/
│       │   ├── Create«BC1_AGGREGATE»Resource.java
│       │   └── «BC1_AGGREGATE»Resource.java
│       └── transform/
│           ├── Create«BC1_AGGREGATE»CommandFromResourceAssembler.java
│           └── «BC1_AGGREGATE»ResourceFromEntityAssembler.java
│
├── «BC2_PACKAGE»/                          ← BC2: «BC2_NOMBRE»
│   ├── domain/model/aggregates/
│   │   └── «BC2_AGGREGATE».java
│   ├── domain/model/valueobjects/
│   │   └── (value objects específicos de BC2)
│   ├── domain/model/queries/
│   │   └── GetAll«BC2_AGGREGATE»sQuery.java
│   ├── domain/repositories/
│   │   └── I«BC2_AGGREGATE»Repository.java
│   ├── application/queryservices/
│   │   └── I«BC2_AGGREGATE»QueryService.java
│   ├── application/internal/queryservices/
│   │   └── «BC2_AGGREGATE»QueryServiceImpl.java
│   ├── application/internal/eventhandlers/  ← Si BC2 recibe eventos de BC1
│   │   └── «DOMAIN_EVENT_NAME»Handler.java
│   ├── infrastructure/persistence/jpa/adapters/
│   │   └── «BC2_AGGREGATE»RepositoryImpl.java
│   ├── infrastructure/persistence/jpa/assemblers/
│   │   └── «BC2_AGGREGATE»PersistenceAssembler.java
│   ├── infrastructure/persistence/jpa/entities/
│   │   └── «BC2_AGGREGATE»PersistenceEntity.java
│   └── infrastructure/persistence/jpa/repositories/
│       └── «BC2_AGGREGATE»PersistenceRepository.java
│   └── interfaces/rest/
│       ├── «BC2_AGGREGATE»sController.java
│       ├── resources/
│       │   └── «BC2_AGGREGATE»Resource.java
│       └── transform/
│           └── «BC2_AGGREGATE»ResourceFromEntityAssembler.java
```

---

## PASO 10 — Shared Package (SE COPIA SIEMPRE IGUAL, no cambia)

### 10.1 AbstractDomainAggregateRoot.java

Ruta: `shared/domain/model/aggregates/AbstractDomainAggregateRoot.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.domain.model.aggregates`

```java
package «PACKAGE_RAIZ».u202418655.shared.domain.model.aggregates;

import org.springframework.data.domain.AbstractAggregateRoot;
import java.util.Collection;

/**
 * Base class for all domain aggregate roots.
 *
 * @param <T> the concrete aggregate root type
 * @author Victor Jhosef Laura Acosta
 */
public abstract class AbstractDomainAggregateRoot<T extends AbstractDomainAggregateRoot<T>>
        extends AbstractAggregateRoot<T> {

    protected void registerDomainEvent(Object event) {
        super.registerEvent(event);
    }

    @Override
    public Collection<Object> domainEvents() {
        return super.domainEvents();
    }

    @Override
    public void clearDomainEvents() {
        super.clearDomainEvents();
    }
}
```

---

### 10.2 Result.java

Ruta: `shared/application/result/Result.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.application.result`

```java
package «PACKAGE_RAIZ».u202418655.shared.application.result;

import java.util.Optional;
import java.util.function.Function;

/**
 * Represents the result of a command or operation.
 *
 * @param <T> The type of the successful result value
 * @param <E> The type of the error/failure information
 * @author Victor Jhosef Laura Acosta
 */
public sealed interface Result<T, E> {

    record Success<T, E>(T value) implements Result<T, E> {}
    record Failure<T, E>(E error) implements Result<T, E> {}

    static <T, E> Result<T, E> success(T value) { return new Success<>(value); }
    static <T, E> Result<T, E> failure(E error)  { return new Failure<>(error); }

    default boolean isSuccess() { return this instanceof Success; }
    default boolean isFailure() { return this instanceof Failure; }

    default Optional<T> toOptional() {
        return switch (this) {
            case Success<T, E> s -> Optional.of(s.value);
            case Failure<T, E> f -> Optional.empty();
        };
    }

    default T getOrElse(T defaultValue) {
        return switch (this) {
            case Success<T, E> s -> s.value;
            case Failure<T, E> f -> defaultValue;
        };
    }

    default <E2> Result<T, E2> mapError(Function<E, E2> f) {
        return switch (this) {
            case Success<T, E> s -> Result.success(s.value);
            case Failure<T, E> failure -> Result.failure(f.apply(failure.error));
        };
    }

    default <T2> Result<T2, E> flatMap(Function<T, Result<T2, E>> f) {
        return switch (this) {
            case Success<T, E> s -> f.apply(s.value);
            case Failure<T, E> failure -> Result.failure(failure.error);
        };
    }

    default <T2> Result<T2, E> map(Function<T, T2> f) {
        return switch (this) {
            case Success<T, E> s -> Result.success(f.apply(s.value));
            case Failure<T, E> failure -> Result.failure(failure.error);
        };
    }

    default Result<T, E> recover(Function<E, Result<T, E>> f) {
        return switch (this) {
            case Success<T, E> s -> this;
            case Failure<T, E> failure -> f.apply(failure.error);
        };
    }
}
```

---

### 10.3 ApplicationError.java

Ruta: `shared/application/result/ApplicationError.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.application.result`

```java
package «PACKAGE_RAIZ».u202418655.shared.application.result;

/**
 * Represents an error that occurred in the application layer.
 *
 * @param code     A machine-readable error code
 * @param message  A human-readable error message
 * @param details  Optional additional context
 * @author Victor Jhosef Laura Acosta
 */
public record ApplicationError(String code, String message, String details) {

    public ApplicationError(String code, String message) {
        this(code, message, null);
    }

    public static ApplicationError validationError(String fieldOrConcept, String reason) {
        return new ApplicationError("VALIDATION_ERROR",
                "Validation failed: %s".formatted(fieldOrConcept), reason);
    }

    public static ApplicationError notFound(String resourceType, String identifier) {
        return new ApplicationError("%s_NOT_FOUND".formatted(resourceType.toUpperCase()),
                "%s not found: %s".formatted(resourceType, identifier), null);
    }

    public static ApplicationError businessRuleViolation(String rule, String reason) {
        return new ApplicationError("BUSINESS_RULE_VIOLATION",
                "Business rule violation: %s".formatted(rule), reason);
    }

    public static ApplicationError conflict(String resource, String reason) {
        return new ApplicationError("%s_CONFLICT".formatted(resource.toUpperCase()),
                "Conflict with %s".formatted(resource), reason);
    }

    public static ApplicationError unexpected(String context, String reason) {
        return new ApplicationError("UNEXPECTED_ERROR",
                "Unexpected error in %s".formatted(context), reason);
    }
}
```

---

### 10.4 SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java

Ruta: `shared/infrastructure/persistence/jpa/configuration/strategy/SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.configuration.strategy`

```java
package «PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.configuration.strategy;

import org.hibernate.boot.model.naming.Identifier;
import org.hibernate.boot.model.naming.PhysicalNamingStrategy;
import org.hibernate.engine.jdbc.env.spi.JdbcEnvironment;

import static io.github.encryptorcode.pluralize.Pluralize.pluralize;

/**
 * Snake Case With Pluralized Table Physical Naming Strategy.
 *
 * @author Victor Jhosef Laura Acosta
 */
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

    private Identifier toSnakeCase(final Identifier identifier) {
        if (identifier == null) return null;
        final String newName = identifier.getText()
                .replaceAll("([a-z])([A-Z])", "$1_$2")
                .toLowerCase();
        return Identifier.toIdentifier(newName);
    }

    private Identifier toPlural(final Identifier identifier) {
        return Identifier.toIdentifier(pluralize(identifier.getText()));
    }
}
```

---

### 10.5 AuditableAbstractPersistenceEntity.java

Ruta: `shared/infrastructure/persistence/jpa/entities/AuditableAbstractPersistenceEntity.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.entities`

```java
package «PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.entities;

import jakarta.persistence.*;
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import java.util.Date;

/**
 * Base JPA persistence entity for all persistence entities that require auditing.
 *
 * @author Victor Jhosef Laura Acosta
 */
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

    public void setId(Long id) {
        this.id = id;
    }
}
```

---

### 10.6 OpenApiConfiguration.java

Ruta: `shared/infrastructure/documentation/openapi/configuration/OpenApiConfiguration.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.infrastructure.documentation.openapi.configuration`

```java
package «PACKAGE_RAIZ».u202418655.shared.infrastructure.documentation.openapi.configuration;

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

/**
 * Configures the OpenAPI specification exposed by the platform.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Configuration
public class OpenApiConfiguration {

    @Value("${spring.application.name}")
    String applicationName;

    @Value("${documentation.application.description}")
    String applicationDescription;

    @Value("${documentation.application.version}")
    String applicationVersion;

    /**
     * Builds the OpenAPI document used by Swagger UI.
     *
     * @return configured OpenAPI descriptor
     */
    @Bean
    public OpenAPI platformOpenApi() {
        var openApi = new OpenAPI();
        openApi.info(new Info()
                        .title(this.applicationName)
                        .description(this.applicationDescription)
                        .version(this.applicationVersion)
                        .contact(new Contact()
                                .name("Victor Jhosef Laura Acosta")
                                .email("u202418655@upc.edu.pe"))
                        .license(new License()
                                .name("Apache 2.0")
                                .url("https://www.apache.org/licenses/LICENSE-2.0.html")));
        openApi.servers(List.of(
                new Server().url("http://localhost:«PUERTO»").description("Local Development Environment")
        ));
        return openApi;
    }
}
```

---

### 10.7 LocaleConfiguration.java

Ruta: `shared/infrastructure/i18n/configuration/LocaleConfiguration.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.infrastructure.i18n.configuration`

```java
package «PACKAGE_RAIZ».u202418655.shared.infrastructure.i18n.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver;

import java.util.List;
import java.util.Locale;

/**
 * Configures locale resolution for REST requests via Accept-Language header.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Configuration
public class LocaleConfiguration {

    /**
     * Creates a LocaleResolver that uses the Accept-Language header.
     *
     * @return the locale resolver instance
     */
    @Bean
    public LocaleResolver localeResolver() {
        var resolver = new AcceptHeaderLocaleResolver();
        resolver.setDefaultLocale(Locale.ENGLISH);
        resolver.setSupportedLocales(List.of(Locale.ENGLISH, Locale.forLanguageTag("es")));
        return resolver;
    }
}
```

---

### 10.8 ErrorResource.java

Ruta: `shared/interfaces/rest/resources/ErrorResource.java`

Package: `«PACKAGE_RAIZ».u202418655.shared.interfaces.rest.resources`

```java
package «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.resources;

import com.fasterxml.jackson.annotation.JsonInclude;

/**
 * Standard error response resource returned from REST endpoints.
 *
 * @author Victor Jhosef Laura Acosta
 */
@JsonInclude(JsonInclude.Include.NON_NULL)
public record ErrorResource(String code, String message, String details) {
    public ErrorResource(String code, String message) {
        this(code, message, null);
    }
}
```

---

### 10.9 MessageResource.java

Ruta: `shared/interfaces/rest/resources/MessageResource.java`

```java
package «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.resources;

/**
 * Resource used for simple success or informational REST responses.
 *
 * @author Victor Jhosef Laura Acosta
 */
public record MessageResource(String message) {}
```

---

### 10.10 ErrorResponseAssembler.java

Ruta: `shared/interfaces/rest/transform/ErrorResponseAssembler.java`

```java
package «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.transform;

import «PACKAGE_RAIZ».u202418655.shared.application.result.ApplicationError;
import «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.resources.ErrorResource;
import org.springframework.http.HttpStatus;
import org.springframework.http.HttpStatusCode;
import org.springframework.http.ResponseEntity;

/**
 * Assembler for converting application errors to HTTP responses.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class ErrorResponseAssembler {

    private ErrorResponseAssembler() {}

    public static ResponseEntity<ErrorResource> toErrorResponseFromApplicationError(ApplicationError error) {
        HttpStatusCode status = toStatusFromErrorCode(error.code());
        return new ResponseEntity<>(
                new ErrorResource(error.code(), error.message(), error.details()), status);
    }

    public static HttpStatusCode toStatusFromErrorCode(String errorCode) {
        return switch (errorCode) {
            case "VALIDATION_ERROR" -> HttpStatus.BAD_REQUEST;
            case String s when s.endsWith("_NOT_FOUND") -> HttpStatus.NOT_FOUND;
            case "BUSINESS_RULE_VIOLATION" -> HttpStatusCode.valueOf(422);
            case String s when s.endsWith("_CONFLICT") -> HttpStatus.CONFLICT;
            case "UNEXPECTED_ERROR" -> HttpStatus.INTERNAL_SERVER_ERROR;
            default -> HttpStatus.INTERNAL_SERVER_ERROR;
        };
    }
}
```

---

### 10.11 ResponseEntityAssembler.java

Ruta: `shared/interfaces/rest/transform/ResponseEntityAssembler.java`

```java
package «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.transform;

import «PACKAGE_RAIZ».u202418655.shared.application.result.ApplicationError;
import «PACKAGE_RAIZ».u202418655.shared.application.result.Result;
import org.springframework.http.HttpStatusCode;
import org.springframework.http.ResponseEntity;

import java.util.function.Function;

/**
 * Assembler that translates application Result values into HTTP responses.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class ResponseEntityAssembler {

    private ResponseEntityAssembler() {}

    public static <T, R> ResponseEntity<?> toResponseEntityFromResult(
            Result<T, ApplicationError> result,
            Function<T, R> successResourceAssembler,
            HttpStatusCode successStatus) {
        return switch (result) {
            case Result.Success<T, ApplicationError> success ->
                    new ResponseEntity<>(successResourceAssembler.apply(success.value()), successStatus);
            case Result.Failure<T, ApplicationError> failure ->
                    ErrorResponseAssembler.toErrorResponseFromApplicationError(failure.error());
        };
    }
}
```

---

### 10.12 GlobalExceptionHandler.java

Ruta: `shared/interfaces/rest/GlobalExceptionHandler.java`

```java
package «PACKAGE_RAIZ».u202418655.shared.interfaces.rest;

import «PACKAGE_RAIZ».u202418655.shared.application.result.ApplicationError;
import «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.transform.ErrorResponseAssembler;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

/**
 * Global exception handler for REST API.
 *
 * @author Victor Jhosef Laura Acosta
 */
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleMethodArgumentNotValid(MethodArgumentNotValidException ex) {
        var fieldErrors = ex.getBindingResult().getFieldErrors();
        var errorDetails = fieldErrors.isEmpty()
                ? "Request validation failed"
                : fieldErrors.stream()
                        .map(error -> "Field %s: %s".formatted(error.getField(), error.getDefaultMessage()))
                        .reduce((a, b) -> a + "; " + b)
                        .orElse("Request validation failed");
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.validationError("request-body", errorDetails));
    }

    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<?> handleIllegalArgumentException(IllegalArgumentException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.validationError("request-argument",
                        ex.getMessage() != null ? ex.getMessage() : "Invalid request argument"));
    }

    @ExceptionHandler(IllegalStateException.class)
    public ResponseEntity<?> handleIllegalStateException(IllegalStateException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.businessRuleViolation("state-violation",
                        ex.getMessage() != null ? ex.getMessage() : "Invalid state"));
    }

    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<?> handleRuntimeException(RuntimeException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.unexpected("global-exception-handler",
                        ex.getMessage() != null ? ex.getMessage() : "An unexpected error occurred"));
    }

    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleException(Exception ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.unexpected("global-exception-handler",
                        ex.getMessage() != null ? ex.getMessage() : "An unexpected error occurred"));
    }
}
```

---

### 10.13 «SHARED_VALUE_OBJECT».java

> ⚠️ **Este archivo cambia según tu examen.** Aquí va el Value Object que el enunciado pide en Shared (ej: `IngredientCode`, `Period`, etc.). Sigue el mismo patrón: record con validación en compact constructor, constructor por defecto si aplica.

Ruta: `shared/domain/model/valueobjects/«SHARED_VALUE_OBJECT».java`

```java
package «PACKAGE_RAIZ».u202418655.shared.domain.model.valueobjects;

import java.util.Objects;

/**
 * Value object representing «DESCRIPCIÓN».
 * ← COMPLETAR SEGÚN TU EXAMEN
 *
 * @author Victor Jhosef Laura Acosta
 */
public record «SHARED_VALUE_OBJECT»(/* CAMPOS SEGÚN TU EXAMEN */) {

    public «SHARED_VALUE_OBJECT» {
        // VALIDACIONES SEGÚN TU EXAMEN
        // Ejemplo:
        // if (Objects.isNull(value) || value.isBlank())
        //     throw new IllegalArgumentException("[«SHARED_VALUE_OBJECT»] value cannot be null or blank");
        // if (!value.matches("REGEX DE TU EXAMEN"))
        //     throw new IllegalArgumentException("[«SHARED_VALUE_OBJECT»] Invalid format");
    }

    // Constructor por defecto requerido por JPA (si aplica)
    public «SHARED_VALUE_OBJECT»() {
        this(/* valor por defecto */);
    }
}
```

---

## PASO 11 — BC2: «BC2_AGGREGATE» (el que hace DATA SEEDING y expone GET)

> Normalmente el BC que se siembra con datos es el que tiene el GET. Créalo primero porque el BC1 lo necesita via ACL.

### 11.1 Value Objects de BC2 (si aplica)

> Si BC2 tiene value objects propios (además del que ya está en Shared), créalos aquí.

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.valueobjects;

// Mismo patrón que los value objects del Shared
// ← COMPLETAR SEGÚN TU EXAMEN
```

### 11.2 «BC2_AGGREGATE».java (Aggregate Root)

Ruta: `«BC2_PACKAGE»/domain/model/aggregates/«BC2_AGGREGATE».java`

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates;

import «PACKAGE_RAIZ».u202418655.shared.domain.model.aggregates.AbstractDomainAggregateRoot;
import lombok.Getter;
import lombok.Setter;

/**
 * Aggregate root representing a «BC2_AGGREGATE» in the «BC2_NOMBRE» bounded context.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Getter
@Setter  // ← Solo en los campos que necesiten setter
public class «BC2_AGGREGATE» extends AbstractDomainAggregateRoot<«BC2_AGGREGATE»> {

    private Long id;

    // ← AGREGAR AQUÍ TODOS LOS ATRIBUTOS QUE PIDE TU EXAMEN
    // Ejemplo:
    // private String policyNumber;
    // private String applianceModel;
    // private «SHARED_VALUE_OBJECT» coveragePeriod;  ← Si usa VO de Shared
    // private Long lastClaimId;                       ← Si es nullable

    public «BC2_AGGREGATE»() {}

    // Constructor con parámetros (según lo que pida tu examen)
    public «BC2_AGGREGATE»(/* PARÁMETROS */) {
        // INICIALIZACIÓN DE ATRIBUTOS
    }
}
```

### 11.3 Enums de BC2 (si aplica)

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.valueobjects;

/**
 * Enumeration for «NOMBRE_ENUM».
 * ← COMPLETAR SEGÚN TU EXAMEN
 *
 * @author Victor Jhosef Laura Acosta
 */
public enum «NOMBRE_ENUM» {
    // VALORES SEGÚN TU EXAMEN
    // Ejemplo: OPEN, IN_PROGRESS, RESOLVED, DENIED
}
```

### 11.4 I«BC2_AGGREGATE»Repository.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.repositories;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import java.util.List;
import java.util.Optional;

/**
 * Repository interface for «BC2_AGGREGATE» aggregate root.
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface I«BC2_AGGREGATE»Repository {

    «BC2_AGGREGATE» save(«BC2_AGGREGATE» entity);

    Optional<«BC2_AGGREGATE»> findById(Long id);

    List<«BC2_AGGREGATE»> findAll();

    // ← AGREGAR MÉTODOS ESPECÍFICOS QUE TU EXAMEN NECESITE
    // Ejemplo: boolean existsByPolicyNumber(String policyNumber);
}
```

### 11.5 GetAll«BC2_AGGREGATE»sQuery.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.queries;

/**
 * Query to retrieve all «BC2_AGGREGATE»s.
 *
 * @author Victor Jhosef Laura Acosta
 */
public record GetAll«BC2_AGGREGATE»sQuery() {}
```

### 11.6 I«BC2_AGGREGATE»QueryService.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».application.queryservices;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.queries.GetAll«BC2_AGGREGATE»sQuery;
import java.util.List;

/**
 * Service interface for handling «BC2_AGGREGATE» queries.
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface I«BC2_AGGREGATE»QueryService {

    List<«BC2_AGGREGATE»> handle(GetAll«BC2_AGGREGATE»sQuery query);
}
```

### 11.7 «BC2_AGGREGATE»QueryServiceImpl.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».application.internal.queryservices;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».application.queryservices.I«BC2_AGGREGATE»QueryService;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.queries.GetAll«BC2_AGGREGATE»sQuery;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.repositories.I«BC2_AGGREGATE»Repository;
import org.springframework.stereotype.Service;
import java.util.List;

/**
 * Implementation of I«BC2_AGGREGATE»QueryService.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class «BC2_AGGREGATE»QueryServiceImpl implements I«BC2_AGGREGATE»QueryService {

    private final I«BC2_AGGREGATE»Repository repository;

    public «BC2_AGGREGATE»QueryServiceImpl(I«BC2_AGGREGATE»Repository repository) {
        this.repository = repository;
    }

    @Override
    public List<«BC2_AGGREGATE»> handle(GetAll«BC2_AGGREGATE»sQuery query) {
        return this.repository.findAll();
    }
}
```

### 11.8 «BC2_AGGREGATE»PersistenceEntity.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.entities;

import «PACKAGE_RAIZ».u202418655.shared.infrastructure.persistence.jpa.entities.AuditableAbstractPersistenceEntity;
import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

/**
 * JPA persistence entity for «BC2_AGGREGATE».
 *
 * @author Victor Jhosef Laura Acosta
 */
@Entity
@Getter
@Setter
public class «BC2_AGGREGATE»PersistenceEntity extends AuditableAbstractPersistenceEntity {

    // ← AGREGAR AQUÍ TODOS LOS CAMPOS PERSISTIBLES SEGÚN TU EXAMEN
    // Usa @Column para los simples
    // Usa @Embedded / @Convert para los value objects
    // NO incluyas createdAt/updatedAt (ya están en el padre)

    // Ejemplo:
    // @Column(nullable = false, unique = true)
    // private String policyNumber;
    //
    // @Column(nullable = false)
    // private String applianceModel;
    //
    // @Column(nullable = true)
    // private Long lastClaimId;

    // Para Value Objects embebidos (si aplica):
    // @Embedded
    // private «SHARED_VALUE_OBJECT» coveragePeriod;
}
```

### 11.9 «BC2_AGGREGATE»PersistenceRepository.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.repositories;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.entities.«BC2_AGGREGATE»PersistenceEntity;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

/**
 * Spring Data JPA repository for «BC2_AGGREGATE»PersistenceEntity.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Repository
public interface «BC2_AGGREGATE»PersistenceRepository
        extends JpaRepository<«BC2_AGGREGATE»PersistenceEntity, Long> {

    // ← AGREGAR MÉTODOS DE CONSULTA NECESARIOS
    // Ejemplo: boolean existsByPolicyNumber(String policyNumber);
    // Ejemplo: Optional<«BC2_AGGREGATE»PersistenceEntity> findByPolicyNumber(String policyNumber);
}
```

### 11.10 «BC2_AGGREGATE»PersistenceAssembler.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.assemblers;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.entities.«BC2_AGGREGATE»PersistenceEntity;

/**
 * Assembler for converting between «BC2_AGGREGATE» (domain) and «BC2_AGGREGATE»PersistenceEntity (JPA).
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class «BC2_AGGREGATE»PersistenceAssembler {

    private «BC2_AGGREGATE»PersistenceAssembler() {}

    public static «BC2_AGGREGATE»PersistenceEntity toPersistenceFromDomain(«BC2_AGGREGATE» domain) {
        var entity = new «BC2_AGGREGATE»PersistenceEntity();
        entity.setId(domain.getId());
        // ← MAPEAR CADA CAMPO DE DOMAIN A ENTITY
        return entity;
    }

    public static «BC2_AGGREGATE» toDomainFromPersistence(«BC2_AGGREGATE»PersistenceEntity entity) {
        var domain = new «BC2_AGGREGATE»();
        domain.setId(entity.getId());
        // ← MAPEAR CADA CAMPO DE ENTITY A DOMAIN
        return domain;
    }
}
```

### 11.11 «BC2_AGGREGATE»RepositoryImpl.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.adapters;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.repositories.I«BC2_AGGREGATE»Repository;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.assemblers.«BC2_AGGREGATE»PersistenceAssembler;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure.persistence.jpa.repositories.«BC2_AGGREGATE»PersistenceRepository;
import org.springframework.stereotype.Repository;
import java.util.List;
import java.util.Optional;

/**
 * Implementation of I«BC2_AGGREGATE»Repository using JPA.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Repository
public class «BC2_AGGREGATE»RepositoryImpl implements I«BC2_AGGREGATE»Repository {

    private final «BC2_AGGREGATE»PersistenceRepository persistenceRepository;

    public «BC2_AGGREGATE»RepositoryImpl(«BC2_AGGREGATE»PersistenceRepository persistenceRepository) {
        this.persistenceRepository = persistenceRepository;
    }

    @Override
    public «BC2_AGGREGATE» save(«BC2_AGGREGATE» entity) {
        var saved = persistenceRepository
                .save(«BC2_AGGREGATE»PersistenceAssembler.toPersistenceFromDomain(entity));
        return «BC2_AGGREGATE»PersistenceAssembler.toDomainFromPersistence(saved);
    }

    @Override
    public Optional<«BC2_AGGREGATE»> findById(Long id) {
        return persistenceRepository.findById(id)
                .map(«BC2_AGGREGATE»PersistenceAssembler::toDomainFromPersistence);
    }

    @Override
    public List<«BC2_AGGREGATE»> findAll() {
        return persistenceRepository.findAll().stream()
                .map(«BC2_AGGREGATE»PersistenceAssembler::toDomainFromPersistence)
                .toList();
    }
}
```

### 11.12 «BC2_AGGREGATE»Resource.java (Response DTO)

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».interfaces.rest.resources;

/**
 * REST resource representing a «BC2_AGGREGATE» response.
 *
 * ← INCLUIR LOS CAMPOS QUE PIDE TU EXAMEN EN EL RESPONSE
 *   (SIN createdAt/updatedAt)
 *
 * @author Victor Jhosef Laura Acosta
 */
public record «BC2_AGGREGATE»Resource(
        Long id
        // ← AGREGAR CAMPOS SEGÚN TU EXAMEN
) {}
```

### 11.13 «BC2_AGGREGATE»ResourceFromEntityAssembler.java

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».interfaces.rest.transform;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».interfaces.rest.resources.«BC2_AGGREGATE»Resource;

/**
 * Assembler for converting a «BC2_AGGREGATE» domain entity to a «BC2_AGGREGATE»Resource.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class «BC2_AGGREGATE»ResourceFromEntityAssembler {

    private «BC2_AGGREGATE»ResourceFromEntityAssembler() {}

    public static «BC2_AGGREGATE»Resource toResourceFromEntity(«BC2_AGGREGATE» entity) {
        return new «BC2_AGGREGATE»Resource(
                entity.getId()
                // ← MAPEAR CAMPOS SEGÚN TU EXAMEN
        );
    }
}
```

### 11.14 «BC2_AGGREGATE»sController.java (GET endpoint)

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».interfaces.rest;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».application.queryservices.I«BC2_AGGREGATE»QueryService;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.queries.GetAll«BC2_AGGREGATE»sQuery;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».interfaces.rest.resources.«BC2_AGGREGATE»Resource;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».interfaces.rest.transform.«BC2_AGGREGATE»ResourceFromEntityAssembler;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import java.util.List;

/**
 * REST controller for «BC2_AGGREGATE» operations.
 *
 * @author Victor Jhosef Laura Acosta
 */
@RestController
@RequestMapping(value = "«BC2_ENDPOINT»")
@Tag(name = "«BC2_AGGREGATE»s", description = "Operations related to «BC2_AGGREGATE» management")
public class «BC2_AGGREGATE»sController {

    private final I«BC2_AGGREGATE»QueryService queryService;

    public «BC2_AGGREGATE»sController(I«BC2_AGGREGATE»QueryService queryService) {
        this.queryService = queryService;
    }

    @GetMapping
    @Operation(summary = "Get all «BC2_AGGREGATE»s", description = "Returns all «BC2_AGGREGATE»s stored in the system.")
    @ApiResponses(value = {
            @ApiResponse(responseCode = "200", description = "List of «BC2_AGGREGATE»s returned successfully")
    })
    public ResponseEntity<List<«BC2_AGGREGATE»Resource>> getAll«BC2_AGGREGATE»s() {
        var query = new GetAll«BC2_AGGREGATE»sQuery();
        var result = queryService.handle(query);
        var resources = result.stream()
                .map(«BC2_AGGREGATE»ResourceFromEntityAssembler::toResourceFromEntity)
                .toList();
        return ResponseEntity.ok(resources);
    }
}
```

### 11.15 Data Seeding — «BC2_AGGREGATE»DataSeeder.java

> El EB siempre tiene un seeder que se activa con `ApplicationReadyEvent`.

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».infrastructure;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.model.aggregates.«BC2_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.repositories.I«BC2_AGGREGATE»Repository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.context.event.ApplicationReadyEvent;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Service;

/**
 * Seeds initial «BC2_AGGREGATE» data when the application starts.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class «BC2_AGGREGATE»DataSeeder {

    private static final Logger LOGGER = LoggerFactory.getLogger(«BC2_AGGREGATE»DataSeeder.class);
    private final I«BC2_AGGREGATE»Repository repository;

    public «BC2_AGGREGATE»DataSeeder(I«BC2_AGGREGATE»Repository repository) {
        this.repository = repository;
    }

    @EventListener
    public void seed(ApplicationReadyEvent event) {
        LOGGER.info("Starting «BC2_AGGREGATE» data seeding...");

        // Solo sembrar si no hay datos
        if (!repository.findAll().isEmpty()) {
            LOGGER.info("«BC2_AGGREGATE» data already seeded. Skipping.");
            return;
        }

        // ← CREAR Y GUARDAR CADA OBJETO SEGÚN LOS DATOS DE TU EXAMEN
        // Ejemplo:
        // var item1 = new «BC2_AGGREGATE»(/* parámetros del primer registro */);
        // repository.save(item1);
        //
        // var item2 = new «BC2_AGGREGATE»(/* parámetros del segundo registro */);
        // repository.save(item2);
        //
        // ... (todos los registros que pida tu examen)

        LOGGER.info("«BC2_AGGREGATE» data seeding completed.");
    }
}
```

---

## PASO 12 — BC1: «BC1_AGGREGATE» (el que expone el POST y usa ACL hacia BC2)

### 12.1 Enums de BC1

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.valueobjects;

/**
 * Enumeration for «NOMBRE_ENUM_BC1».
 * ← COMPLETAR SEGÚN TU EXAMEN (ej: MealType, DietaryTag, ClaimStatus)
 *
 * @author Victor Jhosef Laura Acosta
 */
public enum «NOMBRE_ENUM_BC1» {
    // VALORES SEGÚN TU EXAMEN
}
```

> Si hay varios enums (MealType Y DietaryTag, etc.), crear un archivo por cada uno.

### 12.2 «BC1_AGGREGATE».java (Aggregate Root)

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.aggregates;

import «PACKAGE_RAIZ».u202418655.shared.domain.model.aggregates.AbstractDomainAggregateRoot;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.commands.Create«BC1_AGGREGATE»Command;
import lombok.Getter;
import lombok.Setter;

/**
 * Aggregate root representing a «BC1_AGGREGATE» in the «BC1_NOMBRE» bounded context.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Getter
@Setter
public class «BC1_AGGREGATE» extends AbstractDomainAggregateRoot<«BC1_AGGREGATE»> {

    private Long id;

    // ← AGREGAR AQUÍ TODOS LOS ATRIBUTOS SEGÚN TU EXAMEN

    public «BC1_AGGREGATE»() {}

    /**
     * Creates a new «BC1_AGGREGATE» from the given command.
     *
     * @param command the create command
     */
    public «BC1_AGGREGATE»(Create«BC1_AGGREGATE»Command command) {
        // ← INICIALIZAR ATRIBUTOS DESDE EL COMANDO
        // Ejemplo:
        // this.ingredientCode = new IngredientCode(command.ingredientCode());
        // this.mealType = MealType.valueOf(command.mealType());
        // ...

        // Si debes emitir un evento al crear:
        // registerDomainEvent(new «DOMAIN_EVENT_NAME»(/* parámetros del evento */));
    }
}
```

### 12.3 Create«BC1_AGGREGATE»Command.java

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.commands;

/**
 * Command to create a new «BC1_AGGREGATE».
 *
 * ← PARÁMETROS SEGÚN LO QUE PIDA TU EXAMEN
 *    Los value objects se pasan como primitivos (String, Long, etc.)
 *    La conversión a value objects ocurre en el constructor del aggregate
 *
 * @author Victor Jhosef Laura Acosta
 */
public record Create«BC1_AGGREGATE»Command(
        // ← AGREGAR CAMPOS SEGÚN TU EXAMEN
        // Ejemplo:
        // String ingredientCode,
        // String mealType,
        // Double portionGrams,
        // ...
) {
    public Create«BC1_AGGREGATE»Command {
        // Validaciones básicas (null checks)
        // Ejemplo:
        // if (ingredientCode == null) throw new IllegalArgumentException("ingredientCode cannot be null");
    }
}
```

### 12.4 «DOMAIN_EVENT_NAME».java (Evento de integración)

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.events;

/**
 * Integration event published when a «BC1_AGGREGATE» is successfully created.
 * This event is consumed by the «BC2_NOMBRE» bounded context.
 *
 * ← PARÁMETROS SEGÚN TU EXAMEN
 *
 * @author Victor Jhosef Laura Acosta
 */
public record «DOMAIN_EVENT_NAME»(
        // ← AGREGAR LOS CAMPOS QUE EL EVENTO DEBE LLEVAR
        // Ejemplo (RecipeVault): String ingredientCode, Double portionGrams
        // Ejemplo (Whirlpool): Long policyId, Long claimId, LocalDateTime reportedAt
) {}
```

### 12.5 I«BC1_AGGREGATE»Repository.java

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.repositories;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.aggregates.«BC1_AGGREGATE»;
import java.util.Optional;

/**
 * Repository interface for «BC1_AGGREGATE» aggregate root.
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface I«BC1_AGGREGATE»Repository {

    «BC1_AGGREGATE» save(«BC1_AGGREGATE» entity);

    Optional<«BC1_AGGREGATE»> findById(Long id);

    // ← AGREGAR MÉTODOS ESPECÍFICOS QUE TU EXAMEN NECESITE
}
```

### 12.6 ACL — «BC2_AGGREGATE»ContextFacade.java

> El ACL es la "capa anticorrupción" que BC1 usa para consultar datos de BC2 **sin acoplarse directamente a su infraestructura**.

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».infrastructure.acl;

import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.repositories.I«BC2_AGGREGATE»Repository;
import org.springframework.stereotype.Service;

/**
 * Anti-Corruption Layer facade that allows the «BC1_NOMBRE» bounded context
 * to access information from the «BC2_NOMBRE» bounded context.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class «BC2_AGGREGATE»ContextFacade {

    private final I«BC2_AGGREGATE»Repository «BC2_AGGREGATE_LOWER»Repository;

    public «BC2_AGGREGATE»ContextFacade(I«BC2_AGGREGATE»Repository «BC2_AGGREGATE_LOWER»Repository) {
        this.«BC2_AGGREGATE_LOWER»Repository = «BC2_AGGREGATE_LOWER»Repository;
    }

    /**
     * Checks if a «BC2_AGGREGATE» with the given identifier exists.
     * ← ADAPTAR SEGÚN LO QUE TU EXAMEN NECESITE CONSULTAR
     *
     * @param identifier the identifier to look up
     * @return true if it exists; false otherwise
     */
    public boolean existsBy«CAMPO_CLAVE»(/* TIPO */ identifier) {
        // ← IMPLEMENTAR LA CONSULTA USANDO EL REPOSITORIO DE BC2
        // Ejemplo (RecipeVault):
        // return ingredientRepository.findAll().stream()
        //         .anyMatch(i -> i.getCode().value().equals(code));
        // Ejemplo (Whirlpool):
        // return policyRepository.findById(policyId).isPresent();
        return false; // ← REEMPLAZAR
    }

    /**
     * ← AGREGAR MÁS MÉTODOS SI TU EXAMEN LOS NECESITA
     * Por ejemplo, obtener el coverageStatus de una Policy
     */
}
```

### 12.7 I«BC1_AGGREGATE»CommandService.java

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».application.commandservices;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.commands.Create«BC1_AGGREGATE»Command;
import «PACKAGE_RAIZ».u202418655.shared.application.result.ApplicationError;
import «PACKAGE_RAIZ».u202418655.shared.application.result.Result;

/**
 * Service interface for handling «BC1_AGGREGATE» commands.
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface I«BC1_AGGREGATE»CommandService {

    Result<Long, ApplicationError> handle(Create«BC1_AGGREGATE»Command command);
}
```

### 12.8 «BC1_AGGREGATE»CommandServiceImpl.java

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».application.internal.commandservices;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».application.commandservices.I«BC1_AGGREGATE»CommandService;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.aggregates.«BC1_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.commands.Create«BC1_AGGREGATE»Command;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.repositories.I«BC1_AGGREGATE»Repository;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».infrastructure.acl.«BC2_AGGREGATE»ContextFacade;
import «PACKAGE_RAIZ».u202418655.shared.application.result.ApplicationError;
import «PACKAGE_RAIZ».u202418655.shared.application.result.Result;
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Service;

/**
 * Implementation of I«BC1_AGGREGATE»CommandService.
 *
 * Business rules enforced:
 * ← LISTAR LAS REGLAS DE NEGOCIO DE TU EXAMEN
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class «BC1_AGGREGATE»CommandServiceImpl implements I«BC1_AGGREGATE»CommandService {

    private final I«BC1_AGGREGATE»Repository repository;
    private final «BC2_AGGREGATE»ContextFacade «BC2_AGGREGATE_LOWER»ContextFacade;
    private final ApplicationEventPublisher eventPublisher;

    public «BC1_AGGREGATE»CommandServiceImpl(
            I«BC1_AGGREGATE»Repository repository,
            «BC2_AGGREGATE»ContextFacade «BC2_AGGREGATE_LOWER»ContextFacade,
            ApplicationEventPublisher eventPublisher) {
        this.repository = repository;
        this.«BC2_AGGREGATE_LOWER»ContextFacade = «BC2_AGGREGATE_LOWER»ContextFacade;
        this.eventPublisher = eventPublisher;
    }

    @Override
    public Result<Long, ApplicationError> handle(Create«BC1_AGGREGATE»Command command) {

        // BUSINESS RULE: Validar via ACL que el «BC2_AGGREGATE» referenciado existe
        // ← ADAPTAR SEGÚN TU EXAMEN
        // Ejemplo (RecipeVault):
        // if (!ingredientContextFacade.existsByCode(command.ingredientCode())) {
        //     return Result.failure(ApplicationError.notFound("Ingredient", command.ingredientCode()));
        // }
        //
        // Ejemplo (Whirlpool):
        // var policy = policyContextFacade.findById(command.policyId());
        // if (policy.isEmpty())
        //     return Result.failure(ApplicationError.notFound("Policy", command.policyId().toString()));
        // if (policy.get().getCoverageStatus() == CoverageStatus.EXPIRED)
        //     return Result.failure(ApplicationError.businessRuleViolation("policy-coverage", "Policy is EXPIRED"));

        // BUSINESS RULE: Otras validaciones de tu examen
        // ← AGREGAR AQUÍ

        // Crear el aggregate
        var entity = new «BC1_AGGREGATE»(command);

        try {
            entity = repository.save(entity);
        } catch (Exception e) {
            return Result.failure(ApplicationError.unexpected("create-«BC1_AGGREGATE_LOWER»", e.getMessage()));
        }

        // Publicar el evento de integración (si BC1 lo emite directamente)
        // ← Si el aggregate ya registra el evento via registerDomainEvent(), Spring lo publica automáticamente
        //    Si prefieres publicarlo manualmente aquí:
        // eventPublisher.publishEvent(new «DOMAIN_EVENT_NAME»(/* parámetros */));

        return Result.success(entity.getId());
    }
}
```

### 12.9 Event Handler en BC2 — «DOMAIN_EVENT_NAME»Handler.java

> BC2 escucha el evento que emite BC1 y actualiza sus datos.

```java
package «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».application.internal.eventhandlers;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.events.«DOMAIN_EVENT_NAME»;
import «PACKAGE_RAIZ».u202418655.«BC2_PACKAGE».domain.repositories.I«BC2_AGGREGATE»Repository;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

/**
 * Event handler for «DOMAIN_EVENT_NAME».
 * Updates «BC2_AGGREGATE» data when a «BC1_AGGREGATE» event is received.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class «DOMAIN_EVENT_NAME»Handler {

    private static final Logger LOGGER = LoggerFactory.getLogger(«DOMAIN_EVENT_NAME»Handler.class);
    private final I«BC2_AGGREGATE»Repository repository;

    public «DOMAIN_EVENT_NAME»Handler(I«BC2_AGGREGATE»Repository repository) {
        this.repository = repository;
    }

    @EventListener
    @Transactional
    public void on(«DOMAIN_EVENT_NAME» event) {
        LOGGER.info("Handling «DOMAIN_EVENT_NAME»: {}", event);

        // ← IMPLEMENTAR LA LÓGICA QUE PIDA TU EXAMEN
        // Ejemplo (RecipeVault): actualizar defaultPortionGrams si el nuevo es menor
        // var ingredient = repository.findByCode(event.ingredientCode())
        //         .orElseThrow(() -> new IllegalArgumentException("Ingredient not found"));
        // if (event.portionGrams() < ingredient.getDefaultPortionGrams()) {
        //     ingredient.setDefaultPortionGrams(event.portionGrams());
        //     repository.save(ingredient);
        // }
        //
        // Ejemplo (Whirlpool): actualizar lastClaimId en Policy
        // var policy = repository.findById(event.policyId())
        //         .orElseThrow(() -> new IllegalArgumentException("Policy not found"));
        // policy.setLastClaimId(event.claimId());
        // repository.save(policy);
    }
}
```

### 12.10 «BC1_AGGREGATE»PersistenceEntity.java, Repository, Assembler, RepositoryImpl

> Seguir **el mismo patrón exacto** que BC2 (pasos 11.8 al 11.11), cambiando «BC2» por «BC1» en todos los nombres. La estructura es idéntica.

### 12.11 Create«BC1_AGGREGATE»Resource.java (Request DTO)

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.resources;

/**
 * REST resource for creating a new «BC1_AGGREGATE».
 *
 * ← Los value objects se "aplanan" en campos primitivos.
 *   El id NO se incluye (es autogenerado).
 *
 * @author Victor Jhosef Laura Acosta
 */
public record Create«BC1_AGGREGATE»Resource(
        // ← AGREGAR CAMPOS SEGÚN TU EXAMEN
        // Todos los campos de value objects se pasan como primitivos
        // Ejemplo:
        // String ingredientCode,
        // String mealType,
        // Double portionGrams,
        // Double calories,
        // String dietaryTag,
        // String recordedAt        ← Si es fecha como String "yyyy-MM-dd HH:mm:ss"
) {
    public Create«BC1_AGGREGATE»Resource {
        // Validaciones en el boundary
        // Ejemplo:
        // if (ingredientCode == null || ingredientCode.isBlank())
        //     throw new IllegalArgumentException("ingredientCode cannot be null or blank");
    }
}
```

### 12.12 «BC1_AGGREGATE»Resource.java (Response DTO)

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.resources;

/**
 * REST resource representing a «BC1_AGGREGATE» response.
 *
 * ← INCLUIR LOS CAMPOS QUE PIDE TU EXAMEN (SIN createdAt/updatedAt)
 *
 * @author Victor Jhosef Laura Acosta
 */
public record «BC1_AGGREGATE»Resource(
        Long id
        // ← AGREGAR CAMPOS SEGÚN TU EXAMEN
) {}
```

### 12.13 Create«BC1_AGGREGATE»CommandFromResourceAssembler.java

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.transform;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.commands.Create«BC1_AGGREGATE»Command;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.resources.Create«BC1_AGGREGATE»Resource;

/**
 * Assembler for converting a Create«BC1_AGGREGATE»Resource to a Create«BC1_AGGREGATE»Command.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class Create«BC1_AGGREGATE»CommandFromResourceAssembler {

    private Create«BC1_AGGREGATE»CommandFromResourceAssembler() {}

    public static Create«BC1_AGGREGATE»Command toCommandFromResource(Create«BC1_AGGREGATE»Resource resource) {
        return new Create«BC1_AGGREGATE»Command(
                // ← MAPEAR CAMPOS DEL RESOURCE AL COMANDO
                // Ejemplo:
                // resource.ingredientCode(),
                // resource.mealType(),
                // resource.portionGrams()
        );
    }
}
```

### 12.14 «BC1_AGGREGATE»ResourceFromEntityAssembler.java

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.transform;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.aggregates.«BC1_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.resources.«BC1_AGGREGATE»Resource;

/**
 * Assembler for converting a «BC1_AGGREGATE» domain entity to a «BC1_AGGREGATE»Resource.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class «BC1_AGGREGATE»ResourceFromEntityAssembler {

    private «BC1_AGGREGATE»ResourceFromEntityAssembler() {}

    public static «BC1_AGGREGATE»Resource toResourceFromEntity(«BC1_AGGREGATE» entity) {
        return new «BC1_AGGREGATE»Resource(
                entity.getId()
                // ← MAPEAR CAMPOS SEGÚN TU EXAMEN
        );
    }
}
```

### 12.15 «BC1_AGGREGATE»sController.java (POST endpoint)

```java
package «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest;

import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».application.commandservices.I«BC1_AGGREGATE»CommandService;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.aggregates.«BC1_AGGREGATE»;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».domain.model.queries.GetAll«BC1_AGGREGATE»sQuery;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.resources.Create«BC1_AGGREGATE»Resource;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.resources.«BC1_AGGREGATE»Resource;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.transform.Create«BC1_AGGREGATE»CommandFromResourceAssembler;
import «PACKAGE_RAIZ».u202418655.«BC1_PACKAGE».interfaces.rest.transform.«BC1_AGGREGATE»ResourceFromEntityAssembler;
import «PACKAGE_RAIZ».u202418655.shared.application.result.ApplicationError;
import «PACKAGE_RAIZ».u202418655.shared.application.result.Result;
import «PACKAGE_RAIZ».u202418655.shared.interfaces.rest.transform.ResponseEntityAssembler;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.media.Content;
import io.swagger.v3.oas.annotations.media.Schema;
import io.swagger.v3.oas.annotations.responses.ApiResponse;
import io.swagger.v3.oas.annotations.responses.ApiResponses;
import io.swagger.v3.oas.annotations.tags.Tag;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

/**
 * REST controller for «BC1_AGGREGATE» operations.
 *
 * @author Victor Jhosef Laura Acosta
 */
@RestController
@RequestMapping(value = "«BC1_ENDPOINT»")
@Tag(name = "«BC1_AGGREGATE»s", description = "Operations related to «BC1_AGGREGATE» management")
public class «BC1_AGGREGATE»sController {

    private final I«BC1_AGGREGATE»CommandService commandService;
    // ← Si también necesita queryService (si BC1 también expone GET), agregarlo aquí

    public «BC1_AGGREGATE»sController(I«BC1_AGGREGATE»CommandService commandService) {
        this.commandService = commandService;
    }

    @PostMapping
    @Operation(
            summary = "Create a new «BC1_AGGREGATE»",
            description = "Registers a new «BC1_AGGREGATE» in the system. The id is auto-generated."
    )
    @ApiResponses(value = {
            @ApiResponse(responseCode = "201", description = "«BC1_AGGREGATE» created successfully",
                    content = @Content(schema = @Schema(implementation = «BC1_AGGREGATE»Resource.class))),
            @ApiResponse(responseCode = "400", description = "Invalid input data"),
            @ApiResponse(responseCode = "404", description = "Referenced «BC2_AGGREGATE» not found"),
            @ApiResponse(responseCode = "422", description = "Business rule violation")
    })
    public ResponseEntity<?> create«BC1_AGGREGATE»(@RequestBody Create«BC1_AGGREGATE»Resource resource) {
        var command = Create«BC1_AGGREGATE»CommandFromResourceAssembler.toCommandFromResource(resource);
        var result = commandService.handle(command);

        // Si el servicio retorna el ID y necesitas buscar la entidad completa para el response:
        // result.flatMap(id -> queryService.handle(new GetBy Id(id))
        //     .<Result<«BC1_AGGREGATE», ApplicationError>>map(Result::success)
        //     .orElseGet(() -> Result.failure(ApplicationError.notFound("«BC1_AGGREGATE»", id.toString()))));

        return ResponseEntityAssembler.toResponseEntityFromResult(
                result,
                // Si el result ya es el aggregate:
                // «BC1_AGGREGATE»ResourceFromEntityAssembler::toResourceFromEntity,
                // Si el result es el ID (Long) y necesitas construir el resource de otra forma:
                id -> new «BC1_AGGREGATE»Resource(id /*, otros campos si los tienes */),
                HttpStatus.CREATED);
    }
}
```

---

## PASO 13 — Verificación rápida antes de ejecutar

Antes de hacer `Run`, verificar que:

- [ ] Todos los `package` declarations coinciden con la estructura de carpetas
- [ ] Todos los `import` apuntan a los packages correctos
- [ ] `application.properties` tiene la URL de BD correcta y el esquema correcto
- [ ] `@EnableJpaAuditing` está en la clase principal
- [ ] El `SnakeCaseWithPluralizedTablePhysicalNamingStrategy` está referenciado en `application.properties`
- [ ] Las clases de `@Repository` e `@Service` están anotadas correctamente
- [ ] El `GlobalExceptionHandler` tiene `@RestControllerAdvice`

---

## PASO 14 — Ejecutar y verificar

```bash
# Compilar primero
mvn clean compile

# Si compila OK, ejecutar
mvn spring-boot:run
```

### Verificar endpoints

```
GET http://localhost:«PUERTO»/swagger-ui/index.html     ← Swagger UI
GET http://localhost:«PUERTO»/v3/api-docs               ← OpenAPI JSON
GET «BC2_ENDPOINT»                                      ← Debe retornar los datos del seeder
POST «BC1_ENDPOINT»                                     ← Debe crear y retornar 201
```

---

## PASO 15 — Empaquetar y entregar

### Limpiar antes de ZIP

```bash
# En IntelliJ: Build → Clean Project
# O en terminal:
mvn clean
```

Eliminar manualmente:
- `target/` ← generada por Maven
- `.idea/` ← configuración del IDE
- `*.iml` ← archivos de IntelliJ

### Crear el ZIP

Nombre: `eb«NRC»u202418655.zip`

El ZIP debe contener:
```
eb«NRC»u202418655/
├── src/
├── pom.xml
└── README.md
```

**NO incluir:** `target/`, `.idea/`, `*.iml`

---

## Checklist Final

### Shared (SIEMPRE IGUAL - copiar/pegar)
- [ ] AbstractDomainAggregateRoot.java
- [ ] Result.java + ApplicationError.java
- [ ] SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java
- [ ] AuditableAbstractPersistenceEntity.java
- [ ] OpenApiConfiguration.java
- [ ] LocaleConfiguration.java
- [ ] ErrorResource.java + MessageResource.java
- [ ] ErrorResponseAssembler.java + ResponseEntityAssembler.java
- [ ] GlobalExceptionHandler.java
- [ ] «SHARED_VALUE_OBJECT».java (según tu examen)

### BC2 (GET + Data Seeding)
- [ ] «BC2_AGGREGATE».java (aggregate)
- [ ] I«BC2_AGGREGATE»Repository.java
- [ ] GetAll«BC2_AGGREGATE»sQuery.java
- [ ] I«BC2_AGGREGATE»QueryService.java + impl
- [ ] PersistenceEntity + PersistenceRepository + PersistenceAssembler + RepositoryImpl
- [ ] «BC2_AGGREGATE»Resource.java + Assembler
- [ ] «BC2_AGGREGATE»sController.java (GET)
- [ ] «BC2_AGGREGATE»DataSeeder.java (ApplicationReadyEvent)
- [ ] Event handler para «DOMAIN_EVENT_NAME» (si BC2 actualiza datos al recibir evento de BC1)

### BC1 (POST + ACL + Evento)
- [ ] Enums de BC1 (MealType, ClaimStatus, etc.)
- [ ] «BC1_AGGREGATE».java (aggregate)
- [ ] Create«BC1_AGGREGATE»Command.java
- [ ] «DOMAIN_EVENT_NAME».java (evento de integración)
- [ ] I«BC1_AGGREGATE»Repository.java
- [ ] I«BC1_AGGREGATE»CommandService.java + impl (con ACL y publicación del evento)
- [ ] «BC2_AGGREGATE»ContextFacade.java (ACL)
- [ ] PersistenceEntity + PersistenceRepository + PersistenceAssembler + RepositoryImpl
- [ ] Create«BC1_AGGREGATE»Resource.java + «BC1_AGGREGATE»Resource.java
- [ ] Assemblers
- [ ] «BC1_AGGREGATE»sController.java (POST)

### Configuración
- [ ] application.properties completo
- [ ] messages.properties + messages_es.properties
- [ ] README.md
- [ ] pom.xml con dependencia pluralize y Java version correcta
- [ ] @EnableJpaAuditing en Application.java

### Entrega
- [ ] `mvn clean compile` sin errores
- [ ] Swagger UI accesible
- [ ] GET «BC2_ENDPOINT» retorna datos del seeder
- [ ] POST «BC1_ENDPOINT» retorna 201 con datos correctos
- [ ] Sin `target/` ni `.idea/` en el ZIP
- [ ] ZIP nombrado `eb«NRC»u202418655.zip`
