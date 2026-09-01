# 08 — Decisiones y glosario

> **Propósito**: reunir en un solo lugar el *porqué* de las elecciones del proyecto —con su costo y
> la alternativa descartada— y el vocabulario propio que aparece en el código y en los índices.
> **Fuente primaria**: comentarios de los archivos fuente y `README.md` del proyecto.

## Decisiones de diseño

### D1 — Las E2E son un proyecto de la solución

Se usan las vinculaciones oficiales de Playwright para .NET (`Microsoft.Playwright.NUnit`) y no el
runner de JavaScript, con un objetivo concreto: que las pruebas se descubran, se ejecuten y se
**depuren desde Visual Studio**, sin salir del IDE ni del lenguaje de la aplicación.

**Costo.** El Explorador de pruebas de Visual Studio soporta pruebas de JavaScript, pero solo de
Mocha, Jasmine, Tape, Jest y Vitest: Playwright no está en esa lista. A cambio del IDE se pierden
`--shard`, el reporter `blob` con `merge-reports` y el reporte HTML. El reporte de cada
configuración es un **TRX** y el paralelismo lo maneja NUnit.

**Alternativa descartada.** Dejar las E2E fuera de la solución, en una carpeta `e2e/` con specs de
TypeScript, que es lo que eligió [dotnet/eShop](https://github.com/dotnet/eShop). Las dos son
defendibles; esta prioriza el IDE.

### D2 — Aislamiento por cookie de sesión

En la versión estática de este laboratorio cada prueba tenía su `localStorage`. Con servidor hay una
única base SQLite que todas las pruebas comparten, incluso las que corren en paralelo.

La solución es que la aplicación reparta un **espacio de datos por sesión**: una cookie que emite
`MiddlewareDeSesion` y por la que filtran todos los repositorios. Cada prueba escribe esa cookie con
un valor propio antes de navegar y recibe su juego de localidades recién sembrado. Es lo que permite
correr las clases de prueba en paralelo contra una sola instancia.

Detalle en [03_Persistencia-Y-Sesiones.md](03_Persistencia-Y-Sesiones.md).

### D3 — Publicar e instalar navegadores en el fixture, no en el build

Atado al build, el paso queda a merced de que el entorno decida compilar —Visual Studio evalúa por
su cuenta si el proyecto está al día y cómo invocar targets de otro proyecto—, y cuando esa decisión
no sale como se espera no hay publicación y **todas** las pruebas mueren en `OneTimeSetUp`. En el
fixture corre siempre y de la misma forma en la consola, en el IDE y en CI.

`dotnet publish` es incremental: cuando no cambió nada tarda un par de segundos. Se paga ese costo
una vez por corrida a cambio de no ejercitar nunca un binario viejo. Se desactiva con
`PUBLICAR_ANTES_DE_PROBAR=false` —que es lo que hace CI, porque allá la aplicación llega como
artefacto—.

Lo mismo con el navegador: se instala llamando al instalador que expone el paquete
`Microsoft.Playwright`, en vez de exigir `pwsh playwright.ps1 install` —que obliga a tener
PowerShell 7, un producto aparte del PowerShell que trae Windows—. Se instala solo el navegador de
la corrida, no los tres.

### D4 — Sin el JavaScript de Bootstrap

El menú colapsable y el diálogo modal se resuelven con estado del componente. En la versión estática
hubo que corregir dos defectos alrededor del modal de Bootstrap: el click que llegaba durante la
animación de apertura y el orden del manejador de `data-bs-dismiss`. Con marcado propio esa clase de
carrera no existe y no hace falta desactivar animaciones.

### D5 — `EnsureCreated`, no migraciones

El laboratorio no versiona el esquema, y así el binario publicado arranca en cualquier máquina sin
pasos previos. **Consecuencia**: un cambio de entidades obliga a borrar el archivo `.db`.

### D6 — `IDbContextFactory`, no `DbContext` con alcance de ámbito

En Blazor Server el ámbito dura lo que dura el circuito —minutos u horas—, y un `DbContext` no está
pensado para eso. Cada operación de repositorio abre y cierra su propio contexto.

### D7 — El paralelismo llega hasta la clase

`ParallelScope.Fixtures` y no `Children`: la integración de Playwright con NUnit lleva un registro de
servicios por worker, y paralelizar dentro de una clase la rompe. Es una diferencia real con
`fullyParallel: true` del runner de JavaScript. La cantidad de workers vive solo en
`pruebas.runsettings`, para que no pueda divergir de una segunda declaración.

### D8 — Cultura fija `es-AR`

Los separadores de miles y decimales forman parte de lo que verifican las pruebas, así que no pueden
depender de la cultura del servidor. Se fija en `Program.cs` y se replica en el `BrowserContext` de
las pruebas junto con `TimezoneId`.

### D9 — `oninput` en lugar de `onchange`

`FillAsync` de Playwright dispara `input`. Con el `@bind` por defecto el valor no llegaría al
servidor hasta que el campo pierda el foco, y la validación rechazaría un formulario que en pantalla
se ve completo.

### D10 — Compilar una vez, liberar ese binario

En `e2e.yml` la aplicación se publica en un job y se reutiliza como artefacto en toda la matriz; en
`release.yml` el artefacto de la versión se construye una sola vez y los ambientes lo promocionan.
Recompilar por ambiente liberaría un binario distinto del que se probó.

### D11 — La auditoría de convergencia compara por contenido

`git cherry` y no comparación de SHA: tras un cherry-pick el hash siempre difiere. Los retornos
resueltos a mano quedarían marcados para siempre, así que se los excluye por una línea
`Convergencia:` en el mensaje del commit — la única forma declarada de explicarlos.

### D12 — `skipped` no es aprobación

`ci-ok` falla si algún job previo quedó en `skipped`: el check obligatorio solo da verde con
evidencia positiva de ejecución. Es lo que sostiene que `verificacion-rapida` corra también con el
pull request en borrador.

## Trampas conocidas

Errores que costaron tiempo y que el código documenta para no repetirlos.

| Síntoma | Causa | Dónde |
| --- | --- | --- |
| Prueba intermitente que solo falla en máquinas cargadas | Click antes de que el circuito esté conectado | testigo `estado-app` + `EsperarInteractivoAsync` |
| Recursos estáticos vacíos con `200` y `Content-Length: 0` (no `404`), circuito que no arranca | ASP.NET Core toma el directorio actual como raíz de contenido | `WorkingDirectory` del fixture |
| URL base vacía y queja de Playwright por la cookie | Un `SetUpFixture` cubre su namespace y los que cuelgan, nunca el de arriba | namespace de `ServidorDeLaAplicacion` |
| Descubrimiento del apphost que falla en Windows | El apphost lleva `.exe` en Windows y no lleva extensión en Linux/macOS | `NombreDelApphost` |
| `The given key 'Browser' was not present in the dictionary` | `ParallelScope.Children` con Playwright + NUnit | `ParalelismoDelEnsamblado.cs` |
| Varias cookies de sesión en la primera visita | Emitirla también en peticiones de css/js, que el navegador lanza en paralelo | `EsUnDocumento` del middleware |
| Formulario visualmente completo que la validación rechaza | `@bind` escuchando `onchange` | `@bind:event="oninput"` |
| `Permission denied` al ejecutar la publicación en CI | Los artefactos de Actions se empaquetan en zip y pierden el bit de ejecución | paso `chmod +x` de `e2e.yml` |
| `failed to connect to the docker API at unix:///var/run/docker.sock` en *Initialize containers* | El runner autoalojado es él mismo un contenedor sin socket de Docker | ningún job usa `container:` |
| Chromium que muere a mitad de corrida | `/dev/shm` de 64 MB dentro de un contenedor | medido: a esta escala no ocurre |

## Glosario

| Término | Significado en este proyecto |
| --- | --- |
| **Sesión** | Espacio de datos de un visitante, identificado por la cookie `sesion-movilidad`. No es la sesión de ASP.NET Core |
| **Siembra** | Alta de las dos localidades iniciales (Corrientes y Resistencia) la primera vez que se toca una sesión |
| **Testigo de interactividad** | `<div data-testid="estado-app" data-interactivo="…">` de `MainLayout`; pasa a `true` cuando el circuito quedó conectado |
| **Configuración** (de prueba) | Una entrada de la matriz: `chromium`, `firefox`, `webkit` o `mobile-chrome` |
| **`mobile-chrome`** | No es un navegador: es chromium con el descriptor Pixel 7 (`EMULAR_MOVIL=true`) |
| **Circuito** | Conexión WebSocket de Blazor Server que sostiene el estado de un componente interactivo |
| **Apphost** | Ejecutable nativo que genera `dotnet publish`; `MovilidadUrbana.Web.exe` en Windows, sin extensión en Linux y macOS |
| **Publicación autocontenida** | `--self-contained -r linux-x64`: el binario no depende del runtime instalado en la máquina |
| **Convergencia** | Que toda corrección hecha en una rama de release tenga equivalente en `main` |
| **Commit huérfano** | Cambio presente en una release y ausente en `main`, detectado por `git cherry` |
| **Preliberación** | Tag con sufijo (`-rc1`, `-demo.1`); se publica como *prerelease* |
| **Aplicación sembrada** | La aplicación *Movilidad Urbana* y su suite, traídas de `Lab-E2E.WebBlazor` para que los escenarios de ramas tengan código real |
