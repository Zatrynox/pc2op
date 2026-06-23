# Project Facturease Perú - Invoice Management v1.0

Datos del alumno
- Nombre Victor Jhosef Laura Acosta
- Código u202418655
- NRC 11990
- Profesor Juan Anthonio Flores Moroco
- Proyecto `pc211990u202418655`
- Package raíz `pe.com.facturease.platform.u202418655`
- Puerto 8097
- Esquema BD `facturease_db`

---

## Configuración inicial

### Base de datos

Abrir el `pgAdmin`, ingrese la contraseña `12345678` y crear la base de datos `facturease_db`.

Ejecutar el siguiente SQL para crear el esquema

```sql
CREATE SCHEMA facturease_db;
```

---

## Creación del proyecto (Spring Initializr)

Cargar el navegador y pegar el siguiente enlace

```
https://start.spring.io#!type=maven-project&language=java&platformVersion=4.1.0&packaging=jar&configurationFileFormat=properties&jvmVersion=26&groupId=pe.com.facturease.platform&artifactId=pc211990u202418655&packageName=pe.com.facturease.platform.u202418655&dependencies=data-jpa,validation,web,devtools,postgresql,lombok,springdoc-openapi
```

Generar el proyecto Spring. Guardarlo en la carpeta `IdeaProjects1ASI0729` y luego abrirlo en `IntelliJ IDEA`.

---

### Configuración del SDK

Hacer click en `File` → `Project Structure`. Seleccionar `Project` de `Project Settings` y seleccionar la versión `26` de Java para el `SDK`. Si no está instalado, descárguelo. Luego Seleccionar `SDKs` de `Platform Settings` y seleccionar la versión antes seleccionada. Hacer click en `OK`.

---

### Archivo pom.xml

Abrir `pom.xml` y modificar la versión de Java en `properties`

```xml
<properties>
    <java.version>26</java.version>
</properties>
```

Agregar la siguiente dependencia después de `springdoc-openapi`

```xml
<!-- https://mvnrepository.com/artifact/io.github.encryptorcode/pluralize -->
<dependency>
    <groupId>io.github.encryptorcode</groupId>
    <artifactId>pluralize</artifactId>
    <version>1.0.0</version>
</dependency>
```

Verificar que el `pom.xml` completo quede así

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>4.1.0</version>
        <relativePath/>
    </parent>
    <groupId>pe.com.facturease.platform</groupId>
    <artifactId>pc211990u202418655</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>pc211990u202418655</name>
    <description>Facturease Peru - Invoice Management RESTful API</description>
    <properties>
        <java.version>26</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <scope>runtime</scope>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.postgresql</groupId>
            <artifactId>postgresql</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springdoc</groupId>
            <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
            <version>3.0.2</version>
        </dependency>
        <!-- https://mvnrepository.com/artifact/io.github.encryptorcode/pluralize -->
        <dependency>
            <groupId>io.github.encryptorcode</groupId>
            <artifactId>pluralize</artifactId>
            <version>1.0.0</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <executions>
                    <execution>
                        <id>default-compile</id>
                        <phase>compile</phase>
                        <goals><goal>compile</goal></goals>
                        <configuration>
                            <annotationProcessorPaths>
                                <path>
                                    <groupId>org.projectlombok</groupId>
                                    <artifactId>lombok</artifactId>
                                </path>
                            </annotationProcessorPaths>
                        </configuration>
                    </execution>
                    <execution>
                        <id>default-testCompile</id>
                        <phase>test-compile</phase>
                        <goals><goal>testCompile</goal></goals>
                        <configuration>
                            <annotationProcessorPaths>
                                <path>
                                    <groupId>org.projectlombok</groupId>
                                    <artifactId>lombok</artifactId>
                                </path>
                            </annotationProcessorPaths>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

---

### Proyecto Application

Abrir el archivo `Pc211990u202418655Application.java` ubicado en el package `pe.com.facturease.platform.u202418655` y reemplazar con

```java
package pe.com.facturease.platform.u202418655;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

/**
 * Main application class for the Facturease Peru platform.
 *
 * <p>Enables JPA auditing for automatic management of entity creation
 * and last-updated timestamps across all aggregate roots.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@SpringBootApplication
@EnableJpaAuditing
public class Pc211990u202418655Application {

    /**
     * Entry point of the Spring Boot application.
     *
     * @param args command-line arguments
     */
    public static void main(String[] args) {
        SpringApplication.run(Pc211990u202418655Application.class, args);
    }
}
```

---

### Archivo application.properties

Abrir el archivo `application.properties` en `src/main/resources` y reemplazar con

```ini
spring.application.name=pc211990u202418655

# Spring DataSource Configuration
spring.datasource.url=jdbc:postgresql://localhost:5432/facturease_db?currentSchema=facturease_db
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
spring.jpa.hibernate.naming.physical-strategy=pe.com.facturease.platform.u202418655.shared.infrastructure.persistence.jpa.configuration.strategy.SnakeCaseWithPluralizedTablePhysicalNamingStrategy

server.port=8097

documentation.application.description=Facturease Peru - Invoice Management RESTful API
documentation.application.version=@project.version@

spring.messages.basename=messages
spring.messages.encoding=UTF-8
```

> **Nota sobre el esquema:** dado que el enunciado indica que la base de datos relacional debe usar el esquema `facturease_db` (mismo nombre que la base de datos), si su servidor PostgreSQL requiere que el esquema sea distinto del nombre de la base, puede crear adicionalmente la base `facturease` y dentro de ella el esquema `facturease_db`, ajustando la URL JDBC a `jdbc:postgresql://localhost:5432/facturease?currentSchema=facturease_db`.

---

### Archivo messages.properties (i18n - Inglés)

Crear `src/main/resources/messages.properties`

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

---

### Archivo messages_es.properties (i18n - Español)

Crear `src/main/resources/messages_es.properties`

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

### README.md

Crear `README.md` en la raíz del proyecto

```markdown
# Facturease Peru - Invoice Management API

## Description
Facturease Peru Invoice Management API is a RESTful web service built with Spring Boot
that provides backend support for registering electronic Invoices issued by corporate
clients of Facturease Peru. The API enforces all business rules required by the platform,
including unique invoiceIdentifier validation, IGV (18%) tax consistency validation between
netAmount and calculatedTax, and itemCount validation when an Invoice is registered with a
status other than DRAFT.

## Technologies
- Java 26
- Spring Boot 4.1.0
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

Crear toda la siguiente estructura bajo `src/main/java/pe/com/facturease/platform/u202418655/shared`

```
shared
├── application
│   └── result
│       ├── Result.java
│       └── ApplicationError.java
├── domain
│   └── model
│       ├── aggregates
│       │   └── AbstractDomainAggregateRoot.java
│       └── valueobjects
│           └── TaxSummary.java
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

> **Nota:** A diferencia del proyecto de ejemplo (Sensibo), aquí el bounded context `shared` **sí** contiene un value object de dominio: `TaxSummary`, tal como lo exige expresamente el enunciado de Facturease Perú ("Especifica que el tipo TaxSummary es un value object compuesto ubicado en el bounded context shared"). Por eso se agrega el paquete `shared/domain/model/valueobjects`.

---

## Shared Package - Archivos

### 01. AbstractDomainAggregateRoot.java

Ruta `shared/domain/model/aggregates/AbstractDomainAggregateRoot.java`

Package `pe.com.facturease.platform.u202418655.shared.domain.model.aggregates`

```java
package pe.com.facturease.platform.u202418655.shared.domain.model.aggregates;

import org.springframework.data.domain.AbstractAggregateRoot;

import java.util.Collection;

/**
 * Base class for all domain aggregate roots.
 *
 * <p>Extends Spring Data Commons' {@link AbstractAggregateRoot} to gain
 * domain event registration support without pulling in any JPA or
 * persistence concern. Identity and auditing are intentionally left
 * to the infrastructure layer.</p>
 *
 * <p>All bounded-context domain aggregate roots should extend this class.</p>
 *
 * @param <T> the concrete aggregate root type
 * @author Victor Jhosef Laura Acosta
 */
public abstract class AbstractDomainAggregateRoot<T extends AbstractDomainAggregateRoot<T>>
        extends AbstractAggregateRoot<T> {

    /**
     * Registers a domain event to be published after this aggregate is saved.
     *
     * @param event the domain event to register
     */
    protected void registerDomainEvent(Object event) {
        super.registerEvent(event);
    }

    /**
     * Returns all domain events registered on this aggregate since the last publication.
     *
     * @return the registered domain events
     */
    @Override
    public Collection<Object> domainEvents() {
        return super.domainEvents();
    }

    /**
     * Clears all registered domain events.
     */
    @Override
    public void clearDomainEvents() {
        super.clearDomainEvents();
    }
}
```

---

### 02. TaxSummary.java

Ruta `shared/domain/model/valueobjects/TaxSummary.java`

Package `pe.com.facturease.platform.u202418655.shared.domain.model.valueobjects`

