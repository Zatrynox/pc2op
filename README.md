bueno segun el enunciado del pdf, hazme esto: # Examen Final (EB) — Guía Template Paso a Paso
## 1ASI0730 - Aplicaciones Web (.NET / C# / ASP.NET Core)
### Basado en la estructura de Learning Center Platform

---

## ⚙️ ANTES DE EMPEZAR — Extraer el texto de tu PDF de examen

Si tu enunciado llega como PDF con texto seleccionable:

```python
# pip install pdfplumber
import pdfplumber

with pdfplumber.open("mi_examen.pdf") as pdf:
    for page in pdf.pages:
        print(page.extract_text())
```

Si el PDF tiene partes como **imagen** (texto no seleccionable), usar OCR:

```python
# pip install pdfplumber pytesseract pillow
import pdfplumber, pytesseract

with pdfplumber.open("mi_examen.pdf") as pdf:
    for page in pdf.pages:
        text = page.extract_text()
        if not text:
            img = page.to_image(resolution=300)
            text = pytesseract.image_to_string(img.original)
        print(text)
```

> Para Tesseract en Windows: descargar desde https://github.com/UB-Mannheim/tesseract/wiki

---

## ⚠️ CÓMO USAR ESTA GUÍA

- `«PLACEHOLDER»` → reemplazar con el valor de **tu examen**
- Código sin ángulos → **copiar y pegar tal cual**
- Sigue los pasos **en orden**
- Esta guía soporta hasta **3 Bounded Contexts** (Shared + BC1 + BC2 + BC3 opcional)
- Estructura basada en **Learning Center Platform**: https://github.com/upc-pre-202610-1asi0730-12258/learning-center-platform.git

---

## PASO 0 — Lee tu examen y llena esta tabla ANTES de empezar

| Variable | Ejemplo MiFarma | **TU EXAMEN** |
|---|---|---|
| **NRC** | (el de tu sección) | _______ |
| **Código estudiante** | u202418655 | u202418655 |
| **Nombre solución (ZIP)** | eb«NRC»u202418655.API | eb«NRC»u202418655.API |
| **Nombre proyecto** | MiFarma.PharmacyManagement.Platform.u202418655 | «EMPRESA».«DOMINIO».Platform.u202418655 |
| **Namespace raíz** | MiFarma.PharmacyManagement.Platform.u202418655 | «EMPRESA».«DOMINIO».Platform.u202418655 |
| **BD esquema** | mifarma | _______ |
| **Puerto** | (ver enunciado / launchSettings) | _______ |
| **BC Pharmacy/Contracts** | Pharmacy | _______ (BC que tiene la entidad "principal" con POST) |
| **BC Inventory/Entitlement** | Inventory | _______ (BC que tiene entidades secundarias) |
| **Aggregate BC1** | Prescription | _______ |
| **Aggregate BC2a** | Medicine | _______ |
| **Aggregate BC2b** | Dispensation | _______ (si hay 2 en el mismo BC) |
| **Endpoint BC1** | /api/v1/prescriptions | _______ |
| **Endpoint BC2a** | /api/v1/medicines | _______ |
| **Endpoint BC2b** | /api/v1/dispensations | _______ |
| **Value Objects en Shared** | PrescriptionPeriod | _______ |
| **Value Objects en BC1** | PrescriptionIdentifier, PatientId | _______ |
| **Evento de integración** | DispensationRegisteredEvent | _______ |
| **ACL Facade** | PrescriptionContextFacade | _______ |
| **Tabla idempotencia** | ProcessedEvents | ProcessedEvents (suele ser igual) |
| **Aggregate que hace seeding** | Medicine | _______ |

---

## PASO 1 — Base de datos (MySQL)

Abrir MySQL (XAMPP / MySQL Workbench / CLI) y ejecutar:

```sql
CREATE SCHEMA IF NOT EXISTS «BD_ESQUEMA»
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
```

---

## PASO 2 — Crear el proyecto (JetBrains Rider)

1. Abrir Rider → **New Solution**
2. Template: **ASP.NET Core Web API**
3. Configurar:
   - **Solution name:** `eb«NRC»u202418655.API`
   - **Project name:** `«NAMESPACE_RAIZ»`
   - **Framework:** .NET 10.0
   - **Authentication type:** None
   - **Use controllers:** ✅
   - **Enable OpenAPI support:** ✅
4. Crear.

Eliminar archivos por defecto:
- `WeatherForecast.cs` ← eliminar
- `Controllers/WeatherForecastController.cs` ← eliminar

---

## PASO 3 — Archivo .csproj

Abrir `«NAMESPACE_RAIZ».csproj` y reemplazar con:

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

    <PropertyGroup>
        <TargetFramework>net10.0</TargetFramework>
        <Nullable>enable</Nullable>
        <ImplicitUsings>enable</ImplicitUsings>
        <InvariantGlobalization>false</InvariantGlobalization>
        <LangVersion>14</LangVersion>
    </PropertyGroup>

    <ItemGroup>
        <!-- Entity Framework Core -->
        <PackageReference Include="Microsoft.EntityFrameworkCore" Version="10.0.8"/>
        <PackageReference Include="Microsoft.EntityFrameworkCore.Relational" Version="10.0.8"/>
        <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.8"/>

        <!-- MySQL provider -->
        <PackageReference Include="MySql.EntityFrameworkCore" Version="10.0.7"/>

        <!-- Naming conventions (snake_case + plural) -->
        <PackageReference Include="Humanizer" Version="3.0.10"/>

        <!-- Event handling (Mediator pattern - Learning Center style) -->
        <PackageReference Include="Cortex.Mediator" Version="1.0.0"/>

        <!-- OpenAPI documentation -->
        <PackageReference Include="Swashbuckle.AspNetCore" Version="10.2.0"/>
        <PackageReference Include="Swashbuckle.AspNetCore.Annotations" Version="10.2.0"/>
    </ItemGroup>

    <!-- i18n EmbeddedResource entries — agregar uno por cada BC que tenga .resx -->
    <ItemGroup>
        <EmbeddedResource Update="«BC1_FOLDER»\Resources\«BC1_NAME»Messages.es.resx">
            <DependentUpon>«BC1_NAME»Messages.cs</DependentUpon>
        </EmbeddedResource>
        <EmbeddedResource Update="«BC2_FOLDER»\Resources\«BC2_NAME»Messages.es.resx">
            <DependentUpon>«BC2_NAME»Messages.cs</DependentUpon>
        </EmbeddedResource>
        <EmbeddedResource Update="Shared\Resources\SharedMessages.es.resx">
            <DependentUpon>SharedMessages.cs</DependentUpon>
        </EmbeddedResource>
    </ItemGroup>

