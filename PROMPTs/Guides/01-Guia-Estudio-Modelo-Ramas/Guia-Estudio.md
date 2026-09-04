# Tool-Prompt — GitFlow

> **Invocación**:
> - Leer y ejecutar `/LAB/Lab-Documentos.Documentacion/PROMPTs/Guides/01-Guia-Estudio-Modelo-Ramas/Guia-Estudio.md`
>
> Overview: GitFlow - métodologías de desarrollo y guía de entrenamiento

---

## Contexto

  Leer `/LAB/Lab-GitFlow.Documentacion/PROMPTs/01-Guia-Estudio-Modelo-Ramas/INPUTs/Flujo-De-Trabajo-Ramas.md`, trata de establecer un flujo de trabajo con repositorios git en un entorno de trabajo.

  Actualmente el equipo tiene problemas al introducir un PR sobre una rama estable , al no dispararse pruebas automatizadas puede romper funcionaldiades que ya funcionaban correctamente. La necesidad pasa por tener prodicimientos claros en los PR y en los conceptos de integración  y versionado del producto

---

## Objetivos

   Se busca establecer un procedimiento acorde lo que se usa en la industria y academia dentro del GitFlow, contemplando el ciclo de vida del software y su versionado, procedimientos y alcance en la interversión de las diferentes especialidades dentro del ciclo de desarrollo (QR, Devlopers, Devopts, entre otros)

---

## Solicitudes

  - Se requiere una guía que explique claramente en que consiste gitflow, con ejemplos claros y definiciones enfocado a un desarrollador sin experiencia.

  - Se requiere una guía de práctica. Se puede escenificar una guía práctica de pruebas basandose en este: `/LAB/Lab-E2E.WebBlazor` para construir una guía en el que el lector prueda ir siguiendo y probando cada escenerios. El lector podría seguir paso a paso esa guía en este repositorio `/LAB/Lab-GitFlow`. Esta guía debe capturar la mayoría de los escenerios que se pueden dar en un equipo de desarrollo de 3 desarrolladores , donde hay fix, hotfix, releases, versiones de demostración, pruebas automatizadas, PR con pruebas automatizadas de prueba. Esta guía debería servir como guía de capacitación. Cabe la necesidad de reevaluar este punto pensando en los otros actores del equipo de desarrollo como QAs , entre otros.

  - Configura el wofklow con este runner de github :  `runs-on: [self-hosted, i7infra-dev]`

  - Toda la documentación generarla en `LAB/Lab-GitFlow.Documentacion/Analisis/Procedimiento-GitFlow`

---

## Reglas
  - Los documentos markdown de documentación deben estar organizados en secciones jerárquicas. Deben incluir definiciones, explicaciones, ejemplos claros y en cuando sea necesario gráficos mermaid. 
  - No inventar información. 
  - Toda afirmación debe estar respaldada por evidencia verificable.

---

## Framework

## Profile

  Aplicar:
  - `/IA/IA.Prompts/PromptFramework/Profiles/Study-Guide-Documentation.md`
  