```java
package pe.com.facturease.platform.u202418655.shared.domain.model.valueobjects;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.util.Objects;

/**
 * Value object representing the tax summary of a fiscal document.
 *
 * <p>Composed of {@code netAmount} (the net value before taxes) and
 * {@code calculatedTax} (the accumulated tax amount). This value object
 * is shared across bounded contexts that need to express IGV-related
 * (Peruvian general sales tax) tax amounts.</p>
 *
 * <p>Validates that {@code calculatedTax} is consistent with the application
 * of the eighteen percent (18%) national IGV rate over {@code netAmount}.</p>
 *
 * @param netAmount     the net amount before taxes
 * @param calculatedTax the accumulated tax amount
 * @author Victor Jhosef Laura Acosta
 */
public record TaxSummary(BigDecimal netAmount, BigDecimal calculatedTax) {

    private static final BigDecimal IGV_RATE = new BigDecimal("0.18");

    /**
     * Creates a TaxSummary with validation.
     *
     * @throws IllegalArgumentException if either amount is null, negative,
     *                                   or if calculatedTax is not consistent
     *                                   with the 18% IGV rate over netAmount
     */
    public TaxSummary {
        if (Objects.isNull(netAmount) || netAmount.signum() < 0)
            throw new IllegalArgumentException(
                    "[TaxSummary] netAmount cannot be null or negative");
        if (Objects.isNull(calculatedTax) || calculatedTax.signum() < 0)
            throw new IllegalArgumentException(
                    "[TaxSummary] calculatedTax cannot be null or negative");

        BigDecimal expectedTax = netAmount.multiply(IGV_RATE)
                .setScale(2, RoundingMode.HALF_UP);
        BigDecimal roundedCalculatedTax = calculatedTax.setScale(2, RoundingMode.HALF_UP);

        if (expectedTax.compareTo(roundedCalculatedTax) != 0)
            throw new IllegalArgumentException(
                    "[TaxSummary] calculatedTax is not consistent with 18% IGV over netAmount");
    }

    /**
     * Default constructor required for JPA.
     */
    public TaxSummary() {
        this(BigDecimal.ZERO, BigDecimal.ZERO);
    }
}
```

---

### 03. Result.java

Ruta `shared/application/result/Result.java`

Package `pe.com.facturease.platform.u202418655.shared.application.result`

```java
package pe.com.facturease.platform.u202418655.shared.application.result;

import java.util.Optional;
import java.util.function.Function;

/**
 * Represents the result of a command or operation that can either succeed
 * with a value or fail with an error.
 *
 * <p>This type encodes the possibility of failure in the type system,
 * making error cases explicit and composable.</p>
 *
 * @param <T> The type of the successful result value
 * @param <E> The type of the error/failure information
 * @author Victor Jhosef Laura Acosta
 */
public sealed interface Result<T, E> {

    /**
     * Represents a successful result containing a value.
     */
    record Success<T, E>(T value) implements Result<T, E> {}

    /**
     * Represents a failed result containing error information.
     */
    record Failure<T, E>(E error) implements Result<T, E> {}

    /**
     * Creates a successful result with the given value.
     */
    static <T, E> Result<T, E> success(T value) {
        return new Success<>(value);
    }

    /**
     * Creates a failed result with the given error.
     */
    static <T, E> Result<T, E> failure(E error) {
        return new Failure<>(error);
    }

    /**
     * Returns true if this result is a success.
     */
    default boolean isSuccess() {
        return this instanceof Success;
    }

    /**
     * Returns true if this result is a failure.
     */
    default boolean isFailure() {
        return this instanceof Failure;
    }

    /**
     * Returns an Optional containing the value if success, otherwise empty.
     */
    default Optional<T> toOptional() {
        return switch (this) {
            case Success<T, E> s -> Optional.of(s.value);
            case Failure<T, E> f -> Optional.empty();
        };
    }

    /**
     * Extracts the value if successful, or returns the given default if failed.
     */
    default T getOrElse(T defaultValue) {
        return switch (this) {
            case Success<T, E> s -> s.value;
            case Failure<T, E> f -> defaultValue;
        };
    }

    /**
     * Applies a function to the error if this is a failure.
     */
    default <E2> Result<T, E2> mapError(Function<E, E2> f) {
        return switch (this) {
            case Success<T, E> s -> Result.success(s.value);
            case Failure<T, E> failure -> Result.failure(f.apply(failure.error));
        };
    }

    /**
     * Applies a function to the value if this is a success, producing a new Result.
     */
    default <T2> Result<T2, E> flatMap(Function<T, Result<T2, E>> f) {
        return switch (this) {
            case Success<T, E> s -> f.apply(s.value);
            case Failure<T, E> failure -> Result.failure(failure.error);
        };
    }

    /**
     * Applies a function to the value if this is a success.
     */
    default <T2> Result<T2, E> map(Function<T, T2> f) {
        return switch (this) {
            case Success<T, E> s -> Result.success(f.apply(s.value));
            case Failure<T, E> failure -> Result.failure(failure.error);
        };
    }

    /**
     * Applies a function to the error if this is a failure, allowing fallback recovery.
     */
    default Result<T, E> recover(Function<E, Result<T, E>> f) {
        return switch (this) {
            case Success<T, E> s -> this;
            case Failure<T, E> failure -> f.apply(failure.error);
        };
    }
}
```

---

### 04. ApplicationError.java

Ruta `shared/application/result/ApplicationError.java`

Package `pe.com.facturease.platform.u202418655.shared.application.result`

```java
package pe.com.facturease.platform.u202418655.shared.application.result;

/**
 * Represents an error that occurred in the application layer.
 *
 * <p>Designed to be easily mapped to HTTP responses with structured
 * error information.</p>
 *
 * @param code     A machine-readable error code (e.g., INVOICE_CONFLICT)
 * @param message  A human-readable error message
 * @param details  Optional additional context about the error
 * @author Victor Jhosef Laura Acosta
 */
public record ApplicationError(String code, String message, String details) {

    /**
     * Creates an ApplicationError with code and message only.
     */
    public ApplicationError(String code, String message) {
        this(code, message, null);
    }

    /**
     * Validation error: input data is invalid or violates constraints.
     */
    public static ApplicationError validationError(String fieldOrConcept, String reason) {
        return new ApplicationError("VALIDATION_ERROR",
                "Validation failed: %s".formatted(fieldOrConcept), reason);
    }

    /**
     * Not found error: the requested resource does not exist.
     */
    public static ApplicationError notFound(String resourceType, String identifier) {
        return new ApplicationError("%s_NOT_FOUND".formatted(resourceType.toUpperCase()),
                "%s not found: %s".formatted(resourceType, identifier), null);
    }

    /**
     * Business rule violation error: operation violates domain constraints.
     */
    public static ApplicationError businessRuleViolation(String rule, String reason) {
        return new ApplicationError("BUSINESS_RULE_VIOLATION",
                "Business rule violation: %s".formatted(rule), reason);
    }

    /**
     * Conflict error: operation cannot be completed due to conflicting state.
     */
    public static ApplicationError conflict(String resource, String reason) {
        return new ApplicationError("%s_CONFLICT".formatted(resource.toUpperCase()),
                "Conflict with %s".formatted(resource), reason);
    }

    /**
     * Unexpected error: something went wrong that shouldn't have.
     */
    public static ApplicationError unexpected(String context, String reason) {
        return new ApplicationError("UNEXPECTED_ERROR",
                "Unexpected error in %s".formatted(context), reason);
    }
}
```

---

### 05. SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java

Ruta `shared/infrastructure/persistence/jpa/configuration/strategy/SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java`

Package `pe.com.facturease.platform.u202418655.shared.infrastructure.persistence.jpa.configuration.strategy`

```java
package pe.com.facturease.platform.u202418655.shared.infrastructure.persistence.jpa.configuration.strategy;

import org.hibernate.boot.model.naming.Identifier;
import org.hibernate.boot.model.naming.PhysicalNamingStrategy;
import org.hibernate.engine.jdbc.env.spi.JdbcEnvironment;

import static io.github.encryptorcode.pluralize.Pluralize.pluralize;

/**
 * Snake Case With Pluralized Table Physical Naming Strategy.
 *
 * <p>{@link PhysicalNamingStrategy} implementation that converts entity
 * names to snake_case and table names to pluralized snake_case.</p>
 *
 * @since 1.0.0
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

    /**
     * Converts an identifier to snake_case.
     */
    private Identifier toSnakeCase(final Identifier identifier) {
        if (identifier == null) return null;
        final String regex = "([a-z])([A-Z])";
        final String replacement = "$1_$2";
        final String newName = identifier.getText()
                .replaceAll(regex, replacement)
                .toLowerCase();
        return Identifier.toIdentifier(newName);
    }

    /**
     * Converts an identifier to its plural form.
     */
    private Identifier toPlural(final Identifier identifier) {
        return Identifier.toIdentifier(pluralize(identifier.getText()));
    }
}
```

