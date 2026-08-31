# Changelog

Cambios de la documentación de este repositorio. El formato sigue
[Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/).

Las entradas se agrupan por fecha y no por número de versión: lo que se publica acá es un cuerpo
documental que se lee entero, no un artefacto que alguien instala en una versión determinada. El
versionado semántico que la guía enseña aplica al software que ese procedimiento libera, no a la
guía misma.

## [No publicado]

### Verificado

- `ci.yml` del anexo **corrió en GitHub Actions** por primera vez, en verde, sobre el runner
  `i7infra-dev` y con la aplicación sembrada en `Lab-GitFlow`. Encadenó verificación rápida,
  publicación, la matriz de cuatro configuraciones de navegador, el reporte unificado y el job
  resumen `CI aprobada`. El disparador fue un `push` a `main` y la matriz salió completa, que es lo
  que el procedimiento reserva para lo ya integrado. Con eso caen dos supuestos que hasta ahora se
  sostenían leyendo archivos: que los jobs arrancan sin `container:` sobre ese runner, y que el
  contrato con el `e2e.yml` reutilizable de la aplicación es correcto.
- El estado de verificación del índice y del anexo de workflows deja de decir «validados solo como
  YAML». `release.yml` y `auditoria-convergencia.yml` siguen sin ejecutarse, y ahora está escrito
  por qué: sus disparadores —un tag `v*` y un `push` a `release/**`— llegan recién con el
  escenario 03.

### Agregado

- **`PROMPTs/`** — los tool-prompts con los que se produjo esta documentación, hasta ahora fuera del
  repositorio. Quedan versionados junto a lo que generaron, para que cada documento se pueda leer
  contra el pedido que le dio origen.
  - `01-Guia-Estudio-Modelo-Ramas/Guia-Estudio.md` — encarga la guía de estudio a partir de
    `INPUTs/Flujo-De-Trabajo-Ramas.md`, la propuesta interna del equipo: procedimientos claros de
    pull request, integración y versionado para que un PR sobre una rama estable no rompa lo que
    ya funcionaba.
  - `01-Guia-Estudio-Modelo-Ramas/INPUTs/Flujo-De-Trabajo-Ramas.md` — el documento de entrada, con
    sus afirmaciones separadas entre fundamentadas **[F]** y convenciones del equipo **[C]**.
  - `01-Guia-Estudio-Modelo-Ramas/Mejora-Continuar-Mesa-Evaluadora.md` — el marco de la mesa
    evaluadora: panel compuesto por señales del artefacto, informes independientes, escala de
    evidencia, jurado de cinco, separación entre quien diseña el parche y quien lo aprueba,
    reparación en la capa de origen y criterios de parada.
  - `02-Debate.md/Debate.md` — las preguntas que abrieron el análisis: conventional commits, qué
    otros modelos de ramas existen y qué estándares gobiernan el ciclo de desarrollo.
  - `02-Debate.md/Crear-Guia-GithubFlow.md` — encarga la guía práctica de GitHub Flow como línea de
    base contra la que medir el modelo adoptado.
- Guía práctica de **GitHub Flow** en `Analisis/GitHubFlow-Practice-Guide/`: ocho escenarios con la
  misma estructura que los de GitFlow —objetivo, precondición, pasos, qué observar, errores
  frecuentes y verificación—, sobre el mismo repositorio de práctica. Ejercita el modelo que la
  guía de estudio compara y descarta, porque es la línea de base contra la que se mide cualquier
  otro: corrección hacia adelante sin rama de hotfix, feature flag en lugar de rama larga,
  reversión como plan de contingencia, y vista previa por pull request en lugar de tag de
  demostración. El escenario 07 cierra midiendo, con datos del propio repositorio, si al equipo le
  sirve.

### Cambiado

- Las carpetas de `Analisis/` pasan a `Estandares-Modelo-Ramas-Guide/` y `GitFlow-Practice-Guide/`.
  Se actualizaron las 38 referencias a las rutas anteriores y se enlazó la guía nueva desde el
  README del repositorio y desde el índice de estudio.

### Corregido

