# Lab-GitFlow.Documentacion

Documentación del laboratorio [`Lab-GitFlow`](https://github.com/hdcm-dev/Lab-GitFlow): el
procedimiento de ramas, versionado y pull requests que ese repositorio sirve para practicar.

Este repositorio no tiene código. Guarda el cuerpo documental y los prompts que lo generaron, para
que la guía se pueda revisar, discutir y volver a generar sin depender de la conversación en la que
nació.

## Contenido

| Carpeta | Qué hay |
| --- | --- |
| [Guides/Estandares-Modelo-Ramas-Guide/](Guides/Estandares-Modelo-Ramas-Guide/) | El documento único de estudio —`Estandares-Modelo-Ramas.md`, con sus cinco anexos adentro— y los tres workflows de GitHub Actions |
| [Guides/GitFlow-Practice-Guide/](Guides/GitFlow-Practice-Guide/) | Ocho escenarios ejecutables del modelo adoptado, sobre `Lab-GitFlow` |
| [Guides/GitHubFlow-Practice-Guide/](Guides/GitHubFlow-Practice-Guide/) | Ocho escenarios del mismo tipo para GitHub Flow, el modelo que se comparó y no se adoptó |
| [Guides/GitHub-Action-Guide/](Guides/GitHub-Action-Guide/) | Guía de estudio de GitHub Actions: la herramienta con la que se implementan los workflows que el procedimiento exige |
| [Guides/E2E-Guide/](Guides/E2E-Guide/) | Pruebas de extremo a extremo en .NET con Playwright: guía de estudio para quien empieza y guía rápida para montar el E2E de un ABM |
| [ia-db/](ia-db/) | Base de conocimiento indexada de `Lab-Documentos`, para consultar el repositorio de práctica sin recorrerlo entero |
| [PROMPTs/](PROMPTs/) | Los prompts de generación y sus insumos, versionados junto a lo que produjeron |

El punto de entrada es
[Guides/Estandares-Modelo-Ramas-Guide/README.md](Guides/Estandares-Modelo-Ramas-Guide/README.md), que
indica por dónde empezar según el rol de quien lee: desarrollo, QA, devops, product owner o
autoridad de cambio.

Las tres primeras carpetas responden **qué procedimiento sigue el equipo**; la de GitHub Actions
responde **con qué se implementa**. Quien tenga que escribir o corregir un workflow —el `ci.yml` del
anexo de la guía de estudio, sin ir más lejos— encuentra ahí la sintaxis explicada sección por
sección y diez escenarios completos, todos con ejemplos tomados de workflows que corren en este
workspace.

## Los tres repositorios y cómo se relacionan

La guía se apoya en dos repositorios más, y conviene tener claro qué aporta cada uno:

| Repositorio | Rol |
| --- | --- |
| `Lab-GitFlow.Documentacion` | Este. Explica el procedimiento y provee los workflows |
| [`Lab-GitFlow`](https://github.com/hdcm-dev/Lab-GitFlow) | El repositorio de práctica: se siembra con la aplicación y se ejercitan los escenarios sobre él. Las dos guías prácticas dejan estados incompatibles, así que se corre una a la vez |
| [`Lab-E2E.WebBlazor`](https://github.com/hdcm-dev/Lab-E2E.WebBlazor) | La aplicación bajo prueba, con su suite de extremo a extremo en C# y su definición reutilizable de pruebas en Actions |

Para hacer la práctica, los tres se clonan **como hermanos** bajo un mismo directorio: los comandos
del [escenario 00](Guides/GitFlow-Practice-Guide/Guia-Practica-GitFlow.md#2-escenario-00--preparación) dan por
sentada esa disposición.

## Cómo se lee la evidencia

Cada afirmación no trivial de la guía lleva una marca que dice de dónde sale. Es lo que permite
discutir una convención del equipo sin discutir de paso un estándar de la industria, y al revés.

| Marca | Significado |
| --- | --- |
| **[F]** | Respaldado por una fuente externa, listada en [Anexo E — Fuentes](Guides/Estandares-Modelo-Ramas-Guide/Estandares-Modelo-Ramas.md#anexo-e--fuentes) |
| **[C]** | Convención de este equipo: discutible y cambiable |
| **[E]** | Comprobado ejecutando o leyendo algo en el propio workspace, con fecha |

## Cómo evoluciona

Por pull request contra `main`, con la rama borrada al mergear: el mismo procedimiento que la guía
describe. El historial de cambios está en [CHANGELOG.md](CHANGELOG.md).

Antes de dar por buena una corrección conviene ejecutar lo que se pueda ejecutar. Los dos defectos
más caros encontrados hasta ahora —un workflow que no habría arrancado y una secuencia de pruebas
que dejaba la suite entera en rojo— aparecieron corriendo la guía, no releyéndola.