---

### 06. AuditableAbstractPersistenceEntity.java

Ruta `shared/infrastructure/persistence/jpa/entities/AuditableAbstractPersistenceEntity.java`

Package `pe.com.facturease.platform.u202418655.shared.infrastructure.persistence.jpa.entities`

```java
package pe.com.facturease.platform.u202418655.shared.infrastructure.persistence.jpa.entities;

import jakarta.persistence.*;
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import java.util.Date;

/**
 * Base JPA persistence entity for all persistence entities that require auditing.
 *
 * <p>Provides {@code id}, {@code createdAt}, and {@code updatedAt} fields.
 * This class intentionally lives in the infrastructure layer to keep JPA
 * and Spring Data auditing concerns out of the domain model.</p>
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

    /**
     * Sets the id for reconstruction from an existing domain object.
     *
     * @param id the persistence identity to assign
     */
    public void setId(Long id) {
        this.id = id;
    }
}
```

---

### 07. OpenApiConfiguration.java

Ruta `shared/infrastructure/documentation/openapi/configuration/OpenApiConfiguration.java`

Package `pe.com.facturease.platform.u202418655.shared.infrastructure.documentation.openapi.configuration`

```java
package pe.com.facturease.platform.u202418655.shared.infrastructure.documentation.openapi.configuration;

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
     * Builds the OpenAPI document used by Swagger UI and client generation tools.
     *
     * @return configured OpenAPI descriptor
     */
    @Bean
    public OpenAPI facturperuPlatformOpenApi() {
        var openApi = new OpenAPI();
        openApi
                .info(new Info()
                        .title(this.applicationName)
                        .description(this.applicationDescription)
                        .version(this.applicationVersion)
                        .contact(new Contact()
                                .name("Facturease Peru")
                                .email("support@facturease.com.pe")
                                .url("https://www.facturease.com.pe"))
                        .license(new License()
                                .name("Apache 2.0")
                                .url("https://www.apache.org/licenses/LICENSE-2.0.html")))
                .externalDocs(new ExternalDocumentation()
                        .description("Facturease Peru Platform Wiki Documentation")
                        .url("https://facturease.wiki.github.io/docs"));

        openApi.servers(List.of(
                new Server().url("http://localhost:8097")
                        .description("Local Development Environment"),
                new Server().url("https://staging-api.facturease.com.pe")
                        .description("Staging Environment"),
                new Server().url("https://api.facturease.com.pe")
                        .description("Production Environment")
        ));
        return openApi;
    }
}
```

---

### 08. LocaleConfiguration.java

Ruta `shared/infrastructure/i18n/configuration/LocaleConfiguration.java`

Package `pe.com.facturease.platform.u202418655.shared.infrastructure.i18n.configuration`

```java
package pe.com.facturease.platform.u202418655.shared.infrastructure.i18n.configuration;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.LocaleResolver;
import org.springframework.web.servlet.i18n.AcceptHeaderLocaleResolver;

import java.util.List;
import java.util.Locale;

/**
 * Configures locale resolution for REST requests.
 *
 * <p>Uses the Accept-Language header to determine the response language.
 * Supports English (default) and Spanish.</p>
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

### 09. ErrorResource.java

Ruta `shared/interfaces/rest/resources/ErrorResource.java`

Package `pe.com.facturease.platform.u202418655.shared.interfaces.rest.resources`

```java
package pe.com.facturease.platform.u202418655.shared.interfaces.rest.resources;

import com.fasterxml.jackson.annotation.JsonInclude;

/**
 * Standard error response resource returned from REST endpoints.
 *
 * @param code    A machine-readable error code
 * @param message A human-readable error message
 * @param details Optional additional context about the error
 * @author Victor Jhosef Laura Acosta
 */
@JsonInclude(JsonInclude.Include.NON_NULL)
public record ErrorResource(String code, String message, String details) {

    /**
     * Creates an ErrorResource from code and message only (no details).
     */
    public ErrorResource(String code, String message) {
        this(code, message, null);
    }
}
```

---

### 10. MessageResource.java

Ruta `shared/interfaces/rest/resources/MessageResource.java`

Package `pe.com.facturease.platform.u202418655.shared.interfaces.rest.resources`

```java
package pe.com.facturease.platform.u202418655.shared.interfaces.rest.resources;

/**
 * Resource used for simple success or informational REST responses.
 *
 * @param message An informational message
 * @author Victor Jhosef Laura Acosta
 */
public record MessageResource(String message) {}
```

---

### 11. ErrorResponseAssembler.java

Ruta `shared/interfaces/rest/transform/ErrorResponseAssembler.java`

Package `pe.com.facturease.platform.u202418655.shared.interfaces.rest.transform`

```java
package pe.com.facturease.platform.u202418655.shared.interfaces.rest.transform;

import pe.com.facturease.platform.u202418655.shared.application.result.ApplicationError;
import pe.com.facturease.platform.u202418655.shared.interfaces.rest.resources.ErrorResource;
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

    /**
     * Maps an ApplicationError to an appropriate HTTP ResponseEntity.
     *
     * @param error the ApplicationError to map
     * @return a ResponseEntity with the appropriate HTTP status and error resource
     */
    public static ResponseEntity<ErrorResource> toErrorResponseFromApplicationError(ApplicationError error) {
        HttpStatusCode status = toStatusFromErrorCode(error.code());
        return new ResponseEntity<>(
                new ErrorResource(error.code(), error.message(), error.details()), status);
    }

    /**
     * Determines the appropriate HTTP status code for a given error code.
     *
     * @param errorCode the error code string
     * @return the corresponding HttpStatus
     */
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

### 12. ResponseEntityAssembler.java

Ruta `shared/interfaces/rest/transform/ResponseEntityAssembler.java`

Package `pe.com.facturease.platform.u202418655.shared.interfaces.rest.transform`

```java
package pe.com.facturease.platform.u202418655.shared.interfaces.rest.transform;

import pe.com.facturease.platform.u202418655.shared.application.result.ApplicationError;
import pe.com.facturease.platform.u202418655.shared.application.result.Result;
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

    /**
     * Converts a Result into an HTTP response using the provided assembler.
     *
     * @param result                    the application result
     * @param successResourceAssembler  function that maps success value to response resource
     * @param successStatus             HTTP status to use for successful responses
     * @param <T>                       success value type
     * @param <R>                       success response resource type
     * @return response entity for success or failure
     */
    public static <T, R> ResponseEntity<?> toResponseEntityFromResult(
            Result<T, ApplicationError> result,
            Function<T, R> successResourceAssembler,
            HttpStatusCode successStatus
    ) {
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

### 13. GlobalExceptionHandler.java

Ruta `shared/interfaces/rest/GlobalExceptionHandler.java`

Package `pe.com.facturease.platform.u202418655.shared.interfaces.rest`

```java
package pe.com.facturease.platform.u202418655.shared.interfaces.rest;

import pe.com.facturease.platform.u202418655.shared.application.result.ApplicationError;
import pe.com.facturease.platform.u202418655.shared.interfaces.rest.transform.ErrorResponseAssembler;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.MethodArgumentNotValidException;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

