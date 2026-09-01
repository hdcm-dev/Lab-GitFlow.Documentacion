# 02 — Dominio y aplicación

> **Propósito**: registrar las entidades, las reglas de validación, los catálogos y los casos de
> uso, con sus valores exactos, para poder razonar sobre el comportamiento sin abrir el código.
> **Fuente primaria**: `src/MovilidadUrbana.Web/Dominio/` y `src/MovilidadUrbana.Web/Aplicacion/`.

## Entidades

`Dominio/Entidades/`

| Entidad | Campos | Notas |
| --- | --- | --- |
| `Localidad` | `Id`, `SesionId`, `Nombre`, `Provincia`, `CodigoPostal`, `Habitantes` | `SesionId` es lo que aísla los datos de cada visitante |
| `RespuestaDeEncuesta` | `Id`, `SesionId`, `Nombre`, `Edad`, `Localidad`, `Medios` (lista), `Frecuencia`, `Distancia`, `Minutos`, `Motivo`, `RegistradaEn` | `Medios` se persiste como texto separado por comas |
| `Sesion` | `Id` (clave, ≤64), `CreadaEn` | Marca de que la sesión ya recibió su juego de datos inicial |

La marca `Sesion` existe para que borrar todas las localidades a mano **no** vuelva a sembrarlas en
la lectura siguiente.

## Catálogos

`Dominio/Catalogos.cs` — valores fijos compartidos por pantallas y reglas.

| Catálogo | Valores |
| --- | --- |
| `Provincias` | Buenos Aires · Chaco · Córdoba · Corrientes · Entre Ríos · Mendoza · Santa Fe |
| `Medios` (clave → etiqueta) | `colectivo`→Colectivo · `auto`→Auto particular · `bicicleta`→Bicicleta · `moto`→Moto · `caminata`→A pie · `tren`→Tren |
| `Frecuencias` | `diaria`→Todos los días · `semanal`→Algunos días por semana · `ocasional`→Ocasionalmente |
| `Motivos` | `trabajo`→Trabajo · `estudio`→Estudio · `salud`→Salud · `otros`→Otros |

Cada catálogo con clave expone su traductor (`EtiquetaDeMedio`, `EtiquetaDeFrecuencia`,
`EtiquetaDeMotivo`), que devuelve la clave misma cuando no la reconoce. La clave es lo que se
persiste; la etiqueta, lo que se muestra.

## Reglas de negocio

Viven en `Dominio/Reglas/` y no en atributos del modelo de pantalla, para que la validación no
dependa de la interfaz que la invoque.

### `ReglasDeLocalidad`

| Regla | Criterio |
| --- | --- |
| `NombreValido` | ≥ 3 caracteres una vez recortado (`LargoMinimoDelNombre`); el máximo declarado es 60 |
| `ProvinciaValida` | no vacía |
| `CodigoPostalValido` | exactamente 4 dígitos — `GeneratedRegex(@"^\d{4}$")` |
| `HabitantesValidos` | no nulo y ≥ 1 (`HabitantesMinimos`) |
| `MismaLocalidad` | mismo nombre recortado **sin distinguir mayúsculas** y misma provincia **con** distinción |

`LargoMaximoDelNombre = 60` está declarado pero no se comprueba en código: el tope lo impone el
`maxlength="60"` del campo y el `HasMaxLength(60)` del modelo EF.

### `ReglasDeEncuesta`

| Regla | Rango |
| --- | --- |
| `TotalDePasos` | 3 |
| `NombreValido` | ≥ 3 caracteres recortados |
| `EdadValida` | 16 – 110 |
| `DistanciaValida` | 0 – 500 (km) |
| `MinutosValidos` | 1 – 600 |

## Modelos de pantalla

`Aplicacion/Localidades/ModeloDeLocalidad.cs` — campos crudos tal como se tipean. `Habitantes` es
`int?` **a propósito**: distingue «vacío» de «cero», que son dos errores distintos. `EsEdicion` se
deduce de `Id is not null`.

`Aplicacion/Encuestas/ModeloDeEncuesta.cs` — acumula los tres pasos. `Medios` es un `HashSet<string>`
de solo lectura, mutado por `AlternarMedio(clave, elegido)`.

## `Resultado`

`Aplicacion/Resultado.cs` — `record` sellado: `EsCorrecto`, `Mensaje`, `Errores`.

| Constructor | Devuelve |
| --- | --- |
| `Correcto(mensaje)` | éxito, sin errores |
| `Invalido(errores)` | mensaje fijo «Revise los campos marcados en rojo.» + diccionario |
| `Invalido(campo, mensaje)` | atajo de un solo error |

Las claves del diccionario son los nombres de campo que la pantalla conoce (`nombre`, `provincia`,
`codigoPostal`, `habitantes`), de modo que la vista solo tiene que ubicarlos.

## Casos de uso

### `ServicioDeLocalidades`

| Operación | Comportamiento |
| --- | --- |
| `ListarAsync` | Delega en el repositorio (que siembra si hace falta) |
| `GuardarAsync` | Valida los cuatro campos → comprueba duplicado contra el listado de la sesión → actualiza o agrega |
| `EliminarAsync` | Obtiene, y si no existe devuelve «La localidad ya no existe.» |

Orden de la validación en `GuardarAsync`: primero los cuatro campos; si alguno falla se corta ahí.
Recién después la comprobación de duplicado, que excluye la propia fila (`item.Id != modelo.Id`) y
por eso permite reguardar una localidad en edición sin cambiarle el nombre. El error de duplicado
se reporta bajo la clave `nombre`.

Mensajes de éxito: `Se agregó la localidad {nombre}.`, `Se actualizó la localidad {nombre}.`,
`Se eliminó la localidad {nombre}.` — literales que las pruebas E2E verifican.

### `ServicioDeEncuestas`

| Operación | Comportamiento |
| --- | --- |
| `ContarAsync` | Encuestas registradas por la sesión actual |
| `ValidarPaso(paso, modelo)` | Diccionario de errores del paso indicado; vacío = se puede avanzar |
| `RegistrarAsync(modelo)` | Persiste y devuelve la `RespuestaDeEncuesta` |

`ValidarPaso` es un `switch` por paso: 1 → nombre, edad, localidad · 2 → al menos un medio y
frecuencia · 3 → distancia, minutos y motivo. Un paso fuera de 1–3 devuelve el diccionario vacío,
es decir, valida.

`RegistrarAsync` recorta el nombre, sella `RegistradaEn` con `DateTimeOffset.UtcNow` y **reordena
los medios según el catálogo**, no según el orden en que se tildaron, para que el resumen sea
estable —lo que hace verificable el `data-testid="resumen-medios"`—.

Ni el servicio ni el modelo conocen el `SesionId`: lo estampan los repositorios
(ver [03_Persistencia-Y-Sesiones.md](03_Persistencia-Y-Sesiones.md)).
