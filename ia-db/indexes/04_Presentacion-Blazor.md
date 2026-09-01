# 04 — Presentación (Blazor)

> **Propósito**: describir las pantallas, el layout y —sobre todo— el contrato de `data-testid`
> que las pruebas E2E consumen, junto con las particularidades de Blazor que condicionan el marcado.
> **Fuente primaria**: `src/MovilidadUrbana.Web/Components/`.

## Modo de render y rutas

Toda la aplicación es **interactive server**: `App.razor` monta `<Routes @rendermode="InteractiveServer">`
y `Program.cs` cierra con `AddInteractiveServerRenderMode()`.

| Ruta | Componente | Layout |
| --- | --- | --- |
| `/` | `Pages/Inicio.razor` | `MainLayout` (por defecto del `RouteView`) |
| `/localidades` | `Pages/Localidades.razor` | `MainLayout` |
| `/encuesta` | `Pages/Encuesta.razor` | `MainLayout` |
| `/no-encontrado` | `Pages/NoEncontrado.razor` | `MainLayout` explícito |
| `/Error` | `Pages/Error.razor` | `MainLayout` explícito |

`Routes.razor` declara `NotFoundPage="typeof(Pages.NoEncontrado)"` y el pipeline además hace
`UseStatusCodePagesWithReExecute("/no-encontrado")`, de modo que una URL inexistente llega a la
misma pantalla por cualquiera de los dos caminos.

## Layout

`Layout/MainLayout.razor`:

- Barra de navegación Bootstrap con tres enlaces declarados en un arreglo estático:
  `("", Inicio, nav-inicio)`, `("localidades", …, nav-localidades)`, `("encuesta", …, nav-encuesta)`.
- El **menú colapsable se resuelve con estado del componente** (`_menuAbierto`), sin el bundle
  JavaScript de Bootstrap: en una aplicación interactiva no hace falta, y desaparece la carrera
  entre el click y la animación.
- Se suscribe a `NavigationManager.LocationChanged` para cerrar el menú al navegar (sin recarga de
  página quedaría abierto sobre la pantalla siguiente) y se desuscribe en `Dispose`.
- `EsActiva` compara la ruta relativa —recortando la query y las barras— sin distinguir mayúsculas,
  y aplica `class="active"` más `aria-current="page"`.
- **Testigo de interactividad**: `<div hidden data-testid="estado-app" data-interactivo="@RendererInfo.IsInteractive…">`.
  Vale `false` durante el prerender y pasa a `true` cuando el circuito quedó conectado. Es la señal
  que espera `EsperarInteractivoAsync` en las pruebas.

`Layout/ReconnectModal.razor` (+ `.css` y `.js`) es el diálogo de reconexión que trae la plantilla
de Blazor; no participa de las pruebas.

## Contrato de `data-testid`

Es la interfaz real entre la aplicación y la suite E2E: cambiar uno de estos atributos rompe
pruebas. Selectores por `data-testid`, nunca por texto ni por estructura.

### Comunes

| `data-testid` | Dónde | Qué es |
| --- | --- | --- |
| `estado-app` | `MainLayout` | Testigo de interactividad (`data-interactivo`) |
| `marca` | `MainLayout` | Enlace de marca de la barra |
| `nav-inicio` · `nav-localidades` · `nav-encuesta` | `MainLayout` | Enlaces del menú |
| `titulo` | todas las páginas | `<h1>` de la pantalla |

### `/localidades`

| Grupo | `data-testid` |
| --- | --- |
| Aviso y formulario | `aviso`, `titulo-formulario`, `formulario` |
| Campos | `campo-nombre`, `campo-provincia`, `campo-codigo-postal`, `campo-habitantes` |
| Errores | `error-nombre`, `error-provincia`, `error-codigo-postal`, `error-habitantes` |
| Botones del formulario | `boton-guardar`, `boton-cancelar` |
| Listado | `contador`, `filtro-provincia`, `tabla`, `cuerpo-tabla`, `fila` (con `data-id`), `sin-datos` |
| Celdas | `celda-nombre`, `celda-provincia`, `celda-codigo-postal`, `celda-habitantes` |
| Acciones de fila | `boton-editar`, `boton-eliminar` |
| Diálogo de baja | `modal-nombre`, `boton-cancelar-baja`, `boton-confirmar-baja` |

### `/encuesta`

