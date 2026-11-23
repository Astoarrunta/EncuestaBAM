# PRD actualizado — Encuesta GR 2026

**Versión:** 2.0  
**Organización:** GR Coordinación  
**Campaña:** GRAN RECOGIDA 2026  
**Estado:** Especificación alineada con el prototipo vigente

## 1. Objetivo

Crear una aplicación web distribuible para recopilar información de centros y tiendas después de la Gran Recogida 2026.

La aplicación se distribuirá como fichero HTML autocontenido. Debe poder abrirse desde Windows, Linux, Android y iPhone sin instalar una aplicación y sin que el usuario tenga que iniciar sesión.

## 2. Principios de funcionamiento

- GR Coordinación carga un JSON completo con todos los centros.
- Cada nivel recibe únicamente los registros que corresponden a su ámbito.
- La distribución se realiza progresivamente: GR Coordinación → Zona → División → Coordinador.
- Cada centro conserva su propio estado y sus propias respuestas.
- Las respuestas solo se envían a Drive al pulsar **Enviar respuesta y cerrar**.
- No se realiza control de duplicados en el servidor.
- Si se reciben dos envíos, se conservan los dos ficheros JSON.

## 3. Datos actuales del prototipo

El prototipo vigente utiliza el fichero `datos_carga_GR2026.json`, que contiene:

- 144 centros.
- 8 zonas.
- 72 divisiones.

No se deben añadir centros, zonas, divisiones ni responsables que no estén en el JSON de carga.

## 4. Jerarquía y niveles

1. **GR Coordinación**: carga los datos y distribuye a zonas.
2. **Zona**: distribuye a divisiones.
3. **División**: distribuye a coordinadores.
4. **Coordinador**: cumplimenta centros y no distribuye.

La interfaz solo debe habilitar la función correspondiente al nivel actual.

## 5. Datos de carga

Cada registro del JSON representa un centro e incluye:

- `nombre_campana`
- `codigo_zona`
- `nombre_zona`
- `codigo_division`
- `nombre_division`
- `id_centro` numérico
- `direccion_centro`
- `codigo_postal`
- `municipio`
- `ensena_centro`
- `nombre_coordinador`
- `correo_coordinador`
- `telefono_coordinador`
- `Ha tenido voluntarios`

El último campo es un dato propio del centro, no una pregunta. Se muestra como información no editable:

**Ha tenido voluntarios en la última campaña: Sí/No**

Los datos deben cargarse desde JSON y no escribirse manualmente en el prototipo.

## 6. Pantalla principal

La primera pantalla debe ser sencilla y orientada a la gestión de centros.

### 6.1 Lista vertical de centros

Cada centro se muestra en una tarjeta vertical con:

- Identificador del centro.
- Dirección.
- Enseña.
- Zona.
- División.
- Estado.
- Botón **Cuestionario**.

No se muestran las respuestas completas en esta pantalla. Para verlas se accede al cuestionario.

### 6.2 Estados

Los estados funcionales son:

- **Disponible**: todavía puede tratarse.
- **Guardado**: tiene respuestas guardadas, pero no enviadas.
- **Distribuido**: ya se ha enviado a un nivel inferior.
- **Respondido**: se ha enviado el JSON a Drive.

La etiqueta visible del estado distribuido será específica:

- **Distribuido a Zona**.
- **Distribuido a División**.
- **Distribuido a Coordinador**.

Internamente el estado se denomina `Distribuido`.

### 6.3 Colores

| Estado | Color | Código |
|---|---|---|
| Disponible | Gris | `#8A93A3` |
| Guardado | Azul | `#3B82C4` |
| Distribuido | Naranja | `#E08A1E` |
| Respondido | Verde | `#2E7D4F` |

Las tarjetas distribuidas se muestran con una opacidad del 65 % y sin botón de cuestionario, porque ya no pueden responderse desde ese nivel.

## 7. Filtros de estado

La pantalla incluye botones de filtro con contador dinámico:

- **Todos (n)**
- **Disponible (n)**
- **Guardado (n)**
- **Distribuido a Zona/División/Coordinador (n)**
- **Respondido (n)**

Los contadores se calculan sobre los centros del ámbito actual y se actualizan después de guardar, responder o distribuir.

## 8. KPI principales

La pantalla muestra cuatro KPI:

1. **Centros en esta encuesta**: total de centros del ámbito actual.
2. **Respondidos**: centros cuyo JSON se ha enviado.
3. **Distribuidos a Zona/División/Coordinador**: centros enviados al nivel inferior correspondiente.
4. **Completado**.

La fórmula de completado es:

`(respondidos + distribuidos) / total de centros × 100`

Un centro distribuido no se cuenta como respondido.

## 9. Cuestionario

El botón **Cuestionario** abre el formulario del centro seleccionado.

### 9.1 Datos informativos

En el encabezado aparecen:

- Centro.
- Enseña.
- Dirección.
- Municipio.
- Zona.
- División.
- Ha tenido voluntarios en la última campaña.

El valor de voluntarios procede del JSON inicial y no es editable.

### 9.2 Preguntas editables

Las respuestas utilizan las claves `r1` a `r5`:

| Clave | Campo | Tipo |
|---|---|---|
| `r1` | Número de entradas | Número |
| `r2` | Número de filas de cajas | Número |
| `r3` | Opinión poner voluntarios | Texto `Si` o `No` |
| `r4` | Voluntarios mínimos por turno | Número |
| `r5` | Observaciones | Texto, máximo 200 caracteres |

