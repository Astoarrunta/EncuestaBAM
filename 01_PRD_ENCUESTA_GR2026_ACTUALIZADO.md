# PRD — Encuesta GR 2026

**Versión:** 2.1  
**Organización:** GR Coordinación (Banco de Alimentos de Madrid - BAM)  
**Campaña:** GRAN RECOGIDA 2026  
**Estado:** Especificación definitiva alineada con el prototipo vigente (`MVP-Inicio.html`)

---

## 1. Objetivo

Crear una aplicación web distribuible para recopilar información operativa de centros y tiendas tras la Gran Recogida 2026.

La aplicación se distribuye como un fichero HTML autocontenido (`.html`) sin dependencias externas. Debe abrirse y funcionar directamente desde Windows, Linux, macOS, Android y iPhone sin instalar ninguna aplicación ni requerir inicio de sesión.

## 2. Principios de funcionamiento

- El fichero maestro de **GR Coordinación** incorpora el conjunto completo de centros dentro del bloque `CONFIG GENERADOR`.
- Cada nivel recibe únicamente los registros correspondientes a su ámbito.
- La distribución se realiza en cascada descendente: **GR Coordinación → Zona → División → Centro (Coordinador)**.
- El progreso local (`Disponible`, `Guardado`, `Distribuido`, `Respondido`) se persiste en el `localStorage` del navegador bajo una clave única por nivel y ámbito (`bam_gr2026_<nivel>_<ambito>`).
- Las respuestas solo se envían a Google Drive al pulsar **Enviar respuesta y cerrar**.
- Las peticiones (`POST` y `GET`) incluyen un token compartido (`SUBMIT_TOKEN`) validado por Google Apps Script.
- No se realiza control de duplicados en el servidor: si se reciben dos envíos del mismo centro, se conservan ambos ficheros JSON.

## 3. Datos de carga y estructura del prototipo

El prototipo carga los registros desde el array `records` dentro del bloque `// === CONFIG GENERADOR ===`.

- **Volumen de campaña completo:** 144 centros, 8 zonas y 72 divisiones (`datos_carga_GR2026.json`). *(Nota: el prototipo `MVP-Inicio.html` contiene actualmente una muestra de 72 centros para validación).*
- No se deben añadir centros, zonas, divisiones ni responsables que no existan en los datos de carga.

## 4. Jerarquía y niveles operativos

La constante `NIVEL_ACTUAL` determina las acciones habilitadas en la interfaz:

1. **`coordinacion` (GR Coordinación):** Visualiza todas las zonas y divisiones. Distribuye por zona (`Distribuir a Zona`) y consulta el estado de recepción en Drive.
2. **`zona` (Responsable de Zona):** Visualiza sus divisiones. Distribuye por división (`Distribuir a División`).
3. **`division` (Responsable de División):** Visualiza sus centros en lista plana. Puede cumplimentar cuestionarios directamente (`Cuestionario`) o distribuir cada centro individualmente (`Distribuir`) a su coordinador.
4. **`centro` (Coordinador de Centro):** Nivel terminal. Cumplimenta y envía el cuestionario de su centro; no dispone de acciones de distribución.

## 5. Esquema de datos de entrada

La aplicación soporta un mapeo dual compatible tanto con el esquema completo del JSON de carga como con el esquema abreviado del prototipo:

| Campo interno | Clave JSON carga | Clave prototipo | Descripción |
|---|---|---|---|
| `id` | `id_centro` | `id` | Identificador numérico único del centro |
| `nombre_campana` | `nombre_campana` | `nombre_campana` | Nombre de la campaña |
| `codigo_zona` | `codigo_zona` | `codigo_zona` | Código numérico de zona |
| `nombre_zona` | `nombre_zona` | `z` | Nombre de la zona |
| `responsable_zona` | `responsable_zona` | `responsable_zona` | Responsable de la zona |
| `codigo_division` | `codigo_division` | `codigo_division` | Código numérico de división |
| `nombre_division` | `nombre_division` | `d` | Nombre de la división |
| `supervisor_division` | `nombre_supervisor_division` | `responsable_division` | Supervisor de la división |
| `direccion` | `direccion_centro` | `addr` | Dirección postal del centro |
| `cp` | `codigo_postal` | `cp` | Código postal |
| `municipio` | `municipio` | `city` | Localidad |
| `ensena` | `ensena_centro` | `brand` | Cadena / enseña comercial |
| `coordinador` | `nombre_coordinador` | `coord` | Nombre del coordinador de centro |
| `correo` | `correo_coordinador` | `correo` | Email de contacto |
| `telefono` | `telefono_coordinador` | `telefono` | Teléfono de contacto |
| `ha_tenido_voluntarios` | `Ha tenido voluntarios` | `ha_tenido_voluntarios` | Dato histórico (`Sí` / `No`), no editable |