| Grupo | `data-testid` |
| --- | --- |
| Cabecera | `etiqueta-paso`, `indicador-paso`, `contador-encuestas`, `progreso-contenedor`, `progreso` |
| Pasos | `paso-1`, `paso-2`, `paso-3`, `formulario`, `aviso` |
| Paso 1 | `campo-nombre`, `campo-edad`, `campo-localidad` + `error-*` |
| Paso 2 | `grupo-medios`, `medio-<clave>` (una por medio del catálogo), `campo-frecuencia`, `error-medios`, `error-frecuencia` |
| Paso 3 | `campo-distancia`, `campo-minutos`, `campo-motivo` + `error-*` |
| Navegación | `boton-anterior`, `boton-siguiente`, `boton-finalizar`, `boton-reiniciar` |
| Resumen | `resumen`, `mensaje-envio`, `resumen-persona`, `resumen-localidad`, `resumen-medios`, `resumen-frecuencia`, `resumen-distancia`, `resumen-minutos`, `resumen-motivo` |

### `/` (portada)

`ir-localidades`, `ir-encuesta`.

## Enlace de datos: `oninput`, no `onchange`

Los campos de texto y numéricos usan `@bind:event="oninput"` o un `@oninput` explícito. El motivo es
concreto: **`FillAsync` de Playwright dispara `input`, no `change`**. Con el `@bind` por defecto
—que escucha `onchange`— el valor no llega al servidor hasta que el campo pierde el foco, y la
validación rechaza un formulario que en pantalla se ve completo.

Los desplegables (`select`) sí usan `@bind` a secas: `SelectOptionAsync` dispara `change`.

## `Localidades.razor`

Estado del componente: `_localidades`, `_filtroProvincia`, `_modelo`, `_errores`, `_aviso`
(mensaje + tipo de alerta), `_pendienteDeBaja`.

| Interacción | Efecto |
| --- | --- |
| Guardar | `ServicioDeLocalidades.GuardarAsync`; si falla, aviso `danger` y campos con `is-invalid`; si funciona, aviso `success`, formulario limpio y recarga |
| Editar | Copia la fila al modelo (pasa a modo edición), limpia errores y aviso |
| Cancelar | Vuelve el formulario a modo alta |
| Eliminar | Abre el diálogo; `Confirmar` llama a `EliminarAsync` y deja aviso `warning`; si se estaba editando esa misma fila, el formulario vuelve a modo alta |
| Filtro de provincia | Filtra **en memoria**, sin volver al servidor |

El filtro no consulta la base a propósito: el listado de una sesión es chico y ya está cargado, así
que filtrar en el componente evita una consulta por cada cambio del desplegable. El contador
(`contador`) y el mensaje de vacío (`sin-datos`) reflejan la vista filtrada, y el texto del vacío
cambia según haya filtro o no («No hay localidades cargadas.» / «No hay localidades en X.»).

El **diálogo de confirmación es marcado propio** gobernado por `_pendienteDeBaja`, no el modal de
Bootstrap: sin animación de apertura no existe la ventana en la que un click se pierde.

Los habitantes se muestran con `ToString("N0")`, que con la cultura `es-AR` fijada en `Program.cs`
usa el punto como separador de miles — parte de lo que las pruebas verifican.

## `Encuesta.razor`

Asistente de tres pasos con un único `_paso` y validación por paso.

| Elemento | Comportamiento |
| --- | --- |
| `Porcentaje` | `paso × 100 / 3` redondeado; 100 con la encuesta ya registrada |
| `EtiquetaDelPaso` | «Paso N de 3 — {título}» / «Encuesta completada» |
| Títulos | 1 Datos de la persona · 2 Medios que utiliza para viajar · 3 Distancia recorrida |
| `Siguiente` | En el paso 3 equivale a `Finalizar`; si no, valida y avanza |
| `Anterior` | Limpia aviso y errores, retrocede sin validar (conserva lo cargado) |
| `Finalizar` | Valida el paso 3, registra y refresca el contador |
| `Nueva encuesta` | Reinicia modelo, errores, respuesta, aviso y paso |

El desplegable de localidades del paso 1 **se alimenta del ABM** (`ServicioDeLocalidades.ListarAsync`
en `OnInitializedAsync`): las dos pantallas comparten el mismo almacén de la sesión. El contador
`contador-encuestas` muestra las registradas por esa sesión.

Cuando la validación de un paso falla, el aviso es siempre «Complete los datos del paso antes de
continuar.» y los campos afectados quedan con `is-invalid`.

Con la encuesta ya registrada, el formulario se reemplaza por el bloque `resumen`, donde la
distancia se formatea con `"0.###"` y los medios se listan traducidos a etiqueta y en orden de
catálogo (ver [02_Dominio-Y-Aplicacion.md](02_Dominio-Y-Aplicacion.md)).

## Recursos estáticos

`wwwroot/vendor/bootstrap/bootstrap.min.css` (5.3.8, sin CDN y **sin el bundle JS**),
`wwwroot/css/estilos.css` y los estilos aislados del componente
(`MovilidadUrbana.Web.styles.css`). Se sirven por `MapStaticAssets()` y se referencian con
`@Assets[...]` en `App.razor`.
