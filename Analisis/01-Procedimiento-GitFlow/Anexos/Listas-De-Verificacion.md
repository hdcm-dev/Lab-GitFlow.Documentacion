---
doc_id: GF-AX-LV
doc_type: anexo
title: Anexo — listas de verificación
status: vigente
origin: agente
confidence: media
owner: Lab-GitFlow
last_review: 2026-08-23
audience: [desarrollo, qa, devops, autoridad-de-cambio]
traces: [GF-06, GF-07, GF-08]
---

# Listas de verificación

Una lista por momento del proceso. Sirven para lo que sirven las listas: que lo importante no dependa
de acordarse. No reemplazan el criterio; lo liberan para los casos que sí lo requieren.

## Antes de abrir un pull request — A-DEV

- [ ] La rama nació de `main` actualizado y su nombre sigue la convención.
- [ ] El cambio corresponde a **un solo** issue.
- [ ] Hay pruebas que cubren el criterio de aceptación, incluidos el caso vacío y el de error.
- [ ] Si es una corrección: la prueba **fallaba** antes del arreglo.
- [ ] No hay refactores oportunistas mezclados con el cambio.
- [ ] Ninguna configuración depende del ambiente dentro del código.
- [ ] El bloque «cómo probarlo» está escrito para alguien que no participó del desarrollo.

## Al revisar — A-REV

- [ ] El tamaño permite una revisión real; si no, se pide partirlo **[F: GOOG-1]**.
- [ ] El cambio se entiende sin preguntarle a quien lo escribió.
- [ ] Se puede revertir solo, sin arrastrar nada más.
- [ ] Las pruebas verifican comportamiento, no implementación.
- [ ] La respuesta llega dentro del día hábil **[F: GOOG-2]**.

## Antes de cortar una release — A-OPS + A-PO

- [ ] El alcance está definido y escrito.
- [ ] El punto de corte es un commit elegido, no «la punta porque sí».
- [ ] No quedan más de dos releases vivas contando la nueva **[F: TBD-1]**.
- [ ] Los criterios de admisión están escritos antes del primer pedido de cherry-pick.
- [ ] La protección de rama aplica al patrón `release/*`.
- [ ] El plan de pruebas de A-QA existe.

## Antes de un cherry-pick — A-OPS

- [ ] El cambio **ya está en `main`** —no al revés—.
- [ ] Cumple los criterios de admisión de esa release.
- [ ] Se usa `-x` para dejar el rastro del SHA original.
- [ ] Tras el cherry-pick, la verificación completa corre sobre la rama de release.
- [ ] Queda registrado en la tabla de cherry-picks del registro de release.

## Antes de promocionar a producción — A-OPS + A-AUT

- [ ] Es **el mismo artefacto** que aprobó A-QA, no una recompilación.
- [ ] A-QA emitió el reporte de pruebas sobre esa candidata.
- [ ] La autorización está registrada, con su criterio de riesgo.
- [ ] El tag de versión existe y apunta al commit correcto.
- [ ] Está definido cómo se revierte si sale mal.

## Durante una emergencia — A-DEV + A-OPS

- [ ] Se confirmó que califica como emergencia; si no, va por el circuito normal.
- [ ] La rama nació del **tag** de producción, no de la punta de la release.
- [ ] La corrección es la mínima que resuelve el incidente.
- [ ] Hay una prueba que cubre el caso.
- [ ] **El retorno a `main` se hizo el mismo día.**
- [ ] Se agendó la revisión posterior a la implementación **[F: ITIL-1]**.

## Semanal — todo el equipo

- [ ] Ninguna rama corta tiene más de una semana de vida **[C]**.
- [ ] La auditoría de convergencia pasó en verde.
- [ ] No hay ramas de release en desuso sin borrar.
- [ ] No hay pruebas salteadas para desbloquear un merge.
