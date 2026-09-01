# 05 — Pruebas de extremo a extremo

> **Propósito**: describir cómo está montada la suite Playwright + NUnit, qué hace cada pieza de
> infraestructura, qué casos cubre y qué límites tiene el paralelismo.
> **Fuente primaria**: `tests/MovilidadUrbana.E2ETests/` y `pruebas.runsettings`.

## Composición del proyecto

`MovilidadUrbana.E2ETests.csproj` — `net10.0`, `IsPackable=false`, **sin `ProjectReference` a la
aplicación**: la ejercita como proceso externo sobre el binario publicado.

| Paquete | Versión |
| --- | --- |
| `Microsoft.Playwright.NUnit` | 1.62.0 |
| `NUnit` / `NUnit.Analyzers` / `NUnit3TestAdapter` | 4.3.2 / 4.7.0 / 5.0.0 |
| `Microsoft.NET.Test.Sdk` | 17.14.0 |
| `coverlet.collector` | 6.0.4 |

`Using` implícitos declarados en el `.csproj`: `NUnit.Framework` y `System.Text.RegularExpressions`.

## Ciclo de vida: `ServidorDeLaAplicacion`

`[SetUpFixture]` a nivel de ensamblado. Es el reemplazo del bloque `webServer` que ofrece el runner
de JavaScript, que el binding de .NET no tiene.

**Vive en el namespace `MovilidadUrbana.E2ETests`, no en uno anidado**: un `SetUpFixture` cubre su
propio namespace y los que cuelgan de él, nunca el de arriba. Puesto en `…E2ETests.Infraestructura`
no se ejecutaría, y el síntoma es desconcertante (la URL base llega vacía y Playwright se queja de
la cookie).

`[OneTimeSetUp]`, en orden:

1. **`AsegurarElNavegador`** — llama a `Microsoft.Playwright.Program.Main(["install", navegador])`,
   el mismo instalador que expone el paquete, en vez de exigir `pwsh playwright.ps1 install`
   (que obliga a tener PowerShell 7). Resuelve el navegador con `PlaywrightSettingsProvider.BrowserName`,
   así que respeta `pruebas.runsettings` y lo que se pase por línea de comandos, e instala **solo**
   el de la corrida. Se desactiva con `INSTALAR_NAVEGADORES=false`.
2. Si hay `URL_BASE`, la adopta y **no levanta nada**.
3. Si no, arma `http://127.0.0.1:{PUERTO}` (por defecto **4173**).
4. **`PublicarLaAplicacionAsync`** — `dotnet publish … -c Release -o publicacion`, sin RID ni
   autocontención, salvo que `PUBLICAR_ANTES_DE_PROBAR=false`. Lee las dos salidas en paralelo:
   esperar a una con el búfer de la otra lleno traba el proceso.
5. **`ResolverElArranque`** — usa el apphost nativo si existe (`MovilidadUrbana.Web.exe` en Windows,
   sin extensión en Linux y macOS) y, si no, arranca con `dotnet MovilidadUrbana.Web.dll`. Si no
   encuentra ninguno lanza un `FileNotFoundException` con el comando de publicación sugerido.
6. Lanza el proceso con **`WorkingDirectory` en la carpeta de la publicación**: ASP.NET Core toma
   el directorio actual como raíz de contenido, y desde otra carpeta `wwwroot` no se encuentra —los
   recursos estáticos se sirven vacíos, con `200` y `Content-Length: 0`, no con `404`, y el circuito
   nunca arranca porque `blazor.web.js` llega en blanco—.
7. Fija el entorno del proceso: `ASPNETCORE_URLS`, `ASPNETCORE_ENVIRONMENT=Production`,
   `ConnectionStrings__BaseDeDatos` y `Logging__LogLevel__Default=Warning`.
8. **`EsperarAQueEscucheAsync`** — sondea el puerto con `TcpClient` cada 500 ms hasta 90 segundos,
   abortando si el proceso murió solo. Crear el archivo SQLite lleva unos segundos.

`[OneTimeTearDown]` mata el árbol de procesos y espera hasta 10 segundos.

`UbicarLaRaizDelRepositorio` sube desde `AppContext.BaseDirectory` hasta encontrar un `*.sln`.

## Base de las pruebas: `PruebaE2E`

Hereda de `PageTest`, que da a cada prueba una página nueva en su propio `BrowserContext`.

| Miembro | Qué hace |
| --- | --- |
| `ContextOptions()` | Fija `BaseURL`, `Locale="es-AR"` y `TimezoneId="America/Argentina/Buenos_Aires"`; con `EMULAR_MOVIL=true` parte del descriptor **Pixel 7** |
| `[SetUp] EstrenarSesionAsync` | Agrega la cookie `sesion-movilidad` con un `Guid` nuevo |
| `IrAAsync(ruta)` | `GotoAsync` + espera de interactividad |
| `EsperarInteractivoAsync()` | `Expect(GetByTestId("estado-app")).ToHaveAttributeAsync("data-interactivo","true")` |
| `IrPorMenuAsync(testid)` | Despliega el menú si el `.navbar-toggler` está visible, espera la clase `show` y recién entonces hace click |