/**
 * Global exception handler for REST API.
 *
 * <p>Provides centralized exception handling ensuring all unhandled
 * exceptions are translated to consistent HTTP responses.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@RestControllerAdvice
public class GlobalExceptionHandler {

    /**
     * Handles validation exceptions from Spring's request body validation.
     */
    @ExceptionHandler(MethodArgumentNotValidException.class)
    public ResponseEntity<?> handleMethodArgumentNotValid(MethodArgumentNotValidException ex) {
        var fieldErrors = ex.getBindingResult().getFieldErrors();
        var errorDetails = fieldErrors.isEmpty()
                ? "Request validation failed"
                : fieldErrors.stream()
                        .map(error -> "Field %s: %s".formatted(
                                error.getField(), error.getDefaultMessage()))
                        .reduce((a, b) -> a + "; " + b)
                        .orElse("Request validation failed");

        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.validationError("request-body", errorDetails));
    }

    /**
     * Handles invalid request arguments such as malformed values.
     */
    @ExceptionHandler(IllegalArgumentException.class)
    public ResponseEntity<?> handleIllegalArgumentException(IllegalArgumentException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.validationError("request-argument",
                        ex.getMessage() != null ? ex.getMessage() : "Invalid request argument"));
    }

    /**
     * Handles illegal state exceptions raised by business rule enforcement.
     */
    @ExceptionHandler(IllegalStateException.class)
    public ResponseEntity<?> handleIllegalStateException(IllegalStateException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.businessRuleViolation("invoice-state",
                        ex.getMessage() != null ? ex.getMessage() : "Invalid request state"));
    }

    /**
     * Handles unexpected runtime exceptions.
     */
    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<?> handleRuntimeException(RuntimeException ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.unexpected("global-exception-handler",
                        ex.getMessage() != null ? ex.getMessage() : "An unexpected error occurred"));
    }

    /**
     * Handles all other exceptions not matched by specific handlers.
     */
    @ExceptionHandler(Exception.class)
    public ResponseEntity<?> handleException(Exception ex) {
        return ErrorResponseAssembler.toErrorResponseFromApplicationError(
                ApplicationError.unexpected("global-exception-handler",
                        ex.getMessage() != null ? ex.getMessage() : "An unexpected error occurred"));
    }
}
```

---

## Billing Bounded Context - Estructura de carpetas

Crear la siguiente estructura bajo `src/main/java/pe/com/facturease/platform/u202418655/billing`

```
billing
├── domain
│   ├── model
│   │   ├── aggregates
│   │   │   └── Invoice.java
│   │   ├── commands
│   │   │   └── CreateInvoiceCommand.java
│   │   ├── queries
│   │   │   └── GetInvoiceById.java
│   │   └── valueobjects
│   │       ├── InvoiceIdentifier.java
│   │       └── InvoiceStatus.java
│   └── repositories
│       └── InvoiceRepository.java
├── application
│   ├── commandservices
│   │   └── InvoiceCommandService.java
│   ├── queryservices
│   │   └── InvoiceQueryService.java
│   └── internal
│       ├── commandservices
│       │   └── InvoiceCommandServiceImpl.java
│       └── queryservices
│           └── InvoiceQueryServiceImpl.java
├── infrastructure
│   └── persistence
│       └── jpa
│           ├── adapters
│           │   └── InvoiceRepositoryImpl.java
│           ├── assemblers
│           │   └── InvoicePersistenceAssembler.java
│           ├── converters
│           │   └── InvoiceIdentifierPersistenceConverter.java
│           ├── entities
│           │   └── InvoicePersistenceEntity.java
│           └── repositories
│               └── InvoicePersistenceRepository.java
└── interfaces
    └── rest
        ├── InvoicesController.java
        ├── resources
        │   ├── CreateInvoiceResource.java
        │   └── InvoiceResource.java
        └── transform
            ├── CreateInvoiceCommandFromResourceAssembler.java
            └── InvoiceResourceFromEntityAssembler.java
```

---

## Billing - Domain Layer (Value Objects)

### 14. InvoiceStatus.java

Ruta `billing/domain/model/valueobjects/InvoiceStatus.java`

Package `pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects`

```java
package pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects;

import java.util.Arrays;

/**
 * Enumeration representing the status of an electronic Invoice.
 *
 * <p>Defines the available statuses:</p>
 * <ul>
 *   <li>DRAFT - default status when no status is specified</li>
 *   <li>ISSUED - the invoice has been formally issued to SUNAT</li>
 *   <li>PAID - the invoice has been fully paid</li>
 * </ul>
 *
 * @author Victor Jhosef Laura Acosta
 */
public enum InvoiceStatus {
    DRAFT,
    ISSUED,
    PAID;

    /**
     * Returns the InvoiceStatus from a given name (case-insensitive).
     *
     * @param name the name of the status
     * @return the matching InvoiceStatus
     * @throws IllegalArgumentException if no status matches
     */
    public static InvoiceStatus fromString(String name) {
        return Arrays.stream(InvoiceStatus.values())
                .filter(status -> status.name().equalsIgnoreCase(name))
                .findFirst()
                .orElseThrow(() ->
                        new IllegalArgumentException("[InvoiceStatus] Invalid string: " + name));
    }
}
```

---

### 15. InvoiceIdentifier.java

Ruta `billing/domain/model/valueobjects/InvoiceIdentifier.java`

Package `pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects`

```java
package pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects;

import java.util.Objects;
import java.util.UUID;

/**
 * Value object representing the unique identifier of an electronic Invoice.
 *
 * <p>Wraps a UUID value generated in another bounded context, used to
 * uniquely identify the electronic fiscal document.</p>
 *
 * @param value the UUID value of the invoice identifier
 * @author Victor Jhosef Laura Acosta
 */
public record InvoiceIdentifier(UUID value) {

    /**
     * Creates an InvoiceIdentifier with validation.
     *
     * @throws IllegalArgumentException if value is null
     */
    public InvoiceIdentifier {
        if (Objects.isNull(value))
            throw new IllegalArgumentException(
                    "[InvoiceIdentifier] value cannot be null");
    }

    /**
     * Creates an InvoiceIdentifier from a String representation of a UUID.
     *
     * @param value the string representation of the UUID
     * @return the InvoiceIdentifier instance
     * @throws IllegalArgumentException if value is null, blank, or not a valid UUID
     */
    public static InvoiceIdentifier fromString(String value) {
        if (Objects.isNull(value) || value.isBlank())
            throw new IllegalArgumentException(
                    "[InvoiceIdentifier] value cannot be null or blank");
        try {
            return new InvoiceIdentifier(UUID.fromString(value));
        } catch (IllegalArgumentException e) {
            throw new IllegalArgumentException(
                    "[InvoiceIdentifier] value must be a valid UUID");
        }
    }

    /**
     * Default constructor required for JPA.
     */
    public InvoiceIdentifier() {
        this(UUID.randomUUID());
    }
}
```

---

## Billing - Domain Layer (Aggregate Root)

### 16. Invoice.java

Ruta `billing/domain/model/aggregates/Invoice.java`

Package `pe.com.facturease.platform.u202418655.billing.domain.model.aggregates`

```java
package pe.com.facturease.platform.u202418655.billing.domain.model.aggregates;

import pe.com.facturease.platform.u202418655.billing.domain.model.commands.CreateInvoiceCommand;
import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;
import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceStatus;
import pe.com.facturease.platform.u202418655.shared.domain.model.aggregates.AbstractDomainAggregateRoot;
import pe.com.facturease.platform.u202418655.shared.domain.model.valueobjects.TaxSummary;
import lombok.Getter;
import lombok.Setter;

/**
 * Aggregate root representing an electronic corporate Invoice.
 *
 * <p>An Invoice represents a fiscal document issued by a Facturease Peru
 * corporate client, holding its identifier, description, status, tax
 * summary, and item count.</p>
 *
 * <p>Business rules enforced:</p>
 * <ul>
 *   <li>The {@code calculatedTax} inside {@code taxSummary} must be consistent
 *   with the 18% national IGV rate applied over {@code netAmount}.</li>
 *   <li>An Invoice can only be registered with an initial status different
 *   from DRAFT (i.e., ISSUED or PAID) if {@code itemCount} is strictly
 *   greater than zero.</li>
 * </ul>
 *
 * @author Victor Jhosef Laura Acosta
 */
@Getter
@Setter
public class Invoice extends AbstractDomainAggregateRoot<Invoice> {

    private Long id;
    private InvoiceIdentifier invoiceIdentifier;
    private String description;
    private InvoiceStatus status;
    private TaxSummary taxSummary;
    private Integer itemCount;

    /**
     * Default constructor required by JPA.
     */
    public Invoice() {}

    /**
     * Creates a new Invoice from the given command.
     *
     * @param command the create invoice command
     * @throws IllegalStateException if status is not DRAFT and itemCount is not greater than zero
     */
    public Invoice(CreateInvoiceCommand command) {
        this.invoiceIdentifier = InvoiceIdentifier.fromString(command.invoiceIdentifier());
        this.description = command.description();
        this.status = command.status() == null
                ? InvoiceStatus.DRAFT
                : InvoiceStatus.fromString(command.status());
        this.taxSummary = new TaxSummary(command.netAmount(), command.calculatedTax());
        this.itemCount = command.itemCount();

        // BUSINESS RULE: itemCount must be > 0 for any status other than DRAFT
        if (this.status != InvoiceStatus.DRAFT && (this.itemCount == null || this.itemCount <= 0)) {
            throw new IllegalStateException(
                    "[Invoice] itemCount must be strictly greater than zero when status is ISSUED or PAID");
        }
    }
}
```

---

## Billing - Domain Layer (Commands)

### 17. CreateInvoiceCommand.java

Ruta `billing/domain/model/commands/CreateInvoiceCommand.java`

Package `pe.com.facturease.platform.u202418655.billing.domain.model.commands`

```java
package pe.com.facturease.platform.u202418655.billing.domain.model.commands;

import java.math.BigDecimal;

/**
 * Command to create a new Invoice.
 *
 * <p>Encapsulates all data required to create an Invoice aggregate.
 * Validation of mandatory fields is performed in the constructor.</p>
 *
 * @param invoiceIdentifier the UUID string identifying the invoice
 * @param description       the optional invoice description
 * @param status             the optional initial status name (defaults to DRAFT)
 * @param netAmount          the net amount before taxes
 * @param calculatedTax      the accumulated tax amount
 * @param itemCount          the number of items in the invoice
 * @author Victor Jhosef Laura Acosta
 */