- La secuencia de pruebas del escenario 00 dejaba las 22 pruebas en rojo. `scripts/publicar.sh`
  produce un binario autocontenido y el fixture de pruebas publica otra vez por su cuenta,
  dependiente del framework; la segunda publicación se superpone a la primera y el proceso muere
  con código 150 antes de escuchar. El paso pasa a invocar solo `scripts/pruebas.sh`, y queda
  documentada la variante con `PUBLICAR_ANTES_DE_PROBAR=false` para ejercitar el artefacto
  autocontenido. Detectado corriendo el escenario sobre `Lab-GitFlow`.

### Cambiado

- La guía se reorganiza en dos carpetas hermanas: `Analisis/Estandares-Modelo-Ramas-Guide/` con los
  documentos de estudio y los anexos, y `Analisis/GitFlow-Practice-Guide/` con los ocho escenarios. Se van
  los prefijos numéricos de las carpetas, que ordenaban un solo nivel y no decían nada. Los 27
  enlaces relativos que la mudanza rompió quedaron reparados y verificados.
- La carpeta de estudio se llama `Modelo-Ramas` y no `Procedimiento-GitFlow`: lo que ahí se
  documenta es la elección entre modelos de ramas y el que este equipo adopta, que no es GitFlow.
  Los 19 lugares que nombraban la ruta anterior quedaron actualizados.

### Agregado

- Las 28 preguntas guía de los siete documentos de estudio pasan a llevar respuesta corta. No
  cierran el tema: muestran el razonamiento que se espera del lector. Las que interrogan la
  realidad del propio equipo —cuántas versiones vivas hay, quién cierra los issues, qué regla
  cuesta sostener— no la inventan: dicen qué mirar, qué distingue una respuesta fundada de una
  impresión y qué implica cada resultado.
- `README.md` del repositorio y este `CHANGELOG.md`.

## [2026-08-24]

### Corregido

Cinco defectos detectados al verificar la guía contra el estado real de `Lab-E2E.WebBlazor`
([PR #2](https://github.com/hdcm-dev/Lab-GitFlow.Documentacion/pull/2)):

- `release.yml` declaraba `container:` en el job que publica, sobre un runner que no tiene acceso
  al demonio de Docker: el job habría fallado en *Initialize containers* antes del primer paso.
- El escenario 04 ofrecía reporte HTML, trazas y capturas como evidencia de una corrida fallida.
  Con el binding de .NET no existen: la evidencia es el TRX.
- El escenario 07 afirmaba que la auditoría de convergencia nunca lee el mensaje del commit,
  cuando lo lee para excluir los marcados con `Convergencia:`.
- La siembra del escenario 00 copiaba `bin/` y `obj/`: 472 MB medidos contra 524 KB con las
  exclusiones.
- Se describía el `ci.yml` de la aplicación como disparado solo sobre `main`; también se dispara
  sobre `develop`, y sobre `release/*` no se dispara nunca.

### Verificado

- La lógica de `auditoria-convergencia.yml` se ejecutó fuera de GitHub Actions sobre un repositorio
  de prueba: excluye el commit marcado, reporta el huérfano y cierra en `exit 1`; sin divergencias
  da verde. El anexo de workflows pasó de «no verificado» a «parcialmente verificado».

## [2026-08-23]

### Agregado

Primera entrega del procedimiento
([PR #1](https://github.com/hdcm-dev/Lab-GitFlow.Documentacion/pull/1)):

- Nueve documentos de estudio: marco de referencia, mapa conceptual, fundamentos de git, GitFlow,
  criterios para elegir modelo, el modelo adoptado, integración y versionado, pull requests y
  pruebas.
- Guía práctica de ocho escenarios ejecutables sobre `Lab-GitFlow`, para un equipo de tres
  personas que rotan por los roles.
- Cinco anexos: glosario, plantillas, listas de verificación, preguntas frecuentes y fuentes.
- Tres workflows de GitHub Actions —`ci.yml`, `release.yml` y `auditoria-convergencia.yml`— sobre
  el runner autoalojado `i7infra-dev`.
- Revisión completa del cuerpo documental por una mesa evaluadora, aplicada antes de la entrega.
