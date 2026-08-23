---
doc_id: GF-09-04
doc_type: escenario-practico
title: 04 — Pull request que rompe la regresión
status: vigente
origin: agente
confidence: alta
owner: Lab-GitFlow
last_review: 2026-08-23
audience: [desarrollo, qa, devops]
traces: [GF-09, GF-08]
---

# 04 — Pull request que rompe la regresión (E-08)

## Objetivo

Provocar deliberadamente el problema que originó esta guía —un cambio que rompe funcionalidad que
andaba— y comprobar que el control lo detiene **antes** del merge. Es el escenario más importante de
la práctica, y el único que se hace rompiendo algo a propósito.

**Roles:** I1 es A-DEV y rompe, I2 es A-REV, I3 es A-QA y observa el reporte.

## Precondición

Escenario 01 terminado: hay pruebas de extremo a extremo que cubren el listado de localidades y el
asistente de encuesta.

## Pasos

### 1. Un cambio plausible que rompe otra cosa (I1)

La clave es que el cambio **parezca razonable**. Un ejemplo que funciona bien sobre esta aplicación:
endurecer la validación del formulario de localidades —por caso, exigir que el código postal tenga
exactamente cuatro dígitos y **rechazar** los que empiezan con cero—. Es una regla defendible, tiene
su propia prueba en verde, y rompe una prueba de otra pantalla que sembraba una localidad con código
postal `0400`.

```bash
git checkout main
git pull --ff-only
git checkout -b fix/151-validar-codigo-postal
# ... cambio + su prueba propia ...
git push -u origin fix/151-validar-codigo-postal
```

### 2. Abrir el pull request y esperar el pipeline

Sin tocar nada más. Lo que sigue es lo que hay que mirar.

### 3. Leer el reporte antes que el código (I3)

En la corrida fallida hay tres artefactos: el reporte HTML, la traza de Playwright y las capturas del
momento de la falla. La traza permite ver el estado del navegador en el paso exacto que falló, sin
reproducir a mano.

### 4. Decidir qué está mal: el cambio o la prueba (los tres)

Es la discusión formativa del escenario, y no tiene respuesta única:

- Si la regla nueva es correcta, la prueba que sembraba `0400` estaba codificando un dato inválido:
  se corrige la prueba y se documenta la regla.
- Si la regla nueva es demasiado estricta, el cambio está mal: se corrige el cambio.

Lo que **no** es una opción es mergear con la regresión en rojo, ni marcar la prueba como salteada
para desbloquear el merge. Una prueba salteada es una regresión que nadie va a mirar.

### 5. Corregir y volver a la cola

Se ajusta lo que corresponda, el pipeline vuelve a correr y recién con todo en verde se mergea.

## Qué observar

- **El botón de merge bloqueado.** Es la diferencia entre un pipeline que informa y un pipeline que
  controla. Sin la protección del escenario 00, esto mismo habría sido un comentario que alguien
  podía ignorar.
- **Qué falló y qué no.** La prueba propia del cambio pasa; la que se rompe es de otra pantalla. Ese
  es exactamente el caso que la revisión humana no detecta leyendo el diff.
- **Cuánto tardó en detectarse.** Comparar con el tiempo que habría tardado en aparecer si el cambio
  se descubría en homologación tres días después.

## Errores frecuentes

| Síntoma | Causa | Corrección |
|---|---|---|
| La prueba falla en el runner pero no localmente | El runner corre la matriz completa; localmente se corrió un solo navegador | Reproducir con el mismo proyecto antes de concluir que es intermitencia |
| Se marca la prueba como salteada para desbloquear | Presión de tiempo | Revertir el salteo; si el cambio es urgente, se revierte el cambio, no el control |
| El pipeline queda en rojo por una intermitencia real | Espera fija o dependencia de orden entre pruebas | Corregir la prueba: una regresión intermitente termina siendo ignorada, y ahí se pierde el control entero |

## Verificación

1. Quedó registro de una corrida en rojo, con reporte y traza descargables.
2. El merge estuvo bloqueado mientras el pipeline estuvo en rojo.
3. La decisión —corregir el cambio o corregir la prueba— está escrita en el pull request, con su
   motivo.
4. Ninguna prueba quedó salteada.

---

Sigue: [05 — Emergencia en producción](05-Emergencia-En-Produccion.md).
