# ia-db — Lab-Documentos

> **Instrucción para IA**: este archivo es el **punto de entrada único** a la base de conocimiento
> del repositorio [`Lab-Documentos`](https://github.com/hdcm-dev/Lab-Documentos). Leelo primero,
> ubicá el tema en la tabla de navegación y cargá **solo** el índice o los dos índices que
> correspondan. Ampliá a los archivos fuente únicamente cuando el índice resulte insuficiente:
> cada índice referencia sus fuentes con ruta exacta. No recorras el repositorio completo.

## Navegación

| Necesitás saber… | Leé este índice |
| --- | --- |
| Qué es el proyecto, con qué stack, qué decisiones lo definen | [00_MASTER-INDEX.md](indexes/00_MASTER-INDEX.md) |
| Cómo están separadas las capas y hacia dónde apuntan las dependencias | [01_Arquitectura.md](indexes/01_Arquitectura.md) |
| Entidades, reglas de validación, catálogos y casos de uso | [02_Dominio-Y-Aplicacion.md](indexes/02_Dominio-Y-Aplicacion.md) |
| Base SQLite, repositorios, siembra y aislamiento por sesión | [03_Persistencia-Y-Sesiones.md](indexes/03_Persistencia-Y-Sesiones.md) |
| Pantallas Blazor, componentes, `data-testid` y rutas | [04_Presentacion-Blazor.md](indexes/04_Presentacion-Blazor.md) |
| Suite E2E con Playwright: fixtures, paralelismo, casos cubiertos | [05_Pruebas-E2E.md](indexes/05_Pruebas-E2E.md) |
| Workflows de GitHub Actions: CI, E2E reutilizable, release, auditoría | [06_Integracion-Continua.md](indexes/06_Integracion-Continua.md) |
| Cómo compilar, publicar y correr las pruebas; variables de entorno | [07_Entorno-Y-Ejecucion.md](indexes/07_Entorno-Y-Ejecucion.md) |
| Por qué se decidió cada cosa; vocabulario del proyecto | [08_Decisiones-Y-Glosario.md](indexes/08_Decisiones-Y-Glosario.md) |

## Resumen ejecutivo

| Dato | Valor |
| --- | --- |
| Proyecto indexado | `Lab-Documentos` (`/LAB/Lab-Documentos`) |
| Repositorio | `https://github.com/hdcm-dev/Lab-Documentos` · rama `main` |
| Tipo | Laboratorio de práctica: aplicación web + suite E2E + workflows de GitHub Actions |
| Stack | .NET 10 · Blazor Web App (*interactive server*) · EF Core 10.0.11 sobre SQLite · Playwright 1.62 con NUnit 4 · Bootstrap 5.3.8 vendorizado |
| Solución | `Lab-E2E.WebBlazor.sln` — dos proyectos: `src/MovilidadUrbana.Web`, `tests/MovilidadUrbana.E2ETests` |
| Documentación asociada | `Lab-Documentos.Documentacion` (este repositorio) |

**Función principal.** Es el repositorio de práctica sobre el que se ejercitan los escenarios del
modelo de ramas documentado en `Lab-Documentos.Documentacion`. Está sembrado con la aplicación
*Movilidad Urbana* —un ABM de localidades y una encuesta de transporte en tres pasos— y con su
suite de pruebas de extremo a extremo, para que cada escenario de ramas tenga código real que
compilar, probar y liberar.

**Arquitectura en una línea.** Un único proyecto web con las capas de Clean Architecture separadas
en carpetas (`Dominio` → `Aplicacion` → `Infraestructura` / `Components`, con `Program.cs` como
único punto de composición), persistido en SQLite y aislado por una cookie de sesión que permite
correr las pruebas E2E en paralelo contra una sola instancia.

## Estructura del repositorio indexado

```
Lab-Documentos/
├── Lab-E2E.WebBlazor.sln        Solución: 2 proyectos + 3 carpetas de solución
├── README.md                    Documento del repositorio (ver divergencias en 00_MASTER-INDEX)
├── pruebas.runsettings          Navegador, timeouts y cantidad de workers de NUnit
├── src/MovilidadUrbana.Web/     Aplicación Blazor por capas
├── tests/MovilidadUrbana.E2ETests/  Suite Playwright + NUnit (25 casos)
├── scripts/                     dotnet.sh · publicar.sh · pruebas.sh (todo por contenedor)
└── .github/
    ├── CODEOWNERS               Dueños de workflows y de la capa de persistencia
    └── workflows/               ci · e2e · release · auditoria-convergencia · verificacion-entorno
```

Carpetas presentes en el árbol de trabajo pero **no versionadas** ni indexadas (`.gitignore`):
`publicacion/`, `datos-e2e/`, `.nuget/`, `.dotnet/`, `.navegadores/`, `resultados/`, `bin/`, `obj/`.

## Restricciones para IA

- **No modificar `Lab-Documentos` desde esta base.** La ia-db documenta; no autoriza cambios.
- **No presentar el índice como fuente de verdad del código.** Ante contradicción prevalece el
  archivo fuente: corregir el índice, no el razonamiento sobre el código.
- **No dar por vigente lo que el `README.md` del proyecto afirma** sin contrastarlo: el propio
  índice registra tres divergencias entre ese README y el estado real
  (ver [00_MASTER-INDEX.md](indexes/00_MASTER-INDEX.md#divergencias-detectadas)).
- **No indexar ni escanear** `bin/`, `obj/`, `.nuget/`, `.dotnet/`, `.navegadores/`, `publicacion/`,
  `datos-e2e/` ni la propia `ia-db`.
- **No hacer commit, push ni pull request** como parte de una consulta a esta base.

## Manifiesto de generación

- Generado por : `/IA/PROMPTs/IA.Prompts/Tool-Prompts/Indexado-Documentado/Iniciar-Indexado.md`
- Invocado por : `/LAB/Lab-Documentos.Documentacion/PROMPTs/Indexado/Crear-Indexado.md`
- Perfil       : `/IA/PROMPTs/IA.Prompts/PromptFramework/Profiles/Knowledge-Indexing.md`
- Alcance      : `/LAB/Lab-Documentos` (modo proyecto, destino explícito fuera del proyecto)
- Fuentes      : `README.md`, `Lab-E2E.WebBlazor.sln`, `pruebas.runsettings`, `.gitignore`,
  `src/MovilidadUrbana.Web/**` (sin `bin`/`obj`), `tests/MovilidadUrbana.E2ETests/**` (sin `bin`/`obj`),
  `scripts/**`, `.github/**`
- Estado del repositorio : `main` en `e1a39bf` («Merge pull request #4 … filtro-por-provincia»)
- Generado     : 2026-09-01 · Versión: 1.0
- Actualizar   : `/IA/PROMPTs/IA.Prompts/Tool-Prompts/Indexado-Documentado/Actualizar-Indexado.md`