Cada centro tiene un formulario y un conjunto de respuestas independiente. La primera vez que se abre un centro, las preguntas aparecen vacías.

Ninguna pregunta es obligatoria. El usuario puede dejar campos vacíos.

## 9.3 JSON de respuesta

El fichero de respuesta contiene `id_centro` y únicamente las respuestas que tengan contenido:

```json
{
  "id_centro": 40276,
  "r1": 250,
  "r2": 4,
  "r3": "Si",
  "r4": 2,
  "r5": "Texto de ejemplo"
}
```

`id_centro` es numérico y debe coincidir con el identificador del JSON de carga.

## 10. Botones del formulario

### Cancelar

Cierra el formulario sin guardar cambios.

### Guardar respuesta

- Guarda las respuestas del centro.
- Cierra el formulario.
- Cambia el centro a estado **Guardado**.
- No envía ningún fichero a Drive.
- Se activa únicamente cuando al menos una de las respuestas `r1` a `r5` contiene un valor.
- Conserva los datos aunque se cierre el HTML y se vuelva a abrir en el mismo navegador.

### Enviar respuesta y cerrar

- Guarda las respuestas del centro.
- Genera y envía el JSON a Google Apps Script.
- Google Apps Script crea el fichero en Drive.
- Cambia el centro a estado **Respondido**.
- Cierra el formulario.
- Se activa únicamente cuando al menos una de las respuestas `r1` a `r5` contiene un valor.

## 11. Reglas de distribución

### 11.1 Centros distribuibles

Se pueden distribuir los centros en estado:

- Disponible.
- Guardado.

No se distribuyen centros en estado Respondido.

### 11.2 Centro guardado

Cuando un centro Guardado se distribuye:

- El nivel inferior recibe el centro con el cuestionario vacío.
- Las respuestas guardadas no se heredan.
- Las respuestas guardadas se limpian internamente.
- En el nivel de origen pasa a estado **Distribuido**.

### 11.3 Distribuciones repetidas

Se puede ejecutar la distribución varias veces dentro del mismo nivel.

Cada ejecución solo debe incluir los centros que todavía sean distribuibles. La selección o listado de distribución se actualiza después de cada ejecución:

- Si quedan centros disponibles o guardados, la opción sigue activa.
- Si no queda ninguno, aparece **Sin centros pendientes** y la opción no se puede seleccionar.

## 12. Distribución individual

La pantalla no utiliza botones generales que distribuyan todo de golpe.

La distribución se realiza desde cada fila o ámbito individual:

- En GR Coordinación: una acción por zona.
- En Zona: una acción por división.
- En División: una acción por coordinador.

La columna **Destinatario** debe identificar el receptor previsto según el ámbito. Actualmente el JSON contiene el nombre del coordinador de centro. Para mostrar nombres personales de responsables de zona o división será necesario añadir esos campos al JSON.

## 13. Nombres de los HTML

- `Encuesta_zona_XXXX.html`
- `Encuesta_zona_XXXX_division_YYYY.html`
- `Encuesta_zona_XXXX_division_YYYY_centro_ZZZZ.html`

Los HTML se generan ad hoc y se descargan en el directorio de descargas del navegador. No se almacenan en Drive.

## 14. Envío de los HTML

GR Coordinación y los responsables envían individualmente los HTML por correo electrónico o WhatsApp.

Si se recibe por WhatsApp:

1. Descargar primero el HTML.
2. Elegir **Abrir con**.
3. Seleccionar Chrome, Safari u otro navegador.

No es necesario instalar una aplicación adicional.

## 15. Recepción de resultados

El HTML envía los datos a Google Apps Script. El usuario final:

- No necesita cuenta de Google.
- No necesita permisos sobre Drive.
- No necesita abrir Drive.
- No necesita instalar nada.
- Solo necesita conexión a Internet al pulsar **Enviar respuesta y cerrar**.

Los resultados se guardan en:

`Encuesta_GR2026/Resultados_JSON`

Nombre previsto:

`zonaXXXX_divisionYYYY_centroZZZZ_YYYYMMDD_HHMMSS.json`

No se comprueban duplicados. Dos envíos generan dos ficheros.

## 16. Diseño visual

- Cabecera corporativa BAM.
- Anagrama BAM en cabecera y pestaña.
- Navegación sencilla.
- Pantalla vertical de centros.
- Estados con colores definidos.
- Tarjetas distribuidas atenuadas.
- Botón de ayuda con explicación de la encuesta.
- El panel de configuración de campaña permanece oculto.
- Pie discreto con `Ref:ChatGpt`.

## 17. Puesta en marcha

1. GR Coordinación carga el JSON completo.
2. Se muestran las zonas derivadas del JSON.
3. GR Coordinación distribuye cada zona individualmente.
4. Cada zona recibe sus centros y distribuye sus divisiones.
5. Cada división recibe sus centros y distribuye sus coordinadores.
6. Los coordinadores completan y envían los cuestionarios.
7. GR Coordinación revisa los JSON almacenados en Drive.

## 18. Fuera de alcance

- Inicio de sesión de participantes.
- Aplicación móvil nativa.
- Servidor web propio.
- Base de datos compleja.
- Control de duplicados.
- Reapertura de respuestas enviadas.
- Herencia de respuestas guardadas al distribuir.
- Análisis final de los JSON, que se resolverá en otra fase.
