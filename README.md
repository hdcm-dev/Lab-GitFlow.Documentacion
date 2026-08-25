# Lab-GitFlow.Documentacion

Documentación del laboratorio [`Lab-GitFlow`](https://github.com/hdcm-dev/Lab-GitFlow): el
procedimiento de ramas, versionado y pull requests que ese repositorio sirve para practicar.

Este repositorio no tiene código. Guarda el cuerpo documental y los prompts que lo generaron, para
que la guía se pueda revisar, discutir y volver a generar sin depender de la conversación en la que
nació.

## Contenido

| Carpeta | Qué hay |
| --- | --- |
| [Analisis/Estandares-Modelo-Ramas-Guide/](Analisis/Estandares-Modelo-Ramas-Guide/) | Los nueve documentos de estudio, los cinco anexos y los tres workflows de GitHub Actions |
| [Analisis/GitFlow-Practice-Guide/](Analisis/GitFlow-Practice-Guide/) | Ocho escenarios ejecutables del modelo adoptado, sobre `Lab-GitFlow` |
| [Analisis/GitHubFlow-Practice-Guide/](Analisis/GitHubFlow-Practice-Guide/) | Ocho escenarios del mismo tipo para GitHub Flow, el modelo que se comparó y no se adoptó |
| `PROMPTs/` | Los prompts de generación y sus insumos. No versionados en este repositorio |

El punto de entrada es
[Analisis/Estandares-Modelo-Ramas-Guide/README.md](Analisis/Estandares-Modelo-Ramas-Guide/README.md), que
indica por dónde empezar según el rol de quien lee: desarrollo, QA, devops, product owner o
autoridad de cambio.

## Los tres repositorios y cómo se relacionan

La guía se apoya en dos repositorios más, y conviene tener claro qué aporta cada uno:

| Repositorio | Rol |
| --- | --- |
| `Lab-GitFlow.Documentacion` | Este. Explica el procedimiento y provee los workflows |
| [`Lab-GitFlow`](https://github.com/hdcm-dev/Lab-GitFlow) | El repositorio de práctica: se siembra con la aplicación y se ejercitan los escenarios sobre él. Las dos guías prácticas dejan estados incompatibles, así que se corre una a la vez |
| [`Lab-E2E.WebBlazor`](https://github.com/hdcm-dev/Lab-E2E.WebBlazor) | La aplicación bajo prueba, con su suite de extremo a extremo en C# y su definición reutilizable de pruebas en Actions |

Para hacer la práctica, los tres se clonan **como hermanos** bajo un mismo directorio: los comandos
del [escenario 00](Analisis/GitFlow-Practice-Guide/00-Preparacion.md) dan por
sentada esa disposición.

## Cómo se lee la evidencia

Cada afirmación no trivial de la guía lleva una marca que dice de dónde sale. Es lo que permite
discutir una convención del equipo sin discutir de paso un estándar de la industria, y al revés.

| Marca | Significado |
| --- | --- |
| **[F]** | Respaldado por una fuente externa, listada en [Anexos/Fuentes.md](Analisis/Estandares-Modelo-Ramas-Guide/Anexos/Fuentes.md) |
| **[C]** | Convención de este equipo: discutible y cambiable |
| **[E]** | Comprobado ejecutando o leyendo algo en el propio workspace, con fecha |

## Cómo evoluciona

Por pull request contra `main`, con la rama borrada al mergear: el mismo procedimiento que la guía
describe. El historial de cambios está en [CHANGELOG.md](CHANGELOG.md).

Antes de dar por buena una corrección conviene ejecutar lo que se pueda ejecutar. Los dos defectos
más caros encontrados hasta ahora —un workflow que no habría arrancado y una secuencia de pruebas
que dejaba la suite entera en rojo— aparecieron corriendo la guía, no releyéndola.