## 6. Pantalla principal y navegación

### 6.1 Organización visual por nivel

- **En `coordinacion`:** Estructura en árbol jerárquico plegado por defecto (**Zona → División → Centro**). Cada cabecera muestra el nombre del ámbito, el total de centros y los pendientes de distribuir.
- **En `zona`:** Paneles plegables por **División** con sus tarjetas de centro en su interior.
- **En `division` y `centro`:** Lista plana de tarjetas compactas de centros.

Cada tarjeta de centro muestra: identificador (`Centro <id>`), enseña, dirección, código postal, municipio, división, estado actual y los botones de acción disponibles (`Distribuir` en nivel División y `Cuestionario`). No se muestran las respuestas completas en la vista principal; para consultarlas o editarlas se accede al cuestionario.

### 6.2 Alertas de calidad de datos

- Si todos los centros de una división carecen de supervisor asignado (`Sin asignar` / vacío), la cabecera de la división muestra la etiqueta roja **`Sin supervisor`**.
- Si un centro carece de coordinador asignado, su tarjeta muestra la etiqueta roja **`Sin coordinador`**.

### 6.3 Estados y código de colores

| Estado | Tipo | Color | Código | Comportamiento visual y funcional |
|---|---|---|---|---|
| **Disponible** | Local | Gris | `#8A93A3` | Pendiente de tratar. Botones activos. |
| **Guardado** | Local | Azul | `#3B82C4` | Respuestas guardadas en `localStorage`, sin enviar. |
| **Distribuido** | Local | Naranja | `#E08A1E` | Etiqueta visible: `Distribuido a Zona / División / Centro`. Opacidad al 65 % y sin botones de acción. |
| **Respondido** | Local | Verde | `#2E7D4F` | JSON enviado correctamente a Drive desde este navegador. Sin botones de acción. |
| **Recibido** | Sesión (Drive) | Verde | `#2E7D4F` | Override visual informativo en Coordinación tras consultar Drive. Si hay más de un envío, muestra `Recibido (n envíos)`. |

## 7. Filtros de estado

Barra superior de filtros con contador dinámico actualizado en tiempo real sobre los centros del ámbito actual:

- **Todos (n)**
- **Disponible (n)**
- **Guardado (n)**
- **Distribuido a [Zona / División / Centro] (n)**
- **Respondido (n)**
- **Recibido (Drive) (n)** *(visible en GR Coordinación tras ejecutar la consulta a Drive)*

## 8. KPI principales

La cabecera muestra 4 indicadores en todos los niveles (y un 5.º indicador exclusivo en GR Coordinación):

1. **Centros en esta encuesta:** Total de centros del ámbito actual.
2. **Respondidos:** Centros cuyo JSON se ha enviado directamente desde el archivo actual.
3. **Distribuidos a [Zona / División / Centro]:** Centros delegados al nivel inferior (no se contabilizan como respondidos).
4. **Recibidos en Drive** *(solo GR Coordinación)*: Centros confirmados en `Resultados_JSON` tras pulsar *Actualizar estado (Drive)* (`—` antes de consultar).
5. **Completado:** Porcentaje calculado con la fórmula `Math.round(((respondidos + distribuidos) / total) * 100)`.

## 9. Cuestionario

Al pulsar **Cuestionario** se abre un modal independiente por centro con sus datos informativos y el formulario de recogida. La primera vez que se abre un centro, los campos editables aparecen vacíos.

### 9.1 Cabecera informativa (no editable)