public record CreateInvoiceCommand(
        String invoiceIdentifier, String description, String status,
        BigDecimal netAmount, BigDecimal calculatedTax, Integer itemCount) {

    /**
     * Validates that all required fields are present.
     *
     * @throws IllegalArgumentException if any required field is null
     */
    public CreateInvoiceCommand {
        if (invoiceIdentifier == null)
            throw new IllegalArgumentException("invoiceIdentifier cannot be null");
        if (netAmount == null)
            throw new IllegalArgumentException("netAmount cannot be null");
        if (calculatedTax == null)
            throw new IllegalArgumentException("calculatedTax cannot be null");
        if (itemCount == null)
            throw new IllegalArgumentException("itemCount cannot be null");
    }
}
```

---

## Billing - Domain Layer (Queries)

### 18. GetInvoiceById.java

Ruta `billing/domain/model/queries/GetInvoiceById.java`

Package `pe.com.facturease.platform.u202418655.billing.domain.model.queries`

```java
package pe.com.facturease.platform.u202418655.billing.domain.model.queries;

/**
 * Query to retrieve an Invoice by its identifier.
 *
 * @param invoiceId the unique identifier of the invoice
 * @author Victor Jhosef Laura Acosta
 */
public record GetInvoiceById(Long invoiceId) {

    /**
     * Validates the query parameters.
     *
     * @throws IllegalArgumentException if invoiceId is null or negative
     */
    public GetInvoiceById {
        if (invoiceId == null)
            throw new IllegalArgumentException("invoiceId cannot be null");
        if (invoiceId < 0)
            throw new IllegalArgumentException("invoiceId cannot be negative");
    }
}
```

---

## Billing - Domain Layer (Repository)

### 19. InvoiceRepository.java

Ruta `billing/domain/repositories/InvoiceRepository.java`

Package `pe.com.facturease.platform.u202418655.billing.domain.repositories`

```java
package pe.com.facturease.platform.u202418655.billing.domain.repositories;

import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;

import java.util.Optional;

/**
 * Repository interface for Invoice aggregate root.
 *
 * <p>Defines the contract for persisting and retrieving invoices.
 * Implementations are provided in the infrastructure layer.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface InvoiceRepository {

    /**
     * Saves an invoice.
     *
     * @param invoice the invoice to save
     * @return the saved invoice
     */
    Invoice save(Invoice invoice);

    /**
     * Finds an invoice by its identifier.
     *
     * @param invoiceId the invoice identifier
     * @return an Optional containing the invoice if found
     */
    Optional<Invoice> findById(Long invoiceId);

    /**
     * Checks if an invoice already exists with the given invoiceIdentifier.
     *
     * @param invoiceIdentifier the invoice identifier value object
     * @return true if an invoice with that identifier exists
     */
    boolean existsByInvoiceIdentifier(InvoiceIdentifier invoiceIdentifier);
}
```

---

## Billing - Application Layer (Service Interfaces)

### 20. InvoiceCommandService.java

Ruta `billing/application/commandservices/InvoiceCommandService.java`

Package `pe.com.facturease.platform.u202418655.billing.application.commandservices`

```java
package pe.com.facturease.platform.u202418655.billing.application.commandservices;

import pe.com.facturease.platform.u202418655.billing.domain.model.commands.CreateInvoiceCommand;
import pe.com.facturease.platform.u202418655.shared.application.result.ApplicationError;
import pe.com.facturease.platform.u202418655.shared.application.result.Result;

/**
 * Service interface for handling invoice commands.
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface InvoiceCommandService {

    /**
     * Handles the creation of a new invoice.
     *
     * @param command the create invoice command
     * @return a Result containing the ID of the created invoice on success, or an error on failure
     */
    Result<Long, ApplicationError> handle(CreateInvoiceCommand command);
}
```

---

### 21. InvoiceQueryService.java

Ruta `billing/application/queryservices/InvoiceQueryService.java`

Package `pe.com.facturease.platform.u202418655.billing.application.queryservices`

```java
package pe.com.facturease.platform.u202418655.billing.application.queryservices;

import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.domain.model.queries.GetInvoiceById;

import java.util.Optional;

/**
 * Service interface for handling invoice queries.
 *
 * @author Victor Jhosef Laura Acosta
 */
public interface InvoiceQueryService {

    /**
     * Handles the retrieval of an invoice by its identifier.
     *
     * @param query the get invoice by id query
     * @return an Optional containing the invoice if found
     */
    Optional<Invoice> handle(GetInvoiceById query);
}
```

---

## Billing - Application Layer (Service Implementations)

### 22. InvoiceCommandServiceImpl.java

Ruta `billing/application/internal/commandservices/InvoiceCommandServiceImpl.java`

Package `pe.com.facturease.platform.u202418655.billing.application.internal.commandservices`

```java
package pe.com.facturease.platform.u202418655.billing.application.internal.commandservices;

import pe.com.facturease.platform.u202418655.billing.application.commandservices.InvoiceCommandService;
import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.domain.model.commands.CreateInvoiceCommand;
import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;
import pe.com.facturease.platform.u202418655.billing.domain.repositories.InvoiceRepository;
import pe.com.facturease.platform.u202418655.shared.application.result.ApplicationError;
import pe.com.facturease.platform.u202418655.shared.application.result.Result;
import org.springframework.stereotype.Service;

/**
 * Implementation of {@link InvoiceCommandService}.
 *
 * <p>Business rules enforced:</p>
 * <ul>
 *   <li>No duplicate invoiceIdentifier is allowed.</li>
 * </ul>
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class InvoiceCommandServiceImpl implements InvoiceCommandService {

    private final InvoiceRepository invoiceRepository;

    /**
     * Constructor for dependency injection.
     *
     * @param invoiceRepository the invoice repository
     */
    public InvoiceCommandServiceImpl(InvoiceRepository invoiceRepository) {
        this.invoiceRepository = invoiceRepository;
    }

    /**
     * Creates a new invoice after validating all business rules.
     *
     * @param command the create invoice command
     * @return a Result containing the invoice ID on success, or an error on failure
     */
    @Override
    public Result<Long, ApplicationError> handle(CreateInvoiceCommand command) {

        // BUSINESS RULE: No duplicate invoiceIdentifier
        if (this.invoiceRepository.existsByInvoiceIdentifier(
                InvoiceIdentifier.fromString(command.invoiceIdentifier()))) {
            return Result.failure(ApplicationError.conflict("Invoice",
                    "An invoice with the same invoiceIdentifier already exists"));
        }

        Invoice invoice;
        try {
            invoice = new Invoice(command);
            invoice = invoiceRepository.save(invoice);
        } catch (IllegalArgumentException e) {
            return Result.failure(
                    ApplicationError.validationError("create-invoice", e.getMessage()));
        } catch (IllegalStateException e) {
            return Result.failure(
                    ApplicationError.businessRuleViolation("invoice-status-item-count", e.getMessage()));
        } catch (Exception e) {
            return Result.failure(
                    ApplicationError.unexpected("create-invoice", e.getMessage()));
        }
        return Result.success(invoice.getId());
    }
}
```

---

### 23. InvoiceQueryServiceImpl.java

Ruta `billing/application/internal/queryservices/InvoiceQueryServiceImpl.java`

Package `pe.com.facturease.platform.u202418655.billing.application.internal.queryservices`

```java
package pe.com.facturease.platform.u202418655.billing.application.internal.queryservices;

import pe.com.facturease.platform.u202418655.billing.application.queryservices.InvoiceQueryService;
import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.domain.model.queries.GetInvoiceById;
import pe.com.facturease.platform.u202418655.billing.domain.repositories.InvoiceRepository;
import org.springframework.stereotype.Service;

import java.util.Optional;

/**
 * Implementation of {@link InvoiceQueryService}.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Service
public class InvoiceQueryServiceImpl implements InvoiceQueryService {

    private final InvoiceRepository invoiceRepository;

    /**
     * Constructor for dependency injection.
     *
     * @param invoiceRepository the invoice repository
     */
    public InvoiceQueryServiceImpl(InvoiceRepository invoiceRepository) {
        this.invoiceRepository = invoiceRepository;
    }

    /**
     * Retrieves an invoice by its identifier.
     *
     * @param query the get invoice by id query
     * @return an Optional containing the invoice if found
     */
    @Override
    public Optional<Invoice> handle(GetInvoiceById query) {
        return this.invoiceRepository.findById(query.invoiceId());
    }
}
```

---

## Billing - Infrastructure Layer (JPA Converter)

### 24. InvoiceIdentifierPersistenceConverter.java

Ruta `billing/infrastructure/persistence/jpa/converters/InvoiceIdentifierPersistenceConverter.java`

Package `pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.converters`

```java
package pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.converters;

import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;
import jakarta.persistence.AttributeConverter;
import jakarta.persistence.Converter;

import java.util.UUID;

