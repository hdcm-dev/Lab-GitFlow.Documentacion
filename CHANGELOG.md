# Changelog

Cambios de la documentación de este repositorio. El formato sigue
[Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/).

Las entradas se agrupan por fecha y no por número de versión: lo que se publica acá es un cuerpo
documental que se lee entero, no un artefacto que alguien instala en una versión determinada. El
versionado semántico que la guía enseña aplica al software que ese procedimiento libera, no a la
guía misma.

## [No publicado]

### Cambiado

- Los ocho documentos de estudio de `Guides/Estandares-Modelo-Ramas-Guide/` —`01-Marco-De-Referencia.md`
  a `08-Pull-Requests-Y-Pruebas.md`— y los cinco anexos de `Anexos/` —glosario, plantillas, listas de
  verificación, preguntas frecuentes y fuentes— se unifican en un solo documento autocontenido,
  `Estandares-Modelo-Ramas.md`, con tabla de contenido, una sección numerada por documento y los
  anexos como secciones A a E. El texto es el mismo: lo que cambia es que las referencias cruzadas
  pasan a ser anclas internas, desaparecen las líneas de navegación «Sigue: …» y los encabezados
  bajan un nivel. La convención de marcas **[F]** / **[C]**, que antes vivía solo en el índice y en
  la apertura de `06-Modelo-Adoptado.md`, encabeza ahora el documento único. El `README.md` de la
  carpeta queda como presentación —problema de origen, aclaración sobre el nombre, índice de
  secciones, rutas de lectura y estado de verificación— y apunta a las anclas del documento único.
  Los enlaces externos que apuntaban a los archivos borrados —índice raíz del repositorio, las dos
  guías prácticas y la guía de GitHub Actions— se reescribieron a esas anclas. Fuera del documento
  quedan `Anexos/workflows/` con sus tres `.yml` y su README, que describen archivos ejecutables y
  no material de estudio. La consolidación no volvió a ejecutar ni a verificar nada: los estados de
  verificación son los que ya estaban registrados.