Tres detalles deliberados:

- Si se pide emulación móvil y Playwright no conoce el dispositivo, **se lanza excepción**: caer en
  silencio a escritorio dejaría pasar las pruebas ocultando que la configuración móvil no se ejercitó.
- La cookie propia por prueba es lo que permite el paralelismo: cada una recibe su juego de datos
  recién sembrado sin ver el de las demás, aunque el servidor y el archivo SQLite sean compartidos.
- `IrPorMenuAsync` existe porque en viewport chico el menú viene colapsado: sin desplegarlo, la
  misma prueba pasa en escritorio y falla en móvil.

## Paralelismo

`Infraestructura/ParalelismoDelEnsamblado.cs` declara `[assembly: Parallelizable(ParallelScope.Fixtures)]`
— clases en paralelo entre sí, casos de cada clase en secuencia.

**No se sube a `ParallelScope.Children`**: la integración de Playwright con NUnit lleva un registro
de servicios por worker, y al paralelizar dentro de una misma clase falla con
`The given key 'Browser' was not present in the dictionary` y
`Collection was modified; enumeration operation may not execute`. Es la diferencia real con el
`fullyParallel: true` del runner de JavaScript, que reparte caso por caso.

La **cantidad** de workers vive únicamente en `pruebas.runsettings` (`NumberOfTestWorkers = 4`) y no
se repite con `[assembly: LevelOfParallelism]`, para que no puedan divergir. El *alcance* va en el
código; el *número*, en la configuración.

## `pruebas.runsettings`

| Sección | Valor |
| --- | --- |
| `Playwright/BrowserName` | `chromium` |
| `Playwright/ExpectTimeout` | 5000 ms |
| `Playwright/LaunchOptions/Headless` | `true` |
| `NUnit/NumberOfTestWorkers` | 4 |
| `RunConfiguration/ResultsDirectory` | `resultados` |

Es el reemplazo del bloque `projects` de un `playwright.config.js`: en el binding de .NET el
navegador es una opción de la corrida, no un proyecto del archivo de configuración. Se pisa con
`-- Playwright.BrowserName=firefox` después del separador de argumentos de `dotnet test`.

## Casos cubiertos

**25 casos** en tres clases, todas derivadas de `PruebaE2E`.

### `NavegacionTests` (4)

| Caso |
| --- |
| La portada ofrece acceso a las dos pantallas |
| El menú marca la página activa |
| Desde la portada se llega al ABM y a la encuesta |
| Una dirección inexistente muestra la pantalla de no encontrado |

### `LocalidadesTests` (12) — `[SetUp]` navega a `/localidades`

| Caso |
| --- |
| Muestra el listado sembrado |
| Rechaza el alta con campos inválidos |
| Da de alta una localidad y la persiste tras recargar |
| No permite duplicar nombre dentro de la misma provincia |
| Modifica una localidad existente |
| Cancelar la baja deja la tabla intacta |
| Confirmar la baja elimina la fila |
| Al borrar todas las localidades avisa que no hay datos |
| Cada prueba trabaja sobre su propio conjunto de datos |
| El filtro de provincia deja solo las localidades de esa provincia |
| El filtro sin coincidencias muestra el mensaje de listado vacío |
| Volver al filtro vacío devuelve el listado completo |

«Cada prueba trabaja sobre su propio conjunto de datos» es la que verifica el aislamiento por
sesión descrito en [03_Persistencia-Y-Sesiones.md](03_Persistencia-Y-Sesiones.md). Las tres del
filtro entraron con el PR #4.

### `EncuestaTests` (9) — `[SetUp]` navega a `/encuesta`

| Caso |
| --- |
| Arranca en el paso 1 con el anterior deshabilitado |
| El desplegable de localidades se alimenta del ABM |
| No avanza del paso 1 con datos inválidos |
| No avanza del paso 2 sin medios ni frecuencia |
| Permite volver atrás conservando lo cargado |
| La barra de progreso acompaña el avance |
| No finaliza con el paso 3 incompleto |
| Recorre los tres pasos, muestra el resumen y registra la respuesta |
| «Nueva encuesta» devuelve el asistente al paso 1 |

## Configuraciones de navegador

Cuatro, las mismas que ejercita la matriz de CI: `chromium`, `firefox`, `webkit` y `mobile-chrome`.
La última no es un navegador aparte: es chromium con el descriptor **Pixel 7**, seleccionado por
`EMULAR_MOVIL=true`.

## Cómo correrlas

Ver [07_Entorno-Y-Ejecucion.md](07_Entorno-Y-Ejecucion.md) para los comandos, los scripts de
contenedor y la tabla completa de variables de entorno.
