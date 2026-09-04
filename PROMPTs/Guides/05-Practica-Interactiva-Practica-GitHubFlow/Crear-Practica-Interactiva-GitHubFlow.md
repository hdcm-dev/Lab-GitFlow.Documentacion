# Tool-Prompt — Ejemplo sobre github flow

> **Invocación**:
> - Leer y ejecutar `/LAB/Lab-Documentos.Documentacion/PROMPTs/Guides/05-Estudio-Practica-GitHubFlow/Crear-Estudio-Practica-GitHubFlow.md`
>
> Overview: Crear práctica ejemplo sobre github flow

---

## Contexto

  Leer `/LAB/Lab-Documentos.Documentacion/Guides/GitHubFlow-Practice-Guide/Guia-Practica-GitHubFlow.md`.

  Tengo este repositorio `/LAB/Lab-E2E.WebBlazor.Base` como repositorio de pruebas, ahora estoy así:

```
  fernando@i7infra:~/workspaces/workspace-dev/LAB/Lab-E2E.WebBlazor.Base$ git status
En la rama main
Tu rama está actualizada con 'origin/main'.

Cambios a ser confirmados:
  (usa "git restore --staged <archivo>..." para sacar del área de stage)
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Pages/Error.razor
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Pages/HolaMundo.razor
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Pages/Home.razor
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Pages/NotFound.razor

Cambios no rastreados para el commit:
  (usa "git add/rm <archivo>..." para actualizar a lo que se le va a hacer commit)
  (usa "git restore <archivo>..." para descartar los cambios en el directorio de trabajo)
        modificados:     Ejemplos.WebBlazor.E2E.Base.slnx
        modificados:     README.md
        modificados:     src/WebBlazor.E2E.Base.HolaMundo/Components/App.razor
        modificados:     src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/MainLayout.razor
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/MainLayout.razor.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/NavMenu.razor
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/NavMenu.razor.css
        modificados:     src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/ReconnectModal.razor
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/ReconnectModal.razor.css
        modificados:     src/WebBlazor.E2E.Base.HolaMundo/Components/Routes.razor
        modificados:     src/WebBlazor.E2E.Base.HolaMundo/Components/_Imports.razor
        modificados:     src/WebBlazor.E2E.Base.HolaMundo/Program.cs
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/app.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.js
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.js.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.min.js
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.min.js.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.js
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.js.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.min.js
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.min.js.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.js
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.js.map
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.min.js
        borrados:        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/lib/bootstrap/dist/js/bootstrap.min.js.map
        modificados:     src/WebBlazor.E2E.Base.Login/Components/App.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Layout/DefaultLayout.razor
        modificados:     src/WebBlazor.E2E.Base.Login/Components/Layout/MainLayout.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Layout/MainLayout.razor.css
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Layout/NavMenu.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Layout/NavMenu.razor.css
        modificados:     src/WebBlazor.E2E.Base.Login/Components/Layout/ReconnectModal.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Layout/ReconnectModal.razor.css
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Pages/AuthenticatedHolaMundo.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Pages/Error.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Pages/Home.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Pages/Login.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Pages/Logout.razor
        borrados:        src/WebBlazor.E2E.Base.Login/Components/Pages/NotFound.razor
        modificados:     src/WebBlazor.E2E.Base.Login/Components/Routes.razor
        modificados:     src/WebBlazor.E2E.Base.Login/Components/_Imports.razor
        modificados:     src/WebBlazor.E2E.Base.Login/Program.cs
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/app.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-grid.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-reboot.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap-utilities.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.min.css
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/css/bootstrap.rtl.min.css.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.js
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.js.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.min.js
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.bundle.min.js.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.js
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.js.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.min.js
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.esm.min.js.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.js
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.js.map
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.min.js
        borrados:        src/WebBlazor.E2E.Base.Login/wwwroot/lib/bootstrap/dist/js/bootstrap.min.js.map

Archivos sin seguimiento:
  (usa "git add <archivo>..." para incluirlo a lo que será confirmado)
        Guides/Template-SDD-Aplicado.md
        evidencia/
        src/WebBlazor.E2E.Base.HolaMundo/Components/Componentes/
        src/WebBlazor.E2E.Base.HolaMundo/Components/Layout/BarraLateral.razor
        src/WebBlazor.E2E.Base.HolaMundo/Components/Paginas/
        src/WebBlazor.E2E.Base.HolaMundo/Servicios/
        src/WebBlazor.E2E.Base.HolaMundo/Theme/
        src/WebBlazor.E2E.Base.HolaMundo/wwwroot/css/
        src/WebBlazor.E2E.Base.Login/Components/Componentes/
        src/WebBlazor.E2E.Base.Login/Components/Layout/AccesoLayout.razor
        src/WebBlazor.E2E.Base.Login/Components/Layout/BarraLateral.razor
        src/WebBlazor.E2E.Base.Login/Components/Paginas/
        src/WebBlazor.E2E.Base.Login/Endpoints/
        src/WebBlazor.E2E.Base.Login/Servicios/
        src/WebBlazor.E2E.Base.Login/Theme/
        src/WebBlazor.E2E.Base.Login/wwwroot/css/

fernando@i7infra:~/workspaces/workspace-dev/LAB/Lab-E2E.WebBlazor.Base$ git branch
 -a
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
fernando@i7infra:~/workspaces/workspace-dev/LAB/Lab-E2E.WebBlazor.Base$ 
```

---

## Objetivos

   Poner en práctica conceptos sobre github flow.

---

## Solicitudes

  - Necesito que me ayudes guiandome paso a paso, dando me de a un paso en la que vos me vas diciendo que hacer , yo hago, yo te digo que lo hice y vos seguis diciendome como llevo adelante el github flow de forma manual.

  -  Documentar la experiencia en: `/LAB/Lab-Documentos.Documentacion/PROMPTs/Guides/05-Estudio-Practica-GitHubFlow/OUTPUTs/Experiencia-Ejemplo-GitHubFlow.md`

---

## Reglas
  - Los documentos markdown de documentación deben estar organizados en secciones jerárquicas. Deben incluir definiciones, explicaciones, ejemplos claros y en cuando sea necesario gráficos mermaid, snipped de código. Incluir preguntas formadoras de criterio con respuesta de estas explicando el concepto.
  - No inventar información. 
  - Toda afirmación debe estar respaldada por evidencia verificable.

---

## Framework

## Profile

  Aplicar:
  - `/IA/IA.Prompts/PromptFramework/Profiles/Study-Guide-Documentation.md`
  