/**
 * JPA attribute converter for {@link InvoiceIdentifier}.
 *
 * <p>Converts between InvoiceIdentifier (domain) and UUID (database column).</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@Converter(autoApply = true)
public class InvoiceIdentifierPersistenceConverter implements AttributeConverter<InvoiceIdentifier, UUID> {

    @Override
    public UUID convertToDatabaseColumn(InvoiceIdentifier attribute) {
        return attribute == null ? null : attribute.value();
    }

    @Override
    public InvoiceIdentifier convertToEntityAttribute(UUID dbData) {
        return dbData == null ? null : new InvoiceIdentifier(dbData);
    }
}
```

---

## Billing - Infrastructure Layer (JPA Entity)

### 25. InvoicePersistenceEntity.java

Ruta `billing/infrastructure/persistence/jpa/entities/InvoicePersistenceEntity.java`

Package `pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.entities`

```java
package pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.entities;

import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;
import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceStatus;
import pe.com.facturease.platform.u202418655.shared.infrastructure.persistence.jpa.entities.AuditableAbstractPersistenceEntity;
import jakarta.persistence.*;
import lombok.Getter;
import lombok.Setter;

import java.math.BigDecimal;

/**
 * JPA persistence entity for Invoice.
 *
 * <p>Maps the Invoice aggregate root to the database table. The TaxSummary
 * value object is flattened into two columns: {@code net_amount} and
 * {@code calculated_tax}.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@Entity
@Table(name = "invoices", schema = "facturease_db")
@Getter
@Setter
public class InvoicePersistenceEntity extends AuditableAbstractPersistenceEntity {

    @Convert(converter = pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.converters.InvoiceIdentifierPersistenceConverter.class)
    @Column(name = "invoice_identifier", nullable = false, unique = true)
    private InvoiceIdentifier invoiceIdentifier;

    @Column(name = "description")
    private String description;

    @Enumerated(EnumType.STRING)
    @Column(name = "status", nullable = false)
    private InvoiceStatus status;

    @Column(name = "net_amount", nullable = false)
    private BigDecimal netAmount;

    @Column(name = "calculated_tax", nullable = false)
    private BigDecimal calculatedTax;

    @Column(name = "item_count", nullable = false)
    private Integer itemCount;
}
```

---

## Billing - Infrastructure Layer (JPA Repository)

### 26. InvoicePersistenceRepository.java

Ruta `billing/infrastructure/persistence/jpa/repositories/InvoicePersistenceRepository.java`

Package `pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.repositories`

```java
package pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.repositories;

import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;
import pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.entities.InvoicePersistenceEntity;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;

/**
 * Spring Data JPA repository for InvoicePersistenceEntity.
 *
 * @author Victor Jhosef Laura Acosta
 */
@Repository
public interface InvoicePersistenceRepository extends JpaRepository<InvoicePersistenceEntity, Long> {

    /**
     * Checks if an invoice exists with the given invoiceIdentifier.
     *
     * @param invoiceIdentifier the invoice identifier value object
     * @return true if a matching invoice exists
     */
    boolean existsByInvoiceIdentifier(InvoiceIdentifier invoiceIdentifier);
}
```

---

## Billing - Infrastructure Layer (Assembler)

### 27. InvoicePersistenceAssembler.java

Ruta `billing/infrastructure/persistence/jpa/assemblers/InvoicePersistenceAssembler.java`

Package `pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.assemblers`

```java
package pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.assemblers;

import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.entities.InvoicePersistenceEntity;
import pe.com.facturease.platform.u202418655.shared.domain.model.valueobjects.TaxSummary;

/**
 * Assembler for converting between Invoice (domain) and InvoicePersistenceEntity (JPA).
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class InvoicePersistenceAssembler {

    private InvoicePersistenceAssembler() {}

    /**
     * Converts a domain Invoice to a JPA persistence entity.
     *
     * @param invoice the domain aggregate
     * @return the JPA persistence entity
     */
    public static InvoicePersistenceEntity toPersistenceFromDomain(Invoice invoice) {
        InvoicePersistenceEntity entity = new InvoicePersistenceEntity();
        entity.setId(invoice.getId());
        entity.setInvoiceIdentifier(invoice.getInvoiceIdentifier());
        entity.setDescription(invoice.getDescription());
        entity.setStatus(invoice.getStatus());
        entity.setNetAmount(invoice.getTaxSummary().netAmount());
        entity.setCalculatedTax(invoice.getTaxSummary().calculatedTax());
        entity.setItemCount(invoice.getItemCount());
        return entity;
    }

    /**
     * Converts a JPA persistence entity to a domain Invoice.
     *
     * @param entity the JPA persistence entity
     * @return the domain aggregate
     */
    public static Invoice toDomainFromPersistence(InvoicePersistenceEntity entity) {
        Invoice invoice = new Invoice();
        invoice.setId(entity.getId());
        invoice.setInvoiceIdentifier(entity.getInvoiceIdentifier());
        invoice.setDescription(entity.getDescription());
        invoice.setStatus(entity.getStatus());
        invoice.setTaxSummary(new TaxSummary(entity.getNetAmount(), entity.getCalculatedTax()));
        invoice.setItemCount(entity.getItemCount());
        return invoice;
    }
}
```

---

## Billing - Infrastructure Layer (Adapter)

### 28. InvoiceRepositoryImpl.java

Ruta `billing/infrastructure/persistence/jpa/adapters/InvoiceRepositoryImpl.java`

Package `pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.adapters`

```java
package pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.adapters;

import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.domain.model.valueobjects.InvoiceIdentifier;
import pe.com.facturease.platform.u202418655.billing.domain.repositories.InvoiceRepository;
import pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.assemblers.InvoicePersistenceAssembler;
import pe.com.facturease.platform.u202418655.billing.infrastructure.persistence.jpa.repositories.InvoicePersistenceRepository;
import org.springframework.stereotype.Repository;

import java.util.Optional;

/**
 * Implementation of {@link InvoiceRepository} using JPA.
 *
 * <p>Acts as an adapter between the domain repository interface and
 * Spring Data JPA persistence.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@Repository
public class InvoiceRepositoryImpl implements InvoiceRepository {

    private final InvoicePersistenceRepository invoicePersistenceRepository;

    /**
     * Constructor for dependency injection.
     *
     * @param invoicePersistenceRepository the JPA persistence repository
     */
    public InvoiceRepositoryImpl(InvoicePersistenceRepository invoicePersistenceRepository) {
        this.invoicePersistenceRepository = invoicePersistenceRepository;
    }

    /**
     * Saves an invoice by converting to persistence entity, saving, and converting back.
     */
    @Override
    public Invoice save(Invoice invoice) {
        var saved = this.invoicePersistenceRepository
                .save(InvoicePersistenceAssembler.toPersistenceFromDomain(invoice));
        return InvoicePersistenceAssembler.toDomainFromPersistence(saved);
    }

    /**
     * Finds an invoice by its identifier.
     */
    @Override
    public Optional<Invoice> findById(Long invoiceId) {
        return this.invoicePersistenceRepository.findById(invoiceId)
                .map(InvoicePersistenceAssembler::toDomainFromPersistence);
    }

    /**
     * Checks if an invoice exists with the given invoiceIdentifier.
     */
    @Override
    public boolean existsByInvoiceIdentifier(InvoiceIdentifier invoiceIdentifier) {
        return this.invoicePersistenceRepository
                .existsByInvoiceIdentifier(invoiceIdentifier);
    }
}
```

---

## Billing - Interfaces Layer (Resources)

### 29. CreateInvoiceResource.java

Ruta `billing/interfaces/rest/resources/CreateInvoiceResource.java`

Package `pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources`

```java
package pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources;

import java.math.BigDecimal;

/**
 * REST resource for creating a new Invoice.
 *
 * <p>Value object attributes ({@code invoiceIdentifier}, {@code taxSummary})
 * are flattened into primitive-typed attributes in the request body, as
 * required by the API contract. Note that {@code id} is not included here,
 * since it is auto-generated when storing the information.</p>
 *
 * @param invoiceIdentifier the UUID string identifying the invoice
 * @param description       the optional description of the invoice
 * @param status             the optional initial status (DRAFT, ISSUED, PAID)
 * @param netAmount          the net amount before taxes
 * @param calculatedTax      the accumulated tax amount (must equal 18% of netAmount)
 * @param itemCount          the number of items in the invoice
 * @author Victor Jhosef Laura Acosta
 */
public record CreateInvoiceResource(
        String invoiceIdentifier, String description, String status,
        BigDecimal netAmount, BigDecimal calculatedTax, Integer itemCount) {

    /**
     * Validates the resource fields at the boundary.
     *
     * @throws IllegalArgumentException if any required field violates constraints
     */
    public CreateInvoiceResource {
        if (invoiceIdentifier == null || invoiceIdentifier.isBlank())
            throw new IllegalArgumentException("invoiceIdentifier cannot be null or blank");
        if (netAmount == null)
            throw new IllegalArgumentException("netAmount cannot be null");
        if (calculatedTax == null)
            throw new IllegalArgumentException("calculatedTax cannot be null");
        if (itemCount == null)
            throw new IllegalArgumentException("itemCount cannot be null");
    }
}
```

---

### 30. InvoiceResource.java

Ruta `billing/interfaces/rest/resources/InvoiceResource.java`

Package `pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources`

```java
package pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources;

