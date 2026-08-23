---
doc_id: GF-AX-WF
doc_type: anexo
title: Anexo — workflows del procedimiento
status: vigente
origin: agente
confidence: media
owner: Lab-GitFlow
last_review: 2026-08-23
audience: [devops, desarrollo]
traces: [GF-08, GF-09-00]
---

# Anexo — workflows

Los tres archivos de esta carpeta completan el pipeline para operar el modelo de tronco con ramas de
release. Se copian a `.github/workflows/` del repositorio de práctica en el
[escenario 00](../../09-Guia-Practica/00-Preparacion.md).

| Archivo | Disparadores | Qué hace |
|---|---|---|
| [`ci.yml`](ci.yml) | `pull_request` y `push` a `main` y `release/**`, `merge_group` | Verificación rápida, regresión E2E y check resumen para la protección de rama |
| [`release.yml`](release.yml) | `push` de un tag `v*` | Verifica la referencia etiquetada, construye el artefacto **una vez** y publica la versión |
| [`auditoria-convergencia.yml`](auditoria-convergencia.yml) | diario, `push` a `release/**`, y a pedido | Detecta correcciones de release que nunca volvieron a `main` |

**`ci.yml` reemplaza** al que trae la aplicación sembrada. Aquel solo se dispara sobre `main`; este
protege además las ramas `release/**`, que es precisamente donde el equipo tenía el hueco.

## Lo que estos archivos dan por sentado

- Existe `.github/workflows/e2e.yml`, el workflow reutilizable que define **cómo** se corren las
  pruebas y que viene con la aplicación sembrada. Recibe `navegadores`, `cantidad-shards`,
  `url-base`, `referencia` y `retencion-dias`. **[F: GHA-1]**
- El proyecto .NET está en `src/MovilidadUrbana.Web` y las pruebas en `e2e/`.
- Existe el runner autoalojado con las etiquetas `self-hosted` e `i7infra-dev`. Sobre un runner
  alojado de GitHub alcanza con cambiar el `runs-on:` por `ubuntu-latest`.

## Decisiones que conviene entender antes de copiar

**Ejecución en contenedor.** Los jobs de prueba corren dentro de
`mcr.microsoft.com/playwright:v1.62.1-noble` y los de build dentro de
`mcr.microsoft.com/dotnet/sdk:10.0`. Sobre un runner autoalojado esto no es un lujo: sin contenedor,
la máquina acumula versiones de navegadores y de SDK que nadie recuerda haber instalado, y la corrida
deja de ser reproducible. **[F: PW-1]**

**Un solo check obligatorio.** La protección de rama exige `CI aprobada` y nada más. Listar cada job
obliga a editar la configuración del repositorio cada vez que cambia la matriz, y es la razón por la
que las reglas de protección terminan desactualizadas.

**Permisos mínimos.** `contents: read` salvo en `release.yml`, que necesita `contents: write` para
publicar la versión.

**La auditoría compara por contenido.** `git cherry` es la herramienta correcta porque el SHA cambia
siempre al hacer cherry-pick. Requiere `fetch-depth: 0`: con un clon superficial no hay historia que
comparar.

## Estado de verificación

**No verificado.** Los tres archivos se validaron únicamente como YAML; su comportamiento real en
GitHub Actions no se ejecutó, porque requiere el runner `i7infra-dev` y un repositorio con la
aplicación ya sembrada. Antes de darlos por buenos conviene correr el
[escenario 00](../../09-Guia-Practica/00-Preparacion.md) completo y comprobar los cuatro puntos de su
sección de verificación.