</Project>
```

> **Nota Cortex.Mediator:** Si la versión `1.0.0` no existe, buscar en NuGet la última disponible, o reemplazar con `MediatR` versión 12+ con la misma lógica.

---

## PASO 4 — Program.cs

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Application.CommandServices;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Application.Internal.CommandServices;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Repositories;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Infrastructure.Persistence.EntityFrameworkCore.Repositories;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Application.CommandServices;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Application.Internal.CommandServices;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Application.Internal.EventHandlers;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Application.QueryServices;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Domain.Repositories;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Infrastructure.Persistence.EntityFrameworkCore.Repositories;
using «NAMESPACE_RAIZ».Shared.Domain.Repositories;
using «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;
using «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

var builder = WebApplication.CreateBuilder(args);

// ── Database ──────────────────────────────────────────────────────────────────
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection")
    ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseMySQL(connectionString));

// ── Localization ──────────────────────────────────────────────────────────────
builder.Services.AddLocalization();

// ── Repositories & Unit of Work ───────────────────────────────────────────────
// BC1
builder.Services.AddScoped<I«BC1_AGGREGATE»Repository, «BC1_AGGREGATE»Repository>();
// BC2
builder.Services.AddScoped<I«BC2_AGGREGATE_A»Repository, «BC2_AGGREGATE_A»Repository>();
builder.Services.AddScoped<I«BC2_AGGREGATE_B»Repository, «BC2_AGGREGATE_B»Repository>(); // si aplica
// Shared
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();
// ProcessedEvents (idempotency)
builder.Services.AddScoped<IProcessedEventRepository, ProcessedEventRepository>();

// ── Application Services ──────────────────────────────────────────────────────
// BC1
builder.Services.AddScoped<I«BC1_AGGREGATE»CommandService, «BC1_AGGREGATE»CommandService>();
// BC2
builder.Services.AddScoped<I«BC2_AGGREGATE_A»QueryService, «BC2_AGGREGATE_A»QueryService>();
builder.Services.AddScoped<I«BC2_AGGREGATE_B»CommandService, «BC2_AGGREGATE_B»CommandService>(); // si aplica

// ── ACL (Anti-Corruption Layer) ───────────────────────────────────────────────
builder.Services.AddScoped<«ACL_FACADE_NAME»>();

// ── Event Handlers (Cortex.Mediator) ─────────────────────────────────────────
builder.Services.AddScoped<«DOMAIN_EVENT_NAME»Handler>();
// Si usas Cortex.Mediator:
// builder.Services.AddCortexMediator(typeof(Program).Assembly);

// ── Controllers ───────────────────────────────────────────────────────────────
builder.Services.AddControllers()
    .AddJsonOptions(options =>
    {
        options.JsonSerializerOptions.Converters.Add(
            new System.Text.Json.Serialization.JsonStringEnumConverter());
    });

// ── OpenAPI / Swagger ─────────────────────────────────────────────────────────
builder.Services.AddEndpointsApiExplorer();
builder.Services.AddSwaggerGen(options =>
{
    options.SwaggerDoc("v1", new OpenApiInfo
    {
        Title = "«NOMBRE_CASO» Platform API",
        Version = "v1",
        Description = "RESTful API for the «NOMBRE_CASO» platform.",
        Contact = new OpenApiContact
        {
            Name = "Victor Jhosef Laura Acosta - u202418655"
        }
    });
    options.EnableAnnotations();
});

var app = builder.Build();

// ── Request Localization ──────────────────────────────────────────────────────
var supportedCultures = new[] { "en", "en-US", "es", "es-PE" };
app.UseRequestLocalization(options =>
{
    options.SetDefaultCulture("en");
    options.AddSupportedCultures(supportedCultures);
    options.AddSupportedUICultures(supportedCultures);
    options.ApplyCurrentCultureToResponseHeaders = true;
});

// ── Database Setup (EnsureCreated + UseAsyncSeeding) ─────────────────────────
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    await db.Database.EnsureCreatedAsync();
    // EnsureCreated ya dispara UseAsyncSeeding (configurado en AppDbContext)
}

// ── Middleware ────────────────────────────────────────────────────────────────
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(options =>
    {
        options.SwaggerEndpoint("/swagger/v1/swagger.json", "«NOMBRE_CASO» Platform API v1");
        options.RoutePrefix = "swagger";
    });
}

app.UseHttpsRedirection();
app.MapControllers();
app.Run();
```

---