- Los ocho escenarios de `Guides/GitHubFlow-Practice-Guide/` —`00-Preparacion.md` a
  `07-Cierre-Y-Auditoria.md`— se unifican en un solo documento autocontenido,
  `Guia-Practica-GitHubFlow.md`, con tabla de contenido y una sección por escenario, con la misma
  estructura que la guía práctica de GitFlow. El texto es el mismo: lo que cambia es que las
  referencias cruzadas entre escenarios pasan a ser anclas internas y desaparecen las líneas de
  navegación «Sigue: …», que solo tenían sentido con los documentos separados. El documento suma dos
  cosas para poder leerse solo: la convención de marcas **[F]** / **[C]** / **[E]** y un anexo con
  las cuatro fuentes que sus marcas **[F]** citan —GH-1, GOOG-1, GOOG-2 y DORA-1—, copiadas del
  [anexo de fuentes](Guides/Estandares-Modelo-Ramas-Guide/Estandares-Modelo-Ramas.md#anexo-e--fuentes) con el estado de
  verificación que allí está registrado. Un anexo final mapea cada `doc_id` viejo a su sección. El
  `README.md` de la carpeta queda como presentación —modelo en una página, rotación de roles,
  estructura de un escenario, orden de ejecución y requisitos— y apunta a las anclas del documento
  único. La consolidación no volvió a ejecutar ni a verificar nada: las fechas de las marcas **[E]**
  son las que ya estaban registradas.

- Los ocho escenarios de `Guides/GitFlow-Practice-Guide/` —`00-Preparacion.md` a
  `07-Cierre-Y-Auditoria.md`— se unifican en un solo documento autocontenido,
  `Guia-Practica-GitFlow.md`, con tabla de contenido y una sección por escenario. El texto es el
  mismo: lo que cambia es que las referencias cruzadas entre escenarios pasan a ser anclas internas
  y desaparecen las líneas de navegación «Sigue: …», que solo tenían sentido con los documentos
  separados. El `README.md` de la carpeta queda como índice —rotación de roles, estructura de un
  escenario, orden de ejecución y requisitos— y apunta a las anclas del documento único. Los enlaces
  externos que apuntaban a los archivos borrados (índice raíz, `02-Mapa-Conceptual.md`,
  `06-Modelo-Adoptado.md`, `07-Integracion-Y-Versionado.md` y el anexo de workflows) se
  reescribieron a las anclas nuevas.

- La carpeta `Analisis/` pasa a llamarse **`Guides/`**. El nombre viejo nombraba la actividad que
  produjo los documentos —el análisis del modelo de ramas—, no lo que la carpeta guarda: guías de
  estudio y de práctica. Con la guía de GitHub Actions y la de pruebas E2E adentro, «análisis» ya
  no describía el contenido. Se renombran las carpetas de guías y se reescriben los enlaces que las
  apuntaban desde el `README.md` raíz y desde este `CHANGELOG.md`. El texto de las guías no cambia.

- `PROMPTs/` se reorganiza por lo que cada prompt produjo, en vez de por número de orden:
  `Analisis/` —los que generaron el cuerpo documental del modelo de ramas, que antes colgaban
  directo de `PROMPTs/`—, `Fixs/` —los pedidos de corrección sobre documentos ya escritos—,
  `Guides/` —el que encargó la guía de estudio de E2E— e `Indexado/` —los que crean y actualizan la
  ia-db—. La numeración vieja no dejaba lugar para un prompt que no fuera de análisis.

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

- La guía pasó por tres revisiones independientes antes de publicarse: bibliográfica —cada afirmación
  **[F]** contra su fuente—, de evidencia —cada bloque de código y cada cifra contra el archivo del
  workspace— y editorial contra el marco normativo. Salieron 39 correcciones, y las que más importan
  son de la misma familia: afirmaciones ciertas de los cinco repositorios de referencia enunciadas
  como si valieran para todo el workspace. `GDA.Core.APP` sí firma con certificado de distribución y
  publica a TestFlight, sí firma APK con almacén de claves desde secretos, y seis workflows bajo
  `Repos-Docker/` sí anclan acciones por SHA. Las tres quedaron acotadas a su alcance real.
- La revisión bibliográfica corrigió además tres afirmaciones sobre el comportamiento de la
  plataforma que la guía daba por buenas y sus propias fuentes desmienten: el `GITHUB_TOKEN` se emite
  por job y no por corrida; `concurrency` por omisión cancela la corrida encolada, no la que está
  corriendo; y un bloque `run` sí corta ante un comando suelto que falla, porque el shell por defecto
  ya trae `-e` —lo que se pierde sin `set -euo pipefail` es el fallo de un tramo intermedio de una
  tubería y la variable sin definir—.
- Las veinte URL de la documentación de GitHub se registran con su dirección **efectiva**: todas
  responden 200 desde la ruta vieja, pero redirigen, y en dos casos la página de destino ya no
  contenía la afirmación citada.

### Agregado
- **`ia-db/`** — base de conocimiento indexada del repositorio de práctica
  [`Lab-Documentos`](https://github.com/hdcm-dev/Lab-Documentos), pensada para que un agente ubique
  un tema y cargue solo el índice que corresponde en vez de recorrer el repositorio entero. Un
  [README](ia-db/README.md) hace de punto de entrada único —tabla de navegación, resumen ejecutivo,
  estructura del repositorio indexado y las restricciones de uso— y ocho índices cubren
  arquitectura, dominio y aplicación, persistencia y sesiones, presentación Blazor, pruebas E2E,
  integración continua, entorno y ejecución, y decisiones y glosario. El índice maestro registra
  tres divergencias entre el `README.md` de `Lab-Documentos` y el estado real del código, de modo
  que quien consulte la base no dé por vigente lo que ese README afirma. Está tomada de `main` en
  `e1a39bf`: es una foto con fecha, no un espejo que se actualice solo.

- **`Guides/E2E-Guide/`** — dos documentos sobre pruebas de extremo a extremo en .NET con
  Playwright, traídos desde `Lab-E2E.WebBlazor.Documentacion` para que vivan junto al resto del
  cuerpo documental. `Beginner-Guide.md` es la guía de estudio para quien nunca escribió una prueba
  E2E: anatomía del proyecto, criterios sobre qué se prueba y qué no, cómo se organiza la suite y
  cómo se enganchan esas pruebas en un workflow de GitHub Actions como control de integración del
  pull request. `Quick-Guide-ABM.md` es el camino corto para quien ya sabe: qué se copia tal cual,
  qué se decide en cada ABM y las trampas propias de Blazor con render *interactive server*, sobre
  el ABM de localidades que tiene nueve casos en verde.

- Tres tool-prompts nuevos en `PROMPTs/`, versionados junto a lo que generaron:
  `Analisis/03-GitHub-Action/Guia-GitHub-Action-Estudio.md`, que encarga la guía de GitHub Actions
  tomando como material los workflows de cinco repositorios del workspace;
  `Guides/01-Crear-Developer-Guide.md`, que encarga la guía de estudio de E2E sobre
  `Lab-E2E.WebBlazor`; y los dos de `Indexado/`, que crean y actualizan la ia-db.

- Cuatro tool-prompts de corrección en `PROMPTs/Fixs/`, que son el pedido detrás de las
  consolidaciones registradas más arriba: el primero sobre las preguntas guía de la guía de estudio
  de E2E, y los otros tres pidiendo que `GitFlow-Practice-Guide`, `GitHubFlow-Practice-Guide` y
  `Estandares-Modelo-Ramas-Guide` queden en un solo documento autocontenido, no delta, con tabla de
  contenido y con el README como única pieza aparte. Quedan versionados para poder leer cada
  documento contra el pedido que lo dejó como está.


- Guía de estudio de **GitHub Actions** en `Guides/GitHub-Action-Guide/GitHub-Action-Guide.md`. El cuerpo documental
  explicaba hasta ahora *qué* tiene que verificar el pipeline y daba tres workflows para copiar,
  pero no *cómo* se escribe uno: quien tenía que tocar el `ci.yml` del anexo se quedaba sin la capa
  de abajo. La guía la cubre en un documento autocontenido con marco conceptual (CI, entrega y
  despliegue continuo, stage, puerta), la sintaxis recorrida sección por sección, diez escenarios
  completos —verificación de un pull request, de la línea principal, puertas de calidad, publicación
  de NuGet, publicación por FTP, imagen de contenedor, construcción móvil, corte de versión,
  verificación de un entorno desplegado y regresión programada—, y seis anexos: glosario,
  plantillas, listas de verificación, quince preguntas que forman criterio, fuentes y catálogo de
  evidencia.
- La guía **no usa YAML de ejemplo inventado**: los doce workflows que cita corren en repositorios de
  este workspace, y cada uno aparece en el catálogo de evidencia con su ruta y con lo que aporta.
  Donde no hay implementación propia —construcción de contenedores en los cinco repositorios que el
  prompt tomó como referencia, firma de artefactos, atestaciones de procedencia— está dicho, y el
  ejemplo se trae de otro repositorio del workspace o se declara ausente.
- El anexo de evidencia deja registradas nueve observaciones que salieron al reunir el material y que
  hereda quien copie esos workflows como plantilla: dieciocho pipelines de iOS casi idénticos, un
  Xcode que se descarga de Google Drive, un workflow que apunta a un proyecto que ya no existe, y
  ninguna acción anclada a un SHA en los doce workflows citados.
- El anexo de evidencia numera sus once observaciones como `OBS-n` para que las afirmaciones
  negativas de la guía —«no hay firma», «no hay notificación», «no hay despliegue continuo»— se
  puedan citar y discutir por separado, en vez de quedar como una marca **[E]** sin referente.
- Las fuentes de la plataforma se identifican con el prefijo **`GHDOC-`** y no `GHA-`: ese espacio
  ya estaba ocupado en [Anexo E — Fuentes](Guides/Estandares-Modelo-Ramas-Guide/Estandares-Modelo-Ramas.md#anexo-e--fuentes),
  donde `GHA-1` nombra *Reusing workflows*. Dentro del mismo cuerpo documental un ID resuelve a una
  sola fuente.

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
- Guía práctica de **GitHub Flow** en `Guides/GitHubFlow-Practice-Guide/`: ocho escenarios con la
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