Muestra: Centro (`#id · Enseña`), Dirección, Municipio (`CP Municipio`), Zona (`código · nombre`), División (`código · nombre`) y **Ha tenido voluntarios última campaña** (`Sí` / `No`).

### 9.2 Preguntas editables

Cada campo incluye un botón de ayuda contextual (`?`). Ninguna pregunta es obligatoria:

| Clave | Etiqueta | Tipo / Validación |
|---|---|---|
| `r1` | N.º de entradas | Numérico (`number`) |
| `r2` | N.º de filas de cajas | Numérico (`number`) |
| `r3` | ¿Pondrías voluntarios? | Selector (`""`, `Si`, `No`) |
| `r4` | Voluntarios mínimos por turno | Numérico (`number`) |
| `r5` | Observaciones | Texto multilínea, máximo 200 caracteres |

### 9.3 Contrato JSON enviado a Drive (`POST`)

Al enviar, el cliente realiza una petición `POST` (`Content-Type: text/plain;charset=utf-8`) al endpoint de Google Apps Script con la siguiente estructura, incluyendo únicamente las claves respondidas dentro de `respuestas`:

```json
{
  "codigo_zona": 1,
  "codigo_division": 7,
  "codigo_centro": 3043,
  "respuestas": {
    "r1": 2,
    "r2": 4,
    "r3": "Si",
    "r4": 2,
    "r5": "Texto de ejemplo"
  },
  "token": "GR2026-bam"
}
```

## 10. Botones del formulario

- **Cancelar:** Cierra el modal sin guardar cambios ni modificar el estado del centro.
- **Guardar respuesta:** Activo cuando al menos uno de los campos (`r1`–`r5`) tiene contenido. Guarda las respuestas en `localStorage` (conservándolas aunque se cierre el navegador), cambia el estado a **Guardado** y cierra el modal sin enviar a Drive.
- **Enviar respuesta y cerrar:** Activo cuando al menos uno de los campos (`r1`–`r5`) tiene contenido. Envía el payload por `POST` a Google Apps Script, cambia el estado a **Respondido**, persiste en `localStorage` y cierra el modal.

## 11. Reglas de distribución

### 11.1 Centros distribuibles

Solo pueden distribuirse centros en estado **Disponible** o **Guardado**. Los centros en estado **Respondido** o **Distribuido** quedan excluidos.

### 11.2 Protección ante centros en estado Guardado

Si un responsable intenta distribuir un ámbito o centro que contiene respuestas en estado **Guardado**:

- En nivel **División** (distribución individual por centro), se muestra un diálogo de confirmación (`confirm`) advirtiendo que las respuestas guardadas localmente **no se heredan** y que el archivo del coordinador se generará vacío.
- Al confirmar la distribución, las respuestas guardadas se limpian internamente (`respuestas = {}`) y el centro pasa a estado **Distribuido**.

### 11.3 Distribuciones sucesivas

La distribución puede ejecutarse múltiples veces en el mismo nivel. Cada ejecución filtra únicamente los centros que sigan siendo distribuibles:

- Si quedan centros pendientes, el botón muestra el contador actualizado: `Distribuir a [Nivel] (n)`.
- Si no quedan centros pendientes, el botón cambia a **`Sin centros pendientes`** y queda deshabilitado.

## 12. Distribución individual por ámbito

La interfaz no incluye botones masivos para distribuir todos los ámbitos a la vez. La distribución se ejecuta de forma individualizada:

- **En GR Coordinación:** Una acción por cada **Zona**.
- **En Zona:** Una acción por cada **División**.
- **En División:** Una acción por cada **Centro** (para su coordinador).

Los datos de destinatarios (`responsable_zona`, `supervisor_division` y `nombre_coordinador`) se leen directamente de los registros del JSON de carga.

## 13. Generación y nomenclatura de ficheros HTML

Al distribuir, el navegador genera ad hoc y descarga en la carpeta de descargas local un nuevo fichero HTML autocontenido (sin almacenarlo en Drive), reescribiendo el bloque `// === CONFIG GENERADOR ===` con el `NIVEL_ACTUAL`, `AMBITO` y `records` filtrados:

- **Nivel Zona:** `Encuesta_zona_<NOMBRE_ZONA>.html`
- **Nivel División:** `Encuesta_zona_<NOMBRE_ZONA>_division_<NOMBRE_DIVISION>.html`
- **Nivel Centro:** `Encuesta_zona_<NOMBRE_ZONA>_division_<NOMBRE_DIVISION>_centro_<ID_CENTRO>.html`

*(Los nombres de zona y división se normalizan eliminando acentos y caracteres especiales).*

## 14. Envío y apertura en dispositivos

Los responsables envían los ficheros descargados individualmente por correo electrónico o WhatsApp.

Si se recibe por WhatsApp en Android o iPhone:

1. Descargar primero el fichero `.html` en el dispositivo.
2. Elegir **Abrir con** y seleccionar Chrome, Safari u otro navegador web.

## 15. Integración con Google Apps Script y recepción en Drive

La comunicación con Drive se realiza mediante Google Apps Script. Para el usuario final:

- No requiere cuenta de Google ni permisos sobre Drive.
- No requiere abrir Drive ni instalar aplicaciones adicionales.
- Solo requiere conexión a Internet en el momento de pulsar **Enviar respuesta y cerrar** (o **Actualizar estado (Drive)**).

Operaciones soportadas en el servidor:

- **Recepción (`doPost`):** Valida el `token` y guarda cada envío como un fichero independiente en `Encuesta_GR2026/Resultados_JSON` con formato `zonaXXXX_divisionYYYY_centroZZZZ_YYYYMMDD_HHMMSS.json`. No se comprueban duplicados: dos envíos generan dos ficheros.
- **Seguimiento en tiempo real (`doGet`):** Desde GR Coordinación, el botón **Actualizar estado (Drive)** envía una petición `GET` autenticada (`?token=<SUBMIT_TOKEN>`). El servidor parsea los nombres de fichero de `Resultados_JSON` y devuelve la lista de centros recibidos (`recibidos`), marcándolos visualmente como **Recibido** (y contabilizando envíos múltiples) sin alterar el `localStorage`.

## 16. Diseño visual y controles de entorno

- Cabecera corporativa BAM con anagrama en pestaña y barra superior, más indicador visual del nivel activo en la barra de subnavegación.
- Modal de **Ayuda** accesible desde la cabecera con la nota explicativa de GR Coordinación.
- Tarjetas distribuidas atenuadas (opacidad 65 %) y bloqueadas.
- **Control de pruebas (`Reset pruebas`):** Botón visible exclusivamente en `NIVEL_ACTUAL === 'coordinacion'` para limpiar las claves `bam_gr2026*` de `localStorage` durante las pruebas. *(Nota de despliegue: limpiar las etiquetas internas de prototipado como `MVP-Inicio DEEPSEEK visual` y `Ref:ChatGpt` antes de la distribución oficial).*

## 17. Puesta en marcha

1. GR Coordinación abre el HTML maestro con los datos completos cargados en `records`.
2. Se muestran las zonas derivadas del JSON en árbol plegado.
3. GR Coordinación distribuye cada zona individualmente y envía los `.html` generados.
4. Cada responsable de zona recibe su fichero y distribuye sus divisiones.
5. Cada responsable de división cumplimenta centros directamente o los distribuye a sus coordinadores.
6. Los coordinadores completan y envían los cuestionarios a Drive.
7. GR Coordinación monitoriza el avance con **Actualizar estado (Drive)** y recopila los JSON almacenados en `Encuesta_GR2026/Resultados_JSON`.

## 18. Fuera de alcance

- Inicio de sesión de participantes.
- Aplicación móvil nativa.
- Servidor web propio o base de datos compleja.
- Control o bloqueo de duplicados en Drive.
- Reapertura de respuestas ya enviadas.
- Herencia de respuestas guardadas al distribuir a un nivel inferior.

## 19. Evolución futura (Fase POST 2)

- **Panel de consolidación e informes:** Herramienta en el nivel central de Coordinación para leer y agregar el contenido de todos los JSON recibidos en `Resultados_JSON` (totales por enseña, zona y división, exportación a Excel/CSV y listado de centros sin responder).