# 07 — Entorno y ejecución

> **Propósito**: reunir todo lo necesario para compilar, publicar, ejecutar y probar el proyecto:
> comandos, scripts de contenedor, variables de entorno, puertos y carpetas de trabajo.
> **Fuente primaria**: `scripts/`, `pruebas.runsettings`, `.gitignore`,
> `tests/MovilidadUrbana.E2ETests/Infraestructura/ServidorDeLaAplicacion.cs`.

## Requisitos

Una de dos:

- **SDK de .NET 10** instalado en la máquina; o
- **Docker**, y todo se resuelve con los scripts de `scripts/`, que no exigen nada más.

## Comandos directos

```bash
# Compilar la solución como lo hace la CI
dotnet build Lab-E2E.WebBlazor.sln --configuration Release -warnaserror

# Correr las pruebas (publica la aplicación e instala el navegador por sí sola)
dotnet test tests/MovilidadUrbana.E2ETests --settings pruebas.runsettings
dotnet test tests/MovilidadUrbana.E2ETests --settings pruebas.runsettings -- Playwright.BrowserName=firefox

# Solo el descubrimiento, sin navegadores ni aplicación
dotnet test tests/MovilidadUrbana.E2ETests --list-tests

# Levantar la aplicación para mirarla a mano
dotnet run --project src/MovilidadUrbana.Web        # perfil http → localhost:5232
```

**No hace falta publicar a mano antes de probar**: el fixture publica y también instala el navegador
(ver [05_Pruebas-E2E.md](05_Pruebas-E2E.md)).

## Scripts

Los tres montan la raíz del repositorio en `/trabajo`, corren con el UID/GID del usuario y fijan
`NUGET_PACKAGES=/trabajo/.nuget`, de modo que la caché queda en el árbol de trabajo y no en `$HOME`.

### `scripts/dotnet.sh`

Ejecuta el SDK dentro del contenedor oficial, para máquinas sin SDK instalado.

```bash
scripts/dotnet.sh dotnet build Lab-E2E.WebBlazor.sln --configuration Release -warnaserror
scripts/dotnet.sh dotnet publish -c Release
```

Imagen: `mcr.microsoft.com/dotnet/sdk:10.0`, pisable con `IMAGEN_SDK`.

### `scripts/publicar.sh`

Publica la aplicación como binario **autocontenido** `linux-x64` en `publicacion/`, delegando en
`dotnet.sh`. Autocontenido a propósito: así el mismo artefacto corre dentro del contenedor de
Playwright, que trae los navegadores pero no el runtime de .NET. Es el artefacto que usa CI; para
correr las pruebas localmente no hace falta invocarlo.

### `scripts/pruebas.sh`

Corre la suite completa en máquinas sin SDK ni navegadores.

```bash
scripts/pruebas.sh                     # chromium
scripts/pruebas.sh firefox
scripts/pruebas.sh webkit
NAVEGADOR=webkit scripts/pruebas.sh
EMULAR_MOVIL=true scripts/pruebas.sh   # chromium emulando un Pixel 7
URL_BASE=https://ejemplo.test scripts/pruebas.sh chromium   # contra un entorno desplegado
```

Usa `mcr.microsoft.com/playwright:v1.62.1-noble` (pisable con `IMAGEN_E2E` o `VERSION_PLAYWRIGHT`),
que ya trae las librerías de sistema que los navegadores necesitan, y **le agrega el SDK de .NET**
descargándolo a `.dotnet/` la primera vez con `dotnet-install.sh --channel 10.0`. Los navegadores
quedan en `.navegadores/` vía `PLAYWRIGHT_BROWSERS_PATH`, así que solo se descargan una vez. Corre
con `--ipc=host`. Dentro hace `dotnet build --configuration Debug` y luego `dotnet test --no-build`.

## Variables de entorno

Las lee el fixture de pruebas salvo donde se indique.

| Variable | Efecto | Por defecto |
| --- | --- | --- |
| `URL_BASE` | Prueba contra un entorno ya desplegado; no levanta nada local | vacío |
| `PUERTO` | Puerto del servidor bajo prueba | `4173` |
| `CARPETA_APLICACION` | Dónde está la publicación a ejecutar | `<raíz>/publicacion` |
| `BASE_DE_DATOS` | Ruta del archivo SQLite de las pruebas | `<raíz>/datos-e2e/movilidad.db` |
| `PUBLICAR_ANTES_DE_PROBAR` | `false` desactiva el `dotnet publish` del fixture (lo usa CI) | publicar |
| `INSTALAR_NAVEGADORES` | `false` desactiva la instalación del navegador | instalar |
| `EMULAR_MOVIL` | `true` activa el descriptor Pixel 7 | `false` |
| `IMAGEN_SDK` | Imagen de `dotnet.sh` | `mcr.microsoft.com/dotnet/sdk:10.0` |
| `IMAGEN_E2E` / `VERSION_PLAYWRIGHT` | Imagen de `pruebas.sh` | `mcr.microsoft.com/playwright:v1.62.1-noble` |
| `NAVEGADOR` | Navegador de `pruebas.sh` (equivale al primer argumento) | `chromium` |
| `ConnectionStrings__BaseDeDatos` | Cadena de conexión de la aplicación | `Data Source=datos/movilidad.db;Default Timeout=30` |
| `ASPNETCORE_URLS` · `ASPNETCORE_ENVIRONMENT` | Los fija el fixture al lanzar el proceso | — |

## Puertos

| Contexto | URL |
| --- | --- |
| `dotnet run` (perfil `http` de `launchSettings.json`) | `http://localhost:5232` |
| Pruebas E2E | `http://127.0.0.1:4173` (o `PUERTO`) |

## Carpetas de trabajo

Todas ignoradas por git; ninguna se indexa.

| Carpeta | Contenido | Quién la crea |
| --- | --- | --- |
| `publicacion/` | Aplicación publicada que ejercitan las pruebas | fixture o `publicar.sh` |
| `datos-e2e/` | `movilidad.db` + `-wal` + `-shm` de las corridas | la aplicación al arrancar |
| `resultados/` | TRX de `dotnet test` | `RunConfiguration/ResultsDirectory` |
| `.nuget/` | Caché de paquetes dentro del árbol | scripts |
| `.dotnet/` | SDK descargado para el contenedor de Playwright | `pruebas.sh` |
| `.navegadores/` | Navegadores de Playwright | `pruebas.sh` |
| `bin/` · `obj/` | Artefactos de compilación | MSBuild |

Además de estas, el `.gitignore` excluye `datos/`, `*.db`, `*.db-wal` y `*.db-shm`.

## Archivos versionados

El repositorio rastrea trece archivos fuera de `src/` y `tests/`:
`.gitignore`, `README.md`, `Lab-E2E.WebBlazor.sln`, `pruebas.runsettings`, los tres `scripts/*.sh`,
`.github/CODEOWNERS` y los cinco workflows.

## Estado de verificación

La sección «Evidencia» del `README.md` del proyecto reporta una corrida del **2026-08-23** con
`sdk:10.0` (SDK 10.0.400) y `playwright:v1.62.1-noble`: build sin advertencias y 22 pruebas en verde
en las cuatro configuraciones.

Esas cifras corresponden al estado **anterior al PR #4**, que agregó tres pruebas. **Durante esta
indexación no se compiló ni se ejecutó nada**: no hay corrida verificada del estado actual
(`e1a39bf`). Tampoco está verificada la ejecución desde el Explorador de pruebas de Visual Studio
—no hay Windows en esta máquina— ni el comportamiento real de los workflows.
