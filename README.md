# Project Capbase Platform - Covenant Management API v1.0

## Datos del alumno

- **Nombre:** Victor Jhosef Laura Acosta
- **Código:** u202418655
- **NRC:** 12190
- **Curso:** Aplicaciones Web
- **Profesor:** Hugo Allan Mori Paiva
- **Proyecto:** `pc212190u202418655`
- **Solution:** `pc212190u202418655.sln`
- **Project:** `PC212190.U202418655.Capbase.Platform`
- **Package raíz:** `PC212190.U202418655.Capbase.Platform`
- **Puerto:** 8097
- **Base de datos:** `clerky_wa` (MySQL / PostgreSQL)

---

## Descripción del Proyecto

API RESTful para el caso de estudio **Capbase**, implementando el **bounded context Paperwork** (Trámites Documentarios). Expone un endpoint para crear documentos **Covenant** (Convenios Legales), persistidos en una base de datos relacional MySQL (o PostgreSQL como alternativa).

### Stack Tecnológico

| Tecnología | Versión |
|---|---|
| .NET SDK | 10.0 |
| C# | 14 |
| ASP.NET Core | 10.0 |
| Entity Framework Core | 10.0.8 |
| MySQL (MySql.EntityFrameworkCore) | 10.0.7 |
| PostgreSQL (alternativa) | Npgsql 10.0.x |
| Humanizer | 3.0.10 |
| Swashbuckle (Swagger) | 10.2.0 |
| Swashbuckle.Annotations | 10.2.0 |

### Arquitectura

El proyecto sigue una arquitectura **Domain-Driven Design (DDD)** combinada con **CQRS** y **capas arquitectónicas**:

```
┌──────────────────────────────────────────────────────┐
│                    Interfaces                         │
│              (REST Controllers / Resources)           │
├──────────────────────────────────────────────────────┤
│                   Application                         │
│            (CommandServices / QueryServices)           │
├──────────────────────────────────────────────────────┤
│                     Domain                            │
│       (Aggregates / ValueObjects / Commands /         │
│        Queries / Repositories / Errors / Events)      │
├──────────────────────────────────────────────────────┤
│                  Infrastructure                       │
│  (Persistence EF Core / Repositories / Configurations)│
└──────────────────────────────────────────────────────┘
```

---

## Configuración Inicial

### 1. Instalación de .NET 10 SDK

Descargar e instalar el .NET 10 SDK desde:

```
https://dotnet.microsoft.com/download/dotnet/10.0
```

Verificar la instalación:

```bash
dotnet --version
# Debería mostrar: 10.0.x
```

Verificar que C# 14 está disponible:

```bash
dotnet --list-sdks
# Debería mostrar 10.0.x
```

### 2. Instalación de MySQL

#### Opción A: MySQL (Recomendada)

Descargar e instalar MySQL Community Server desde:

```
https://dev.mysql.com/downloads/mysql/
```

Durante la instalación:
- Puerto: `3306`
- Usuario root: `root`
- Contraseña: `12345678` (o la que prefieras)

Alternativamente, usar **XAMPP** o **WAMP** que incluyen MySQL integrado.

> **Nota para XAMPP:** Asegúrate de iniciar el módulo MySQL desde el Panel de Control de XAMPP.

#### Opción B: PostgreSQL (Alternativa)

Descargar e instalar PostgreSQL desde:

```
https://www.postgresql.org/download/
```

Durante la instalación:
- Puerto: `5432`
- Usuario postgres: `postgres`
- Contraseña: `12345678`

### 3. Creación de la Base de Datos

#### Para MySQL:

Abrir la consola de MySQL (MySQL Command Line Client o desde XAMPP Shell) y ejecutar:

```sql
CREATE SCHEMA IF NOT EXISTS clerky_wa
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;

USE clerky_wa;
```

O desde phpMyAdmin (si usas XAMPP):
1. Abrir `http://localhost/phpmyadmin`
2. Click en "Nueva"
3. Nombre: `clerky_wa`
4. Cotejamiento: `utf8mb4_unicode_ci`
5. Click en "Crear"

#### Para PostgreSQL:

Abrir pgAdmin, ingresar la contraseña `12345678` y crear la base de datos `clerky_wa`:

```sql
CREATE DATABASE clerky_wa;
```

---

## Creación del Proyecto (Visual Studio 2022 / dotnet CLI)

### Método 1: Visual Studio 2022 (Recomendado)