import java.math.BigDecimal;

/**
 * REST resource representing an Invoice response.
 *
 * <p>Includes id, invoiceId (as String), description (as String),
 * status (as String), netAmountValue (as BigDecimal), calculatedTaxValue
 * (as BigDecimal), and itemCount (as Integer).</p>
 *
 * @param id                 the invoice identifier (database primary key)
 * @param invoiceId          the invoiceIdentifier as a String (UUID)
 * @param description        the invoice description
 * @param status              the invoice status name
 * @param netAmountValue      the net amount before taxes
 * @param calculatedTaxValue  the accumulated tax amount
 * @param itemCount           the number of items in the invoice
 * @author Victor Jhosef Laura Acosta
 */
public record InvoiceResource(
        Long id, String invoiceId, String description, String status,
        BigDecimal netAmountValue, BigDecimal calculatedTaxValue, Integer itemCount) {
}
```

---

## Billing - Interfaces Layer (Assemblers)

### 31. CreateInvoiceCommandFromResourceAssembler.java

Ruta `billing/interfaces/rest/transform/CreateInvoiceCommandFromResourceAssembler.java`

Package `pe.com.facturease.platform.u202418655.billing.interfaces.rest.transform`

```java
package pe.com.facturease.platform.u202418655.billing.interfaces.rest.transform;

import pe.com.facturease.platform.u202418655.billing.domain.model.commands.CreateInvoiceCommand;
import pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources.CreateInvoiceResource;

/**
 * Assembler for converting a {@link CreateInvoiceResource} to a
 * {@link CreateInvoiceCommand}.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class CreateInvoiceCommandFromResourceAssembler {

    private CreateInvoiceCommandFromResourceAssembler() {}

    /**
     * Converts a REST resource to a domain command.
     *
     * @param resource the REST resource from the request body
     * @return the create invoice command
     */
    public static CreateInvoiceCommand toCommandFromResource(CreateInvoiceResource resource) {
        return new CreateInvoiceCommand(
                resource.invoiceIdentifier(), resource.description(), resource.status(),
                resource.netAmount(), resource.calculatedTax(), resource.itemCount());
    }
}
```

---

### 32. InvoiceResourceFromEntityAssembler.java

Ruta `billing/interfaces/rest/transform/InvoiceResourceFromEntityAssembler.java`

Package `pe.com.facturease.platform.u202418655.billing.interfaces.rest.transform`

```java
package pe.com.facturease.platform.u202418655.billing.interfaces.rest.transform;

import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources.InvoiceResource;

/**
 * Assembler for converting an {@link Invoice} domain entity to an
 * {@link InvoiceResource}.
 *
 * @author Victor Jhosef Laura Acosta
 */
public final class InvoiceResourceFromEntityAssembler {

    private InvoiceResourceFromEntityAssembler() {}

    /**
     * Converts a domain Invoice to a REST resource.
     *
     * @param entity the domain aggregate
     * @return the REST resource
     */
    public static InvoiceResource toResourceFromEntity(Invoice entity) {
        return new InvoiceResource(
                entity.getId(),
                entity.getInvoiceIdentifier().value().toString(),
                entity.getDescription(),
                entity.getStatus().name(),
                entity.getTaxSummary().netAmount(),
                entity.getTaxSummary().calculatedTax(),
                entity.getItemCount());
    }
}
```

---

## Billing - Interfaces Layer (Controller con OpenAPI - Punto 14 de la rúbrica)

### 33. InvoicesController.java

Ruta `billing/interfaces/rest/InvoicesController.java`

Package `pe.com.facturease.platform.u202418655.billing.interfaces.rest`

```java
package pe.com.facturease.platform.u202418655.billing.interfaces.rest;

import pe.com.facturease.platform.u202418655.billing.application.commandservices.InvoiceCommandService;
import pe.com.facturease.platform.u202418655.billing.application.queryservices.InvoiceQueryService;
import pe.com.facturease.platform.u202418655.billing.domain.model.aggregates.Invoice;
import pe.com.facturease.platform.u202418655.billing.domain.model.queries.GetInvoiceById;
import pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources.CreateInvoiceResource;
import pe.com.facturease.platform.u202418655.billing.interfaces.rest.resources.InvoiceResource;
import pe.com.facturease.platform.u202418655.billing.interfaces.rest.transform.CreateInvoiceCommandFromResourceAssembler;
import pe.com.facturease.platform.u202418655.billing.interfaces.rest.transform.InvoiceResourceFromEntityAssembler;
import pe.com.facturease.platform.u202418655.shared.application.result.ApplicationError;
import pe.com.facturease.platform.u202418655.shared.application.result.Result;
import pe.com.facturease.platform.u202418655.shared.interfaces.rest.transform.ResponseEntityAssembler;
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
 * REST controller for invoice operations.
 *
 * <p>Exposes endpoints for managing invoices under the
 * {@code /api/v1/invoices} base path.</p>
 *
 * @author Victor Jhosef Laura Acosta
 */
@RestController
@RequestMapping(value = "/api/v1/invoices")
@Tag(name = "Invoices", description = "Operations related to invoice management in the Facturease Peru platform")
public class InvoicesController {

    private final InvoiceCommandService invoiceCommandService;
    private final InvoiceQueryService invoiceQueryService;

    /**
     * Constructor for dependency injection.
     *
     * @param invoiceCommandService the command service
     * @param invoiceQueryService   the query service
     */
    public InvoicesController(
            InvoiceCommandService invoiceCommandService,
            InvoiceQueryService invoiceQueryService) {
        this.invoiceCommandService = invoiceCommandService;
        this.invoiceQueryService = invoiceQueryService;
    }

