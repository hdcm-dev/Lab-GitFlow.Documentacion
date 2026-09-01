# 03 — Persistencia y sesiones

> **Propósito**: explicar cómo se guardan los datos, cómo se crea el esquema y —lo central del
> proyecto— cómo se aísla el espacio de datos de cada visitante para que las pruebas E2E puedan
> correr en paralelo contra una única instancia.
> **Fuente primaria**: `src/MovilidadUrbana.Web/Infraestructura/`.

## Modelo de datos

`Infraestructura/Persistencia/ContextoDeDatos.cs` — único lugar que conoce el motor.

| `DbSet` | Entidad | Configuración |
| --- | --- | --- |
| `Localidades` | `Localidad` | `SesionId` ≤64 requerido e **indexado**; `Nombre`/`Provincia` ≤60; `CodigoPostal` ≤4 |
| `Encuestas` | `RespuestaDeEncuesta` | `SesionId` ≤64 requerido e indexado; `Nombre` ≤80; `Medios` por conversor |
| `Sesiones` | `Sesion` | `Id` como clave, ≤64 |

El índice sobre `SesionId` acompaña el patrón de acceso: **toda** consulta del ABM filtra por sesión.

`Medios` no tiene tipo lista en SQLite, así que se guarda como texto separado por comas mediante un
`ValueConverter` (`string.Join` / `Split` con `RemoveEmptyEntries`) acompañado del `ValueComparer`
que EF Core exige para poder detectar cambios en una colección.

## Creación del esquema

`PreparadorDeBaseDeDatos.Preparar(IServiceProvider)`, invocado desde `Program.cs` antes de construir
el pipeline:

1. Abre un ámbito y un contexto desde la fábrica.
2. Extrae el `DataSource` de la cadena de conexión y **crea la carpeta** si falta.
3. `EnsureCreated()` — no migraciones: el laboratorio no versiona el esquema y así el binario
   publicado arranca en cualquier máquina sin pasos previos.
4. `PRAGMA journal_mode=WAL` — permite leer mientras otra conexión escribe, que es exactamente lo
   que ocurre con varias clases de prueba en paralelo sobre el mismo archivo.

**Consecuencia operativa**: un cambio de entidades obliga a borrar el archivo `.db`; no hay ruta de
migración. El `.gitignore` excluye `datos/`, `*.db`, `*.db-wal`, `*.db-shm` y `/datos-e2e/`.

## Sesión: emisión y propagación

### `ContextoDeSesion` (alcance de ámbito)

| Miembro | Valor |
| --- | --- |
| `NombreDeCookie` | `sesion-movilidad` |
| `LargoMaximo` | 64 |
| `Id` | inicializado con un `Guid` nuevo (`"n"`) |
| `Establecer(id)` | asigna solo si `EsValido` |
| `EsValido(id)` | no vacío y de largo ≤ 64 |

El identificador provisorio del constructor evita que un ámbito sin cookie termine leyendo o
escribiendo en un espacio de datos compartido: en el peor caso trabaja sobre uno propio y efímero.

### `MiddlewareDeSesion`

Lee la cookie; si no es válida **y la petición es un documento**, emite una nueva con
`HttpOnly`, `IsEssential`, `SameSite=Lax`, `Path=/`, `MaxAge=1 día`. Después publica el valor en el
`ContextoDeSesion` del ámbito.

«Documento» = `GET` que no apunta a `/_framework` ni a `/_blazor` y **no tiene extensión**. La
restricción no es cosmética: si la cookie se emitiera también en las peticiones de css y js —que el
navegador lanza en paralelo— la primera visita generaría varios identificadores a la vez y se
quedaría con el último en llegar.

Se registra **antes** de `UseAntiforgery()` en el pipeline.

### Propagación al circuito

`App.razor` es el único componente que se renderiza dentro de la petición HTTP, así que es el único
que puede leer el valor; lo pasa como parámetro a `Routes`, que en `OnParametersSet` lo establece en
el `ContextoDeSesion` del circuito antes de que se renderice ninguna página. Diagrama de secuencia
en [01_Arquitectura.md](01_Arquitectura.md).

## Repositorios

Ambos reciben `IDbContextFactory<ContextoDeDatos>` y `IContextoDeSesion`, y **abren un contexto por
operación**. Es lo recomendado en Blazor Server: un `DbContext` con alcance de ámbito viviría lo que
dura el circuito —minutos u horas— y no está pensado para eso.

### `RepositorioDeLocalidades`

| Método | Filtro de sesión | Siembra antes |
| --- | --- | --- |
| `ListarAsync` | `Where(SesionId == sesion.Id)`, `AsNoTracking`, `OrderBy(Id)` | sí |
| `ObtenerAsync` | `Id == id && SesionId == sesion.Id`, `AsNoTracking` | sí |
| `AgregarAsync` | estampa `localidad.SesionId = sesion.Id` | sí |
| `ActualizarAsync` | **retorna sin hacer nada** si `localidad.SesionId != sesion.Id` | no |
| `EliminarAsync` | `ExecuteDeleteAsync` con `Id == id && SesionId == sesion.Id` | no |

Ninguna consulta sale del espacio de datos del visitante, ni siquiera si le llegara el identificador
de otra sesión. La comprobación de `ActualizarAsync` es redundante —la entidad viene de
`ObtenerAsync`, que ya filtró— y está puesta para dejar la garantía escrita en el código.

### `RepositorioDeEncuestas`

`AgregarAsync` estampa el `SesionId` y devuelve el `Id` generado; `ContarAsync` cuenta solo las de
la sesión. No siembra.

## Siembra por sesión

`SembradorDeSesion.AsegurarAsync()`:

- Corta de inmediato si ya se verificó en este ámbito (`_yaVerificada`).
- Si no existe la fila en `Sesiones`, la inserta junto con las dos localidades iniciales:

| Nombre | Provincia | CP | Habitantes |
| --- | --- | --- | --- |
| Corrientes | Corrientes | 3400 | 346334 |
| Resistencia | Chaco | 3500 | 291720 |

- Traga `DbUpdateException`: otra petición de la misma sesión pudo ganar la carrera insertando la
  marca, y en ese caso los datos ya están.

Es el juego de datos que la primera prueba de `LocalidadesTests` verifica y del que parten todas las
demás.

## Cadena de conexión

| Contexto | Valor |
| --- | --- |
| Respaldo en código | `Data Source=datos/movilidad.db;Default Timeout=30` |
| Fixture de pruebas | `Data Source=<raíz>/datos-e2e/movilidad.db;Default Timeout=30`, o lo que indique `BASE_DE_DATOS` |
| Configuración | clave `ConnectionStrings:BaseDeDatos` (variable de entorno `ConnectionStrings__BaseDeDatos`) |

No figura en ningún `appsettings.json`: los dos archivos solo llevan `Logging` y `AllowedHosts`.