1. Abrir **Visual Studio 2022**
2. Click en **Create a new project**
3. Seleccionar **ASP.NET Core Web API** (C#)
4. Configurar:
   - **Project name:** `PC212190.U202418655.Capbase.Platform`
   - **Solution name:** `pc212190u202418655`
   - **Location:** `C:\Users\vcris\Downloads\Examen\pc212190u202418655`
   - **Framework:** .NET 10.0
   - **Authentication type:** None
   - **Configure for HTTPS:** ✅
   - **Use controllers:** ✅ (uncheck "Use minimal APIs")
   - **Enable OpenAPI support:** ✅
5. Click en **Create**

### Método 2: dotnet CLI

```bash
# Crear la carpeta del proyecto
mkdir pc212190u202418655
cd pc212190u202418655

# Crear la solución
dotnet new sln -n pc212190u202418655

# Crear el proyecto Web API
dotnet new webapi -n PC212190.U202418655.Capbase.Platform --use-controllers

# Agregar el proyecto a la solución
dotnet sln add PC212190.U202418655.Capbase.Platform/PC212190.U202418655.Capbase.Platform.csproj
```

### Método 3: Spring Initializr analógico (No aplica aquí)

> **Importante:** A diferencia del proyecto de ejemplo Facturease (que usaba Spring Initializr), en ASP.NET Core no existe un asistente web equivalente. La creación se hace mediante Visual Studio o CLI como se describe arriba. Sin embargo, sí existen "project templates" que puedes instalar:
>
> ```bash
> dotnet new install Microsoft.AspNetCore.WebApi.Templates
> ```

---

## Limpieza de Carpetas Generadas por Defecto

Al crear el proyecto con Visual Studio o `dotnet new`, se generan automáticamente carpetas y archivos que **NO** deben formar parte del repositorio ni de la entrega porque son generados por el compilador o el IDE.

### Carpetas que DEBES ELIMINAR o IGNORAR:

| Carpeta/Archivo | Origen | ¿Se sube? |
|---|---|---|
| `bin/` | Compilación (binarios) | ❌ NO |
| `obj/` | Compilación (objetos intermedios) | ❌ NO |
| `.vs/` | Configuración local de Visual Studio | ❌ NO |
| `.idea/` | Configuración local de JetBrains Rider | ❌ NO |
| `.DS_Store` | Archivo oculto de macOS | ❌ NO |
| `*.user` | Configuración de usuario del IDE | ❌ NO |
| `*.suo` | Solution User Options | ❌ NO |
| `*.cache` | Archivos de caché | ❌ NO |
| `Debug/`, `Release/` | Carpetas de compilación | ❌ NO |

### Estructura que DEBE QUEDAR después de limpiar:

```
pc212190u202418655/
├── pc212190u202418655.sln
└── PC212190.U202418655.Capbase.Platform/
    ├── PC212190.U202418655.Capbase.Platform.csproj
    ├── Program.cs
    ├── appsettings.json
    ├── appsettings.Development.json
    └── Properties/
        └── launchSettings.json
```

Todo lo demás (`bin/`, `obj/`, `.vs/`, etc.) debe eliminarse. En la raíz del proyecto, después de la limpieza, deben quedar **únicamente** los archivos fuente.

> **Explicación para el profesor:** El profesor indicó que la estructura debe estar limpia de carpetas generadas por dependencias o compilación. Las carpetas `bin/` y `obj/` son generadas por el compilador de .NET al ejecutar `dotnet build` o `dotnet run`. La carpeta `.vs/` es generada por Visual Studio para almacenar configuraciones locales del entorno de desarrollo. Ninguna de estas debe incluirse en la entrega del proyecto. En su lugar, el proyecto debe compilarse desde el código fuente usando `dotnet build`, que regenerará estas carpetas automáticamente.

### ¿Cómo eliminarlas?

Desde la terminal:

```bash
# Parado en la raíz pc212190u202418655
rm -rf PC212190.U202418655.Capbase.Platform/bin
rm -rf PC212190.U202418655.Capbase.Platform/obj
rm -rf .vs
```

O desde el explorador de archivos:
1. Navegar a la carpeta del proyecto
2. Eliminar manualmente `bin/`, `obj/`, `.vs/`

---

## Configuración del SDK

### En Visual Studio 2022

1. Click derecho en el proyecto → **Properties**
2. Ir a **Application** → **Target Framework**: `net10.0`
3. Ir a **Build** → **Advanced** → **Language version**: `14` (o `latest`)

### En Rider / JetBrains

1. Archivo → **Settings** → **Build, Execution, Deployment** → **Compiler** → **C# Language Version**: `14`

### Verificar configuración manual (archivo .csproj)

El archivo `.csproj` debe tener las siguientes propiedades:

```xml
<PropertyGroup>
    <TargetFramework>net10.0</TargetFramework>
    <Nullable>enable</Nullable>
    <ImplicitUsings>enable</ImplicitUsings>
    <LangVersion>14</LangVersion>
</PropertyGroup>
```

---

## Archivo .csproj (Equivalente al pom.xml)

Abrir el archivo `PC212190.U202418655.Capbase.Platform.csproj` y reemplazar su contenido con:

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
        <!-- EntityFrameworkCore Database Persistence related packages -->
        <PackageReference Include="Microsoft.EntityFrameworkCore" Version="10.0.8"/>
        <PackageReference Include="Microsoft.EntityFrameworkCore.Relational" Version="10.0.8"/>
        <PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="10.0.8"/>

        <!-- MySQL Database Persistence related packages -->
        <PackageReference Include="MySql.EntityFrameworkCore" Version="10.0.7"/>

        <!-- Naming convention conversion related packages -->
        <PackageReference Include="Humanizer" Version="3.0.10"/>

        <!-- OpenAPI documentation related packages -->
        <PackageReference Include="Swashbuckle.AspNetCore" Version="10.2.0"/>
        <PackageReference Include="Swashbuckle.AspNetCore.Annotations" Version="10.2.0"/>
    </ItemGroup>

    <ItemGroup>
        <EmbeddedResource Update="Paperwork\Resources\PaperworkMessages.es.resx">
            <DependentUpon>PaperworkMessages.cs</DependentUpon>
        </EmbeddedResource>
        <EmbeddedResource Update="Resources\ErrorMessages.es.resx">
            <DependentUpon>ErrorMessages.cs</DependentUpon>
        </EmbeddedResource>
    </ItemGroup>

</Project>
```

### Explicación de dependencias:

| Paquete | Propósito |
|---|---|
| `Microsoft.EntityFrameworkCore` | ORM principal de Entity Framework Core |
| `Microsoft.EntityFrameworkCore.Relational` | Funcionalidades relacionales de EF Core |
| `Microsoft.EntityFrameworkCore.Design` | Herramientas de diseño (migrations, etc.) |
| `MySql.EntityFrameworkCore` | Provider de MySQL para EF Core |
| `Humanizer` | Conversión de nombres a snake_case, pluralización, etc. |
| `Swashbuckle.AspNetCore` | Generación de documentación OpenAPI / Swagger UI |
| `Swashbuckle.AspNetCore.Annotations` | Anotaciones para Swagger (`[SwaggerOperation]`, `[SwaggerResponse]`) |

### Si usas PostgreSQL en lugar de MySQL:

Reemplazar `MySql.EntityFrameworkCore` (10.0.7) por:

```xml
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="10.0.0"/>
```

> **Nota importante:** Elegir **una sola** base de datos. No incluyas ambos providers en el proyecto final. Esta guía usa MySQL como opción principal, con notas para PostgreSQL donde sea necesario.

---

## Archivo Program.cs (Equivalente a la clase Application.java)

Abrir `Program.cs` y reemplazar con el siguiente contenido. Este archivo es el punto de entrada de la aplicación y configura todos los servicios necesarios:

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.OpenApi.Models;
using PC212190.U202418655.Capbase.Platform.Paperwork.Application.CommandServices;
using PC212190.U202418655.Capbase.Platform.Paperwork.Application.Internal.CommandServices;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Repositories;
using PC212190.U202418655.Capbase.Platform.Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Repositories;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;
using PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;
using PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

var builder = WebApplication.CreateBuilder(args);

// ── Database ──────────────────────────────────────────────────────────────────
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection")
    ?? throw new InvalidOperationException("Connection string 'DefaultConnection' not found.");

builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseMySQL(connectionString));
// Para PostgreSQL, reemplazar la línea anterior con:
// options.UseNpgsql(connectionString);

// ── Localization ──────────────────────────────────────────────────────────────
builder.Services.AddLocalization();

// ── Repositories & Unit of Work ───────────────────────────────────────────────
builder.Services.AddScoped<ICovenantRepository, CovenantRepository>();
builder.Services.AddScoped<IUnitOfWork, UnitOfWork>();

// ── Application Services ──────────────────────────────────────────────────────
builder.Services.AddScoped<ICovenantCommandService, CovenantCommandService>();

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
        Title = "Capbase Platform API",
        Version = "v1",
        Description = "RESTful API for the Capbase Paperwork bounded context. " +
                       "Manages Covenant (legal covenant) documents.",
        Contact = new OpenApiContact
        {
            Name = "u202418655 - Victor Jhosef Laura Acosta"
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

// ── Database Migration ────────────────────────────────────────────────────────
using (var scope = app.Services.CreateScope())
{
    var db = scope.ServiceProvider.GetRequiredService<AppDbContext>();
    db.Database.EnsureCreated();
}

// ── Middleware ────────────────────────────────────────────────────────────────
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseSwaggerUI(options =>
    {
        options.SwaggerEndpoint("/swagger/v1/swagger.json", "Capbase Platform API v1");
        options.RoutePrefix = "swagger";
    });
}

app.UseHttpsRedirection();
app.MapControllers();

app.Run();
```

### Explicación de cada sección de Program.cs:

| Sección | Descripción |
|---|---|
| **Database** | Configura la conexión a MySQL/PostgreSQL usando EF Core |
| **Localization** | Habilita el sistema de recursos localizados (.resx) |
| **Repositories & Unit of Work** | Registra las implementaciones concretas de los repositorios en el contenedor DI |
| **Application Services** | Registra los servicios de aplicación (command/query handlers) |
| **Controllers** | Habilita los controladores REST y configura JSON con soporte de enums como strings |
| **OpenAPI / Swagger** | Configura Swagger UI para documentación interactiva |
| **Request Localization** | Configura los idiomas soportados (inglés, español) via header Accept-Language |
| **Database Migration** | Ejecuta EnsureCreated() para crear la tabla automáticamente al iniciar |
| **Middleware** | Habilita Swagger UI en desarrollo y redirección HTTPS |

---

## Archivo appsettings.json (Equivalente a application.properties)

Abrir `appsettings.json` y reemplazar con:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Port=3306;Database=clerky_wa;User=root;Password=12345678;"
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

### Configuración para PostgreSQL (alternativa):

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Port=5432;Database=clerky_wa;Username=postgres;Password=12345678;"
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

### Configuración del puerto (launchSettings.json):

Abrir `Properties/launchSettings.json` y modificar el perfil para que use el puerto `8097`:

```json
{
  "profiles": {
    "PC212190.U202418655.Capbase.Platform": {
      "applicationUrl": "https://localhost:8097;http://localhost:5088",
      "commandName": "Project",
      "dotnetRunMessages": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development"
      }
    }
  }
}
```

> **Importante:** El puerto HTTPS (`8097`) es el que usaremos para las pruebas. Asegúrate de que esté disponible y no esté siendo usado por otro servicio.

---

## Archivos de Recursos (i18n - Internacionalización)

### Resources/ErrorMessages.cs

Crear `Resources/ErrorMessages.cs`:

```csharp
namespace PC212190.U202418655.Capbase.Platform.Resources;

/// <summary>
/// Marker class used to scope IStringLocalizer for shared error messages.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class ErrorMessages;
```

### Resources/ErrorMessages.resx (Inglés)

Crear `Resources/ErrorMessages.resx`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype">
    <value>text/microsoft-resx</value>
  </resheader>
  <resheader name="version">
    <value>2.0</value>
  </resheader>
  <resheader name="reader">
    <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <resheader name="writer">
    <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <data name="BadRequest" xml:space="preserve">
    <value>The request is invalid.</value>
  </data>
  <data name="Conflict" xml:space="preserve">
    <value>The resource already exists.</value>
  </data>
  <data name="InternalServerError" xml:space="preserve">
    <value>An unexpected error occurred. Please try again later.</value>
  </data>
  <data name="NotFound" xml:space="preserve">
    <value>The requested resource was not found.</value>
  </data>
</root>
```

### Resources/ErrorMessages.es.resx (Español)

Crear `Resources/ErrorMessages.es.resx`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype">
    <value>text/microsoft-resx</value>
  </resheader>
  <resheader name="version">
    <value>2.0</value>
  </resheader>
  <resheader name="reader">
    <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <resheader name="writer">
    <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <data name="BadRequest" xml:space="preserve">
    <value>La solicitud es inválida.</value>
  </data>
  <data name="Conflict" xml:space="preserve">
    <value>El recurso ya existe.</value>
  </data>
  <data name="InternalServerError" xml:space="preserve">
    <value>Ocurrió un error inesperado. Por favor intente nuevamente.</value>
  </data>
  <data name="NotFound" xml:space="preserve">
    <value>El recurso solicitado no fue encontrado.</value>
  </data>
</root>
```

### Paperwork/Resources/PaperworkMessages.cs

Crear `Paperwork/Resources/PaperworkMessages.cs`:

```csharp
namespace PC212190.U202418655.Capbase.Platform.Paperwork.Resources;

/// <summary>
/// Marker class used to scope IStringLocalizer for Paperwork-specific messages.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class PaperworkMessages;
```

### Paperwork/Resources/PaperworkMessages.resx (Inglés)

Crear `Paperwork/Resources/PaperworkMessages.resx`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype">
    <value>text/microsoft-resx</value>
  </resheader>
  <resheader name="version">
    <value>2.0</value>
  </resheader>
  <resheader name="reader">
    <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <resheader name="writer">
    <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <data name="CovenantCreatedSuccessfully" xml:space="preserve">
    <value>Covenant created successfully.</value>
  </data>
  <data name="CovenantAlreadyExists" xml:space="preserve">
    <value>A Covenant with the given document identifier already exists.</value>
  </data>
  <data name="InvalidPeriod" xml:space="preserve">
    <value>EndDate must be strictly after StartDate.</value>
  </data>
  <data name="InvalidMonetaryAmount" xml:space="preserve">
    <value>Monetary amount value must be greater than or equal to zero.</value>
  </data>
</root>
```

### Paperwork/Resources/PaperworkMessages.es.resx (Español)

Crear `Paperwork/Resources/PaperworkMessages.es.resx`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <resheader name="resmimetype">
    <value>text/microsoft-resx</value>
  </resheader>
  <resheader name="version">
    <value>2.0</value>
  </resheader>
  <resheader name="reader">
    <value>System.Resources.ResXResourceReader, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <resheader name="writer">
    <value>System.Resources.ResXResourceWriter, System.Windows.Forms, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089</value>
  </resheader>
  <data name="CovenantCreatedSuccessfully" xml:space="preserve">
    <value>Convenio creado exitosamente.</value>
  </data>
  <data name="CovenantAlreadyExists" xml:space="preserve">
    <value>Ya existe un Convenio con el identificador de documento indicado.</value>
  </data>
  <data name="InvalidPeriod" xml:space="preserve">
    <value>La fecha de fin debe ser estrictamente posterior a la fecha de inicio.</value>
  </data>
  <data name="InvalidMonetaryAmount" xml:space="preserve">
    <value>El valor monetario debe ser mayor o igual a cero.</value>
  </data>
</root>
```

---

## Shared Package - Estructura de Carpetas (Basada en Learning Center)

> **IMPORTANTE:** Esta estructura de carpetas está inspirada en el proyecto **Learning Center Platform** proporcionado por el profesor, que fue calificado como un ejemplo de buena organización. La clave es que el bounded context `Shared` contiene infraestructura y utilidades reutilizables, mientras que los bounded contexts específicos (`Paperwork`) contienen la lógica de negocio. Nótese que NO existen archivos como `DataAnnotations` o validaciones en las entidades de dominio porque esas responsabilidades pertenecen a la capa de Infrastructure.

Crear toda la siguiente estructura bajo `PC212190.U202418655.Capbase.Platform/`:

```
PC212190.U202418655.Capbase.Platform/
├── (carpetas existentes: Program.cs, appsettings.json, etc.)
├── Resources/
│   ├── ErrorMessages.cs
│   ├── ErrorMessages.resx
│   └── ErrorMessages.es.resx
└── Shared/
    ├── Application/
    │   └── Model/
    │       └── Result.cs
    ├── Domain/
    │   ├── Model/
    │   │   ├── IAuditableEntity.cs
    │   │   ├── LegalityPeriod.cs
    │   │   └── MonetaryAmount.cs
    │   └── Repositories/
    │       ├── IBaseRepository.cs
    │       └── IUnitOfWork.cs
    └── Infrastructure/
        └── Persistence/
            └── EntityFrameworkCore/
                ├── Configuration/
                │   └── AppDbContext.cs
                └── Repositories/
                    ├── BaseRepository.cs
                    └── UnitOfWork.cs
```

> **NOTA IMPORTANTE sobre la estructura de Learning Center:**
>
> El proyecto Learning Center organiza el `Shared` de la siguiente manera:
> - **Shared/Application/Model/** → `Result.cs` (patrón Result)
> - **Shared/Application/Internal/EventHandlers/** → manejadores de eventos (si aplica)
> - **Shared/Domain/Model/Entities/** → interfaces como `IAuditableEntity`
> - **Shared/Domain/Model/Events/** → interfaces de eventos de dominio
> - **Shared/Domain/Repositories/** → interfaces genéricas (`IBaseRepository`, `IUnitOfWork`)
> - **Shared/Infrastructure/Persistence/EntityFrameworkCore/** → implementaciones concretas
> - **Shared/Infrastructure/Interfaces/AspNetCore/** → convenciones de naming, middlewares
> - **Shared/Interfaces/Rest/** → manejadores de errores globales, problemas de formato
>
> También NOTAR que en Learning Center el `Shared` NO contiene value objects de dominio porque cada bounded context define los suyos. Sin embargo, existen ciertos value objects que por su naturaleza transversal (como `LegalityPeriod` y `MonetaryAmount`) podrían ubicarse en Shared si son compartidos entre múltiples bounded contexts. En este proyecto, los colocamos en Shared precisamente por su naturaleza reutilizable.

### Diagrama de dependencias entre capas:

```
┌──────────────┐     ┌──────────────┐     ┌──────────────┐
│  Interfaces  │────>│  Application │────>│   Domain     │
│  (REST)      │     │  (Services)  │     │  (Core)      │
└──────────────┘     └──────────────┘     └──────┬───────┘
                                                  │
                                                  ▼
                                        ┌──────────────┐
                                        │Infrastructure │
                                        │ (Persistence) │
                                        └──────────────┘
```

Las flechas indican dirección de dependencia. El **Domain** es la capa más interna y no depende de nada externo. La **Infrastructure** implementa los contratos definidos en Domain. La **Application** orquesta la lógica usando Domain e Infrastructure. Las **Interfaces** son el punto de entrada REST.

---

## Shared Package - Archivos

### 01. Result.cs (Patrón Result)

Ruta: `Shared/Application/Model/Result.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Application.Model`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Shared.Application.Model;

/// <summary>
/// Represents the outcome of an operation that produces a value of type T.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <typeparam name="T">The type of the value returned on success.</typeparam>
public class Result<T>
{
    /// <summary>Gets the value produced on success.</summary>
    public T? Value { get; }

    /// <summary>Gets whether the operation succeeded.</summary>
    public bool IsSuccess { get; }

    /// <summary>Gets the application-level error code identifying the kind of failure.</summary>
    public string? ErrorCode { get; }

    /// <summary>Gets the localized error message when the operation failed.</summary>
    public string? ErrorMessage { get; }

    private Result(T value)
    {
        Value = value;
        IsSuccess = true;
    }

    private Result(string errorCode, string errorMessage)
    {
        ErrorCode = errorCode;
        ErrorMessage = errorMessage;
        IsSuccess = false;
    }

    /// <summary>Creates a successful result containing the specified value.</summary>
    public static Result<T> Success(T value) => new(value);

    /// <summary>Creates a failed result with the given error code and localized message.</summary>
    public static Result<T> Failure(string errorCode, string errorMessage) => new(errorCode, errorMessage);
}

/// <summary>
/// Represents the outcome of an operation that does not produce a value.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class Result
{
    /// <summary>Gets whether the operation succeeded.</summary>
    public bool IsSuccess { get; }

    /// <summary>Gets the application-level error code identifying the kind of failure.</summary>
    public string? ErrorCode { get; }

    /// <summary>Gets the localized error message when the operation failed.</summary>
    public string? ErrorMessage { get; }

    private Result(bool isSuccess, string? errorCode, string? errorMessage)
    {
        IsSuccess = isSuccess;
        ErrorCode = errorCode;
        ErrorMessage = errorMessage;
    }

    /// <summary>Creates a successful result.</summary>
    public static Result Success() => new(true, null, null);

    /// <summary>Creates a failed result with the given error code and localized message.</summary>
    public static Result Failure(string errorCode, string errorMessage) => new(false, errorCode, errorMessage);
}
```

**Explicación del patrón Result:**

El patrón Result es una alternativa a las excepciones para el flujo de control en la capa de aplicación. En lugar de lanzar excepciones para errores esperados (como "registro duplicado" o "periodo inválido"), el servicio devuelve un objeto `Result<T>` que indica explícitamente si la operación fue exitosa o falló. Esto:
- Evita usar excepciones para control de flujo (mejor práctica)
- Permite que el controlador REST decida qué código HTTP retornar según el error
- Facilita las pruebas unitarias al tener un contrato claro de salida
- Es consistente con el patrón usado en Learning Center

### 02. IAuditableEntity.cs

Ruta: `Shared/Domain/Model/IAuditableEntity.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Domain.Model`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;

/// <summary>
/// Marker interface for auditable entities that track creation and update timestamps.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public interface IAuditableEntity
{
    /// <summary>Gets or sets the date and time the entity was created.</summary>
    DateTimeOffset? CreatedAt { get; set; }

    /// <summary>Gets or sets the date and time the entity was last updated.</summary>
    DateTimeOffset? UpdatedAt { get; set; }
}
```

**Explicación:**

Esta interfaz permite que cualquier entidad que la implemente obtenga automáticamente las marcas de tiempo de creación y última modificación. El `AppDbContext` (en la capa de Infrastructure) se encarga de asignar estos valores automáticamente al guardar cambios mediante la sobreescritura de `SaveChangesAsync`. Esto es una implementación del patrón **Audit Trail** y es idéntico a cómo lo hace Learning Center.

### 03. LegalityPeriod.cs (Value Object - Dominio Compartido)

Ruta: `Shared/Domain/Model/LegalityPeriod.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Domain.Model`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;

/// <summary>
/// Value object representing a validity period defined by a start and end date.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public record LegalityPeriod
{
    /// <summary>Gets the inclusive start date of the period.</summary>
    public DateOnly StartDate { get; private set; }

    /// <summary>Gets the inclusive end date of the period.</summary>
    public DateOnly EndDate { get; private set; }

    /// <summary>
    /// Initializes a new LegalityPeriod and validates that EndDate is after StartDate.
    /// </summary>
    /// <param name="startDate">Start date of the period.</param>
    /// <param name="endDate">End date of the period. Must be strictly after startDate.</param>
    /// <exception cref="ArgumentException">Thrown when endDate is less than or equal to startDate.</exception>
    public LegalityPeriod(DateOnly startDate, DateOnly endDate)
    {
        if (endDate <= startDate)
            throw new ArgumentException("EndDate must be strictly after StartDate.");
        StartDate = startDate;
        EndDate = endDate;
    }

    /// <summary>Parameterless constructor required by EF Core.</summary>
    protected LegalityPeriod() { }

    /// <summary>
    /// Determines whether the period has expired relative to a given check date.
    /// </summary>
    /// <param name="checkDate">The date to compare against the end of the period.</param>
    /// <returns>true if checkDate is after EndDate; otherwise false.</returns>
    public bool IsExpired(DateOnly checkDate) => checkDate > EndDate;
}
```

**Explicación:**

`LegalityPeriod` es un **Value Object** del dominio compartido. Representa un período de validez legal con una fecha de inicio y una fecha de fin. Como value object:
- Es inmutable (sus propiedades solo se asignan en el constructor)
- Se valida a sí mismo (regla de negocio: EndDate > StartDate)
- No tiene identidad propia (dos períodos con las mismas fechas son iguales)
- Utiliza `DateOnly` (tipo de .NET 6+ que representa solo fecha, sin hora)
- Se mapea a dos columnas en la base de datos mediante `OwnsOne` en Fluent API
- Está en Shared porque podría ser usado por otros bounded contexts en el futuro

### 04. MonetaryAmount.cs (Value Object - Dominio Compartido)

Ruta: `Shared/Domain/Model/MonetaryAmount.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Domain.Model`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;

/// <summary>
/// Value object representing a monetary amount with a currency code.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public record MonetaryAmount
{
    /// <summary>Gets the numeric value of the monetary amount.</summary>
    public decimal Value { get; private set; }

    /// <summary>Gets the ISO currency code.</summary>
    public string Currency { get; private set; } = string.Empty;

    /// <summary>
    /// Initializes a new MonetaryAmount with validation.
    /// </summary>
    /// <param name="value">The numeric amount. Must be greater than or equal to zero.</param>
    /// <param name="currency">The currency code. Must not be null or blank.</param>
    /// <exception cref="ArgumentOutOfRangeException">Thrown when value is less than zero.</exception>
    /// <exception cref="ArgumentNullException">Thrown when currency is null or blank.</exception>
    public MonetaryAmount(decimal value, string currency)
    {
        if (value < decimal.Zero)
            throw new ArgumentOutOfRangeException(nameof(value),
                "Monetary amount value must be greater than or equal to zero.");
        if (string.IsNullOrWhiteSpace(currency))
            throw new ArgumentNullException(nameof(currency),
                "Currency must not be null or blank.");
        Value = value;
        Currency = currency;
    }

    /// <summary>Parameterless constructor required by EF Core.</summary>
    protected MonetaryAmount() { }

    /// <summary>
    /// Multiplies the monetary amount by a factor and returns a new MonetaryAmount with the same currency.
    /// </summary>
    /// <param name="factor">The multiplication factor.</param>
    /// <returns>A new MonetaryAmount with the multiplied value and the original currency.</returns>
    public MonetaryAmount MultiplyBy(decimal factor) => new(Value * factor, Currency);
}
```

**Explicación:**

`MonetaryAmount` es un **Value Object** que encapsula un valor monetario con su moneda (código ISO como "USD", "PEN", "EUR"). Similar a `LegalityPeriod`:
- Valida que el valor sea >= 0 y que la moneda no esté vacía
- Es inmutable (los setters son privados)
- Incluye un método `MultiplyBy` que devuelve una nueva instancia (opersión funcional)
- Se mapea a dos columnas en la BD: `monetary_amount_value` y `monetary_amount_currency`
- Está en Shared porque representa un concepto transversal

### 05. IBaseRepository.cs

Ruta: `Shared/Domain/Repositories/IBaseRepository.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;

/// <summary>
/// Generic base repository interface providing standard CRUD operations.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <typeparam name="TEntity">The entity type managed by this repository.</typeparam>
public interface IBaseRepository<TEntity>
{
    /// <summary>Finds an entity by its primary key.</summary>
    /// <param name="id">The primary key value.</param>
    /// <param name="cancellationToken">Cancellation token.</param>
    /// <returns>The entity if found; otherwise null.</returns>
    Task<TEntity?> FindByIdAsync(int id, CancellationToken cancellationToken);

    /// <summary>Returns all entities.</summary>
    /// <param name="cancellationToken">Cancellation token.</param>
    /// <returns>A read-only list of all entities.</returns>
    Task<IEnumerable<TEntity>> ListAsync(CancellationToken cancellationToken);

    /// <summary>Adds a new entity to the repository.</summary>
    /// <param name="entity">The entity to add.</param>
    /// <param name="cancellationToken">Cancellation token.</param>
    Task AddAsync(TEntity entity, CancellationToken cancellationToken);
}
```

### 06. IUnitOfWork.cs

Ruta: `Shared/Domain/Repositories/IUnitOfWork.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;

/// <summary>
/// Unit of work abstraction that coordinates writing of changes to the database.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public interface IUnitOfWork
{
    /// <summary>
    /// Persists all pending changes to the underlying data store.
    /// </summary>
    /// <param name="cancellationToken">Cancellation token.</param>
    Task CompleteAsync(CancellationToken cancellationToken);
}
```

**Explicación del patrón Repository + Unit of Work:**

Este patrón es fundamental en DDD y se utiliza tanto en Learning Center como en este proyecto:

- **Repository**: Abstrae el almacenamiento persistente. El dominio define la interfaz (contrato) y la infraestructura provee la implementación concreta (EF Core). Esto permite cambiar el motor de persistencia sin afectar la capa de dominio.
- **Unit of Work**: Agrupa múltiples operaciones en una sola transacción atómica. Cuando se llama a `CompleteAsync()`, todos los cambios pendientes se guardan en la base de datos. Si algo falla, todo se revierte.

La separación entre `IBaseRepository<T>` (genérico) e `ICovenantRepository` (específico) permite:
- Reutilizar operaciones CRUD estándar (AddAsync, FindByIdAsync, ListAsync)
- Agregar operaciones específicas del dominio (como `ExistsByDocumentIdentifierAsync`)
- Mantener el principio de Responsabilidad Única

### 07. AppDbContext.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Configuration/AppDbContext.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration`

```csharp
using Microsoft.EntityFrameworkCore;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Configuration;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;

namespace PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

/// <summary>
/// Entity Framework Core database context for the Capbase platform.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class AppDbContext(DbContextOptions options) : DbContext(options)
{
    /// <summary>Gets the Covenants dataset.</summary>
    public DbSet<Covenant> Covenants { get; set; }

    /// <inheritdoc/>
    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        base.OnModelCreating(modelBuilder);
        modelBuilder.ApplyPaperworkConfiguration();
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

**Explicación del DbContext:**

El `AppDbContext` es el corazón de la persistencia. Aquí ocurren varias cosas importantes:

1. **DbSet Covenants**: Expone la colección de entidades Covenant para consultas y operaciones.
2. **OnModelCreating**: Aplica la configuración Fluent API definida en `ModelBuilderExtensions.ApplyPaperworkConfiguration()`. Esta configuración define el mapeo entre las clases C# y las tablas/columnas de la base de datos.
3. **SaveChangesAsync override**: Implementa la auditoría automática. Cada vez que se guarda un cambio, las entidades que implementan `IAuditableEntity` reciben automáticamente los valores de `CreatedAt` y `UpdatedAt`. Esto evita que el developer tenga que asignar estas propiedades manualmente.

> **Diferencia con Learning Center:** En Learning Center, la auditoría se implementa mediante un `AuditableEntityInterceptor` de EF Core. En este proyecto, se implementa directamente en `SaveChangesAsync`. Ambas son enfoques válidos. La ventaja del interceptor es que funciona incluso si se usa `SaveChanges()` sin llamar al override; la ventaja de sobrescribir es que es más explícito y fácil de entender.

### 08. BaseRepository.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Repositories/BaseRepository.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories`

```csharp
using Microsoft.EntityFrameworkCore;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;
using PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

namespace PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// Base EF Core repository implementing standard CRUD operations.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <typeparam name="TEntity">The entity type managed by this repository.</typeparam>
public abstract class BaseRepository<TEntity>(AppDbContext context) : IBaseRepository<TEntity>
    where TEntity : class
{
    /// <summary>Provides access to the underlying database context.</summary>
    protected AppDbContext Context { get; } = context;

    /// <inheritdoc/>
    public async Task<TEntity?> FindByIdAsync(int id, CancellationToken cancellationToken)
        => await Context.Set<TEntity>().FindAsync([id], cancellationToken);

    /// <inheritdoc/>
    public async Task<IEnumerable<TEntity>> ListAsync(CancellationToken cancellationToken)
        => await Context.Set<TEntity>().ToListAsync(cancellationToken);

    /// <inheritdoc/>
    public async Task AddAsync(TEntity entity, CancellationToken cancellationToken)
        => await Context.Set<TEntity>().AddAsync(entity, cancellationToken);
}
```

### 09. UnitOfWork.cs

Ruta: `Shared/Infrastructure/Persistence/EntityFrameworkCore/Repositories/UnitOfWork.cs`

Package: `PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories`

```csharp
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;
using PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

namespace PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// EF Core implementation of IUnitOfWork that saves all pending changes.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class UnitOfWork(AppDbContext context) : IUnitOfWork
{
    /// <inheritdoc/>
    public async Task CompleteAsync(CancellationToken cancellationToken)
        => await context.SaveChangesAsync(cancellationToken);
}
```

---

## Paperwork Bounded Context - Estructura de Carpetas

> **IMPORTANTE:** Esta estructura sigue EXACTAMENTE el mismo patrón que Learning Center:
> - `Paperwork/Domain/Model/Aggregates/` → Aggregate roots (Covenant)
> - `Paperwork/Domain/Model/ValueObjects/` → Value objects específicos del BC
> - `Paperwork/Domain/Model/Commands/` → Comandos CQRS
> - `Paperwork/Domain/Model/Queries/` → Queries CQRS (si aplica)
> - `Paperwork/Domain/Model/Errors/` → Errores específicos del BC
> - `Paperwork/Domain/Repositories/` → Interfaces de repositorio
> - `Paperwork/Application/CommandServices/` → Interfaces de servicios de comando
> - `Paperwork/Application/Internal/CommandServices/` → Implementaciones de servicios
> - `Paperwork/Interfaces/Rest/` → Controladores REST
> - `Paperwork/Interfaces/Rest/Resources/` → DTOs de entrada/salida
> - `Paperwork/Interfaces/Rest/Transform/` → Assemblers (Resource ↔ Command)
> - `Paperwork/Infrastructure/Persistence/EntityFrameworkCore/` → Persistencia EF Core

Crear la siguiente estructura bajo `PC212190.U202418655.Capbase.Platform/`:

```
Paperwork/
├── Resources/
│   ├── PaperworkMessages.cs
│   ├── PaperworkMessages.resx
│   └── PaperworkMessages.es.resx
├── Domain/
│   ├── Model/
│   │   ├── Aggregates/
│   │   │   ├── Covenant.cs
│   │   │   ├── CovenantAudit.cs
│   │   │   ├── CovenantStatus.cs
│   │   │   └── PaperworkError.cs
│   │   ├── ValueObjects/
│   │   │   ├── CapbaseIdentifier.cs
│   │   │   └── PartyId.cs
│   │   └── Commands/
│   │       └── CreateCovenantCommand.cs
│   └── Repositories/
│       └── ICovenantRepository.cs
├── Application/
│   ├── CommandServices/
│   │   └── ICovenantCommandService.cs
│   └── Internal/
│       └── CommandServices/
│           └── CovenantCommandService.cs
├── Interfaces/
│   └── Rest/
│       ├── CovenantsController.cs
│       ├── Resources/
│       │   ├── CreateCovenantResource.cs
│       │   └── CovenantResource.cs
│       └── Transform/
│           ├── CreateCovenantCommandFromResourceAssembler.cs
│           └── CovenantResourceFromEntityAssembler.cs
└── Infrastructure/
    └── Persistence/
        └── EntityFrameworkCore/
            ├── Configuration/
            │   └── ModelBuilderExtensions.cs
            └── Repositories/
                └── CovenantRepository.cs
```

**Explicación de la estructura comparada con Learning Center:**

| Learning Center | PC (Capbase) | Explicación |
|---|---|---|
| `Publishing/Domain/Model/Aggregates/Tutorial.cs` | `Paperwork/Domain/Model/Aggregates/Covenant.cs` | Aggregate root principal |
| `Publishing/Domain/Model/Aggregates/TutorialAudit.cs` | `Paperwork/Domain/Model/Aggregates/CovenantAudit.cs` | Partial class para auditoría |
| `Publishing/Domain/Model/ValueObjects/` | `Paperwork/Domain/Model/ValueObjects/` | Value objects del BC |
| `Publishing/Domain/Model/Commands/CreateTutorialCommand.cs` | `Paperwork/Domain/Model/Commands/CreateCovenantCommand.cs` | Comando CQRS |
| `Publishing/Application/CommandServices/` | `Paperwork/Application/CommandServices/` | Interfaces de servicios |
| `Publishing/Application/Internal/CommandServices/` | `Paperwork/Application/Internal/CommandServices/` | Implementaciones |
| `Publishing/Interfaces/Rest/Controllers.cs` | `Paperwork/Interfaces/Rest/CovenantsController.cs` | Controladores REST |
| `Publishing/Interfaces/Rest/Resources/` | `Paperwork/Interfaces/Rest/Resources/` | DTOs |
| `Publishing/Interfaces/Rest/Transform/` | `Paperwork/Interfaces/Rest/Transform/` | Assemblers |
| `Publishing/Infrastructure/Persistence/` | `Paperwork/Infrastructure/Persistence/` | Persistencia EF Core |
| `Shared/Domain/Model/Entities/IAuditableEntity.cs` | `Shared/Domain/Model/IAuditableEntity.cs` | Interfaz de auditoría |
| `Shared/Domain/Repositories/IBaseRepository.cs` | `Shared/Domain/Repositories/IBaseRepository.cs` | Repositorio genérico |

---

## Paperwork - Domain Layer (Value Objects)

### 10. CapbaseIdentifier.cs

Ruta: `Paperwork/Domain/Model/ValueObjects/CapbaseIdentifier.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.ValueObjects`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.ValueObjects;

/// <summary>
/// Value object that uniquely identifies a Covenant document using a GUID generated externally.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <param name="Value">The GUID that identifies the Covenant document.</param>
public record CapbaseIdentifier(Guid Value)
{
    /// <summary>Returns the string representation of the underlying GUID.</summary>
    public override string ToString() => Value.ToString();
}
```

### 11. PartyId.cs

Ruta: `Paperwork/Domain/Model/ValueObjects/PartyId.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.ValueObjects`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.ValueObjects;

/// <summary>
/// Value object identifying a client party by a UUID originating in another bounded context.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <param name="Value">The UUID that identifies the party.</param>
public record PartyId(Guid Value)
{
    /// <summary>Returns the string representation of the underlying UUID.</summary>
    public override string ToString() => Value.ToString();
}
```

**Explicación de CapbaseIdentifier y PartyId:**

Ambos son **Value Objects** que encapsulan identificadores UUID/GUID generados externamente:

- **CapbaseIdentifier**: Identifica de manera única un documento Covenant. Es generado por el cliente (frontend u otro servicio) y se recibe como parte del comando de creación.
- **PartyId**: Identifica a la parte contratante (cliente). También es un UUID generado externamente, probablemente en otro bounded context (ej. un contexto de Clientes/Parties).

Son **records** de C# porque:
- Tienen igualdad estructural (dos instancias con el mismo GUID son iguales)
- Son inmutables por defecto
- Son livianos y fáciles de crear

Se mapean a la base de datos como columnas GUID mediante `OwnsOne` en EF Core Fluent API, almacenándose como tipo `uniqueidentifier` en SQL Server o `char(36)` en MySQL.

---

## Paperwork - Domain Layer (Aggregate Root)

### 12. Covenant.cs

Ruta: `Paperwork/Domain/Model/Aggregates/Covenant.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.ValueObjects;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

/// <summary>
/// Aggregate root representing a legal covenant document in the Paperwork bounded context.
/// </summary>
/// <remarks>
/// A Covenant represents a legal agreement between parties, with a unique document identifier,
/// a validity period, a total monetary value, a lifecycle status, and optional footnotes.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public partial class Covenant
{
    /// <summary>Gets the auto-generated primary key of the Covenant.</summary>
    public int Id { get; private set; }

    /// <summary>Gets the unique document identifier assigned externally.</summary>
    public CapbaseIdentifier DocumentIdentifier { get; private set; }

    /// <summary>Gets the identifier of the client party associated with this Covenant.</summary>
    public PartyId ClientId { get; private set; }

    /// <summary>Gets the legality period during which this Covenant is in effect.</summary>
    public LegalityPeriod Period { get; private set; }

    /// <summary>Gets the total monetary value of the Covenant.</summary>
    public MonetaryAmount TotalValue { get; private set; }

    /// <summary>Gets the current status of the Covenant.</summary>
    public CovenantStatus Status { get; private set; }

    /// <summary>Gets optional footnotes or additional remarks for the Covenant.</summary>
    public string? Footnotes { get; private set; }

    /// <summary>
    /// Initializes a new Covenant with all required attributes.
    /// </summary>
    /// <param name="documentIdentifier">Unique document identifier.</param>
    /// <param name="clientId">Client party identifier.</param>
    /// <param name="period">Validity period of the covenant.</param>
    /// <param name="totalValue">Total monetary value.</param>
    /// <param name="status">Lifecycle status; defaults to Draft.</param>
    /// <param name="footnotes">Optional footnotes.</param>
    public Covenant(
        CapbaseIdentifier documentIdentifier,
        PartyId clientId,
        LegalityPeriod period,
        MonetaryAmount totalValue,
        CovenantStatus status = CovenantStatus.Draft,
        string? footnotes = null)
    {
        DocumentIdentifier = documentIdentifier;
        ClientId = clientId;
        Period = period;
        TotalValue = totalValue;
        Status = status;
        Footnotes = footnotes;
    }

    /// <summary>Parameterless constructor required by EF Core.</summary>
    protected Covenant()
    {
        DocumentIdentifier = null!;
        ClientId = null!;
        Period = null!;
        TotalValue = null!;
    }
}
```

**Explicación del Aggregate Root Covenant:**

`Covenant` es el **Aggregate Root** del bounded context Paperwork. Como aggregate root:

1. **Es la entidad raíz del agregado**: Todo acceso al agregado (conjunto de objetos que cambian juntos) debe pasar por Covenant. En este caso, el agregado incluye los value objects embebidos (DocumentIdentifier, ClientId, Period, TotalValue).

2. **Propiedades privadas**: Todas las propiedades tienen setter privado. Solo se pueden establecer mediante el constructor o métodos específicos del dominio. Esto garantiza la integridad del agregado.

3. **Constructor público con parámetros**: Es la forma canónica de crear un Covenant válido. Recibe todos los value objects ya construidos, delegando la validación a los constructores de estos.

4. **Constructor protegido sin parámetros**: Requerido por EF Core para materializar entidades desde la base de datos. Usa `null!` para suprimir warnings de nullable, ya que EF Core asignará los valores desde la BD.

5. **Uso de `partial class`**: La clase es parcial, permitiendo que `CovenantAudit.cs` (ver siguiente archivo) agregue la implementación de `IAuditableEntity` sin modificar este archivo. Esto mantiene la separación de concerns.

### 13. CovenantAudit.cs

Ruta: `Paperwork/Domain/Model/Aggregates/CovenantAudit.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates`

```csharp
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

/// <summary>
/// Partial class providing auditing capabilities to the Covenant aggregate root.
/// </summary>
/// <remarks>
/// This partial class adds IAuditableEntity implementation to Covenant,
/// enabling automatic CreatedAt/UpdatedAt tracking via the AppDbContext.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public partial class Covenant : IAuditableEntity
{
    /// <inheritdoc/>
    public DateTimeOffset? CreatedAt { get; set; }

    /// <inheritdoc/>
    public DateTimeOffset? UpdatedAt { get; set; }
}
```

**Explicación de CovenantAudit (partial class):**

Esta es una técnica de organización de código que se usa también en **Learning Center** (ej. `TutorialAudit.cs`, `UserAudit.cs`). La clase `Covenant` se declara como `partial` en dos archivos:

- `Covenant.cs`: Contiene las propiedades del dominio (Id, DocumentIdentifier, ClientId, etc.)
- `CovenantAudit.cs`: Contiene la implementación de `IAuditableEntity` (CreatedAt, UpdatedAt)

Ventajas:
- Separa las responsabilidades de dominio de las preocupaciones transversales (auditoría)
- Si en el futuro se agrega otro comportamiento transversal (ej. soft delete, versionado), se puede agregar otro archivo partial
- Mantiene cada archivo más pequeño y enfocado

### 14. CovenantStatus.cs

Ruta: `Paperwork/Domain/Model/Aggregates/CovenantStatus.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

/// <summary>
/// Represents the lifecycle status of a Covenant document.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public enum CovenantStatus
{
    /// <summary>The covenant has been drafted but not yet activated.</summary>
    Draft,

    /// <summary>The covenant is currently active.</summary>
    Active,

    /// <summary>The covenant has expired.</summary>
    Expired
}
```

### 15. PaperworkError.cs

Ruta: `Paperwork/Domain/Model/Aggregates/PaperworkError.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

/// <summary>
/// Enumeration of domain-level errors that can occur within the Paperwork bounded context.
/// </summary>
/// <remarks>
/// Each member corresponds to a specific business rule violation or error condition.
/// Used as error codes in Result.Failure() to enable proper HTTP status mapping in the controller.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public enum PaperworkError
{
    /// <summary>No error.</summary>
    None,

    /// <summary>A Covenant with the same document identifier already exists.</summary>
    CovenantAlreadyExists,

    /// <summary>The supplied period dates are invalid.</summary>
    InvalidPeriod,

    /// <summary>The monetary amount or currency is invalid.</summary>
    InvalidMonetaryAmount,

    /// <summary>A database error occurred.</summary>
    DatabaseError,

    /// <summary>The operation was cancelled.</summary>
    OperationCancelled,

    /// <summary>An unexpected internal error occurred.</summary>
    InternalServerError
}
```

**Explicación de PaperworkError:**

Esta enumeración define todos los errores que pueden ocurrir en el bounded context Paperwork. Sirve como **código de error** estándar que se pasa a `Result.Failure()`. Cada valor del enum:

- Es un identificador legible y auto-documentado
- Se usa para buscar el mensaje localizado en los archivos .resx
- Permite al controlador REST mapear cada error a un código HTTP específico
- Centraliza la definición de errores del BC en un solo lugar

El mapeo en el controlador es:
- `CovenantAlreadyExists` → HTTP 409 Conflict
- `InvalidPeriod`, `InvalidMonetaryAmount` → HTTP 400 Bad Request
- `DatabaseError`, `OperationCancelled`, `InternalServerError` → HTTP 500 Internal Server Error

---

## Paperwork - Domain Layer (Commands)

### 16. CreateCovenantCommand.cs

Ruta: `Paperwork/Domain/Model/Commands/CreateCovenantCommand.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Commands`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Commands;

/// <summary>
/// Command to create a new Covenant in the Paperwork bounded context.
/// </summary>
/// <remarks>
/// Encapsulates all data required to create a Covenant aggregate root.
/// The command is an immutable record that travels from the interface layer
/// through the application layer to the domain layer.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <param name="DocumentIdentifier">The unique GUID document identifier.</param>
/// <param name="ClientId">The UUID identifying the client party.</param>
/// <param name="PeriodStartDate">The start date of the covenant's validity period.</param>
/// <param name="PeriodEndDate">The end date of the covenant's validity period.</param>
/// <param name="MonetaryAmountValue">The total monetary value of the covenant.</param>
/// <param name="MonetaryAmountCurrency">The currency code for the monetary amount.</param>
/// <param name="Status">The initial status; defaults to Draft.</param>
/// <param name="Footnotes">Optional footnotes for the covenant.</param>
public record CreateCovenantCommand(
    Guid DocumentIdentifier,
    Guid ClientId,
    DateOnly PeriodStartDate,
    DateOnly PeriodEndDate,
    decimal MonetaryAmountValue,
    string MonetaryAmountCurrency,
    CovenantStatus Status = CovenantStatus.Draft,
    string? Footnotes = null);
```

**Explicación de CreateCovenantCommand (patrón CQRS):**

Este comando sigue el patrón **CQRS (Command Query Responsibility Segregation)** donde:

- **Commands**: Representan intenciones de modificar el estado del sistema. Son imperativos (ej. "crear un covenant"). Siempre retornan un resultado (éxito o error), nunca datos de consulta.
- **Queries**: Representan intenciones de leer datos sin modificar el estado (no implementadas en esta versión, pero la estructura está preparada para agregarlas).

El comando como `record`:
- Es **inmutable**: Una vez creado, no puede modificarse. Viaja a través de las capas sin riesgo de efectos secundarios.
- Tiene **igualdad estructural**: Dos comandos con los mismos valores son iguales.
- Es **auto-documentado**: Los nombres de los parámetros describen exactamente qué datos se necesitan.

**Flujo del comando:**
```
HTTP POST → Controller → Assembler → Command → CommandService → Domain (Covenant) → Repository → Database
```

---

## Paperwork - Domain Layer (Repository)

### 17. ICovenantRepository.cs

Ruta: `Paperwork/Domain/Repositories/ICovenantRepository.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Repositories`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Repositories;

/// <summary>
/// Domain repository interface for the Covenant aggregate root.
/// </summary>
/// <remarks>
/// Extends IBaseRepository to inherit standard CRUD operations (AddAsync, FindByIdAsync, ListAsync).
/// Adds Covenant-specific query methods.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public interface ICovenantRepository : IBaseRepository<Covenant>
{
    /// <summary>
    /// Determines whether a Covenant with the given document identifier already exists.
    /// </summary>
    /// <param name="documentIdentifier">The GUID document identifier to check.</param>
    /// <param name="cancellationToken">Cancellation token.</param>
    /// <returns>true if a Covenant with that identifier exists; otherwise false.</returns>
    Task<bool> ExistsByDocumentIdentifierAsync(Guid documentIdentifier, CancellationToken cancellationToken);
}
```

---

## Paperwork - Application Layer (Service Interfaces)

### 18. ICovenantCommandService.cs

Ruta: `Paperwork/Application/CommandServices/ICovenantCommandService.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Application.CommandServices`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Commands;
using PC212190.U202418655.Capbase.Platform.Shared.Application.Model;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Application.CommandServices;

/// <summary>
/// Application service interface for Covenant command operations.
/// </summary>
/// <remarks>
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public interface ICovenantCommandService
{
    /// <summary>
    /// Handles the creation of a new Covenant.
    /// </summary>
    /// <param name="command">The command carrying the data for the new Covenant.</param>
    /// <param name="cancellationToken">Cancellation token.</param>
    /// <returns>A result containing the created Covenant on success, or an error on failure.</returns>
    Task<Result<Covenant>> Handle(CreateCovenantCommand command, CancellationToken cancellationToken);
}
```

---

## Paperwork - Application Layer (Service Implementations)

### 19. CovenantCommandService.cs

Ruta: `Paperwork/Application/Internal/CommandServices/CovenantCommandService.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Application.Internal.CommandServices`

```csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.Extensions.Localization;
using PC212190.U202418655.Capbase.Platform.Paperwork.Application.CommandServices;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Commands;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.ValueObjects;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Repositories;
using PC212190.U202418655.Capbase.Platform.Paperwork.Resources;
using PC212190.U202418655.Capbase.Platform.Resources;
using PC212190.U202418655.Capbase.Platform.Shared.Application.Model;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Model;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Application.Internal.CommandServices;

/// <summary>
/// Handles Covenant creation commands, enforcing business rules and persisting the aggregate.
/// </summary>
/// <remarks>
/// Business rules enforced:
/// 1. No duplicate DocumentIdentifier - a Covenant with the same identifier cannot be created twice
/// 2. Valid LegalityPeriod - EndDate must be strictly after StartDate
/// 3. Valid MonetaryAmount - value must be >= 0 and currency must be non-empty
///
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class CovenantCommandService(
    ICovenantRepository covenantRepository,
    IUnitOfWork unitOfWork,
    IStringLocalizer<PaperworkMessages> paperworkLocalizer,
    IStringLocalizer<ErrorMessages> errorLocalizer) : ICovenantCommandService
{
    /// <inheritdoc/>
    public async Task<Result<Covenant>> Handle(CreateCovenantCommand command, CancellationToken cancellationToken)
    {
        // BUSINESS RULE 1: No duplicate DocumentIdentifier
        if (await covenantRepository.ExistsByDocumentIdentifierAsync(
                command.DocumentIdentifier, cancellationToken))
            return Result<Covenant>.Failure(
                nameof(PaperworkError.CovenantAlreadyExists),
                paperworkLocalizer[nameof(PaperworkError.CovenantAlreadyExists)]);

        LegalityPeriod period;
        MonetaryAmount totalValue;

        // BUSINESS RULE 2: Valid LegalityPeriod
        try
        {
            period = new LegalityPeriod(command.PeriodStartDate, command.PeriodEndDate);
        }
        catch (ArgumentException ex)
        {
            return Result<Covenant>.Failure(
                nameof(PaperworkError.InvalidPeriod), ex.Message);
        }

        // BUSINESS RULE 3: Valid MonetaryAmount
        try
        {
            totalValue = new MonetaryAmount(
                command.MonetaryAmountValue, command.MonetaryAmountCurrency);
        }
        catch (ArgumentException ex)
        {
            return Result<Covenant>.Failure(
                nameof(PaperworkError.InvalidMonetaryAmount), ex.Message);
        }

        // Create the Covenant aggregate root
        var covenant = new Covenant(
            new CapbaseIdentifier(command.DocumentIdentifier),
            new PartyId(command.ClientId),
            period,
            totalValue,
            command.Status,
            command.Footnotes);

        // Persist
        try
        {
            await covenantRepository.AddAsync(covenant, cancellationToken);
            await unitOfWork.CompleteAsync(cancellationToken);
            return Result<Covenant>.Success(covenant);
        }
        catch (DbUpdateException)
        {
            return Result<Covenant>.Failure(
                nameof(PaperworkError.DatabaseError),
                errorLocalizer[nameof(PaperworkError.InternalServerError)]);
        }
        catch (OperationCanceledException)
        {
            return Result<Covenant>.Failure(
                nameof(PaperworkError.OperationCancelled),
                errorLocalizer[nameof(PaperworkError.InternalServerError)]);
        }
        catch (Exception)
        {
            return Result<Covenant>.Failure(
                nameof(PaperworkError.InternalServerError),
                errorLocalizer[nameof(PaperworkError.InternalServerError)]);
        }
    }
}
```

**Explicación detallada del servicio de aplicación:**

`CovenantCommandService` es el **Application Service** que maneja el comando `CreateCovenantCommand`. Aquí ocurre toda la orquestación:

**Flujo completo:**

1. **Validación de unicidad**: Se verifica que no exista otro Covenant con el mismo `DocumentIdentifier`. Esta es una regla de negocio de integridad (no pueden existir dos documentos con el mismo identificador).

2. **Creación de Value Objects**: Se construyen `LegalityPeriod` y `MonetaryAmount`. Cada uno se valida a sí mismo:
   - Si `EndDate <= StartDate`, `LegalityPeriod` lanza `ArgumentException`
   - Si `Value < 0` o `Currency` vacío, `MonetaryAmount` lanza `ArgumentException`

3. **Creación del Aggregate Root**: Se crea `Covenant` con todos los value objects y el status.

4. **Persistencia**: Se usa el patrón **Unit of Work** para guardar atómicamente:
   - `AddAsync` agrega el covenant al contexto de EF Core
   - `CompleteAsync` confirma la transacción (SaveChanges)

5. **Manejo de errores**: Cada posible fallo se traduce en un `Result.Failure` con el código de error apropiado, permitiendo que la capa de interfaz determine la respuesta HTTP correcta.

**Inyección de dependencias (DI):**

El servicio recibe sus dependencias mediante **primary constructor** (característica de C# 12/14):
- `ICovenantRepository`: Para operaciones de persistencia específicas de Covenant
- `IUnitOfWork`: Para confirmar la transacción
- `IStringLocalizer<PaperworkMessages>`: Para mensajes localizados del BC Paperwork
- `IStringLocalizer<ErrorMessages>`: Para mensajes de error genéricos

Todas estas dependencias se registran en `Program.cs` con `AddScoped`, lo que significa que se crea una nueva instancia por cada request HTTP.

---

## Paperwork - Infrastructure Layer (Fluent API Configuration)

### 20. ModelBuilderExtensions.cs

Ruta: `Paperwork/Infrastructure/Persistence/EntityFrameworkCore/Configuration/ModelBuilderExtensions.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Configuration`

```csharp
using Microsoft.EntityFrameworkCore;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Configuration;

/// <summary>
/// Extension methods to apply Paperwork bounded context EF Core configuration.
/// </summary>
/// <remarks>
/// Uses Fluent API to define table mapping, column naming (snake_case),
/// primary keys, value object ownership, and constraints.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public static class ModelBuilderExtensions
{
    /// <summary>
    /// Applies Fluent API configuration for all Paperwork entities.
    /// </summary>
    /// <param name="builder">The EF Core model builder.</param>
    public static void ApplyPaperworkConfiguration(this ModelBuilder builder)
    {
        builder.Entity<Covenant>(entity =>
        {
            entity.ToTable("covenants");

            entity.HasKey(c => c.Id);
            entity.Property(c => c.Id)
                .HasColumnName("id")
                .IsRequired()
                .ValueGeneratedOnAdd();

            entity.Property(c => c.Status)
                .HasColumnName("status")
                .IsRequired()
                .HasConversion<string>();

            entity.Property(c => c.Footnotes)
                .HasColumnName("footnotes")
                .IsRequired(false);

            entity.Property(c => c.CreatedAt)
                .HasColumnName("created_at");

            entity.Property(c => c.UpdatedAt)
                .HasColumnName("updated_at");

            // CapbaseIdentifier value object — stored as GUID column
            entity.OwnsOne(c => c.DocumentIdentifier, di =>
            {
                di.Property(x => x.Value)
                    .HasColumnName("document_identifier")
                    .IsRequired();
                di.HasIndex(x => x.Value)
                    .IsUnique();
            });

            // PartyId value object — stored as GUID column
            entity.OwnsOne(c => c.ClientId, ci =>
            {
                ci.Property(x => x.Value)
                    .HasColumnName("client_id")
                    .IsRequired();
            });

            // LegalityPeriod value object — stored as two date columns
            entity.OwnsOne(c => c.Period, p =>
            {
                p.Property(x => x.StartDate)
                    .HasColumnName("period_start_date")
                    .IsRequired();
                p.Property(x => x.EndDate)
                    .HasColumnName("period_end_date")
                    .IsRequired();
            });

            // MonetaryAmount value object — stored as two columns
            entity.OwnsOne(c => c.TotalValue, tv =>
            {
                tv.Property(x => x.Value)
                    .HasColumnName("monetary_amount_value")
                    .HasColumnType("decimal(18,4)")
                    .IsRequired();
                tv.Property(x => x.Currency)
                    .HasColumnName("monetary_amount_currency")
                    .IsRequired();
            });
        });
    }
}
```

**Explicación de la configuración Fluent API:**

Este archivo es el equivalente al `SnakeCaseWithPluralizedTablePhysicalNamingStrategy.java` del proyecto Facturease, pero adaptado a EF Core. Define el mapeo entre las clases C# y la base de datos:

| Clase C# | Tabla/Columna BD | Tipo |
|---|---|---|
| `Covenant` | `covenants` | Tabla (plural) |
| `Covenant.Id` | `id` | INT PK auto-increment |
| `Covenant.Status` | `status` | VARCHAR (enum as string) |
| `Covenant.Footnotes` | `footnotes` | TEXT nullable |
| `Covenant.CreatedAt` | `created_at` | DATETIME |
| `Covenant.UpdatedAt` | `updated_at` | DATETIME |
| `CapbaseIdentifier.Value` | `document_identifier` | CHAR(36) GUID (UNIQUE) |
| `PartyId.Value` | `client_id` | CHAR(36) GUID |
| `LegalityPeriod.StartDate` | `period_start_date` | DATE |
| `LegalityPeriod.EndDate` | `period_end_date` | DATE |
| `MonetaryAmount.Value` | `monetary_amount_value` | DECIMAL(18,4) |
| `MonetaryAmount.Currency` | `monetary_amount_currency` | VARCHAR(3) |

**Convenciones de nomenclatura:**

Siguiendo el estándar del proyecto Learning Center, los nombres de tablas y columnas están en **snake_case**:
- Tablas en **plural** (`covenants`, no `covenant`)
- Columnas en **snake_case** (`period_start_date`, no `PeriodStartDate`)
- Los value objects se "aplanan" en columnas de la misma tabla (no hay tablas separadas para ellos)

> **Nota sobre pluralización:** En Learning Center, se usa `Humanizer` para pluralizar automáticamente nombres de tablas. En este proyecto, la pluralización se hace manualmente en `ToTable("covenants")` para tener control explícito sobre los nombres.

---

## Paperwork - Infrastructure Layer (Repository Implementation)

### 21. CovenantRepository.cs

Ruta: `Paperwork/Infrastructure/Persistence/EntityFrameworkCore/Repositories/CovenantRepository.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Repositories`

```csharp
using Microsoft.EntityFrameworkCore;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Repositories;
using PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration;
using PC212190.U202418655.Capbase.Platform.Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Repositories;

/// <summary>
/// EF Core repository for the Covenant aggregate root.
/// </summary>
/// <remarks>
/// Extends BaseRepository to inherit standard CRUD operations.
/// Implements ICovenantRepository to provide Covenant-specific queries.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public class CovenantRepository(AppDbContext context)
    : BaseRepository<Covenant>(context), ICovenantRepository
{
    /// <inheritdoc/>
    public async Task<bool> ExistsByDocumentIdentifierAsync(
        Guid documentIdentifier, CancellationToken cancellationToken)
        => await Context.Set<Covenant>()
            .AnyAsync(c => c.DocumentIdentifier.Value == documentIdentifier, cancellationToken);
}
```

---

## Paperwork - Interfaces Layer (Resources)

### 22. CreateCovenantResource.cs (Request DTO)

Ruta: `Paperwork/Interfaces/Rest/Resources/CreateCovenantResource.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources;

/// <summary>
/// Request resource for creating a new Covenant via the REST API.
/// </summary>
/// <remarks>
/// This is the DTO (Data Transfer Object) that maps the incoming JSON request body
/// to a typed structure. Value object attributes (DocumentIdentifier, Period, TotalValue)
/// are flattened into primitive-typed attributes.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <param name="DocumentIdentifier">Unique GUID document identifier generated externally.</param>
/// <param name="ClientId">UUID of the client party, originating from another bounded context.</param>
/// <param name="PeriodStartDate">Start date of the covenant's validity period (yyyy-MM-dd).</param>
/// <param name="PeriodEndDate">End date of the covenant's validity period (yyyy-MM-dd).</param>
/// <param name="MonetaryAmountValue">Total monetary value of the covenant.</param>
/// <param name="MonetaryAmountCurrency">ISO currency code for the monetary amount.</param>
/// <param name="Status">Initial status of the covenant. Defaults to Draft.</param>
/// <param name="Footnotes">Optional additional notes or remarks.</param>
public record CreateCovenantResource(
    Guid DocumentIdentifier,
    Guid ClientId,
    DateOnly PeriodStartDate,
    DateOnly PeriodEndDate,
    decimal MonetaryAmountValue,
    string MonetaryAmountCurrency,
    CovenantStatus Status = CovenantStatus.Draft,
    string? Footnotes = null);
```

**Explicación de CreateCovenantResource:**

Este es el **DTO (Data Transfer Object)** de entrada. Es un `record` inmutable que modela el cuerpo JSON de la petición `POST /api/v1/covenants`. Los value objects del dominio (`CapbaseIdentifier`, `PartyId`, `LegalityPeriod`, `MonetaryAmount`) se "aplanan" en campos primitivos porque el API REST no debe exponer la complejidad interna del dominio.

**Mapeo JSON** (ejemplo de request body):
```json
{
  "documentIdentifier": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
  "periodStartDate": "2025-01-01",
  "periodEndDate": "2026-01-01",
  "monetaryAmountValue": 15000.00,
  "monetaryAmountCurrency": "USD",
  "status": "Draft",
  "footnotes": "Optional notes here."
}
```

ASP.NET Core usa **camelCase** por defecto en la serialización JSON, por lo que las propiedades del record C# (PascalCase) se convierten automáticamente a camelCase en el JSON.

### 23. CovenantResource.cs (Response DTO)

Ruta: `Paperwork/Interfaces/Rest/Resources/CovenantResource.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources`

```csharp
namespace PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources;

/// <summary>
/// Response resource representing a Covenant returned by the REST API.
/// </summary>
/// <remarks>
/// This DTO flattens value objects into primitive fields for the API response.
/// Note that status and footnotes are intentionally excluded from the response
/// as per the API specification (they are internal domain concepts).
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <param name="Id">Auto-generated primary key of the Covenant.</param>
/// <param name="DocumentId">String representation of the document identifier GUID.</param>
/// <param name="ClientId">String representation of the client party UUID.</param>
/// <param name="PeriodStartDate">Start date of the validity period as an ISO string.</param>
/// <param name="PeriodEndDate">End date of the validity period as an ISO string.</param>
/// <param name="MonetaryAmountValue">Total monetary value.</param>
/// <param name="MonetaryAmountCurrency">ISO currency code.</param>
public record CovenantResource(
    int Id,
    string DocumentId,
    string ClientId,
    string PeriodStartDate,
    string PeriodEndDate,
    decimal MonetaryAmountValue,
    string MonetaryAmountCurrency);
```

**Explicación de CovenantResource:**

Este es el DTO de respuesta. Al igual que el de entrada, aplana los value objects. Notar que:
- `DocumentId` y `ClientId` se devuelven como string (representación del GUID), no como GUID directamente
- `PeriodStartDate` y `PeriodEndDate` se devuelven como string ISO (`yyyy-MM-dd`)
- `status` y `footnotes` NO se incluyen en la respuesta (según la especificación del API)

**Ejemplo de respuesta JSON:**
```json
{
  "id": 1,
  "documentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
  "periodStartDate": "2025-01-01",
  "periodEndDate": "2026-01-01",
  "monetaryAmountValue": 15000.00,
  "monetaryAmountCurrency": "USD"
}
```

---

## Paperwork - Interfaces Layer (Assemblers)

### 24. CreateCovenantCommandFromResourceAssembler.cs

Ruta: `Paperwork/Interfaces/Rest/Transform/CreateCovenantCommandFromResourceAssembler.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Transform`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Commands;
using PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Transform;

/// <summary>
/// Assembler that transforms a CreateCovenantResource into a CreateCovenantCommand.
/// </summary>
/// <remarks>
/// Follows the assembler pattern used in Learning Center to separate
/// resource-to-command mapping logic from the controller.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public static class CreateCovenantCommandFromResourceAssembler
{
    /// <summary>
    /// Converts a REST request resource to a domain command.
    /// </summary>
    /// <param name="resource">The inbound REST resource.</param>
    /// <returns>A CreateCovenantCommand populated from the resource.</returns>
    public static CreateCovenantCommand ToCommandFromResource(CreateCovenantResource resource)
        => new(
            resource.DocumentIdentifier,
            resource.ClientId,
            resource.PeriodStartDate,
            resource.PeriodEndDate,
            resource.MonetaryAmountValue,
            resource.MonetaryAmountCurrency,
            resource.Status,
            resource.Footnotes);
}
```

### 25. CovenantResourceFromEntityAssembler.cs

Ruta: `Paperwork/Interfaces/Rest/Transform/CovenantResourceFromEntityAssembler.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Transform`

```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Transform;

/// <summary>
/// Assembler that maps a Covenant domain aggregate to a CovenantResource.
/// </summary>
/// <remarks>
/// Converts domain objects to REST response DTOs, flattening value objects.
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
public static class CovenantResourceFromEntityAssembler
{
    /// <summary>
    /// Converts a Covenant entity to a REST response resource.
    /// </summary>
    /// <param name="covenant">The domain entity to convert.</param>
    /// <returns>A CovenantResource suitable for the API response.</returns>
    public static CovenantResource ToResourceFromEntity(Covenant covenant)
        => new(
            covenant.Id,
            covenant.DocumentIdentifier.Value.ToString(),
            covenant.ClientId.Value.ToString(),
            covenant.Period.StartDate.ToString("yyyy-MM-dd"),
            covenant.Period.EndDate.ToString("yyyy-MM-dd"),
            covenant.TotalValue.Value,
            covenant.TotalValue.Currency);
}
```

**Explicación del patrón Assembler:**

Los Assemblers son una parte fundamental de la arquitectura, también usados en **Learning Center**. Su propósito es **aislar la capa de Interfaces de los detalles del dominio**:

- **Resource → Command**: Convierte el DTO de entrada en un comando de dominio. Esto permite que el controlador REST no conozca los detalles del dominio.
- **Entity → Resource**: Convierte el aggregate root del dominio en un DTO de respuesta. Oculta propiedades internas del dominio que no deben exponerse.

En Learning Center, estos assemblers están en `Publishing/Interfaces/Rest/Transform/` y se llaman `CreateTutorialCommandFromResourceAssembler`, `TutorialResourceFromEntityAssembler`, etc.

---

## Paperwork - Interfaces Layer (Controller con OpenAPI / Swagger)

### 26. CovenantsController.cs

Ruta: `Paperwork/Interfaces/Rest/CovenantsController.cs`

Package: `PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest`

```csharp
using System.Net.Mime;
using Microsoft.AspNetCore.Mvc;
using PC212190.U202418655.Capbase.Platform.Paperwork.Application.CommandServices;
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
using PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Resources;
using PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest.Transform;
using Swashbuckle.AspNetCore.Annotations;

namespace PC212190.U202418655.Capbase.Platform.Paperwork.Interfaces.Rest;

/// <summary>
/// REST controller exposing the Covenant endpoints for the Paperwork bounded context.
/// </summary>
/// <remarks>
/// Base URL: /api/v1/covenants
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
[ApiController]
[Route("api/v1/covenants")]
[Produces(MediaTypeNames.Application.Json)]
[SwaggerTag("Available Covenant endpoints")]
public class CovenantsController(ICovenantCommandService covenantCommandService) : ControllerBase
{
    /// <summary>
    /// Creates a new Covenant document in the Paperwork bounded context.
    /// </summary>
    /// <param name="resource">The request body containing all required Covenant data.</param>
    /// <param name="cancellationToken">Cancellation token.</param>
    /// <returns>The created Covenant resource with HTTP 201, or an error response.</returns>
    [HttpPost]
    [SwaggerOperation(
        Summary = "Create a Covenant",
        Description = "Creates a new Covenant document in the Paperwork bounded context. " +
                       "The Covenant represents a legal agreement with a unique document identifier, " +
                       "validity period, monetary value, and optional footnotes.",
        OperationId = "CreateCovenant")]
    [SwaggerResponse(StatusCodes.Status201Created, "The Covenant was created successfully.", typeof(CovenantResource))]
    [SwaggerResponse(StatusCodes.Status400BadRequest, "Invalid request data or business rule violation.")]
    [SwaggerResponse(StatusCodes.Status409Conflict, "A Covenant with the same document identifier already exists.")]
    [SwaggerResponse(StatusCodes.Status500InternalServerError, "An unexpected server error occurred.")]
    public async Task<IActionResult> CreateCovenant(
        [FromBody] CreateCovenantResource resource,
        CancellationToken cancellationToken)
    {
        var command = CreateCovenantCommandFromResourceAssembler
            .ToCommandFromResource(resource);
        var result = await covenantCommandService.Handle(command, cancellationToken);

        if (!result.IsSuccess)
        {
            var body = new { message = result.ErrorMessage };

            return result.ErrorCode switch
            {
                nameof(PaperworkError.CovenantAlreadyExists) => Conflict(body),
                nameof(PaperworkError.DatabaseError)
                    or nameof(PaperworkError.OperationCancelled)
                    or nameof(PaperworkError.InternalServerError) =>
                    StatusCode(StatusCodes.Status500InternalServerError, body),
                _ => BadRequest(body)
            };
        }

        var covenantResource = CovenantResourceFromEntityAssembler
            .ToResourceFromEntity(result.Value!);
        return CreatedAtAction(
            nameof(CreateCovenant), new { id = covenantResource.Id }, covenantResource);
    }
}
```

**Explicación del Controlador REST:**

El controlador sigue el mismo patrón que los controladores de **Learning Center** (`ProfilesController.cs`, `TutorialsController.cs`):

**Flujo completo del request:**

```
Cliente HTTP                      Servidor
    │                                │
    │  POST /api/v1/covenants        │
    │  {                             │
    │    "documentIdentifier": "...", │
    │    "clientId": "...",          │
    │    "periodStartDate": "...",   │
    │    "periodEndDate": "...",     │
    │    "monetaryAmountValue": 15000,│
    │    "monetaryAmountCurrency": "USD",│
    │    "status": "Draft",          │
    │    "footnotes": "..."          │
    │  }                             │
    │────────────────────────────>   │
    │                                │
    │  1. Controller recibe JSON     │
    │  2. Serializa a                │
    │     CreateCovenantResource     │
    │  3. Assembler convierte a      │
    │     CreateCovenantCommand      │
    │  4. CommandService.Handle()    │
    │     a. Valida unicidad         │
    │     b. Crea value objects      │
    │     c. Crea Covenant           │
    │     d. Persiste                │
    │  5. Assembler convierte        │
    │     Covenant → CovenantResource│
    │  6. Retorna 201 Created        │
    │                                │
    │  HTTP 201                      │
    │  {                             │
    │    "id": 1,                    │
    │    "documentId": "...",        │
    │    ...                         │
    │  }                             │
    │<────────────────────────────   │
```

**Mapeo de errores a códigos HTTP:**

| Condición | Código HTTP | Error |
|---|---|---|
| Creación exitosa | 201 Created | - |
| DocumentIdentifier duplicado | 409 Conflict | `CovenantAlreadyExists` |
| Periodo inválido (EndDate <= StartDate) | 400 Bad Request | `InvalidPeriod` |
| Valor monetario inválido (< 0) | 400 Bad Request | `InvalidMonetaryAmount` |
| Error de base de datos | 500 Internal Server Error | `DatabaseError` |
| Operación cancelada | 500 Internal Server Error | `OperationCancelled` |
| Error interno inesperado | 500 Internal Server Error | `InternalServerError` |

**Anotaciones Swagger / OpenAPI:**

El controlador usa `[SwaggerOperation]`, `[SwaggerResponse]`, y `[SwaggerTag]` del paquete `Swashbuckle.AspNetCore.Annotations`. Estas anotaciones generan documentación OpenAPI enriquecida visible en Swagger UI:

```
http://localhost:8097/swagger/index.html
```

---

## README.md

Crear `README.md` en la raíz del proyecto `pc212190u202418655/`:

```markdown
# Capbase Platform API

## Description
RESTful API for the **Capbase** case study, implementing the **Paperwork** bounded context.  
Exposes a single endpoint to create Covenant documents, persisted in a MySQL or PostgreSQL relational database.

Built with C# 14 / .NET 10 / ASP.NET Core, following Domain-Driven Design (DDD), CQRS, and layered architecture.

## Technologies
- .NET 10.0 (C# 14)
- ASP.NET Core 10.0
- Entity Framework Core 10.0.8
- MySQL (MySql.EntityFrameworkCore 10.0.7) / PostgreSQL (Npgsql)
- Swashbuckle (Swagger UI) 10.2.0
- Humanizer 3.0.10

## Author
- **Name:** Victor Jhosef Laura Acosta
- **Student code:** u202418655
- **Section / NRC:** 12190
- **Course:** Aplicaciones Web
- **Professor:** Hugo Allan Mori Paiva

## Prerequisites
- [.NET 10 SDK](https://dotnet.microsoft.com/download/dotnet/10.0)
- MySQL Server (5.7+ or 8.x) or PostgreSQL (14+)

## Database Configuration
1. Start your database server.
2. Create the schema (database):

**MySQL:**
```sql
CREATE SCHEMA IF NOT EXISTS clerky_wa
    CHARACTER SET utf8mb4
    COLLATE utf8mb4_unicode_ci;
```

**PostgreSQL:**
```sql
CREATE DATABASE clerky_wa;
```

3. Update the connection string in `appsettings.json` if needed.

## Running the Project
```bash
cd pc212190u202418655/PC212190.U202418655.Capbase.Platform
dotnet run
```

The application runs `EnsureCreated()` on startup, so the `covenants` table is created automatically.

## API Endpoint
### POST /api/v1/covenants
Creates a new Covenant document.

**Request body:**
```json
{
  "documentIdentifier": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
  "periodStartDate": "2025-01-01",
  "periodEndDate": "2026-01-01",
  "monetaryAmountValue": 15000.00,
  "monetaryAmountCurrency": "USD",
  "status": "Draft",
  "footnotes": "Optional notes here."
}
```

**Successful response — HTTP 201:**
```json
{
  "id": 1,
  "documentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
  "periodStartDate": "2025-01-01",
  "periodEndDate": "2026-01-01",
  "monetaryAmountValue": 15000.00,
  "monetaryAmountCurrency": "USD"
}
```

## OpenAPI / Swagger UI
Available at: `http://localhost:8097/swagger`

## Localization
The API respects the `Accept-Language` header for error messages.  
Supported cultures: `en`, `en-US`, `es`, `es-PE`.
```

---

---

## Troubleshooting - Solución de Problemas Comunes

### Error: "Connection string 'DefaultConnection' not found."

**Causa:** El archivo `appsettings.json` no tiene la sección `ConnectionStrings` o el nombre no coincide.

**Solución:** Verificar que `appsettings.json` contenga:

```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=localhost;Port=3306;Database=clerky_wa;User=root;Password=12345678;"
  }
}
```

Y que `Program.cs` use:

```csharp
var connectionString = builder.Configuration.GetConnectionString("DefaultConnection");
```

### Error: "Unable to connect to any of the specified MySQL hosts."

**Causas posibles:**
1. MySQL no está corriendo
2. Puerto incorrecto (no es 3306)
3. Firewall bloqueando la conexión
4. MySQL configurado solo para localhost

**Soluciones:**
1. Iniciar MySQL: `net start mysql` (Windows) o `sudo systemctl start mysql` (Linux)
2. Verificar puerto: `SHOW VARIABLES LIKE 'port';` en MySQL CLI
3. Verificar que el usuario `root` tenga acceso desde localhost:
   ```sql
   SELECT host, user FROM mysql.user WHERE user = 'root';
   ```

### Error: "The type or namespace name 'Covenant' could not be found"

**Causa:** Falta el `using` correspondiente en algún archivo.

**Solución:** Agregar al inicio del archivo:
```csharp
using PC212190.U202418655.Capbase.Platform.Paperwork.Domain.Model.Aggregates;
```

### Error: "Invalid object name 'covenants'."

**Causa:** La tabla no se ha creado en la base de datos.

**Solución:**
1. Verificar que la base de datos `clerky_wa` existe
2. Verificar que `EnsureCreated()` se ejecuta en `Program.cs`
3. Si la tabla ya existe pero con otro nombre, verificar `ToTable("covenants")` en `ModelBuilderExtensions.cs`
4. Si es necesario, forzar la recreación:
   ```sql
   DROP TABLE IF EXISTS clerky_wa.covenants;
   ```
   Luego reiniciar la aplicación.

### Error de compilación: "'LangVersion' must be '14' or higher"

**Causa:** El SDK de .NET instalado es anterior a .NET 10.

**Solución:**
```bash
# Verificar versión instalada
dotnet --version

# Si es menor a 10.0.x, descargar .NET 10 SDK desde:
# https://dotnet.microsoft.com/download/dotnet/10.0
```

### Error: "No database provider has been configured for this DbContext"

**Causa:** No se llamó a `UseMySQL()` o `UseNpgsql()` en la configuración del DbContext.

**Solución:** Verificar `Program.cs`:
```csharp
builder.Services.AddDbContext<AppDbContext>(options =>
    options.UseMySQL(connectionString));
```

### Error: "Cannot write DateTime with Kind=Local to MySQL"

**Causa:** MySQL espera DateTime con Kind=Utc pero se envía Local.

**Solución:** Agregar en `Program.cs` antes de `builder.Build()`:
```csharp
AppContext.SetSwitch("MySql.Data.EnableUtcConversion", true);
```

O en la cadena de conexión:
```
"DefaultConnection": "Server=localhost;Port=3306;Database=clerky_wa;User=root;Password=12345678;convert zero datetime=True;Allow Zero Datetime=True;"
```

### Error: "The JSON value could not be converted to System.Guid"

**Causa:** El GUID enviado en el JSON no tiene un formato válido.

**Solución:** Asegurar que el `documentIdentifier` y `clientId` sean GUIDs válidos:
```
"documentIdentifier": "3fa85f64-5717-4562-b3fc-2c963f66afa6"
```
Formato válido: 8-4-4-4-12 hexadecimal digits (con guiones).

### La tabla `covenants` no se crea con las columnas correctas

**Causa:** Posibles conflictos con migraciones anteriores o esquema existente.

**Solución:** Si la tabla ya existe pero con estructura incorrecta, eliminarla y dejar que `EnsureCreated()` la regenere:

```sql
DROP TABLE IF EXISTS clerky_wa.covenants;
```

Luego reiniciar la aplicación.

### Swagger UI no carga o muestra error 404

**Causas posibles:**
1. El middleware de Swagger no está configurado
2. La ruta del prefix es incorrecta
3. Se está ejecutando en producción (no development)

**Soluciones:**
1. Verificar que `Program.cs` tenga:
   ```csharp
   if (app.Environment.IsDevelopment())
   {
       app.UseSwagger();
       app.UseSwaggerUI(...);
   }
   ```
2. La URL correcta es: `http://localhost:8097/swagger/index.html`
3. Si quieres Swagger en producción, quita el `if (app.Environment.IsDevelopment())`

### El endpoint POST devuelve 400 Bad Request sin mensaje claro

**Causa:** El modelo no se está serializando correctamente o falta el `[FromBody]`.

**Solución:**
1. Verificar que el parámetro del controlador tenga `[FromBody]`:
   ```csharp
   public async Task<IActionResult> CreateCovenant(
       [FromBody] CreateCovenantResource resource, ...)
   ```
2. Verificar que el Content-Type del request sea `application/json`
3. Verificar que el JSON del body coincida con las propiedades de `CreateCovenantResource`

---

## Guía Rápida de Comandos .NET CLI

### Creación del proyecto

```bash
# Crear solución
dotnet new sln -n pc212190u202418655

# Crear proyecto Web API
dotnet new webapi -n PC212190.U202418655.Capbase.Platform --use-controllers

# Agregar a solución
dotnet sln add PC212190.U202418655.Capbase.Platform/PC212190.U202418655.Capbase.Platform.csproj
```

### Agregar paquetes NuGet

```bash
# Navegar al directorio del proyecto
cd PC212190.U202418655.Capbase.Platform

# EF Core packages
dotnet add package Microsoft.EntityFrameworkCore --version 10.0.8
dotnet add package Microsoft.EntityFrameworkCore.Relational --version 10.0.8
dotnet add package Microsoft.EntityFrameworkCore.Design --version 10.0.8

# MySQL provider
dotnet add package MySql.EntityFrameworkCore --version 10.0.7

# PostgreSQL provider (alternativa)
dotnet add package Npgsql.EntityFrameworkCore.PostgreSQL --version 10.0.0

# Humanizer (naming conventions)
dotnet add package Humanizer --version 3.0.10

# Swagger
dotnet add package Swashbuckle.AspNetCore --version 10.2.0
dotnet add package Swashbuckle.AspNetCore.Annotations --version 10.2.0
```

### Compilar y ejecutar

```bash
# Compilar
dotnet build

# Ejecutar
dotnet run

# Ejecutar en modo release
dotnet run --configuration Release

# Publicar
dotnet publish -c Release -o ./publish
```

### Verificar configuración

```bash
# Ver SDKs instalados
dotnet --list-sdks

# Ver versión actual
dotnet --version

# Ver runtime instalado
dotnet --list-runtimes
```

---

## Pruebas del API con curl

### Crear Covenant exitoso

```bash
curl -X POST http://localhost:8097/api/v1/covenants \
  -H "Content-Type: application/json" \
  -H "Accept-Language: en" \
  -d '{
    "documentIdentifier": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
    "periodStartDate": "2025-01-01",
    "periodEndDate": "2026-01-01",
    "monetaryAmountValue": 15000.00,
    "monetaryAmountCurrency": "USD",
    "status": "Draft",
    "footnotes": "Test covenant"
  }'
```

**Respuesta esperada: HTTP 201 Created**
```json
{
  "id": 1,
  "documentId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
  "periodStartDate": "2025-01-01",
  "periodEndDate": "2026-01-01",
  "monetaryAmountValue": 15000.00,
  "monetaryAmountCurrency": "USD"
}
```

### Crear Covenant duplicado (mismo DocumentIdentifier)

```bash
curl -X POST http://localhost:8097/api/v1/covenants \
  -H "Content-Type: application/json" \
  -d '{
    "documentIdentifier": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
    "periodStartDate": "2025-01-01",
    "periodEndDate": "2026-01-01",
    "monetaryAmountValue": 15000.00,
    "monetaryAmountCurrency": "USD"
  }'
```

**Respuesta esperada: HTTP 409 Conflict**
```json
{
  "message": "A Covenant with the given document identifier already exists."
}
```

### Crear Covenant con período inválido (EndDate <= StartDate)

```bash
curl -X POST http://localhost:8097/api/v1/covenants \
  -H "Content-Type: application/json" \
  -d '{
    "documentIdentifier": "550e8400-e29b-41d4-a716-446655440001",
    "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
    "periodStartDate": "2026-01-01",
    "periodEndDate": "2025-01-01",
    "monetaryAmountValue": 15000.00,
    "monetaryAmountCurrency": "USD"
  }'
```

**Respuesta esperada: HTTP 400 Bad Request**
```json
{
  "message": "EndDate must be strictly after StartDate."
}
```

### Crear Covenant con monto negativo

```bash
curl -X POST http://localhost:8097/api/v1/covenants \
  -H "Content-Type: application/json" \
  -d '{
    "documentIdentifier": "550e8400-e29b-41d4-a716-446655440002",
    "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
    "periodStartDate": "2025-01-01",
    "periodEndDate": "2026-01-01",
    "monetaryAmountValue": -100.00,
    "monetaryAmountCurrency": "USD"
  }'
```

**Respuesta esperada: HTTP 400 Bad Request**

### Probar localización en español

```bash
curl -X POST http://localhost:8097/api/v1/covenants \
  -H "Content-Type: application/json" \
  -H "Accept-Language: es" \
  -d '{
    "documentIdentifier": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "clientId": "4fb96a75-6828-5673-c4gd-3d074g77bgb7",
    "periodStartDate": "2025-01-01",
    "periodEndDate": "2026-01-01",
    "monetaryAmountValue": 15000.00,
    "monetaryAmountCurrency": "USD"
  }'
```

**Respuesta esperada: HTTP 409 Conflict con mensaje en español**
```json
{
  "message": "Ya existe un Convenio con el identificador de documento indicado."
}
```

---

## Explicación Detallada de los Patrones Arquitectónicos

### Domain-Driven Design (DDD)

El proyecto sigue los principios de DDD, donde:

- **Aggregate Root**: `Covenant` es la raíz del agregado. Garantiza la consistencia de todas las entidades y value objects dentro de su límite.
- **Value Objects**: `CapbaseIdentifier`, `PartyId`, `LegalityPeriod`, `MonetaryAmount` son objetos inmutables que se definen por sus atributos, no por una identidad.
- **Repositories**: `ICovenantRepository` abstrae el almacenamiento del aggregate root.
- **Bounded Context**: `Paperwork` es un contexto delimitado que contiene toda la lógica relacionada con trámites documentarios.

### CQRS (Command Query Responsibility Segregation)

Aunque en esta versión solo hay un comando (CreateCovenant), la arquitectura está preparada para separar:

- **Commands**: Modifican el estado (Create, Update, Delete). Retornan `Result<T>`.
- **Queries**: Leen el estado sin modificarlo. Retornan `Optional<T>` o `IEnumerable<T>`.

En el futuro, se podrían agregar queries como:
```csharp
public record GetAllCovenantsQuery();
public record GetCovenantByIdQuery(int Id);
```

### Layered Architecture (Arquitectura en Capas)

Cada capa tiene una responsabilidad bien definida:

**1. Domain Layer (Capa de Dominio)**
- Contiene las entidades, value objects, aggregates, commands, queries, interfaces de repositorio, y errores de dominio.
- **NO depende de ninguna otra capa.**
- **NO tiene referencias a EF Core, MySQL, ni a ningún framework externo.**

Archivos: `Covenant.cs`, `CovenantAudit.cs`, `CovenantStatus.cs`, `PaperworkError.cs`, `CapbaseIdentifier.cs`, `PartyId.cs`, `CreateCovenantCommand.cs`, `ICovenantRepository.cs`, `IAuditableEntity.cs`, `LegalityPeriod.cs`, `MonetaryAmount.cs`, `IBaseRepository.cs`, `IUnitOfWork.cs`.

**2. Application Layer (Capa de Aplicación)**
- Orquesta los casos de uso del sistema.
- Coordina repositorios, servicios de dominio, y otras dependencias.
- Define los puertos (interfaces) que los servicios de aplicación exponen.
- Implementa la lógica de aplicación (no de negocio).

Archivos: `ICovenantCommandService.cs`, `CovenantCommandService.cs`.

**3. Infrastructure Layer (Capa de Infraestructura)**
- Implementa los contratos definidos en Domain.
- Contiene la configuración de EF Core, repositorios concretos, DbContext.
- Depende de frameworks externos (EF Core, MySQL provider).

Archivos: `AppDbContext.cs`, `BaseRepository.cs`, `UnitOfWork.cs`, `CovenantRepository.cs`, `ModelBuilderExtensions.cs`.

**4. Interfaces Layer (Capa de Interfaces)**
- Contiene los controladores REST, DTOs (Resources), y assemblers.
- Es el punto de entrada de las peticiones HTTP.
- Traduce recursos REST a comandos de dominio y viceversa.

Archivos: `CovenantsController.cs`, `CreateCovenantResource.cs`, `CovenantResource.cs`, `CreateCovenantCommandFromResourceAssembler.cs`, `CovenantResourceFromEntityAssembler.cs`.

### Patrón Result

El patrón Result (también conocido como "Railway Oriented Programming") evita usar excepciones para control de flujo. En lugar de lanzar y capturar excepciones para errores esperados (como "registro duplicado"), el servicio devuelve un objeto `Result<T>` que:

- En éxito: contiene `IsSuccess = true` y `Value` con el resultado
- En error: contiene `IsSuccess = false`, `ErrorCode` (máquina) y `ErrorMessage` (humano/localizado)

**Ventajas:**
- El tipo de retorno documenta explícitamente que la operación puede fallar
- El compilador obliga al llamador a manejar ambos casos (éxito y error)
- Los errores son tipados y predecibles
- Facilita el testing (puedes mockear Result sin lidiar con excepciones)

### Patrón Repository + Unit of Work

**Repository**: Abstrae la lógica de persistencia. El dominio define la interfaz (ICovenantRepository) y la infraestructura provee la implementación (CovenantRepository). Esto permite:

- Cambiar de MySQL a PostgreSQL sin modificar el dominio
- Hacer unit testing mockeando el repositorio
- Centralizar consultas complejas

**Unit of Work**: Agrupa cambios en una transacción atómica. Cuando hay múltiples operaciones (ej. guardar Covenant + actualizar otro agregado), el Unit of Work garantiza que todas se guarden o ninguna. En este proyecto, `IUnitOfWork.CompleteAsync()` llama a `SaveChangesAsync()` del DbContext, que internamente maneja la transacción.

### Patrón Assembler

Los Assemblers transforman objetos entre capas:

**Resource → Command**: Convierte el DTO de entrada plana en un comando de dominio estructurado. Ejemplo:
- `CreateCovenantResource.DocumentIdentifier` (Guid) → `CreateCovenantCommand.DocumentIdentifier` (Guid)
- `CreateCovenantResource.PeriodStartDate` (DateOnly) → `CreateCovenantCommand.PeriodStartDate` (DateOnly)

**Entity → Resource**: Convierte el aggregate root en un DTO de respuesta plano. Ejemplo:
- `Covenant.DocumentIdentifier.Value.ToString()` → `CovenantResource.DocumentId` (string)
- `Covenant.TotalValue.Value` → `CovenantResource.MonetaryAmountValue` (decimal)

---

## Comparación Detallada con Learning Center Platform

A continuación se muestra cómo cada componente de Learning Center se mapea a este proyecto:

### Shared Infrastructure

| Learning Center | Capbase PC2 | Propósito |
|---|---|---|
| `Shared.Application.Model.Result<T>` | `Shared.Application.Model.Result<T>` | Patrón Result para operaciones |
| `Shared.Domain.Model.Entities.IAuditableEntity` | `Shared.Domain.Model.IAuditableEntity` | Interfaz de auditoría (CreatedAt/UpdatedAt) |
| `Shared.Domain.Repositories.IBaseRepository<T>` | `Shared.Domain.Repositories.IBaseRepository<T>` | Repositorio genérico CRUD |
| `Shared.Domain.Repositories.IUnitOfWork` | `Shared.Domain.Repositories.IUnitOfWork` | Transacciones atómicas |
| `Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration.AppDbContext` | `Shared.Infrastructure.Persistence.EntityFrameworkCore.Configuration.AppDbContext` | DbContext principal |
| `Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories.BaseRepository<T>` | `Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories.BaseRepository<T>` | Implementación base de repositorio |
| `Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories.UnitOfWork` | `Shared.Infrastructure.Persistence.EntityFrameworkCore.Repositories.UnitOfWork` | Implementación de Unit of Work |
| `Shared.Infrastructure.Persistence.EntityFrameworkCore.Interceptors.AuditableEntityInterceptor` | (Implementado en SaveChangesAsync) | Auditoría automática |
| `Shared.Interfaces.Rest.ProblemDetails.ProblemDetailsFactory` | (Implementado en Controller con switch) | Manejo centralizado de errores |

### Paperwork (PC) vs Publishing (Learning Center)

| Learning Center | Capbase PC2 | Propósito |
|---|---|---|
| `Publishing.Domain.Model.Aggregates.Tutorial` | `Paperwork.Domain.Model.Aggregates.Covenant` | Aggregate root principal |
| `Publishing.Domain.Model.Aggregates.TutorialAudit` | `Paperwork.Domain.Model.Aggregates.CovenantAudit` | Partial class para auditoría |
| `Publishing.Domain.Model.Commands.CreateTutorialCommand` | `Paperwork.Domain.Model.Commands.CreateCovenantCommand` | Comando de creación CQRS |
| `Publishing.Domain.Repositories.ITutorialRepository` | `Paperwork.Domain.Repositories.ICovenantRepository` | Interfaz de repositorio específico |
| `Publishing.Application.CommandServices.ITutorialCommandService` | `Paperwork.Application.CommandServices.ICovenantCommandService` | Servicio de aplicación (interfaz) |
| `Publishing.Application.Internal.CommandServices.TutorialCommandService` | `Paperwork.Application.Internal.CommandServices.CovenantCommandService` | Servicio de aplicación (implementación) |
| `Publishing.Interfaces.Rest.TutorialsController` | `Paperwork.Interfaces.Rest.CovenantsController` | Controlador REST |
| `Publishing.Interfaces.Rest.Resources.CreateTutorialResource` | `Paperwork.Interfaces.Rest.Resources.CreateCovenantResource` | DTO de entrada |
| `Publishing.Interfaces.Rest.Resources.TutorialResource` | `Paperwork.Interfaces.Rest.Resources.CovenantResource` | DTO de salida |
| `Publishing.Interfaces.Rest.Transform.CreateTutorialCommandFromResourceAssembler` | `Paperwork.Interfaces.Rest.Transform.CreateCovenantCommandFromResourceAssembler` | Assembler entrada |
| `Publishing.Interfaces.Rest.Transform.TutorialResourceFromEntityAssembler` | `Paperwork.Interfaces.Rest.Transform.CovenantResourceFromEntityAssembler` | Assembler salida |
| `Publishing.Infrastructure.Persistence.EntityFrameworkCore.Configuration.ModelBuilderExtensions` | `Paperwork.Infrastructure.Persistence.EntityFrameworkCore.Configuration.ModelBuilderExtensions` | Configuración Fluent API |

### Shared Domain (Value Objects Transversales)

| Learning Center | Capbase PC2 | Propósito |
|---|---|---|
| `Publishing.Domain.Model.ValueObjects.ProfileId` | `Paperwork.Domain.Model.ValueObjects.CapbaseIdentifier` | UUID de otro BC |
| `Publishing.Domain.Model.ValueObjects.EAssetType` | `Paperwork.Domain.Model.Aggregates.CovenantStatus` | Enumeración de estado |
| - | `Shared.Domain.Model.LegalityPeriod` | Periodo de validez (compartido) |
| - | `Shared.Domain.Model.MonetaryAmount` | Monto monetario (compartido) |
| - | `Paperwork.Domain.Model.ValueObjects.PartyId` | Identificador de parte contratante |

### Lo que Learning Center tiene que PC2 NO tiene (y por qué)

| Componente | Learning Center | PC2 | Razón |
|---|---|---|---|
| **Authentication (IAM)** | Bounded context IAM con JWT, BCrypt, SignIn/SignUp | No implementado | PC2 no requiere autenticación |
| **Cortex.Mediator** | Mediator pattern para eventos de dominio | No implementado | PC2 no tiene eventos de dominio complejos |
| **Middlewares** | GlobalExceptionHandlerMiddleware, RequestAuthorizationMiddleware | No implementado | PC2 maneja errores en el controlador |
| **Queries Múltiples** | GetAllTutorialsQuery, GetTutorialByIdQuery, etc. | No implementado | PC2 solo tiene un endpoint POST |
| **Eventos de Dominio** | CategoryCreatedEvent | No implementado | No hay eventos asíncronos en PC2 |
| **ACL (Anti-Corruption Layer)** | ProfilesContextFacade, IamContextFacade | No implementado | PC2 no interactúa con otros BCs |

---

## Notas sobre la Creación del Proyecto con Visual Studio 2022

### Paso a paso detallado:

1. Abrir **Visual Studio 2022**
2. Click en **"Create a new project"**
3. En el buscador, escribir **"ASP.NET Core Web API"**
4. Seleccionar la plantilla **"ASP.NET Core Web API"** con los tags C#, Web, API
5. Click **"Next"**
6. Configurar:
   - **Project name:** `PC212190.U202418655.Capbase.Platform`
   - **Location:** `C:\Users\vcris\Downloads\Examen\pc212190u202418655\`
   - **Solution name:** `pc212190u202418655`
   - **Place solution and project in same directory:** DESMARCAR (para que la solución esté en la carpeta raíz y el proyecto dentro)
7. Click **"Next"**
8. Configurar:
   - **Framework:** .NET 10.0
   - **Authentication type:** None (ninguna)
   - **Configure for HTTPS:** ✅ Marcado
   - **Enable OpenAPI support:** ✅ Marcado
   - **Use controllers:** ✅ Marcado (importante: NO usar minimal APIs)
   - **Enable Docker:** ❌ Desmarcado
9. Click **"Create"**

### Después de crear el proyecto:

1. **Agregar paquetes NuGet:**
   - Tools → NuGet Package Manager → Manage NuGet Packages for Solution
   - Buscar e instalar:
     - `Microsoft.EntityFrameworkCore` (10.0.8)
     - `Microsoft.EntityFrameworkCore.Relational` (10.0.8)
     - `Microsoft.EntityFrameworkCore.Design` (10.0.8)
     - `MySql.EntityFrameworkCore` (10.0.7)
     - `Humanizer` (3.0.10)
     - `Swashbuckle.AspNetCore` (10.2.0)
     - `Swashbuckle.AspNetCore.Annotations` (10.2.0)

2. **Verificar el framework:**
   - Click derecho en el proyecto → Properties
   - Application → Target Framework: `net10.0`

3. **Verificar el archivo launchSettings.json:**
   - En `Properties/launchSettings.json`, cambiar el puerto a 8097

4. **Eliminar archivos por defecto que sobran:**
   - Eliminar `WeatherForecast.cs` (si existe)
   - Eliminar `Controllers/WeatherForecastController.cs` (si existe)

### Creación de carpetas manualmente:

En el Explorador de Soluciones, crear las siguientes carpetas (click derecho → Add → New Folder):

```
Shared/
Shared/Application/
Shared/Application/Model/
Shared/Domain/
Shared/Domain/Model/
Shared/Domain/Repositories/
Shared/Infrastructure/
Shared/Infrastructure/Persistence/
Shared/Infrastructure/Persistence/EntityFrameworkCore/
Shared/Infrastructure/Persistence/EntityFrameworkCore/Configuration/
Shared/Infrastructure/Persistence/EntityFrameworkCore/Repositories/
Paperwork/
Paperwork/Resources/
Paperwork/Domain/
Paperwork/Domain/Model/
Paperwork/Domain/Model/Aggregates/
Paperwork/Domain/Model/ValueObjects/
Paperwork/Domain/Model/Commands/
Paperwork/Domain/Repositories/
Paperwork/Application/
Paperwork/Application/CommandServices/
Paperwork/Application/Internal/
Paperwork/Application/Internal/CommandServices/
Paperwork/Interfaces/
Paperwork/Interfaces/Rest/
Paperwork/Interfaces/Rest/Resources/
Paperwork/Interfaces/Rest/Transform/
Paperwork/Infrastructure/
Paperwork/Infrastructure/Persistence/
Paperwork/Infrastructure/Persistence/EntityFrameworkCore/
Paperwork/Infrastructure/Persistence/EntityFrameworkCore/Configuration/
Paperwork/Infrastructure/Persistence/EntityFrameworkCore/Repositories/
Resources/
```

Luego crear cada archivo .cs dentro de su carpeta correspondiente.

---

## Análisis de la Estructura de Carpetas de Learning Center vs el Proyecto Creado por Defecto

### Estructura por defecto al crear un ASP.NET Core Web API:

```
PC212190.U202418655.Capbase.Platform/
├── (archivos de proyecto)
├── Controllers/
│   └── WeatherForecastController.cs   ← ELIMINAR
├── Models/
│   └── WeatherForecast.cs              ← ELIMINAR
├── Properties/
│   └── launchSettings.json
├── appsettings.json
├── appsettings.Development.json
└── Program.cs
```

### Estructura DESPUÉS de aplicar la guía (estilo Learning Center):

```
PC212190.U202418655.Capbase.Platform/
├── (archivos de proyecto)
├── Properties/
│   └── launchSettings.json
├── Program.cs (modificado)
├── appsettings.json (modificado)
├── appsettings.Development.json
├── Resources/                          ← NUEVO (i18n compartido)
│   ├── ErrorMessages.cs
│   ├── ErrorMessages.resx
│   └── ErrorMessages.es.resx
├── Shared/                             ← NUEVO (infraestructura compartida)
│   ├── Application/Model/
│   │   └── Result.cs
│   ├── Domain/Model/
│   │   ├── IAuditableEntity.cs
│   │   ├── LegalityPeriod.cs
│   │   └── MonetaryAmount.cs
│   ├── Domain/Repositories/
│   │   ├── IBaseRepository.cs
│   │   └── IUnitOfWork.cs
│   └── Infrastructure/Persistence/EntityFrameworkCore/
│       ├── Configuration/
│       │   └── AppDbContext.cs
│       └── Repositories/
│           ├── BaseRepository.cs
│           └── UnitOfWork.cs
└── Paperwork/                          ← NUEVO (bounded context)
    ├── Resources/
    │   ├── PaperworkMessages.cs
    │   ├── PaperworkMessages.resx
    │   └── PaperworkMessages.es.resx
    ├── Domain/Model/Aggregates/
    │   ├── Covenant.cs
    │   ├── CovenantAudit.cs
    │   ├── CovenantStatus.cs
    │   └── PaperworkError.cs
    ├── Domain/Model/ValueObjects/
    │   ├── CapbaseIdentifier.cs
    │   └── PartyId.cs
    ├── Domain/Model/Commands/
    │   └── CreateCovenantCommand.cs
    ├── Domain/Repositories/
    │   └── ICovenantRepository.cs
    ├── Application/CommandServices/
    │   └── ICovenantCommandService.cs
    ├── Application/Internal/CommandServices/
    │   └── CovenantCommandService.cs
    ├── Interfaces/Rest/
    │   ├── CovenantsController.cs
    │   ├── Resources/
    │   │   ├── CreateCovenantResource.cs
    │   │   └── CovenantResource.cs
    │   └── Transform/
    │       ├── CreateCovenantCommandFromResourceAssembler.cs
    │       └── CovenantResourceFromEntityAssembler.cs
    └── Infrastructure/Persistence/EntityFrameworkCore/
        ├── Configuration/
        │   └── ModelBuilderExtensions.cs
        └── Repositories/
            └── CovenantRepository.cs
```

### ¿Qué se elimina? (Archivos por defecto que sobran)

| Archivo/Carpeta | ¿Se elimina? | Razón |
|---|---|---|
| `Controllers/WeatherForecastController.cs` | ✅ SÍ | Es un ejemplo de plantilla, no parte del proyecto |
| `Models/WeatherForecast.cs` | ✅ SÍ | Es un modelo de ejemplo, no parte del dominio |
| `bin/` | ✅ SÍ | Generado por compilación |
| `obj/` | ✅ SÍ | Generado por compilación |
| `.vs/` | ✅ SÍ | Configuración local de Visual Studio |
| `wwwroot/` | ❌ Solo si no se usa | En APIs web, normalmente no se necesita |
| `appsettings.Development.json` | ❌ NO | Configuración de desarrollo útil |

---

## Migrando de MySQL a PostgreSQL (Guía de Cambios)

Si decides usar PostgreSQL en lugar de MySQL, estos son todos los cambios necesarios:

### 1. .csproj: Cambiar el provider

```xml
<!-- ELIMINAR -->
<!-- <PackageReference Include="MySql.EntityFrameworkCore" Version="10.0.7"/> --> -->

<!-- AGREGAR -->
<PackageReference Include="Npgsql.EntityFrameworkCore.PostgreSQL" Version="10.0.0"/>
```

### 2. Program.cs: Cambiar UseMySQL por UseNpgsql

```csharp
// ELIMINAR
// options.UseMySQL(connectionString);

// AGREGAR
options.UseNpgsql(connectionString);
```

### 3. appsettings.json: Cambiar la cadena de conexión

```json
// MySQL (ELIMINAR)
// "DefaultConnection": "Server=localhost;Port=3306;Database=clerky_wa;User=root;Password=12345678;"

// PostgreSQL (AGREGAR)
"DefaultConnection": "Host=localhost;Port=5432;Database=clerky_wa;Username=postgres;Password=12345678;"
```

### 4. ModelBuilderExtensions.cs: Posibles ajustes de tipos

```csharp
// Para PostgreSQL, el tipo decimal puede necesitar ajuste
tv.Property(x => x.Value)
    .HasColumnName("monetary_amount_value")
    .HasColumnType("numeric(18,4)")  // PostgreSQL usa 'numeric' en lugar de 'decimal'
    .IsRequired();
```

### 5. Base de datos: Crear en PostgreSQL

```sql
CREATE DATABASE clerky_wa;
```

### 6. Verificación: Tabla creada

En PostgreSQL, la tabla se crea en el esquema `public` por defecto. Para verificar:

```sql
SELECT table_name, column_name, data_type
FROM information_schema.columns
WHERE table_name = 'covenants'
ORDER BY ordinal_position;
```

---

---

## Convenciones de Nomenclatura (Coding Standards)

Para mantener un código consistente y profesional, seguir estas convenciones en todo el proyecto:

### C# Naming Conventions

| Elemento | Convención | Ejemplo |
|---|---|---|
| Clases | PascalCase | `Covenant`, `CovenantCommandService`, `CovenantsController` |
| Interfaces | PascalCase con prefijo I | `ICovenantRepository`, `IAuditableEntity`, `IUnitOfWork` |
| Records | PascalCase | `CapbaseIdentifier`, `CreateCovenantCommand`, `CovenantResource` |
| Enums | PascalCase | `CovenantStatus`, `PaperworkError` |
| Enum members | PascalCase | `Draft`, `Active`, `Expired` |
| Métodos | PascalCase | `Handle()`, `ToCommandFromResource()`, `ToResourceFromEntity()` |
| Propiedades públicas | PascalCase | `Id`, `DocumentIdentifier`, `Period`, `Status` |
| Propiedades privadas | camelCase | `context`, `unitOfWork` |
| Parámetros de métodos | camelCase | `command`, `cancellationToken`, `resource` |
| Variables locales | camelCase | `period`, `totalValue`, `covenant` |
| Constantes | PascalCase (SCREAMING_CASE opcional) | `IGV_RATE` o `IgvRate` |
| Archivos | PascalCase (coincide con clase) | `Covenant.cs`, `CovenantCommandService.cs` |

### Base de Datos Naming Conventions

| Elemento | Convención | Ejemplo |
|---|---|---|
| Tablas | snake_case, plural | `covenants` |
| Columnas | snake_case | `document_identifier`, `period_start_date` |
| Primary Key | `id` | `id` |
| Foreign Keys | `{tabla}_id` | `covenant_id` |
| Índices | `ix_{tabla}_{columna}` | `ix_covenants_document_identifier` |
| Esquema | `clerky_wa` | `clerky_wa` |

### REST API Conventions

| Elemento | Convención | Ejemplo |
|---|---|---|
| URLs | minúsculas, plural | `/api/v1/covenants` |
| Versionado | `v1`, `v2` en URL | `/api/v1/covenants` |
| Método POST | Crear recurso | `POST /api/v1/covenants` |
| Método GET | Obtener recurso(s) | `GET /api/v1/covenants/{id}` |
| Request body | JSON con camelCase | `{ "documentIdentifier": "..." }` |
| Response body | JSON con camelCase | `{ "documentId": "..." }` |
| Códigos HTTP | Estándar REST | 201 Created, 400 Bad Request, 409 Conflict |
| Content-Type | `application/json` | Header de request/response |
| Accept-Language | Para i18n | `en`, `es`, `es-PE` |

### XML Comments Conventions

Todos los archivos .cs deben tener XML comments en inglés siguiendo este formato:

```csharp
/// <summary>
/// Breve descripción de la clase/método.
/// </summary>
/// <remarks>
/// Descripción detallada (opcional).
/// Author: Victor Jhosef Laura Acosta - u202418655
/// </remarks>
/// <param name="paramName">Descripción del parámetro.</param>
/// <returns>Descripción del valor de retorno.</returns>
```

### Organización de Usings

Los `using` statements deben organizarse en este orden (separados por líneas en blanco):

```csharp
// 1. System namespaces
using System.Net.Mime;

// 2. Microsoft/ASP.NET Core namespaces
using Microsoft.AspNetCore.Mvc;
using Microsoft.EntityFrameworkCore;

// 3. Proyecto namespaces (del mismo proyecto)
using PC212190.U202418655.Capbase.Platform.Paperwork.Application.CommandServices;
using PC212190.U202418655.Capbase.Platform.Shared.Domain.Repositories;
```

---

## Preparación para la Entrega (ZIP)

### Pasos para generar el ZIP de entrega:

1. **Limpiar el proyecto** (eliminar bin/ obj/):
   ```bash
   # Desde la raíz del proyecto pc212190u202418655/
   dotnet clean
   rm -rf PC212190.U202418655.Capbase.Platform/bin
   rm -rf PC212190.U202418655.Capbase.Platform/obj
   rm -rf .vs
   ```

2. **Verificar que NO existen estos archivos/carpetas:**
   - `bin/` ❌
   - `obj/` ❌
   - `.vs/` ❌
   - `.idea/` ❌
   - `packages/` ❌
   - `*.user` ❌
   - `*.suo` ❌
   - `*.cache` ❌
   - `.DS_Store` ❌

3. **Verificar que SÍ existen estos archivos:**
   - `pc212190u202418655.sln` ✅
   - `PC212190.U202418655.Capbase.Platform/PC212190.U202418655.Capbase.Platform.csproj` ✅
   - `PC212190.U202418655.Capbase.Platform/Program.cs` ✅
   - `PC212190.U202418655.Capbase.Platform/appsettings.json` ✅
   - Todos los archivos .cs de Shared/ y Paperwork/ ✅
   - `README.md` ✅

4. **Compilar para verificar:**
   ```bash
   dotnet build
   ```
   Debe mostrar: `Build succeeded.`

5. **Crear el ZIP:**
   ```bash
   # En la carpeta raíz pc212190u202418655/
   # Windows PowerShell:
   Compress-Archive -Path * -DestinationPath ..\pc212190u202418655.zip
   
   # O desde el explorador:
   # 1. Seleccionar todos los archivos (Ctrl+E)
   # 2. Click derecho → Enviar a → Carpeta comprimida (zip)
   # 3. Nombrar: pc212190u202418655.zip
   ```

6. **Verificar contenido del ZIP:**
   - Abrir el ZIP y confirmar que contiene solo los archivos fuente
   - NO debe contener `bin/`, `obj/`, `.vs/`

---

## Resumen de Cumplimiento de la Rúbrica

| Punto | Requisito | Estado | Dónde |
|---|---|---|---|
| 1 | .NET 10.0, C# 14 | ✅ | `PC212190.U202418655.Capbase.Platform.csproj` |
| 2 | Nombre `pc2<NRC>u<código>` | ✅ | `pc212190u202418655` |
| 3 | MySQL, BD `clerky_wa` | ✅ | `appsettings.json` |
| 4 | Package raíz `PC212190.U202418655.Capbase.Platform` | ✅ | `PC212190.U202418655.Capbase.Platform.csproj` |
| 5 | Bounded context `Paperwork` para Covenant | ✅ | Package `Paperwork` |
| 6 | Bounded context `Shared` con IAuditableEntity, Result | ✅ | Package `Shared` |
| 7 | Validación en Value Objects | ✅ | LegalityPeriod, MonetaryAmount con validación en constructores |
| 8 | i18n EN/ES/ES-PE via Accept-Language | ✅ | `UseRequestLocalization` + `PaperworkMessages.resx` |
| 9 | DDD, CQRS, layered architecture | ✅ | Domain/Application/Interfaces/Infrastructure |
| 10 | Separación de conceptos (partial class Covenant + CovenantAudit) | ✅ | `Covenant.cs` + `CovenantAudit.cs` |
| 11 | Convención snake_case en BD | ✅ | Fluent API con `.HasColumnName("snake_case")` |
| 12 | Shared bounded context (estilo Learning Center) | ✅ | `Shared` package con Application/Domain/Infrastructure |
| 13 | XML comments en inglés con @author/Author | ✅ | Todos los archivos |
| 14 | OpenAPI con Swagger UI + SwaggerResponse | ✅ | `[SwaggerOperation]` + `[SwaggerResponse]` en `CovenantsController.cs` |
| 15 | Puerto 8097 | ✅ | `launchSettings.json` |
| 16 | URLs minúsculas y plural | ✅ | `/api/v1/covenants` |
| 17 | Result Pattern en lugar de excepciones | ✅ | `Result<T>` en `CovenantCommandService` |
| 18 | Mensajes en inglés y español | ✅ | `.resx` files con EN/ES |
| 19 | Gestión de errores con HTTP Status correctos | ✅ | Switch en `CovenantsController` |
| 20 | README.md | ✅ | `README.md` |
| 21 | ZIP `pc2<NRC>u<código>.zip` | ✅ | `pc212190u202418655.zip` |
| 22 | Estructura limpia sin bin/obj/.vs | ✅ | Explicado en sección "Limpieza de Carpetas" |

---

## Reglas de Negocio Implementadas

| # | Regla | Implementación | Archivo |
|---|---|---|---|
| 1 | No duplicado de `DocumentIdentifier` | `ExistsByDocumentIdentifierAsync()` verifica antes de crear | `CovenantCommandService.cs` |
| 2 | `Period.EndDate` > `Period.StartDate`, sino `ArgumentException` | Validación en constructor de `LegalityPeriod` | `LegalityPeriod.cs` |
| 3 | `MonetaryAmount.Value` >= 0, sino `ArgumentOutOfRangeException` | Validación en constructor de `MonetaryAmount` | `MonetaryAmount.cs` |
| 4 | `MonetaryAmount.Currency` no vacío, sino `ArgumentNullException` | Validación en constructor de `MonetaryAmount` | `MonetaryAmount.cs` |
| 5 | `CapbaseIdentifier` es un value object UUID generado externamente | `record CapbaseIdentifier(Guid Value)` | `CapbaseIdentifier.cs` |
| 6 | `PartyId` es un value object UUID de otro bounded context | `record PartyId(Guid Value)` | `PartyId.cs` |
| 7 | `CovenantStatus` enum con Draft por defecto | `CovenantStatus.Draft` como default en `CreateCovenantCommand` | `CovenantStatus.cs` + `CreateCovenantCommand.cs` |
| 8 | Covenant auditable con CreatedAt/UpdatedAt | `IAuditableEntity` + `SaveChangesAsync` override | `CovenantAudit.cs` + `AppDbContext.cs` |
| 9 | i18n EN/ES/ES-PE | `UseRequestLocalization` + `IStringLocalizer` | `Program.cs` + archivos .resx |
| 10 | `Footnotes` opcional | `string?` nullable + `.IsRequired(false)` en Fluent API | `Covenant.cs` + `ModelBuilderExtensions.cs` |
| 11 | Response NO incluye status ni footnotes | `CovenantResource` solo expone 7 campos | `CovenantResource.cs` |
| 12 | `Id` autogenerado por BD | `.ValueGeneratedOnAdd()` + `HasKey(c => c.Id)` | `ModelBuilderExtensions.cs` |
| 13 | `Id` no se ingresa al crear | No incluido en `CreateCovenantResource` | `CreateCovenantResource.cs` |
| 14 | Status como string en JSON | `JsonStringEnumConverter()` en `Program.cs` | `Program.cs` |
| 15 | HTTP 201 Created en éxito | `CreatedAtAction(nameof(CreateCovenant), ...)` | `CovenantsController.cs` |
| 16 | HTTP Status correcto en errores | Switch por `result.ErrorCode` | `CovenantsController.cs` |

---

## Verificación Final (Checklist)

### Requisitos previos
- [ ] .NET 10 SDK instalado (`dotnet --version` → 10.0.x)
- [ ] MySQL Server instalado y corriendo (puerto 3306)
- [ ] Base de datos `clerky_wa` creada
- [ ] Visual Studio 2022 o JetBrains Rider instalado

### Ejecución
- [ ] `dotnet restore` → sin errores
- [ ] `dotnet build` → sin errores de compilación
- [ ] `dotnet run` → aplicación inicia sin excepciones

### Verificación de Endpoints
- [ ] Swagger UI: `http://localhost:8097/swagger/index.html`
- [ ] OpenAPI JSON: `http://localhost:8097/swagger/v1/swagger.json`
- [ ] `POST /api/v1/covenants` → 201 Created (con datos válidos)
- [ ] `POST /api/v1/covenants` → 409 Conflict (mismo `documentIdentifier`)
- [ ] `POST /api/v1/covenants` → 400 Bad Request (`periodEndDate` <= `periodStartDate`)
- [ ] `POST /api/v1/covenants` → 400 Bad Request (`monetaryAmountValue` < 0)
- [ ] `POST /api/v1/covenants` → 400 Bad Request (`monetaryAmountCurrency` vacío)

### Verificación de Localización
- [ ] Header `Accept-Language: es` → mensajes en español
- [ ] Header `Accept-Language: en` → mensajes en inglés
- [ ] Header `Accept-Language: es-PE` → mensajes en español
- [ ] Sin header Accept-Language → mensajes en inglés (default)

### Verificación de Base de Datos
- [ ] Tabla `covenants` creada automáticamente en `clerky_wa`
- [ ] Columnas en snake_case: `id`, `document_identifier`, `client_id`, `period_start_date`, `period_end_date`, `monetary_amount_value`, `monetary_amount_currency`, `status`, `footnotes`, `created_at`, `updated_at`
- [ ] Índice único en `document_identifier`
- [ ] `id` auto-incrementable

### Verificación de Estructura
- [ ] Sin carpetas `bin/` ni `obj/`
- [ ] Sin carpeta `.vs/`
- [ ] Sin carpeta `.idea/`
- [ ] Sin archivos `.DS_Store`
- [ ] Sin archivos `.user` o `.suo`

### Convenciones de Código
- [ ] XML comments en inglés con `Author: Victor Jhosef Laura Acosta - u202418655`
- [ ] Nombres de clases en PascalCase
- [ ] Nombres de métodos en PascalCase (C# convention)
- [ ] Nombres de variables en camelCase
- [ ] Nombres de tablas en plural y snake_case
- [ ] Nombres de columnas en snake_case
- [ ] URLs en minúsculas y plural (`/api/v1/covenants`)

---

## Diferencias clave entre este proyecto (Aplicaciones Web) y Facturease (Open Source)

| Aspecto | Facturease (Open Source - Java/Spring) | Capbase (Aplicaciones Web - .NET/C#) |
|---|---|---|
| **Lenguaje** | Java 26 | C# 14 |
| **Framework** | Spring Boot 4.1.0 | ASP.NET Core 10.0 |
| **ORM** | Spring Data JPA / Hibernate | Entity Framework Core 10.0 |
| **Gestor dependencias** | Maven (pom.xml) | NuGet (.csproj) |
| **Configuración** | application.properties | appsettings.json |
| **Clase principal** | `@SpringBootApplication` | `WebApplication.CreateBuilder` |
| **Base de datos** | PostgreSQL | MySQL (o PostgreSQL) |
| **Documentación API** | springdoc-openapi (Swagger) | Swashbuckle.AspNetCore (Swagger) |
| **Arquitectura** | DDD + CQRS + Layered | DDD + CQRS + Layered (mismo patrón) |
| **Patrón Result** | `sealed interface Result<T,E>` | `class Result<T>` (misma idea) |
| **Value Objects** | Java records + validación en compact constructor | C# records + validación en constructor |
| **i18n** | ResourceBundle + messages.properties | .resx files + IStringLocalizer |
| **Naming BD** | SnakeCaseWithPluralizedTablePhysicalNamingStrategy | Fluent API manual snake_case |
| **IDE** | IntelliJ IDEA | Visual Studio 2022 / Rider |
| **Curso** | Open Source (OpenSource) | Aplicaciones Web |

---

## Estructura Final del Proyecto (26 archivos .cs + 6 archivos de configuración)

```
pc212190u202418655/
├── pc212190u202418655.sln
├── README.md
└── PC212190.U202418655.Capbase.Platform/
    ├── PC212190.U202418655.Capbase.Platform.csproj
    ├── Program.cs
    ├── appsettings.json
    ├── appsettings.Development.json
    ├── Properties/
    │   └── launchSettings.json
    ├── Resources/
    │   ├── ErrorMessages.cs
    │   ├── ErrorMessages.resx
    │   └── ErrorMessages.es.resx
    ├── Shared/
    │   ├── Application/
    │   │   └── Model/
    │   │       └── Result.cs
    │   ├── Domain/
    │   │   ├── Model/
    │   │   │   ├── IAuditableEntity.cs
    │   │   │   ├── LegalityPeriod.cs
    │   │   │   └── MonetaryAmount.cs
    │   │   └── Repositories/
    │   │       ├── IBaseRepository.cs
    │   │       └── IUnitOfWork.cs
    │   └── Infrastructure/
    │       └── Persistence/
    │           └── EntityFrameworkCore/
    │               ├── Configuration/
    │               │   └── AppDbContext.cs
    │               └── Repositories/
    │                   ├── BaseRepository.cs
    │                   └── UnitOfWork.cs
    └── Paperwork/
        ├── Resources/
        │   ├── PaperworkMessages.cs
        │   ├── PaperworkMessages.resx
        │   └── PaperworkMessages.es.resx
        ├── Domain/
        │   ├── Model/
        │   │   ├── Aggregates/
        │   │   │   ├── Covenant.cs
        │   │   │   ├── CovenantAudit.cs
        │   │   │   ├── CovenantStatus.cs
        │   │   │   └── PaperworkError.cs
        │   │   ├── ValueObjects/
        │   │   │   ├── CapbaseIdentifier.cs
        │   │   │   └── PartyId.cs
        │   │   └── Commands/
        │   │       └── CreateCovenantCommand.cs
        │   └── Repositories/
        │       └── ICovenantRepository.cs
        ├── Application/
        │   ├── CommandServices/
        │   │   └── ICovenantCommandService.cs
        │   └── Internal/
        │       └── CommandServices/
        │           └── CovenantCommandService.cs
        ├── Interfaces/
        │   └── Rest/
        │       ├── CovenantsController.cs
        │       ├── Resources/
        │       │   ├── CreateCovenantResource.cs
        │       │   └── CovenantResource.cs
        │       └── Transform/
        │           ├── CreateCovenantCommandFromResourceAssembler.cs
        │           └── CovenantResourceFromEntityAssembler.cs
        └── Infrastructure/
            └── Persistence/
                └── EntityFrameworkCore/
                    ├── Configuration/
                    │   └── ModelBuilderExtensions.cs
                    └── Repositories/
                        └── CovenantRepository.cs
```

**Resumen de archivos:**
- 26 archivos `.cs` (código fuente C#)
- 4 archivos `.resx` (recursos localizados)
- 2 archivos de configuración (`appsettings.json`, `appsettings.Development.json`)
- 1 archivo de proyecto (`.csproj`)
- 1 solución (`.sln`)
- 1 punto de entrada (`Program.cs`)
- 1 configuración de lanzamiento (`launchSettings.json`)
- 1 README.md
- **Total: 37 archivos fuente** (excluyendo bin/, obj/, .vs/)

### Leyenda de la estructura:

```
📁 pc212190u202418655/          ← Solución (contiene .sln)
   📁 PC212190.U202418655.Capbase.Platform/   ← Proyecto Web API
      📁 Shared/                ← Infraestructura transversal
         📁 Application/        ← Patrón Result
         📁 Domain/             ← Interfaces y Value Objects compartidos
         📁 Infrastructure/     ← DbContext, BaseRepository, UnitOfWork
      📁 Paperwork/             ← Bounded Context de Trámites Documentarios
         📁 Domain/             ← Aggregate, Value Objects, Commands
         📁 Application/        ← Servicios de aplicación
         📁 Infrastructure/     ← Persistencia EF Core
         📁 Interfaces/         ← Controladores REST, DTOs, Assemblers
      📁 Resources/             ← Archivos .resx para i18n
```

### ¿Por qué esta estructura?

| Razón | Explicación |
|---|---|
| **Separación de concerns** | Cada capa tiene una responsabilidad única y bien definida |
| **Testabilidad** | Las capas se pueden probar de forma aislada (unit testing) |
| **Mantenibilidad** | Los cambios en una capa no afectan a las otras |
| **Escalabilidad** | Se pueden agregar nuevos bounded contexts fácilmente |
| **Profesionalismo** | Sigue los estándares de la industria (DDD, CQRS) |
| **Alineación con el profesor** | Estructura idéntica a Learning Center, aprobada por el profesor |

---

---

## Apéndice A: Referencia Rápida de Entity Framework Core

### Conceptos Clave de EF Core usados en el proyecto:

| Concepto | Uso en el proyecto | Archivo |
|---|---|---|
| `DbContext` | Clase principal que coordina las operaciones con la BD | `AppDbContext.cs` |
| `DbSet<T>` | Representa una tabla en la BD | `AppDbContext.Covenants` |
| `Entity<T>()` | Configuración Fluent API para una entidad | `ModelBuilderExtensions.cs` |
| `ToTable()` | Especifica el nombre de la tabla en la BD | `ModelBuilderExtensions.cs` línea 22 |
| `HasKey()` | Define la clave primaria | `ModelBuilderExtensions.cs` línea 24 |
| `Property()` | Configura una propiedad/columna | `ModelBuilderExtensions.cs` líneas 25-43 |
| `HasColumnName()` | Especifica el nombre de la columna en snake_case | `ModelBuilderExtensions.cs` varias líneas |
| `IsRequired()` | Marca columna como NOT NULL | `ModelBuilderExtensions.cs` varias líneas |
| `ValueGeneratedOnAdd()` | Marca columna como auto-increment | `ModelBuilderExtensions.cs` línea 28 |
| `HasConversion<T>()` | Convierte enum a string en la BD | `ModelBuilderExtensions.cs` línea 33 |
| `HasColumnType()` | Especifica el tipo SQL exacto | `ModelBuilderExtensions.cs` línea 78 |
| `OwnsOne()` | Mapea un value object como columnas embebidas | `ModelBuilderExtensions.cs` líneas 46-84 |
| `HasIndex().IsUnique()` | Crea un índice único | `ModelBuilderExtensions.cs` líneas 51-52 |
| `FindAsync()` | Busca por clave primaria | `BaseRepository.cs` línea 22 |
| `AddAsync()` | Agrega una entidad al contexto | `BaseRepository.cs` línea 30 |
| `SaveChangesAsync()` | Persiste todos los cambios | `UnitOfWork.cs` línea 16 |
| `EnsureCreated()` | Crea la BD y tablas si no existen | `Program.cs` línea 71 |

### Flujo de SaveChanges con Auditoría:

```
1. Llamada a unitOfWork.CompleteAsync()
2. → AppDbContext.SaveChangesAsync()
3.    → Itera ChangeTracker.Entries<IAuditableEntity>()
4.    → Si Added: asigna CreatedAt = UtcNow
5.    → Si Added o Modified: asigna UpdatedAt = UtcNow
6.    → base.SaveChangesAsync() ejecuta el INSERT/UPDATE en BD
```

### Mapeo de Value Objects con OwnsOne:

EF Core permite mapear value objects (como `LegalityPeriod`) como columnas embebidas en la misma tabla, en lugar de crear una tabla separada. Esto se logra con `OwnsOne()`:

```csharp
entity.OwnsOne(c => c.Period, p =>
{
    p.Property(x => x.StartDate)
        .HasColumnName("period_start_date")
        .IsRequired();
    p.Property(x => x.EndDate)
        .HasColumnName("period_end_date")
        .IsRequired();
});
```

Esto produce la siguiente estructura en la tabla `covenants`:

```sql
CREATE TABLE covenants (
    id INT AUTO_INCREMENT PRIMARY KEY,
    document_identifier CHAR(36) NOT NULL,
    client_id CHAR(36) NOT NULL,
    period_start_date DATE NOT NULL,     -- ← De LegalityPeriod
    period_end_date DATE NOT NULL,       -- ← De LegalityPeriod
    monetary_amount_value DECIMAL(18,4) NOT NULL,  -- ← De MonetaryAmount
    monetary_amount_currency VARCHAR(3) NOT NULL,  -- ← De MonetaryAmount
    status VARCHAR(20) NOT NULL,
    footnotes TEXT,
    created_at DATETIME,
    updated_at DATETIME,
    UNIQUE INDEX ix_covenants_document_identifier (document_identifier)
);
```

---

## Apéndice B: Glosario de Términos

| Término | Definición |
|---|---|
| **Aggregate Root** | Entidad raíz que garantiza la consistencia de un grupo de objetos relacionados (Covenant) |
| **Bounded Context** | Límite explícito dentro del cual un modelo de dominio es válido (Paperwork) |
| **CQRS** | Command Query Responsibility Segregation - separación de operaciones de escritura y lectura |
| **DDD** | Domain-Driven Design - diseño guiado por el dominio |
| **DTO** | Data Transfer Object - objeto para transferir datos entre capas |
| **Fluent API** | API de EF Core para configurar el mapeo objeto-relacional mediante código |
| **i18n** | Internacionalización - soporte para múltiples idiomas |
| **ORM** | Object-Relational Mapper - mapeador objeto-relacional (EF Core) |
| **Result Pattern** | Patrón que encapsula el éxito/fracaso de una operación en un objeto |
| **Unit of Work** | Patrón que agrupa operaciones en una transacción atómica |
| **Value Object** | Objeto inmutable definido por sus atributos, no por identidad (LegalityPeriod) |

---

## Nota Final

Esta guía ha sido elaborada por Victor Jhosef Laura Acosta (u202418655) como material de apoyo para el curso de Aplicaciones Web (NRC 12190) bajo la supervisión del profesor Hugo Allan Mori Paiva.

El objetivo es proporcionar una referencia completa, paso a paso, para construir un proyecto ASP.NET Core siguiendo la arquitectura DDD + CQRS, con la misma estructura de carpetas y patrones que el proyecto Learning Center Platform, pero adaptado al dominio de negocio del caso Capbase (Paperwork bounded context).

> **Importante:** Antes de entregar, asegúrate de:
> 1. Reemplazar `u202418655` con tu código si es diferente
> 2. Verificar el NRC 12190 en nombres de archivos
> 3. Eliminar carpetas bin/, obj/, .vs/
> 4. Compilar con `dotnet build` para verificar que no hay errores
> 5. Probar el endpoint con curl o Postman
> 6. Verificar Swagger UI en `http://localhost:8097/swagger`
> 7. Crear el ZIP con nombre `pc212190u202418655.zip`

---

## Créditos

- **Plantilla de guía basada en:** Proyecto Facturease Perú - Invoice Management (Open Source - Java/Spring)
- **Estructura de carpetas basada en:** Learning Center Platform (.NET - ASP.NET Core)
- **Lógica de negocio basada en:** Capbase Platform - Paperwork bounded context
- **Curso:** Aplicaciones Web - NRC 12190
- **Profesor:** Hugo Allan Mori Paiva
- **Alumno:** Victor Jhosef Laura Acosta - u202418655

---

## Licencia

Este documento es de uso educativo para el curso de Aplicaciones Web. Distribución libre entre fines académicos.

---

**Fin de la guía** — Total: ~3700 líneas
