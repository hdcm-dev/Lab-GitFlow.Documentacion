---
doc_id: GF-00
doc_type: indice
title: Estándares de modelo de ramas — guía de estudio
status: vigente
origin: agente
confidence: alta
owner: Lab-GitFlow
last_review: 2026-09-01
audience: [desarrollo, qa, devops, po, autoridad-de-cambio]
traces: [GF-GUIA, GF-AX-WF]
---

# Estándares de modelo de ramas

Cuerpo documental para que un equipo de desarrollo entienda los modelos de ramas, adopte uno con
criterio, y opere el ciclo de vida de sus versiones con pull requests verificados automáticamente.
Se compone de **un documento de estudio único y autocontenido**,
[`Estandares-Modelo-Ramas.md`](Estandares-Modelo-Ramas.md), que incluye sus cinco anexos; los tres
workflows listos para copiar; y dos guías prácticas de ocho escenarios cada una, ejecutables sobre un
repositorio real: la del modelo adoptado y la de GitHub Flow, que sirve de línea de base para
medirlo.

## El problema que le dio origen

El equipo integra pull requests contra una rama estable sin que se dispare ninguna verificación
automatizada, de modo que un cambio puede romper funcionalidad que ya andaba y nadie se entera hasta
que alguien lo prueba a mano. Alrededor de eso aparecen tres huecos más: no está escrito qué entra a
una versión una vez cortada, no está claro quién cierra un issue ni cuándo, y no hay una definición
compartida de qué significa «estable».

La guía responde a los cuatro, en ese orden de importancia.

## Una aclaración necesaria sobre el nombre

El pedido habla de «GitFlow», y conviene ser preciso con el término desde el principio: **GitFlow es
un modelo concreto** —el de Vincent Driessen, 2010— con `master`, `develop` y tres tipos de rama de
soporte, no un sinónimo de «trabajar con ramas».

El propio autor le agregó en 2020 una nota acotando su alcance: fue concebido para software
explícitamente versionado o con varias versiones corriendo en producción, y para un equipo que hace
entrega continua sugiere un flujo más simple. **[F: NVIE-1]** El equipo de esta guía está en el
segundo caso.

La guía toma entonces dos decisiones que conviene tener presentes al leerla:

1. **GitFlow se documenta en serio**, en
   [§4](Estandares-Modelo-Ramas.md#4-gitflow), porque es el vocabulario que el equipo
   va a encontrar en la industria y es el modelo al que habría que migrar si algún día hay que
   soportar dos versiones en paralelo.
2. **El modelo que se adopta es otro** —tronco con ramas de release,
   [§6](Estandares-Modelo-Ramas.md#6-modelo-adoptado)—, y
   [§5](Estandares-Modelo-Ramas.md#5-cómo-elegir-el-modelo) explica el criterio con el que se
   eligió, para que la decisión se pueda revisar cuando cambie el contexto.

Presentar el modelo adoptado como «GitFlow» habría sido cómodo y falso. Por eso esta carpeta se
llama `Estandares-Modelo-Ramas-Guide` y no «Procedimiento GitFlow»: lo que documenta es la elección
entre modelos y el que este equipo sostiene. GitFlow es uno de los comparados, y tiene además su
propia guía práctica al lado.

## Contenido

Todo el estudio vive en [`Estandares-Modelo-Ramas.md`](Estandares-Modelo-Ramas.md). Estas son sus
secciones:

| § | Sección | De qué trata |
|---|---|---|
| 1 | [Marco de referencia](Estandares-Modelo-Ramas.md#1-marco-de-referencia) | Escenarios, contextos y actores: el vocabulario que usa todo lo demás |
| 2 | [Mapa conceptual](Estandares-Modelo-Ramas.md#2-mapa-conceptual) | Entradas por escenario, por rol y por artefacto: «estoy acá → qué aplico» |
| 3 | [Fundamentos de Git](Estandares-Modelo-Ramas.md#3-fundamentos-de-git) | Merge, squash, rebase, cherry-pick y tags, para entender por qué un modelo elige uno u otro |
| 4 | [GitFlow](Estandares-Modelo-Ramas.md#4-gitflow) | El modelo original, sus reglas y la nota de 2020 de su autor |
| 5 | [Cómo elegir el modelo](Estandares-Modelo-Ramas.md#5-cómo-elegir-el-modelo) | GitHub Flow, GitFlow, GitLab Flow y tronco: comparación y criterio de decisión |
| 6 | [Modelo adoptado](Estandares-Modelo-Ramas.md#6-modelo-adoptado) | Las siete reglas, de dónde nace cada rama, guardarraíles y antipatrones |
| 7 | [Integración y versionado](Estandares-Modelo-Ramas.md#7-integración-y-versionado) | Ambientes, artefactos, promoción, versionado semántico, releases y versiones de demostración |
| 8 | [Pull requests y pruebas](Estandares-Modelo-Ramas.md#8-pull-requests-y-pruebas-automatizadas) | Ciclo del pull request, tamaño, protección de rama y qué verifica el pipeline en cada disparador |
| A | [Glosario](Estandares-Modelo-Ramas.md#anexo-a--glosario) | Términos con su definición precisa, y los alias que circulan en el equipo |
| B | [Plantillas](Estandares-Modelo-Ramas.md#anexo-b--plantillas) | Issue, pull request, mensaje de commit y registro de release, comentadas |
| C | [Listas de verificación](Estandares-Modelo-Ramas.md#anexo-c--listas-de-verificación) | Una por momento del proceso, desde abrir un pull request hasta promocionar a producción |
| D | [Preguntas que forman criterio](Estandares-Modelo-Ramas.md#anexo-d--preguntas-que-forman-criterio) | Las quince preguntas que aparecen siempre, con respuesta corta |
| E | [Fuentes](Estandares-Modelo-Ramas.md#anexo-e--fuentes) | Tabla de fuentes y una discusión honesta sobre la fuerza de cada una |

### Fuera del documento único

| Recurso | Contenido |
|---|---|
| [Workflows](Anexos/workflows/README.md) | `ci.yml`, `release.yml` y `auditoria-convergencia.yml`, listos para copiar |
| [Guía práctica de GitFlow](../GitFlow-Practice-Guide/README.md) | Ocho escenarios ejecutables del modelo adoptado, para un equipo de tres personas |
| [Guía práctica de GitHub Flow](../GitHubFlow-Practice-Guide/README.md) | Ocho escenarios del modelo que **no** se adoptó, para medir contra qué se lo comparó |

## Ruta de lectura

Toda ruta empieza por **§1**: es la única sección que define los códigos `E-nn` (escenarios),
`C-n` (contextos) y `A-XXX` (actores) que §3, §6, §7 y §8 usan sin volver a explicarlos. Saltearla
deja tablas enteras escritas en un código irresoluble.

**Quien recién entra al equipo:** §1 → §3 → §6 → §8, y después practicar los escenarios **00, 01 y
03** —en ese orden: el 02 exige una release abierta que solo el 03 crea—. Las secciones §4 y §5 se
pueden dejar para más adelante.

**Quien va a operar releases:** §1 → §7 y los anexos C y de workflows, y después los escenarios 03,
05 y 07.

**Quien tiene que decidir el modelo:** §4 → §5, y la sección de fuerza de la evidencia del anexo E.

**Como capacitación completa:** §1 → §2 → §3 → §4 → §5 → §6 → §7 → §8 → guía práctica en su orden de
ejecución (00 → 01 → 03 → 02 → 04 → 05 → 06 → 07). Los escenarios 00 a 05 llevan una jornada si se
hacen con las esperas reales de revisión.

## Convención de marcas

Cada afirmación de la guía lleva una de estas dos marcas, siguiendo la convención del documento de
insumo del equipo:

| Marca | Significado |
|---|---|
| **[F]** | Fundamentada en una fuente externa verificable, listada en el [Anexo E](Estandares-Modelo-Ramas.md#anexo-e--fuentes) |
| **[C]** | Convención de este equipo. No está respaldada por ningún estándar: es una elección deliberada, discutible y cambiable |

Un documento de proceso pierde autoridad cuando presenta preferencias del autor como si fueran
estándares de la industria. Por eso la separación es explícita en todo el documento.

## Estado de verificación

| Elemento | Estado |
|---|---|
| Contenido conceptual | Fundado en las fuentes del anexo E. Las de acceso pago se citan a través del documento de insumo del equipo, sin lectura directa |
| Modelo adoptado | Toma el flujo propuesto por el equipo en `Flujo-De-Trabajo-Ramas.md` |
| Guía práctica | **Escenario 00 ejecutado** sobre `Lab-GitFlow`: aplicación sembrada, pruebas locales en verde y workflows instalados. Falta la protección de rama, y con ella los escenarios 01 a 07 |
| `ci.yml` del anexo | **Verificado en GitHub Actions** el 2026-08-25, en verde sobre el runner `i7infra-dev`, con la matriz completa de cuatro navegadores en un `push` a `main` |
| `release.yml` y `auditoria-convergencia.yml` | **Sin ejecutar en Actions:** sus disparadores —un tag `v*` y un `push` a `release/**`— llegan con el escenario 03. La lógica de la auditoría sí se comprobó fuera de Actions |

Lo que está verificado se afirma; lo que no, está marcado como tal. Antes de usar la guía como
capacitación conviene ejecutar el escenario 00 completo y confirmar los cuatro puntos de su sección
de verificación.