    /**
     * Creates a new invoice.
     *
     * <p>The id is auto-generated by the database. The status defaults to
     * DRAFT when not specified. The netAmount and calculatedTax values are
     * validated to be consistent with the 18% national IGV rate.</p>
     *
     * @param resource the request body containing invoice details
     * @return HTTP 201 with the created invoice resource on success,
     *         or appropriate error status on failure
     */
    @PostMapping
    @Operation(
            summary = "Create a new invoice",
            description = "Registers a new electronic Invoice in the Facturease Peru platform. "
                    + "The status defaults to DRAFT when not specified. The calculatedTax value "
                    + "must be consistent with the 18% national IGV rate applied over netAmount. "
                    + "When status is ISSUED or PAID, itemCount must be strictly greater than zero. "
                    + "The id is auto-generated by the database."
    )
    @ApiResponses(value = {
            @ApiResponse(
                    responseCode = "201",
                    description = "Invoice created successfully",
                    content = @Content(
                            schema = @Schema(implementation = InvoiceResource.class))
            ),
            @ApiResponse(
                    responseCode = "400",
                    description = "Invalid input data - validation error"),
            @ApiResponse(
                    responseCode = "409",
                    description = "Conflict - Invoice with same invoiceIdentifier already exists"),
            @ApiResponse(
                    responseCode = "422",
                    description = "Business rule violation")
    })
    public ResponseEntity<?> createInvoice(@RequestBody CreateInvoiceResource resource) {
        var command = CreateInvoiceCommandFromResourceAssembler
                .toCommandFromResource(resource);
        var result = this.invoiceCommandService.handle(command)
                .flatMap(invoiceId -> this.invoiceQueryService
                        .handle(new GetInvoiceById(invoiceId))
                        .<Result<Invoice, ApplicationError>>map(Result::success)
                        .orElseGet(() -> Result.failure(
                                ApplicationError.notFound("Invoice",
                                        invoiceId.toString()))));

        return ResponseEntityAssembler.toResponseEntityFromResult(
                result,
                InvoiceResourceFromEntityAssembler::toResourceFromEntity,
                HttpStatus.CREATED);
    }
}
```

---

## Resumen de Cumplimiento de la Rúbrica

| Punto | Requisito | Estado | Dónde |
|---|---|---|---|
| 1 | Java 26, Spring Boot 4.1.0 | ✅ | `pom.xml` |
| 2 | Nombre `pc2<NRC>u<código>` | ✅ | `pc211990u202418655` |
| 3 | PostgreSQL, esquema `facturease_db` | ✅ | `application.properties` + `@Table(schema = "facturease_db")` |
| 4 | Package raíz `pe.com.facturease.platform.u<código>` | ✅ | `pe.com.facturease.platform.u202418655` |
| 5 | Bounded context `billing` para Invoice | ✅ | Package `billing` |
| 6 | Bounded context `shared` para TaxSummary | ✅ | Package `shared.domain.model.valueobjects` |
| 7 | Jakarta Validation / @Pattern | ✅ | Value Objects con validación propia |
| 8 | i18n EN/ES via Accept-Language | ✅ | `LocaleConfiguration.java` + `messages.properties` |
| 9 | DDD, CQRS, layered architecture | ✅ | domain/application/interfaces/infrastructure |
| 10 | Separación de domain vs persistence concepts | ✅ | `Invoice` vs `InvoicePersistenceEntity` |
| 11 | SnakeCase + plural naming strategy | ✅ | `SnakeCaseWithPluralizedTablePhysicalNamingStrategy` |
| 12 | Shared bounded context (estilo learning center) | ✅ | `shared` package |
| 13 | JavaDoc en inglés con @author | ✅ | Todos los archivos |
| 14 | OpenAPI con Swagger UI | ✅ | `@Operation` + `@ApiResponses` en `InvoicesController.java` |
| 15 | Puerto 8097 | ✅ | `application.properties` |
| 16 | URLs minúsculas y plural | ✅ | `/api/v1/invoices` |
| 17 | Lombok | ✅ | `@Getter`, `@Setter` |
| 18 | Mensajes en inglés | ✅ | `messages.properties` |
| 19 | Gestión de excepciones | ✅ | `GlobalExceptionHandler.java` |
| 20 | README.md | ✅ | `README.md` |
| 21 | ZIP `pc2<NRC>u<código>.zip` | ✅ | `pc211990u202418655.zip` |
| 22 | Subir a actividad PC2 | ✅ | — |

---

## Reglas de Negocio Implementadas

| # | Regla | Implementación | Archivo |
|---|---|---|---|
| 1 | No duplicado de `invoiceIdentifier` | `existsByInvoiceIdentifier()` | `InvoiceCommandServiceImpl.java` |
| 2 | `calculatedTax` debe ser 18% IGV de `netAmount`, sino `IllegalArgumentException` | Validación en compact constructor | `TaxSummary.java` |
| 3 | Si `status` ≠ DRAFT (ISSUED o PAID), `itemCount` debe ser > 0, sino `IllegalStateException` | Validación en constructor del aggregate | `Invoice.java` |
| 4 | `InvoiceIdentifier` es un value object UUID generado en otro bounded context | `InvoiceIdentifier.fromString()` | `InvoiceIdentifier.java` |
| 5 | `InvoiceStatus` enum con DRAFT por defecto | `InvoiceStatus.fromString()` + default en `Invoice` | `InvoiceStatus.java` + `Invoice.java` |
| 6 | `TaxSummary` value object compuesto en bounded context `shared` | `record TaxSummary(netAmount, calculatedTax)` | `TaxSummary.java` |
| 7 | Invoice auditable | `@EnableJpaAuditing` + `AuditableAbstractPersistenceEntity` | `Application.java` + shared |
| 8 | i18n EN/ES | `AcceptHeaderLocaleResolver` + ResourceBundle | `LocaleConfiguration.java` |
| 9 | `description` opcional | Sin `@Column(nullable = false)` en persistencia | `InvoicePersistenceEntity.java` |
| 10 | Response incluye id, invoiceId, description, status, netAmountValue, calculatedTaxValue, itemCount | `InvoiceResource` con 7 campos | `InvoiceResource.java` |
| 11 | `id` autogenerado | `@GeneratedValue(IDENTITY)` | `AuditableAbstractPersistenceEntity.java` |
| 12 | `id` no se ingresa al crear | No incluido en `CreateInvoiceResource` | `CreateInvoiceResource.java` |
| 13 | Status como String (input), con default DRAFT | `InvoiceStatus.fromString()` en `Invoice` | `Invoice.java` |
| 14 | HTTP 201 en éxito | `HttpStatus.CREATED` | `InvoicesController.java` |
| 15 | HTTP Status correcto en errores | Switch en `ErrorResponseAssembler` | `ErrorResponseAssembler.java` |

---

## Verificación Final (Checklist)

- [ ] Puerto `http://localhost:8097`
- [ ] Swagger UI `http://localhost:8097/swagger-ui/index.html`
- [ ] OpenAPI JSON `http://localhost:8097/v3/api-docs`
- [ ] BD PostgreSQL `facturease_db` con esquema `facturease_db`
- [ ] Endpoint `POST /api/v1/invoices` → 201 Created
- [ ] Regla 1: mismo `invoiceIdentifier` → 409 Conflict
- [ ] Regla 2: `calculatedTax` inconsistente con 18% IGV → 400 Bad Request
- [ ] Regla 3: `status` ISSUED/PAID con `itemCount` ≤ 0 → 422 Unprocessable Entity
- [ ] Status DRAFT por defecto cuando no se envía `status`
- [ ] Convenciones snake_case, tablas en plural, inglés, UpperCamelCase clases, lowerCamelCase métodos/variables
- [ ] JavaDoc `@author Victor Jhosef Laura Acosta` en todos los archivos
- [ ] OpenAPI `@Operation` + `@ApiResponses` visibles en Swagger UI
- [ ] i18n con header `Accept-Language: es` → mensajes en español
- [ ] Nombre del ZIP `pc211990u202418655.zip`

---

## Estructura Final del Proyecto (33 archivos .java + 5 archivos de configuración)

```
pc211990u202418655
├── pom.xml
├── README.md
├── mvnw
├── mvnw.cmd
├── HELP.md
├── src
│   ├── main
│   │   ├── java/pe/com/facturease/platform/u202418655
│   │   │   ├── Pc211990u202418655Application.java
│   │   │   ├── shared
│   │   │   │   ├── application/result
│   │   │   │   │   ├── Result.java
│   │   │   │   │   └── ApplicationError.java
│   │   │   │   ├── domain/model
│   │   │   │   │   ├── aggregates
│   │   │   │   │   │   └── AbstractDomainAggregateRoot.java
│   │   │   │   │   └── valueobjects
│   │   │   │   │       └── TaxSummary.java
│   │   │   │   ├── infrastructure
│   │   │   │   │   ├── documentation/openapi/configuration
│   │   │   │   │   │   └── OpenApiConfiguration.java
│   │   │   │   │   ├── i18n/configuration
│   │   │   │   │   │   └── LocaleConfiguration.java
│   │   │   │   │   └── persistence/jpa
│   │   │   │   │       ├── configuration/strategy
│   │   │   │   │       │   └── SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java
│   │   │   │   │       └── entities
│   │   │   │   │           └── AuditableAbstractPersistenceEntity.java
│   │   │   │   └── interfaces/rest
│   │   │   │       ├── GlobalExceptionHandler.java
│   │   │   │       ├── resources
│   │   │   │       │   ├── ErrorResource.java
│   │   │   │       │   └── MessageResource.java
│   │   │   │       └── transform
│   │   │   │           ├── ErrorResponseAssembler.java
│   │   │   │           └── ResponseEntityAssembler.java
│   │   │   └── billing
│   │   │       ├── domain
│   │   │       │   ├── model
│   │   │       │   │   ├── aggregates
│   │   │       │   │   │   └── Invoice.java
│   │   │       │   │   ├── commands
│   │   │       │   │   │   └── CreateInvoiceCommand.java
│   │   │       │   │   ├── queries
│   │   │       │   │   │   └── GetInvoiceById.java
│   │   │       │   │   └── valueobjects
│   │   │       │   │       ├── InvoiceIdentifier.java
│   │   │       │   │       └── InvoiceStatus.java
│   │   │       │   └── repositories
│   │   │       │       └── InvoiceRepository.java
│   │   │       ├── application
│   │   │       │   ├── commandservices
│   │   │       │   │   └── InvoiceCommandService.java
│   │   │       │   ├── queryservices
│   │   │       │   │   └── InvoiceQueryService.java
│   │   │       │   └── internal
│   │   │       │       ├── commandservices
│   │   │       │       │   └── InvoiceCommandServiceImpl.java
│   │   │       │       └── queryservices
│   │   │       │           └── InvoiceQueryServiceImpl.java
│   │   │       ├── infrastructure/persistence/jpa
│   │   │       │   ├── adapters
│   │   │       │   │   └── InvoiceRepositoryImpl.java
│   │   │       │   ├── assemblers
│   │   │       │   │   └── InvoicePersistenceAssembler.java
│   │   │       │   ├── converters
│   │   │       │   │   └── InvoiceIdentifierPersistenceConverter.java
│   │   │       │   ├── entities
│   │   │       │   │   └── InvoicePersistenceEntity.java
│   │   │       │   └── repositories
│   │   │       │       └── InvoicePersistenceRepository.java
│   │   │       └── interfaces/rest
│   │   │           ├── InvoicesController.java
│   │   │           ├── resources
│   │   │           │   ├── CreateInvoiceResource.java
│   │   │           │   └── InvoiceResource.java
│   │   │           └── transform
│   │   │               ├── CreateInvoiceCommandFromResourceAssembler.java
│   │   │               └── InvoiceResourceFromEntityAssembler.java
│   │   └── resources
│   │       ├── application.properties
│   │       ├── messages.properties
│   │       ├── messages_es.properties
│   │       ├── static
│   │       └── templates
│   └── test/java/pe/com/facturease/platform/u202418655
│       └── Pc211990u202418655ApplicationTests.java
```
