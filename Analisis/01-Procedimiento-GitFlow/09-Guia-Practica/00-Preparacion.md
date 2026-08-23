---
doc_id: GF-09-00
doc_type: escenario-practico
title: 00 — Preparación del repositorio de práctica
status: vigente
origin: agente
confidence: media
owner: Lab-GitFlow
last_review: 2026-08-23
audience: [desarrollo, devops]
traces: [GF-09, GF-08]
---

# 00 — Preparación

## Objetivo

Dejar el repositorio de práctica con una aplicación real, pruebas automatizadas que corren, y los
controles que el resto de los escenarios va a ejercitar. Sin esto, los escenarios siguientes son
teatro: no hay nada que se pueda romper ni ninguna verificación que lo detecte.

## Precondición

- `Lab-GitFlow` existe y tiene solo el commit inicial.
- `Lab-E2E.WebBlazor` está disponible localmente como fuente de la aplicación bajo prueba.
- Docker instalado.

## Pasos

### 1. Sembrar la aplicación

La aplicación no se escribe para esta práctica: se toma la de `Lab-E2E.WebBlazor`, que ya tiene la
solución .NET, las pruebas de extremo a extremo con Playwright y los scripts de contenedor.

```bash
cd Lab-GitFlow
git checkout -b chore/1-sembrar-aplicacion

# Copiar la aplicación y sus pruebas desde el laboratorio de E2E.
rsync -a --exclude .git --exclude node_modules --exclude publicacion \
      --exclude test-results --exclude playwright-report --exclude .nuget \
      ../Lab-E2E.WebBlazor/ ./

git add -A
git commit -m "chore: sembrar la aplicación de práctica y sus pruebas E2E"
git push -u origin chore/1-sembrar-aplicacion
```

Se integra por pull request, no por push directo: es la primera oportunidad de ver el circuito
completo antes de que haya protección que lo obligue.

### 2. Comprobar que las pruebas corren localmente

```bash
scripts/publicar.sh                       # publica el binario autocontenido
scripts/e2e.sh npm ci
scripts/e2e.sh npx playwright test --project=chromium
```

Si esto no pasa en verde en la máquina de cada integrante, no tiene sentido seguir: los escenarios
siguientes distinguen «la prueba falla porque el cambio está mal» de «la prueba falla porque el
entorno está mal», y esa distinción requiere una línea base verde.

### 3. Instalar los workflows del procedimiento

Los del laboratorio de E2E cubren el pull request y la línea principal. Faltan los que el
procedimiento de release necesita: verificación de las ramas `release/*`, corte de versión y
auditoría de convergencia. Están en [../Anexos/workflows/](../Anexos/workflows/README.md). El `ci.yml` de esa carpeta
**reemplaza** al que vino con la aplicación: aquel solo se dispara sobre `main`.

```bash
git checkout -b chore/2-workflows-de-gitflow
cp ../Lab-GitFlow.Documentacion/Analisis/01-Procedimiento-GitFlow/Anexos/workflows/*.yml \
   .github/workflows/
git add .github/workflows
git commit -m "chore: agregar los workflows de release y auditoría de convergencia"
git push -u origin chore/2-workflows-de-gitflow
```

### 4. Configurar la protección de rama

En *Settings → Branches* del repositorio, sobre `main` y sobre el patrón `release/*`:

| Control | Valor |
|---|---|
| Require a pull request before merging | sí, con 1 aprobación |
| Require status checks to pass | sí, check obligatorio: `CI aprobada` |
| Require branches to be up to date | sí |
| Do not allow bypassing | sí, incluidos administradores |
| Automatically delete head branches | sí (*Settings → General*) |

Se exige **un solo check** —el job resumen— y no la lista completa de jobs: así la regla no hay que
tocarla cada vez que cambia la matriz de navegadores.

### 5. Declarar dueños de los archivos sensibles

```
# .github/CODEOWNERS
.github/workflows/   @equipo/devops
src/**/Persistencia/ @equipo/datos
```

Son los dos lugares donde un error no se arregla con un revert: el pipeline y las migraciones de
datos.

## Qué observar

- El primer pull request corre el pipeline **antes** de que exista la protección: ver la diferencia
  entre «el pipeline informa» y «el pipeline bloquea» es el punto del escenario 04.
- Cuánto tarda la verificación rápida frente a la matriz completa. Esa diferencia es la que justifica
  separarlas.
- Que el reporte de las pruebas quede como artefacto de la corrida, y por cuántos días.

## Errores frecuentes

| Síntoma | Causa habitual |
|---|---|
| Los jobs quedan en cola para siempre | El runner autoalojado no tiene la etiqueta `i7infra-dev`, o está apagado |
| Las pruebas pasan localmente y fallan en el runner | La aplicación no se publicó antes de correr; el artefacto no llegó al job |
| El check obligatorio nunca aparece en la lista | El nombre configurado no coincide **exactamente** con el `name:` del job |

## Verificación

El escenario está resuelto cuando se cumplen las cuatro condiciones:

1. `git ls-remote --heads origin` muestra solo `main`.
2. Un push directo a `main` es rechazado por el servidor.
3. Un pull request de prueba dispara la verificación rápida y la regresión, y el botón de merge queda
   bloqueado hasta que terminan.
4. La corrida deja el reporte de pruebas como artefacto descargable.

---

Sigue: [01 — Funcionalidad nueva](01-Funcionalidad-Nueva.md).
