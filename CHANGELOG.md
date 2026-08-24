# Changelog

Cambios de la documentación de este repositorio. El formato sigue
[Keep a Changelog](https://keepachangelog.com/es-ES/1.1.0/).

Las entradas se agrupan por fecha y no por número de versión: lo que se publica acá es un cuerpo
documental que se lee entero, no un artefacto que alguien instala en una versión determinada. El
versionado semántico que la guía enseña aplica al software que ese procedimiento libera, no a la
guía misma.

## [No publicado]

### Corregido

- La secuencia de pruebas del escenario 00 dejaba las 22 pruebas en rojo. `scripts/publicar.sh`
  produce un binario autocontenido y el fixture de pruebas publica otra vez por su cuenta,
  dependiente del framework; la segunda publicación se superpone a la primera y el proceso muere
  con código 150 antes de escuchar. El paso pasa a invocar solo `scripts/pruebas.sh`, y queda
  documentada la variante con `PUBLICAR_ANTES_DE_PROBAR=false` para ejercitar el artefacto
  autocontenido. Detectado corriendo el escenario sobre `Lab-GitFlow`.

### Cambiado

- La guía se reorganiza en dos carpetas hermanas: `Analisis/Modelo-Ramas/` con los
  documentos de estudio y los anexos, y `Analisis/Guia-Practica/` con los ocho escenarios. Se van
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