## PASO 5 — appsettings.json

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Port=3306;Database=«BD_ESQUEMA»;User=root;Password=12345678;"
  },
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning"
    }
  },
  "AllowedHosts": "*"
}
```

### Properties/launchSettings.json

```json
{
  "profiles": {
    "«NAMESPACE_RAIZ»": {
      "applicationUrl": "https://localhost:«PUERTO»;http://localhost:5088",
      "commandName": "Project",
      "dotnetRunMessages": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

---

## PASO 6 — README.md

```markdown
# «NOMBRE_CASO» Platform API

## Description
RESTful API for the «NOMBRE_CASO» case study.
Implements Domain-Driven Design, CQRS, Anti-Corruption Layer, and domain events
following the Learning Center Platform architecture.
Built with C# / .NET 10 / ASP.NET Core / Entity Framework Core / MySQL.

## Technologies
- .NET 10.0 (C# 14)
- ASP.NET Core 10.0
- Entity Framework Core 10.0.8
- MySQL (MySql.EntityFrameworkCore 10.0.7)
- Humanizer 3.0.10
- Cortex.Mediator
- Swashbuckle (Swagger UI) 10.2.0

## Author
- **Name:** Victor Jhosef Laura Acosta
- **Student code:** u202418655
- **Course:** 1ASI0730 - Aplicaciones Web

## Running
```bash
dotnet run
```

## API Documentation
Swagger UI: `/swagger`

## Localization
Supports `Accept-Language: en`, `en-US`, `es`, `es-PE`.
```

---

## PASO 7 — Estructura de carpetas completa del proyecto

```
«NAMESPACE_RAIZ»/
├── Shared/
│   ├── Application/
│   │   └── Model/
│   │       └── Result.cs
│   ├── Domain/
│   │   ├── Model/
│   │   │   ├── IAuditableEntity.cs
│   │   │   └── «SHARED_VALUE_OBJECT».cs     ← VO que pide tu examen en Shared
│   │   └── Repositories/
│   │       ├── IBaseRepository.cs
│   │       └── IUnitOfWork.cs
│   ├── Infrastructure/
│   │   └── Persistence/
│   │       └── EntityFrameworkCore/
│   │           ├── Configuration/
│   │           │   ├── AppDbContext.cs
│   │           │   └── NamingConventions.cs  ← snake_case + plural strategy
│   │           └── Repositories/
│   │               ├── BaseRepository.cs
│   │               └── UnitOfWork.cs
│   ├── Interfaces/
│   │   └── Rest/
│   │       ├── GlobalExceptionHandler.cs (middleware)
│   │       └── ProblemDetailsFactory.cs
│   └── Resources/
│       ├── SharedMessages.cs
│       ├── SharedMessages.resx
│       └── SharedMessages.es.resx
│
├── «BC1_FOLDER»/                    ← Bounded Context 1 (ej: Pharmacy)
│   ├── Application/
│   │   ├── CommandServices/
│   │   │   └── I«BC1_AGGREGATE»CommandService.cs
│   │   └── Internal/
│   │       └── CommandServices/
│   │           └── «BC1_AGGREGATE»CommandService.cs
│   ├── Domain/
│   │   ├── Model/
│   │   │   ├── Aggregates/
│   │   │   │   ├── «BC1_AGGREGATE».cs
│   │   │   │   └── «BC1_AGGREGATE»Audit.cs  ← partial class para auditoría
│   │   │   ├── Commands/
│   │   │   │   └── Create«BC1_AGGREGATE»Command.cs
│   │   │   └── ValueObjects/
│   │   │       └── (VOs específicos de BC1)
│   │   └── Repositories/
│   │       └── I«BC1_AGGREGATE»Repository.cs
│   ├── Infrastructure/
│   │   └── Persistence/
│   │       └── EntityFrameworkCore/
│   │           ├── Configuration/
│   │           │   └── ModelBuilderExtensions.cs
│   │           └── Repositories/
│   │               └── «BC1_AGGREGATE»Repository.cs
│   ├── Interfaces/
│   │   └── Rest/
│   │       ├── «BC1_AGGREGATE»sController.cs
│   │       ├── Resources/
│   │       │   ├── Create«BC1_AGGREGATE»Resource.cs
│   │       │   └── «BC1_AGGREGATE»Resource.cs
│   │       └── Transform/
│   │           ├── Create«BC1_AGGREGATE»CommandFromResourceAssembler.cs
│   │           └── «BC1_AGGREGATE»ResourceFromEntityAssembler.cs
│   └── Resources/
│       ├── «BC1_NAME»Messages.cs
│       ├── «BC1_NAME»Messages.resx
│       └── «BC1_NAME»Messages.es.resx
│
├── «BC2_FOLDER»/                    ← Bounded Context 2 (ej: Inventory)
│   ├── Application/
│   │   ├── CommandServices/
│   │   │   └── I«BC2_AGGREGATE_B»CommandService.cs
│   │   ├── Internal/
│   │   │   ├── CommandServices/
│   │   │   │   └── «BC2_AGGREGATE_B»CommandService.cs
│   │   │   ├── EventHandlers/
│   │   │   │   └── «DOMAIN_EVENT_NAME»Handler.cs
│   │   │   └── QueryServices/
│   │   │       └── «BC2_AGGREGATE_A»QueryService.cs
│   │   └── QueryServices/
│   │       └── I«BC2_AGGREGATE_A»QueryService.cs
│   ├── Domain/
│   │   ├── Model/
│   │   │   ├── Aggregates/
│   │   │   │   ├── «BC2_AGGREGATE_A».cs
│   │   │   │   ├── «BC2_AGGREGATE_A»Audit.cs
│   │   │   │   ├── «BC2_AGGREGATE_B».cs       ← si hay 2 en BC2
│   │   │   │   └── «BC2_AGGREGATE_B»Audit.cs
│   │   │   ├── Commands/
│   │   │   │   └── Create«BC2_AGGREGATE_B»Command.cs
│   │   │   ├── Events/
│   │   │   │   └── «DOMAIN_EVENT_NAME».cs
│   │   │   ├── Queries/
│   │   │   │   └── GetAll«BC2_AGGREGATE_A»sQuery.cs
│   │   │   └── ValueObjects/
│   │   │       └── (VOs específicos de BC2)
│   │   └── Repositories/
│   │       ├── I«BC2_AGGREGATE_A»Repository.cs
│   │       └── I«BC2_AGGREGATE_B»Repository.cs
│   ├── Infrastructure/
│   │   ├── Acl/
│   │   │   └── «ACL_FACADE_NAME».cs           ← Anti-Corruption Layer
│   │   └── Persistence/
│   │       └── EntityFrameworkCore/
│   │           ├── Configuration/
│   │           │   └── ModelBuilderExtensions.cs
│   │           └── Repositories/
│   │               ├── «BC2_AGGREGATE_A»Repository.cs
│   │               └── «BC2_AGGREGATE_B»Repository.cs
│   ├── Interfaces/
│   │   └── Rest/
│   │       ├── «BC2_AGGREGATE_A»sController.cs
│   │       ├── «BC2_AGGREGATE_B»sController.cs
│   │       ├── Resources/
│   │       │   ├── «BC2_AGGREGATE_A»Resource.cs
│   │       │   ├── Create«BC2_AGGREGATE_B»Resource.cs
│   │       │   └── «BC2_AGGREGATE_B»Resource.cs
│   │       └── Transform/
│   │           ├── «BC2_AGGREGATE_A»ResourceFromEntityAssembler.cs
│   │           ├── Create«BC2_AGGREGATE_B»CommandFromResourceAssembler.cs
│   │           └── «BC2_AGGREGATE_B»ResourceFromEntityAssembler.cs
│   └── Resources/
│       ├── «BC2_NAME»Messages.cs
│       ├── «BC2_NAME»Messages.resx
│       └── «BC2_NAME»Messages.es.resx
│
└── Program.cs
```

---

## PASO 8 — Shared Package (SE COPIA SIEMPRE IGUAL)

### 8.1 Result.cs

Ruta: `Shared/Application/Model/Result.cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Application.Model;

/// <summary>
/// Represents the outcome of an operation that produces a value of type T.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class Result<T>
{
    public T? Value { get; }
    public bool IsSuccess { get; }
    public string? ErrorCode { get; }
    public string? ErrorMessage { get; }

    private Result(T value) { Value = value; IsSuccess = true; }
    private Result(string errorCode, string errorMessage)
    {
        ErrorCode = errorCode; ErrorMessage = errorMessage; IsSuccess = false;
    }

    /// <summary>Creates a successful result containing the specified value.</summary>
    public static Result<T> Success(T value) => new(value);

    /// <summary>Creates a failed result with the given error code and localized message.</summary>
    public static Result<T> Failure(string errorCode, string errorMessage) => new(errorCode, errorMessage);
}
```

---

### 8.2 IAuditableEntity.cs

Ruta: `Shared/Domain/Model/IAuditableEntity.cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Domain.Model;

/// <summary>
/// Marker interface for auditable entities that track creation and update timestamps.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public interface IAuditableEntity
{
    DateTimeOffset? CreatedAt { get; set; }
    DateTimeOffset? UpdatedAt { get; set; }
}
```

---

### 8.3 IBaseRepository.cs

Ruta: `Shared/Domain/Repositories/IBaseRepository.cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Domain.Repositories;

/// <summary>
/// Generic base repository interface providing standard CRUD operations.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public interface IBaseRepository<TEntity>
{
    Task<TEntity?> FindByIdAsync(int id, CancellationToken cancellationToken);
    Task<IEnumerable<TEntity>> ListAsync(CancellationToken cancellationToken);
    Task AddAsync(TEntity entity, CancellationToken cancellationToken);
}
```

---

### 8.4 IUnitOfWork.cs

Ruta: `Shared/Domain/Repositories/IUnitOfWork.cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Domain.Repositories;

/// <summary>
/// Unit of work abstraction that coordinates writing of changes to the database.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public interface IUnitOfWork
{
    Task CompleteAsync(CancellationToken cancellationToken);
}
```

---

### 8.5 «SHARED_VALUE_OBJECT».cs

> ⚠️ **Cambia el nombre y campos según lo que pida tu examen en Shared**.
> (Ejemplo: `PrescriptionPeriod`, `Period`, `ValidityPeriod`, etc.)

Ruta: `Shared/Domain/Model/«SHARED_VALUE_OBJECT».cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Domain.Model;

/// <summary>
/// Value object representing «DESCRIPCIÓN DEL VO SEGÚN TU EXAMEN».
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public record «SHARED_VALUE_OBJECT»
{
    // ← AGREGAR LAS PROPIEDADES SEGÚN TU EXAMEN
    // Ejemplo PrescriptionPeriod: public DateOnly IssuedDate { get; private set; }
    //                              public DateOnly ExpirationDate { get; private set; }

    public «SHARED_VALUE_OBJECT»(/* PARÁMETROS SEGÚN TU EXAMEN */)
    {
        // VALIDACIONES SEGÚN TU EXAMEN
        // Ejemplo:
        // if (expirationDate <= issuedDate)
        //     throw new ArgumentException("ExpirationDate must be after IssuedDate.");
        // if ((expirationDate.ToDateTime(TimeOnly.MinValue) - issuedDate.ToDateTime(TimeOnly.MinValue)).Days > 30)
        //     throw new ArgumentException("Period cannot exceed 30 calendar days.");
        // IssuedDate = issuedDate;
        // ExpirationDate = expirationDate;
    }

    /// <summary>Parameterless constructor required by EF Core.</summary>
    protected «SHARED_VALUE_OBJECT»() { }

    // ← AGREGAR MÉTODOS DE NEGOCIO SI EL EXAMEN LOS PIDE
    // Ejemplo: public bool IsExpired(DateOnly checkDate) => checkDate > ExpirationDate;
}
```

---

### 8.6 AppDbContext.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Configuration/AppDbContext.cs`

```csharp
using Microsoft.EntityFrameworkCore;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Domain.Model.Aggregates;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Infrastructure.Persistence.EntityFrameworkCore.Configuration;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Infrastructure.Persistence.EntityFrameworkCore.Configuration;
using «NAMESPACE_RAIZ».Shared.Domain.Model;

namespace «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

/// <summary>
/// Entity Framework Core database context for the «NOMBRE_CASO» platform.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class AppDbContext(DbContextOptions options) : DbContext(options)
{
    // ← DbSets para cada aggregate
    public DbSet<«BC1_AGGREGATE»> «BC1_AGGREGATE»s { get; set; }
    public DbSet<«BC2_AGGREGATE_A»> «BC2_AGGREGATE_A»s { get; set; }
    public DbSet<«BC2_AGGREGATE_B»> «BC2_AGGREGATE_B»s { get; set; } // si aplica

    /// <inheritdoc/>
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
        // Aplicar naming strategy (snake_case + plural)
        modelBuilder.UseSnakeCaseNamingConventionWithPluralization();
        // Configuraciones por BC
        modelBuilder.Apply«BC1_NAME»Configuration();
        modelBuilder.Apply«BC2_NAME»Configuration();
    }

    /// <inheritdoc/>
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        // UseAsyncSeeding — forma recomendada por EF Core para poblar datos iniciales
        optionsBuilder.UseAsyncSeeding(async (context, _, cancellationToken) =>
        {
            // ← SEED DEL «BC2_AGGREGATE_A» (el aggregate que pide tu examen sembrar)
            var existingCount = await context.Set<«BC2_AGGREGATE_A»>()
                .CountAsync(cancellationToken: cancellationToken);
            if (existingCount == 0)
            {
                context.Set<«BC2_AGGREGATE_A»>().AddRange(
                    // AGREGAR AQUÍ LOS REGISTROS INICIALES QUE PIDE TU EXAMEN
                    // Ejemplo:
                    // new «BC2_AGGREGATE_A» { /* campos */ },
                    // new «BC2_AGGREGATE_A» { /* campos */ }
                );
                await context.SaveChangesAsync(cancellationToken);
            }
        });
    }

    /// <inheritdoc/>
    public override Task<int> SaveChangesAsync(CancellationToken cancellationToken = default)
    {
        var now = DateTimeOffset.UtcNow;
        foreach (var entry in ChangeTracker.Entries<IAuditableEntity>())
        {
            if (entry.State == EntityState.Added)
                entry.Entity.CreatedAt = now;
            if (entry.State is EntityState.Added or EntityState.Modified)
                entry.Entity.UpdatedAt = now;
        }
        return base.SaveChangesAsync(cancellationToken);
    }
}
```

---

### 8.7 NamingConventions.cs (snake_case + plural)

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Configuration/NamingConventions.cs`

```csharp
using Humanizer;
using Microsoft.EntityFrameworkCore;

namespace «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

/// <summary>
/// Extension methods to apply snake_case and pluralized table naming conventions.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public static class NamingConventions
{
    /// <summary>
    /// Applies snake_case naming for columns and plural snake_case for table names.
    /// </summary>
    public static void UseSnakeCaseNamingConventionWithPluralization(this ModelBuilder modelBuilder)
    {
        foreach (var entity in modelBuilder.Model.GetEntityTypes())
        {
            // Table name: PascalCase → snake_case plural
            var tableName = entity.GetTableName();
            if (tableName != null)
                entity.SetTableName(tableName.Underscore().Pluralize());

            // Column names: PascalCase → snake_case
            foreach (var property in entity.GetProperties())
            {
                var columnName = property.GetColumnName();
                if (columnName != null)
                    property.SetColumnName(columnName.Underscore());
            }
        }
    }
}
```

---

### 8.8 BaseRepository.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Repositories/BaseRepository.cs`

```csharp
using Microsoft.EntityFrameworkCore;
using «NAMESPACE_RAIZ».Shared.Domain.Repositories;
using «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

namespace «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// Base EF Core repository implementing standard CRUD operations.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public abstract class BaseRepository<TEntity>(AppDbContext context) : IBaseRepository<TEntity>
    where TEntity : class
{
    protected AppDbContext Context { get; } = context;

    public async Task<TEntity?> FindByIdAsync(int id, CancellationToken cancellationToken)
        => await Context.Set<TEntity>().FindAsync([id], cancellationToken);

    public async Task<IEnumerable<TEntity>> ListAsync(CancellationToken cancellationToken)
        => await Context.Set<TEntity>().ToListAsync(cancellationToken);

    public async Task AddAsync(TEntity entity, CancellationToken cancellationToken)
        => await Context.Set<TEntity>().AddAsync(entity, cancellationToken);
}
```

---

### 8.9 UnitOfWork.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Repositories/UnitOfWork.cs`

```csharp
using «NAMESPACE_RAIZ».Shared.Domain.Repositories;
using «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

namespace «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// EF Core implementation of IUnitOfWork.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class UnitOfWork(AppDbContext context) : IUnitOfWork
{
    public async Task CompleteAsync(CancellationToken cancellationToken)
        => await context.SaveChangesAsync(cancellationToken);
}
```

---

### 8.10 GlobalExceptionHandler.cs (Middleware — RFC 7807 ProblemDetails)

Ruta: `Shared/Interfaces/Rest/GlobalExceptionHandler.cs`

```csharp
using Microsoft.AspNetCore.Mvc;

namespace «NAMESPACE_RAIZ».Shared.Interfaces.Rest;

/// <summary>
/// Global exception handling middleware that converts unhandled exceptions
/// to RFC 7807 ProblemDetails responses.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class GlobalExceptionHandler(RequestDelegate next, ILogger<GlobalExceptionHandler> logger)
{
    public async Task InvokeAsync(HttpContext context)
    {
        try
        {
            await next(context);
        }
        catch (ArgumentException ex)
        {
            logger.LogWarning(ex, "Argument exception occurred");
            await WriteProblemDetailsAsync(context, StatusCodes.Status400BadRequest,
                "Bad Request", ex.Message);
        }
        catch (InvalidOperationException ex)
        {
            logger.LogWarning(ex, "Invalid operation exception occurred");
            await WriteProblemDetailsAsync(context, StatusCodes.Status422UnprocessableEntity,
                "Business Rule Violation", ex.Message);
        }
        catch (KeyNotFoundException ex)
        {
            logger.LogWarning(ex, "Not found exception occurred");
            await WriteProblemDetailsAsync(context, StatusCodes.Status404NotFound,
                "Not Found", ex.Message);
        }
        catch (Exception ex)
        {
            logger.LogError(ex, "An unexpected error occurred");
            await WriteProblemDetailsAsync(context, StatusCodes.Status500InternalServerError,
                "Internal Server Error", "An unexpected error occurred. Please try again later.");
        }
    }

    private static async Task WriteProblemDetailsAsync(
        HttpContext context, int statusCode, string title, string detail)
    {
        var problem = new ProblemDetails
        {
            Status = statusCode,
            Title = title,
            Detail = detail,
            Instance = context.Request.Path
        };
        context.Response.StatusCode = statusCode;
        context.Response.ContentType = "application/problem+json";
        await context.Response.WriteAsJsonAsync(problem);
    }
}
```

> Registrar en `Program.cs` ANTES de `app.UseHttpsRedirection()`:
> ```csharp
> app.UseMiddleware<GlobalExceptionHandler>();
> ```

---

### 8.11 Shared Resources (.resx)

Crear `Shared/Resources/SharedMessages.cs`:

```csharp
namespace «NAMESPACE_RAIZ».Shared.Resources;
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class SharedMessages;
```

Crear `Shared/Resources/SharedMessages.resx` (EN):
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype"><value>text/microsoft-resx</value></resheader>
  <resheader name="version"><value>2.0</value></resheader>
  <data name="NotFound" xml:space="preserve"><value>The requested resource was not found.</value></data>
  <data name="Conflict" xml:space="preserve"><value>The resource already exists.</value></data>
  <data name="BadRequest" xml:space="preserve"><value>The request is invalid.</value></data>
  <data name="InternalServerError" xml:space="preserve"><value>An unexpected error occurred.</value></data>
</root>
```

Crear `Shared/Resources/SharedMessages.es.resx` (ES):
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype"><value>text/microsoft-resx</value></resheader>
  <resheader name="version"><value>2.0</value></resheader>
  <data name="NotFound" xml:space="preserve"><value>El recurso solicitado no fue encontrado.</value></data>
  <data name="Conflict" xml:space="preserve"><value>El recurso ya existe.</value></data>
  <data name="BadRequest" xml:space="preserve"><value>La solicitud es inválida.</value></data>
  <data name="InternalServerError" xml:space="preserve"><value>Ocurrió un error inesperado.</value></data>
</root>
```

---

## PASO 9 — ProcessedEvents (Idempotencia)

### IProcessedEventRepository.cs

Ruta: `Shared/Domain/Repositories/IProcessedEventRepository.cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Domain.Repositories;

/// <summary>
/// Repository for tracking processed domain events (idempotency).
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public interface IProcessedEventRepository
{
    Task<bool> ExistsByEventIdAsync(Guid eventId, CancellationToken cancellationToken);
    Task AddAsync(Guid eventId, string eventType, string aggregateId, CancellationToken cancellationToken);
}
```

### ProcessedEvent.cs (Entity)

Ruta: `Shared/Domain/Model/ProcessedEvent.cs`

```csharp
namespace «NAMESPACE_RAIZ».Shared.Domain.Model;

/// <summary>
/// Tracks processed domain events to ensure idempotent event handling.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class ProcessedEvent
{
    public Guid EventId { get; private set; }
    public string EventType { get; private set; } = string.Empty;
    public string AggregateId { get; private set; } = string.Empty;
    public DateTimeOffset ProcessedAt { get; private set; }

    protected ProcessedEvent() { }

    public ProcessedEvent(Guid eventId, string eventType, string aggregateId)
    {
        EventId = eventId;
        EventType = eventType;
        AggregateId = aggregateId;
        ProcessedAt = DateTimeOffset.UtcNow;
    }
}
```

### ProcessedEventRepository.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Repositories/ProcessedEventRepository.cs`

```csharp
using Microsoft.EntityFrameworkCore;
using «NAMESPACE_RAIZ».Shared.Domain.Model;
using «NAMESPACE_RAIZ».Shared.Domain.Repositories;
using «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

namespace «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// EF Core implementation of IProcessedEventRepository.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class ProcessedEventRepository(AppDbContext context) : IProcessedEventRepository
{
    public async Task<bool> ExistsByEventIdAsync(Guid eventId, CancellationToken cancellationToken)
        => await context.Set<ProcessedEvent>()
            .AnyAsync(e => e.EventId == eventId, cancellationToken);

    public async Task AddAsync(Guid eventId, string eventType, string aggregateId,
        CancellationToken cancellationToken)
    {
        var processed = new ProcessedEvent(eventId, eventType, aggregateId);
        await context.Set<ProcessedEvent>().AddAsync(processed, cancellationToken);
    }
}
```

> Agregar `DbSet<ProcessedEvent>` al `AppDbContext` y agregar a la tabla en `ModelBuilderExtensions`.

---

## PASO 10 — BC1: «BC1_NAME» (Aggregate con POST)

### 10.1 Value Objects de BC1

> Crear uno por cada VO que BC1 tenga propio (no los de Shared).

Ruta: `«BC1_FOLDER»/Domain/Model/ValueObjects/«VO_NAME».cs`

```csharp
namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.ValueObjects;

/// <summary>
/// Value object representing «DESCRIPCIÓN».
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
/// <param name="Value">The underlying value.</param>
public record «VO_NAME»(Guid Value)
{
    // ← Si el VO es un UUID generado en otro BC, solo necesita el Guid
    // Si tiene validación propia, agregar aquí:
    // public «VO_NAME» { if (Value == Guid.Empty) throw new ArgumentException("..."); }

    public override string ToString() => Value.ToString();
}
```

### 10.2 Enums de BC1 (si aplica)

```csharp
namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;

/// <summary>Represents the status of a «BC1_AGGREGATE».</summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public enum «BC1_ENUM_NAME»
{
    // ← VALORES SEGÚN TU EXAMEN (ej: Active, Inactive, Expired)
}
```

### 10.3 «BC1_AGGREGATE».cs (Aggregate Root)

Ruta: `«BC1_FOLDER»/Domain/Model/Aggregates/«BC1_AGGREGATE».cs`

```csharp
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.ValueObjects;
using «NAMESPACE_RAIZ».Shared.Domain.Model;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;

/// <summary>
/// Aggregate root representing a «BC1_AGGREGATE» in the «BC1_NAME» bounded context.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public partial class «BC1_AGGREGATE»
{
    public int Id { get; private set; }

    // ← AGREGAR TODOS LOS ATRIBUTOS QUE PIDE TU EXAMEN
    // Ejemplo:
    // public «VO_NAME» «VO_PROP_NAME» { get; private set; }
    // public string Name { get; private set; } = string.Empty;
    // public «BC1_ENUM_NAME» Status { get; private set; }
    // public «SHARED_VALUE_OBJECT» Period { get; private set; }
    // public int? LastDispensationId { get; private set; }  ← nullable

    protected «BC1_AGGREGATE»() { }

    public «BC1_AGGREGATE»(/* PARÁMETROS SEGÚN TU EXAMEN */)
    {
        // INICIALIZAR ATRIBUTOS
        // Ejemplo:
        // «VO_PROP_NAME» = new «VO_NAME»(prescriptionIdentifier);
        // Period = new «SHARED_VALUE_OBJECT»(issuedDate, expirationDate);
        // Status = «BC1_ENUM_NAME».Active;
    }

    // ← MÉTODOS DE NEGOCIO QUE PIDA TU EXAMEN
    // Ejemplo:
    // public bool CanBeDispensed() =>
    //     Status == «BC1_ENUM_NAME».Active &&
    //     !Period.IsExpired(DateOnly.FromDateTime(DateTime.Today)) &&
    //     LastDispensationId == null;
    //
    // public void SetLastDispensationId(int id) => LastDispensationId = id;
}
```

### 10.4 «BC1_AGGREGATE»Audit.cs (partial class — IMPORTANTE para C04)

Ruta: `«BC1_FOLDER»/Domain/Model/Aggregates/«BC1_AGGREGATE»Audit.cs`

```csharp
using «NAMESPACE_RAIZ».Shared.Domain.Model;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;

/// <summary>
/// Partial class providing auditing capabilities to the «BC1_AGGREGATE» aggregate root.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public partial class «BC1_AGGREGATE» : IAuditableEntity
{
    public DateTimeOffset? CreatedAt { get; set; }
    public DateTimeOffset? UpdatedAt { get; set; }
}
```

### 10.5 Create«BC1_AGGREGATE»Command.cs

```csharp
namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Commands;

/// <summary>
/// Command to create a new «BC1_AGGREGATE».
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public record Create«BC1_AGGREGATE»Command(
    // ← CAMPOS SEGÚN TU EXAMEN (como primitivos, no como VOs)
    // Ejemplo:
    // Guid PrescriptionIdentifier,
    // Guid PatientId,
    // DateOnly IssuedDate,
    // DateOnly ExpirationDate,
    // string Status
);
```

### 10.6 I«BC1_AGGREGATE»Repository.cs

```csharp
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Repositories;

/// <summary>
/// Domain repository interface for the «BC1_AGGREGATE» aggregate root.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public interface I«BC1_AGGREGATE»Repository
{
    Task<«BC1_AGGREGATE»?> FindByIdAsync(int id, CancellationToken cancellationToken);
    Task AddAsync(«BC1_AGGREGATE» entity, CancellationToken cancellationToken);

    // ← MÉTODOS ESPECÍFICOS DEL DOMINIO
    // Ejemplo: Task<bool> ExistsByPrescriptionIdentifierAsync(Guid id, CancellationToken ct);
    // Ejemplo: Task<«BC1_AGGREGATE»?> FindByPrescriptionIdentifierAsync(Guid id, CancellationToken ct);
}
```

### 10.7 I«BC1_AGGREGATE»CommandService.cs

```csharp
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Commands;
using «NAMESPACE_RAIZ».Shared.Application.Model;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Application.CommandServices;

/// <summary>
/// Application service interface for «BC1_AGGREGATE» command operations.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public interface I«BC1_AGGREGATE»CommandService
{
    Task<Result<«BC1_AGGREGATE»>> Handle(
        Create«BC1_AGGREGATE»Command command, CancellationToken cancellationToken);
}
```

### 10.8 «BC1_NAME»Messages (.resx)

Crear `«BC1_FOLDER»/Resources/«BC1_NAME»Messages.cs`:

```csharp
namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Resources;
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class «BC1_NAME»Messages;
```

Crear `«BC1_FOLDER»/Resources/«BC1_NAME»Messages.resx` (EN):
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype"><value>text/microsoft-resx</value></resheader>
  <resheader name="version"><value>2.0</value></resheader>
  <data name="«BC1_AGGREGATE»AlreadyExists" xml:space="preserve">
    <value>A «BC1_AGGREGATE» with the given identifier already exists.</value>
  </data>
  <!-- ← AGREGAR MÁS MENSAJES SEGÚN LAS REGLAS DE NEGOCIO DE TU EXAMEN -->
</root>
```

Crear `«BC1_FOLDER»/Resources/«BC1_NAME»Messages.es.resx` (ES):
```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype"><value>text/microsoft-resx</value></resheader>
  <resheader name="version"><value>2.0</value></resheader>
  <data name="«BC1_AGGREGATE»AlreadyExists" xml:space="preserve">
    <value>Ya existe un «BC1_AGGREGATE» con el identificador indicado.</value>
  </data>
</root>
```

### 10.9 «BC1_AGGREGATE»CommandService.cs (Internal)

```csharp
using Microsoft.Extensions.Localization;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Application.CommandServices;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Commands;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Repositories;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Resources;
using «NAMESPACE_RAIZ».Shared.Application.Model;
using «NAMESPACE_RAIZ».Shared.Domain.Repositories;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Application.Internal.CommandServices;

/// <summary>
/// Handles «BC1_AGGREGATE» creation commands, enforcing business rules.
/// Business rules enforced:
/// ← LISTAR LAS REGLAS DE NEGOCIO DE TU EXAMEN AQUÍ
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class «BC1_AGGREGATE»CommandService(
    I«BC1_AGGREGATE»Repository repository,
    IUnitOfWork unitOfWork,
    IStringLocalizer<«BC1_NAME»Messages> localizer) : I«BC1_AGGREGATE»CommandService
{
    public async Task<Result<«BC1_AGGREGATE»>> Handle(
        Create«BC1_AGGREGATE»Command command, CancellationToken cancellationToken)
    {
        // BUSINESS RULE: No duplicate «UNIQUE_FIELD»
        // ← Adaptar según tu examen
        // if (await repository.ExistsByAsync(command.«UNIQUE_FIELD»ID, cancellationToken))
        //     return Result<«BC1_AGGREGATE»>.Failure(
        //         "«BC1_AGGREGATE»AlreadyExists",
        //         localizer["«BC1_AGGREGATE»AlreadyExists"]);

        // BUSINESS RULE: Validaciones adicionales
        // ← IMPLEMENTAR SEGÚN TU EXAMEN

        // Crear el aggregate
        «BC1_AGGREGATE» entity;
        try
        {
            entity = new «BC1_AGGREGATE»(/* parámetros del command */);
        }
        catch (ArgumentException ex)
        {
            return Result<«BC1_AGGREGATE»>.Failure("ValidationError", ex.Message);
        }

        await repository.AddAsync(entity, cancellationToken);
        await unitOfWork.CompleteAsync(cancellationToken);

        return Result<«BC1_AGGREGATE»>.Success(entity);
    }
}
```

### 10.10 Fluent API para BC1

Ruta: `«BC1_FOLDER»/Infrastructure/Persistence/EntityFrameworkCore/Configuration/ModelBuilderExtensions.cs`

```csharp
using Microsoft.EntityFrameworkCore;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Infrastructure.Persistence.EntityFrameworkCore.Configuration;

/// <summary>
/// Extension methods to apply «BC1_NAME» bounded context EF Core configuration.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public static class ModelBuilderExtensions
{
    public static void Apply«BC1_NAME»Configuration(this ModelBuilder builder)
    {
        builder.Entity<«BC1_AGGREGATE»>(entity =>
        {
            entity.ToTable("«BC1_AGGREGATE_PLURAL_SNAKE»"); // ej: prescriptions

            entity.HasKey(e => e.Id);
            entity.Property(e => e.Id)
                .HasColumnName("id")
                .IsRequired()
                .ValueGeneratedOnAdd();

            // ← MAPEAR CADA PROPIEDAD CON HasColumnName Y CONSTRAINTS
            // Ejemplo Value Object embebido:
            // entity.OwnsOne(e => e.Period, vo =>
            // {
            //     vo.Property(x => x.IssuedDate).HasColumnName("issued_date").IsRequired();
            //     vo.Property(x => x.ExpirationDate).HasColumnName("expiration_date").IsRequired();
            // });
            //
            // Ejemplo enum como string:
            // entity.Property(e => e.Status).HasColumnName("status").HasConversion<string>().IsRequired();
            //
            // Ejemplo nullable:
            // entity.Property(e => e.LastDispensationId).HasColumnName("last_dispensation_id").IsRequired(false);
            //
            // Auditoría:
            entity.Property(e => e.CreatedAt).HasColumnName("created_at");
            entity.Property(e => e.UpdatedAt).HasColumnName("updated_at");
        });
    }
}
```

### 10.11 «BC1_AGGREGATE»Repository.cs (Infrastructure)

```csharp
using Microsoft.EntityFrameworkCore;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Repositories;
using «NAMESPACE_RAIZ».Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// EF Core repository for the «BC1_AGGREGATE» aggregate root.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class «BC1_AGGREGATE»Repository(AppDbContext context) : I«BC1_AGGREGATE»Repository
{
    public async Task<«BC1_AGGREGATE»?> FindByIdAsync(int id, CancellationToken cancellationToken)
        => await context.Set<«BC1_AGGREGATE»>().FindAsync([id], cancellationToken);

    public async Task AddAsync(«BC1_AGGREGATE» entity, CancellationToken cancellationToken)
        => await context.Set<«BC1_AGGREGATE»>().AddAsync(entity, cancellationToken);

    // ← IMPLEMENTAR MÉTODOS ESPECÍFICOS
    // Ejemplo:
    // public async Task<bool> ExistsByPrescriptionIdentifierAsync(Guid id, CancellationToken ct)
    //     => await context.Set<«BC1_AGGREGATE»>()
    //         .AnyAsync(e => e.PrescriptionIdentifier.Value == id, ct);
}
```

### 10.12 Resources (Request/Response DTOs)

`«BC1_FOLDER»/Interfaces/Rest/Resources/Create«BC1_AGGREGATE»Resource.cs`:

```csharp
namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Resources;

/// <summary>
/// Request resource for creating a new «BC1_AGGREGATE».
/// Value object attributes are flattened into primitives.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public record Create«BC1_AGGREGATE»Resource(
    // ← CAMPOS SEGÚN TU EXAMEN (primitivos, sin el ID autogenerado)
    // Ejemplo:
    // Guid PrescriptionIdentifier,
    // Guid PatientId,
    // DateOnly IssuedDate,
    // DateOnly ExpirationDate,
    // string Status
);
```

`«BC1_FOLDER»/Interfaces/Rest/Resources/«BC1_AGGREGATE»Resource.cs`:

```csharp
namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Resources;

/// <summary>
/// Response resource representing a «BC1_AGGREGATE».
/// Does NOT include audit fields (CreatedAt, UpdatedAt).
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public record «BC1_AGGREGATE»Resource(
    int Id
    // ← AGREGAR CAMPOS SEGÚN LO QUE PIDA TU EXAMEN EN EL RESPONSE
    // (SIN CreatedAt ni UpdatedAt)
);
```

### 10.13 Assemblers

`Transform/Create«BC1_AGGREGATE»CommandFromResourceAssembler.cs`:

```csharp
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Commands;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Resources;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Transform;

/// <summary>
/// Assembler that transforms a Create«BC1_AGGREGATE»Resource into a Create«BC1_AGGREGATE»Command.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public static class Create«BC1_AGGREGATE»CommandFromResourceAssembler
{
    public static Create«BC1_AGGREGATE»Command ToCommandFromResource(
        Create«BC1_AGGREGATE»Resource resource)
        => new(/* MAPEAR CAMPOS DEL RESOURCE AL COMMAND */);
}
```

`Transform/«BC1_AGGREGATE»ResourceFromEntityAssembler.cs`:

```csharp
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Model.Aggregates;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Resources;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Transform;

/// <summary>
/// Assembler that maps a «BC1_AGGREGATE» domain entity to a «BC1_AGGREGATE»Resource.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public static class «BC1_AGGREGATE»ResourceFromEntityAssembler
{
    public static «BC1_AGGREGATE»Resource ToResourceFromEntity(«BC1_AGGREGATE» entity)
        => new(entity.Id /* , MAPEAR DEMÁS CAMPOS */);
}
```

### 10.14 Controller BC1 (POST)

```csharp
using System.Net.Mime;
using Microsoft.AspNetCore.Mvc;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Application.CommandServices;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Resources;
using «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest.Transform;
using Swashbuckle.AspNetCore.Annotations;

namespace «NAMESPACE_RAIZ».«BC1_FOLDER».Interfaces.Rest;

/// <summary>
/// REST controller for «BC1_AGGREGATE» operations.
/// Base URL: «BC1_ENDPOINT»
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
[ApiController]
[Route("«BC1_ENDPOINT»")]
[Produces(MediaTypeNames.Application.Json)]
[SwaggerTag("«BC1_AGGREGATE» management operations")]
public class «BC1_AGGREGATE»sController(
    I«BC1_AGGREGATE»CommandService commandService) : ControllerBase
{
    [HttpPost]
    [SwaggerOperation(
        Summary = "Create a new «BC1_AGGREGATE»",
        Description = "Registers a new «BC1_AGGREGATE». The id is auto-generated.",
        OperationId = "Create«BC1_AGGREGATE»")]
    [SwaggerResponse(StatusCodes.Status201Created,
        "«BC1_AGGREGATE» created successfully.", typeof(«BC1_AGGREGATE»Resource))]
    [SwaggerResponse(StatusCodes.Status400BadRequest, "Invalid input data.")]
    [SwaggerResponse(StatusCodes.Status409Conflict,
        "A «BC1_AGGREGATE» with the same identifier already exists.")]
    public async Task<IActionResult> Create«BC1_AGGREGATE»(
        [FromBody] Create«BC1_AGGREGATE»Resource resource,
        CancellationToken cancellationToken)
    {
        var command = Create«BC1_AGGREGATE»CommandFromResourceAssembler
            .ToCommandFromResource(resource);
        var result = await commandService.Handle(command, cancellationToken);

        if (!result.IsSuccess)
        {
            var body = new { message = result.ErrorMessage };
            return result.ErrorCode switch
            {
                "«BC1_AGGREGATE»AlreadyExists" => Conflict(body),
                _ => BadRequest(body)
            };
        }

        var response = «BC1_AGGREGATE»ResourceFromEntityAssembler
            .ToResourceFromEntity(result.Value!);
        return CreatedAtAction(nameof(Create«BC1_AGGREGATE»),
            new { id = response.Id }, response);
    }
}
```

---

## PASO 11 — BC2: «BC2_NAME» (GET + POST + ACL + Evento)

### 11.1 Aggregate con GET: «BC2_AGGREGATE_A».cs + Audit partial

> Seguir el mismo patrón que BC1 (pasos 10.3 y 10.4), con su propio partial class de auditoría.

```csharp
// «BC2_FOLDER»/Domain/Model/Aggregates/«BC2_AGGREGATE_A».cs
public partial class «BC2_AGGREGATE_A»
{
    public int Id { get; private set; }

    // ← CAMPOS SEGÚN TU EXAMEN
    // Ejemplo Medicine:
    // public string MedicineCode { get; private set; }
    // public string CommercialName { get; private set; }
    // public int InitialStock { get; private set; }
    // public decimal UnitPrice { get; private set; }
    // public int TotalDispensedQuantity { get; private set; }  ← si se actualiza via evento

    // ← PROPIEDADES CALCULADAS (NO se persisten, computed domain invariants)
    // Ejemplo:
    // public int AvailableStock => InitialStock - TotalDispensedQuantity;
    // public string AvailabilityStatus => AvailableStock == 0 ? "OutOfStock"
    //     : AvailableStock < 5 ? "LowStock" : "Available";

    // ← MÉTODOS DE NEGOCIO
    // Ejemplo:
    // public bool HasAvailableStock(int requestedQuantity) =>
    //     AvailableStock >= requestedQuantity;

    protected «BC2_AGGREGATE_A»() { }
    public «BC2_AGGREGATE_A»(/* parámetros */) { /* inicializar */ }
}

// «BC2_FOLDER»/Domain/Model/Aggregates/«BC2_AGGREGATE_A»Audit.cs
public partial class «BC2_AGGREGATE_A» : IAuditableEntity
{
    public DateTimeOffset? CreatedAt { get; set; }
    public DateTimeOffset? UpdatedAt { get; set; }
}
```

### 11.2 Aggregate con POST: «BC2_AGGREGATE_B».cs + Audit partial

```csharp
// Mismo patrón que arriba
// ← INCLUYE LOS CAMPOS QUE PIDE TU EXAMEN
// Ejemplo Dispensation:
// PrescriptionId, MedicineId, Quantity, DispensationStatus, DispensedAt
// Nota: DispensedAt puede venir como string "yyyy-MM-dd HH:mm:ss" → parsear en command service
```

### 11.3 Evento de integración

`«BC2_FOLDER»/Domain/Model/Events/«DOMAIN_EVENT_NAME».cs`:

```csharp
namespace «NAMESPACE_RAIZ».«BC2_FOLDER».Domain.Model.Events;

/// <summary>
/// Integration event published when a «BC2_AGGREGATE_B» is successfully created.
/// Consumed by the event handler in the «BC2_NAME» bounded context.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public record «DOMAIN_EVENT_NAME»(
    Guid EventId,              // Para idempotencia
    // ← CAMPOS DEL EVENTO SEGÚN TU EXAMEN
    // Ejemplo: int DispensationId, int MedicineId, int Quantity
);
```

### 11.4 ACL Facade

`«BC2_FOLDER»/Infrastructure/Acl/«ACL_FACADE_NAME».cs`:

```csharp
using «NAMESPACE_RAIZ».«BC1_FOLDER».Domain.Repositories;

namespace «NAMESPACE_RAIZ».«BC2_FOLDER».Infrastructure.Acl;

/// <summary>
/// Anti-Corruption Layer facade that allows the «BC2_NAME» bounded context
/// to access information from the «BC1_NAME» bounded context
/// WITHOUT direct coupling to its repositories or domain internals.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class «ACL_FACADE_NAME»(I«BC1_AGGREGATE»Repository repository)
{
    /// <summary>Validates that a «BC1_AGGREGATE» with the given id exists.</summary>
    public async Task<bool> ExistsByIdAsync(int id, CancellationToken cancellationToken)
        => await repository.FindByIdAsync(id, cancellationToken) != null;

    // ← AGREGAR MÁS MÉTODOS SEGÚN LO QUE TU EXAMEN NECESITE
    // Ejemplo (MiFarma):
    // public async Task<bool> IsActiveAsync(int prescriptionId, CancellationToken ct) { ... }
    // public async Task<bool> IsExpiredAsync(int prescriptionId, CancellationToken ct) { ... }
    // public async Task<bool> CanBeDispensedAsync(int prescriptionId, CancellationToken ct) { ... }
}
```

### 11.5 Command Service BC2 (con ACL + Evento)

```csharp
public class «BC2_AGGREGATE_B»CommandService(
    I«BC2_AGGREGATE_B»Repository repository,
    I«BC2_AGGREGATE_A»Repository «BC2_AGGREGATE_A_LOWER»Repository,
    «ACL_FACADE_NAME» aclFacade,
    IProcessedEventRepository processedEventRepository,
    IUnitOfWork unitOfWork,
    IStringLocalizer<«BC2_NAME»Messages> localizer) : I«BC2_AGGREGATE_B»CommandService
{
    public async Task<Result<«BC2_AGGREGATE_B»>> Handle(
        Create«BC2_AGGREGATE_B»Command command, CancellationToken cancellationToken)
    {
        // BUSINESS RULE: Verificar via ACL que el «BC1_AGGREGATE» existe
        if (!await aclFacade.ExistsByIdAsync(command.«BC1_REF_ID», cancellationToken))
            return Result<«BC2_AGGREGATE_B»>.Failure("NotFound",
                localizer["«BC1_AGGREGATE»NotFound"]);

        // ← AGREGAR MÁS VALIDACIONES VIA ACL SEGÚN TU EXAMEN
        // Ejemplo:
        // if (!await aclFacade.IsActiveAsync(command.PrescriptionId, cancellationToken))
        //     return Result<Dispensation>.Failure("InvalidStatus",
        //         localizer["PrescriptionNotActive"]);
        // if (await aclFacade.IsExpiredAsync(command.PrescriptionId, cancellationToken))
        //     return Result<Dispensation>.Failure("Expired",
        //         localizer["PrescriptionExpired"]);

        // BUSINESS RULE: Verificar stock (si aplica)
        // var medicine = await medicineRepository.FindByIdAsync(command.MedicineId, ct);
        // if (medicine == null || !medicine.HasAvailableStock(command.Quantity))
        //     return Result<Dispensation>.Failure("InsufficientStock", localizer["InsufficientStock"]);

        // Crear el aggregate
        «BC2_AGGREGATE_B» entity;
        try
        {
            entity = new «BC2_AGGREGATE_B»(/* parámetros */);
        }
        catch (ArgumentException ex)
        {
            return Result<«BC2_AGGREGATE_B»>.Failure("ValidationError", ex.Message);
        }

        await repository.AddAsync(entity, cancellationToken);
        await unitOfWork.CompleteAsync(cancellationToken);

        // Publicar el evento de integración
        var eventId = Guid.NewGuid();
        var domainEvent = new «DOMAIN_EVENT_NAME»(
            eventId
            /* , otros campos */
        );

        // Verificar idempotencia antes de emitir
        if (!await processedEventRepository.ExistsByEventIdAsync(eventId, cancellationToken))
        {
            await processedEventRepository.AddAsync(
                eventId, nameof(«DOMAIN_EVENT_NAME»),
                entity.Id.ToString(), cancellationToken);
            await unitOfWork.CompleteAsync(cancellationToken);

            // Publicar (con Cortex.Mediator o manualmente)
            // await mediator.Publish(domainEvent, cancellationToken);
            // Si no usas mediator, invocar el handler directamente:
            // await eventHandler.HandleAsync(domainEvent, cancellationToken);
        }

        return Result<«BC2_AGGREGATE_B»>.Success(entity);
    }
}
```

### 11.6 Event Handler

`«BC2_FOLDER»/Application/Internal/EventHandlers/«DOMAIN_EVENT_NAME»Handler.cs`:

```csharp
using «NAMESPACE_RAIZ».«BC2_FOLDER».Domain.Model.Events;
using «NAMESPACE_RAIZ».«BC2_FOLDER».Domain.Repositories;
using «NAMESPACE_RAIZ».Shared.Domain.Repositories;

namespace «NAMESPACE_RAIZ».«BC2_FOLDER».Application.Internal.EventHandlers;

/// <summary>
/// Event handler for «DOMAIN_EVENT_NAME».
/// Handles domain event by updating «BC2_AGGREGATE_A» state.
/// Execution is idempotent and runs within a transaction.
/// </summary>
/// <remarks>Author: Victor Jhosef Laura Acosta - u202418655</remarks>
public class «DOMAIN_EVENT_NAME»Handler(
    I«BC2_AGGREGATE_A»Repository «BC2_AGGREGATE_A_LOWER»Repository,
    IUnitOfWork unitOfWork)
{
    public async Task HandleAsync(«DOMAIN_EVENT_NAME» @event, CancellationToken cancellationToken)
    {
        // ← IMPLEMENTAR LA LÓGICA QUE PIDA TU EXAMEN
        // Ejemplo (MiFarma): Actualizar inventario
        // var medicine = await medicineRepository.FindByIdAsync(@event.MedicineId, cancellationToken);
        // if (medicine == null) return;
        //
        // medicine.AddDispensedQuantity(@event.Quantity);  // método del dominio
        // await unitOfWork.CompleteAsync(cancellationToken);
        //
        // Ejemplo (Whirlpool): Actualizar lastClaimId
        // var policy = await policyRepository.FindByIdAsync(@event.PolicyId, cancellationToken);
        // policy?.SetLastClaimId(@event.ClaimId);
        // await unitOfWork.CompleteAsync(cancellationToken);
    }
}
```

---

## PASO 12 — Verificación y Ejecución

```bash
dotnet restore
dotnet build
dotnet run
```

### URLs a verificar

```
https://localhost:«PUERTO»/swagger                  ← Swagger UI
«BC1_ENDPOINT»                                       ← POST BC1
«BC2_ENDPOINT_A»                                     ← GET BC2a (datos del seeding)
«BC2_ENDPOINT_B»                                     ← POST BC2b
```

---

## PASO 13 — Limpiar y empaquetar

```bash
dotnet clean
```

Eliminar manualmente:
- `bin/` y `obj/` de todos los proyectos
- `.idea/` (Rider)
- `.vs/` (Visual Studio)

Crear el ZIP:

```
PowerShell: Compress-Archive -Path .\* -DestinationPath ..\eb«NRC»u202418655.zip
```

Nombre final: `eb«NRC»u202418655.zip`

---

## ✅ Checklist Final

### Shared (copia/pega todo)
- [ ] `Result<T>.cs`
- [ ] `IAuditableEntity.cs`
- [ ] `«SHARED_VALUE_OBJECT».cs`
- [ ] `IBaseRepository.cs` + `IUnitOfWork.cs`
- [ ] `AppDbContext.cs` (con `UseAsyncSeeding` y `SaveChangesAsync` override)
- [ ] `NamingConventions.cs` (snake_case + plural con Humanizer)
- [ ] `BaseRepository.cs` + `UnitOfWork.cs`
- [ ] `GlobalExceptionHandler.cs` (middleware RFC 7807 ProblemDetails)
- [ ] `SharedMessages.cs` + `.resx` en/es
- [ ] `ProcessedEvent.cs` + `IProcessedEventRepository.cs` + impl

### BC1 («BC1_NAME»)
- [ ] VOs del BC1 (records con validación)
- [ ] Enums del BC1
- [ ] `«BC1_AGGREGATE».cs` (partial) + `«BC1_AGGREGATE»Audit.cs` (partial con IAuditableEntity)
- [ ] `Create«BC1_AGGREGATE»Command.cs`
- [ ] `I«BC1_AGGREGATE»Repository.cs` + impl
- [ ] `I«BC1_AGGREGATE»CommandService.cs` + impl (con validaciones)
- [ ] `ModelBuilderExtensions.cs` (Fluent API)
- [ ] `«BC1_NAME»Messages.cs` + `.resx` en/es
- [ ] `Create«BC1_AGGREGATE»Resource.cs` + `«BC1_AGGREGATE»Resource.cs`
- [ ] 2 Assemblers
- [ ] Controller (POST → 201)

### BC2 («BC2_NAME»)
- [ ] VOs del BC2 + Enums
- [ ] `«BC2_AGGREGATE_A».cs` (partial con propiedades calculadas) + Audit
- [ ] `«BC2_AGGREGATE_B».cs` (partial) + Audit
- [ ] `«DOMAIN_EVENT_NAME».cs` (con EventId para idempotencia)
- [ ] `I«BC2_AGGREGATE_A»Repository.cs` + impl
- [ ] `I«BC2_AGGREGATE_B»Repository.cs` + impl
- [ ] `I«BC2_AGGREGATE_A»QueryService.cs` + impl
- [ ] `I«BC2_AGGREGATE_B»CommandService.cs` + impl (con ACL + evento)
- [ ] `«ACL_FACADE_NAME».cs` (sin acceso directo a repos de BC1 desde BC2 — solo via facade)
- [ ] `«DOMAIN_EVENT_NAME»Handler.cs` (idempotente + transaccional)
- [ ] `ModelBuilderExtensions.cs` para BC2
- [ ] `«BC2_NAME»Messages.cs` + `.resx` en/es
- [ ] Resources (DTOs) + Assemblers + Controllers (GET + POST)

### Configuración
- [ ] `.csproj` con todos los paquetes
- [ ] `Program.cs` registra todos los servicios + middleware
- [ ] `appsettings.json` con `DefaultConnection`
- [ ] `launchSettings.json` con el puerto correcto
- [ ] `README.md` en inglés
- [ ] `GlobalExceptionHandler` registrado ANTES de `UseHttpsRedirection` en `Program.cs`

### Entrega
- [ ] `dotnet build` sin errores
- [ ] Swagger UI accesible
- [ ] GET retorna datos del seeding
- [ ] POST BC1 → 201 Created
- [ ] POST BC2b → 201 Created
- [ ] POST BC2b con datos inválidos → 400 Bad Request
- [ ] Sin `bin/`, `obj/`, `.idea/` en el ZIP
- [ ] ZIP nombrado `eb«NRC»u202418655.zip`

---

## 📌 Diferencias clave entre PC2 y EB (Aplicaciones Web)

| Aspecto | PC2 (1 BC + 1 endpoint) | EB (2-3 BCs + 2-3 endpoints) |
|---|---|---|
| **Nombre ZIP** | `pc2«NRC»u«código».zip` | `eb«NRC»u«código».zip` |
| **Nombre solución** | `pc2«NRC»u«código»` | `eb«NRC»u«código».API` |
| **BCs** | 1 BC + Shared | 2-3 BCs + Shared |
| **Endpoints** | 1 (POST) | 2-3 (GET + POST x2) |
| **Data seeding** | No aplica | `UseAsyncSeeding` en DbContext |
| **Eventos** | No aplica | `«DOMAIN_EVENT_NAME»` + Handler |
| **ACL** | No aplica | `«ACL_FACADE_NAME»` |
| **Idempotencia** | No aplica | `ProcessedEvents` table |
| **Partial class** | `Covenant.cs` + `CovenantAudit.cs` | Mismo patrón, uno por aggregate |
| **Error handling** | `GlobalExceptionHandler` (basic) | `GlobalExceptionHandler` (RFC 7807 ProblemDetails) |
| **Propiedades calculadas** | No aplica | `AvailableStock`, `CoverageStatus`, etc. (NO persisten, son computed) |
| **Cortex.Mediator** | No aplica | Para publicar/manejar eventos |

---

## ⚡ Correcciones incorporadas vs PC2 (C04/C05)

| Problema anterior | Corrección en esta guía |
|---|---|
| **C04**: Estructura no del todo alineada con Learning Center | Partial class `XAudit.cs` para cada aggregate; `Resources/` folder por BC con `.resx`; `NamingConventions.cs` en vez de strategy separada |
| **C04**: Lógica de naming en lugar incorrecto | `NamingConventions.cs` como extension method de `ModelBuilder` en Shared Infrastructure |
| **C05**: Middleware de errores básico | `GlobalExceptionHandler` como middleware RFC 7807 ProblemDetails |
| **C05**: Faltaba `CancellationToken` | Todos los métodos usan `CancellationToken cancellationToken` |
| **C05**: Eventos sin idempotencia | `ProcessedEvents` table + verificación antes de emitir |
| **C05**: ACL sin facade explícita | `«ACL_FACADE_NAME»` con métodos específicos de dominio |
| **C05**: Propiedades calculadas persistidas | Computed properties (`AvailableStock`, etc.) solo en el aggregate, no en BD | , soy victor jhosef laura acosta, mi codigo es u202418655, el nrc del curso es 12190, profesor allan mori. Y si falta algo de esta guia dime que es
