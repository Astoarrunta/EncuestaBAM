# Manual de usuario — Encuesta Gran Recogida 2026

## 1. Acceso y apertura de la encuesta

La encuesta se distribuye como un archivo autónomo con extensión `.html` por correo electrónico o WhatsApp. **No requiere instalar aplicaciones ni iniciar sesión con cuenta de Google.**

* **En ordenador (Windows / Linux / Mac):** Haz doble clic sobre el archivo `.html` para abrirlo en tu navegador habitual (Chrome, Edge, Firefox, Safari).
* **En móvil (Android / iPhone) recibido por WhatsApp:**
  1. Pulsa sobre el archivo recibido para **descargarlo** en el teléfono.
  2. Toca el archivo descargado, elige **Abrir con** y selecciona tu navegador web (**Chrome** en Android; **Safari** o **Chrome** en iPhone). No lo abras desde el visor interno de WhatsApp.

> **Importante:** No cambies el nombre del archivo `.html` antes de usarlo ni borres los datos de navegación de tu navegador mientras estés trabajando, ya que el progreso guardado se almacena localmente en ese navegador.

---

## 2. Panel principal, indicadores y filtros

Al abrir el archivo verás tu ámbito asignado (Coordinación, Zona, División o Centro) junto con:

* **Indicadores superiores (KPI):** Muestran el total de centros de tu ámbito, los centros **Respondidos** (enviados), los **Distribuidos** al siguiente nivel y el **% Completado**. *(En GR Coordinación se muestra además el total de **Recibidos en Drive**).*
* **Filtros rápidos:** Permiten filtrar la vista por estado (*Todos, Disponible, Guardado, Distribuido, Respondido* y *Recibido*).
* **Navegación por grupos:** En los niveles de Coordinación y Zona, los bloques aparecen plegados por defecto. Pulsa sobre la flecha (`▸`) junto al nombre de la Zona o División para desplegar sus centros.
* **Avisos de gestión:** Si un centro o división carece de responsable en la base de datos, aparecerá una etiqueta roja (**Sin supervisor** o **Sin coordinador**).

---

## 3. Cómo cumplimentar y enviar un centro

Cualquier nivel (excepto en centros ya distribuidos o respondidos) puede rellenar directamente los datos de una tienda:

1. Localiza el centro en la lista y pulsa el botón **Cuestionario**.
2. Revisa los datos informativos de la cabecera (dirección, enseña y si tuvo voluntarios en la última campaña; estos datos no son editables).
3. Rellena los campos del cuestionario (puedes pulsar el icono **`?`** junto a cada pregunta para ver su explicación detallada):
   * **r1 · N.º de entradas:** Número de puertas de acceso que tiene la tienda.
   * **r2 · N.º de filas de cajas:** Número de filas o baterías de caja donde se pueden situar voluntarios. Si alguna fila tiene más de 10 cajas, indícalo en observaciones.
   * **r3 · ¿Pondrías voluntarios?:** Tu opinión (`Sí` o `No`) sobre la conveniencia de poner voluntarios en esa tienda, con independencia de si los hubo en campañas anteriores.
   * **r4 · Voluntarios mínimos por turno:** Número mínimo de voluntarios necesarios para cubrir la tienda con una presencia digna.
   * **r5 · Observaciones:** Cualquier particularidad relevante de la tienda o aclaración sobre las filas de cajas (máximo 200 caracteres).
4. Elige una de las dos acciones inferiores *(se activan en cuanto rellenas al menos un campo)*:
   * **Guardar respuesta:** Guarda tus datos en el navegador y marca el centro en azul como **Guardado**. No envía nada todavía; te permite cerrar el navegador y continuar más tarde.
   * **Enviar respuesta y cerrar:** Envía los datos definitivamente a la base central de Drive y marca el centro en verde como **Respondido**. **Requiere conexión a Internet.**
5. Espera a que aparezca el mensaje de confirmación en la esquina inferior derecha antes de cerrar la ventana.

---

## 4. Cómo distribuir a niveles inferiores

Cada responsable dispone únicamente de las acciones de distribución de su nivel:

* **GR Coordinación:** Pulsa **Distribuir a Zona (n)** en la cabecera de cada zona.
* **Responsable de Zona:** Pulsa **Distribuir a División (n)** en la cabecera de cada división.
* **Responsable de División:** Puedes rellenar el cuestionario tú mismo o pulsar **Distribuir** en cada tarjeta de centro para enviárselo a su coordinador.
* **Coordinador de Centro:** Nivel final; no dispone de botones de distribución.

### Qué ocurre al pulsar Distribuir:
1. Se descargará automáticamente en tu dispositivo un nuevo archivo `.html` nombrado específicamente para ese ámbito (por ejemplo: `Encuesta_zona_NORTE.html`).
2. Envía ese archivo descargado al responsable correspondiente por correo electrónico o WhatsApp.
3. Los centros incluidos pasarán a estado **Distribuido** (en naranja y atenuados) y ya no podrán editarse desde tu nivel.

> **Aviso sobre centros guardados:** Si un centro está en estado **Guardado** y decides distribuirlo a un nivel inferior, el sistema te pedirá confirmación porque **las respuestas guardadas localmente no se transfieren** (el destinatario recibirá el cuestionario en blanco). Si deseas conservar tus respuestas, envíalas tú mismo desde *Cuestionario* en lugar de distribuir el centro.

---

## 5. Seguimiento de respuestas (Solo GR Coordinación)

En el nivel central de Coordinación dispones del botón **Actualizar estado (Drive)**:

* Al pulsarlo, la aplicación consulta en tiempo real los cuestionarios recibidos en la carpeta de Drive.
* Los centros recibidos mostrarán la etiqueta verde **Recibido** (y el número de envíos si se recibió más de uno), permitiéndote conocer el avance real de las divisiones y coordinadores sin afectar a los datos guardados en tu navegador.

---

## 6. Preguntas frecuentes y avisos importantes

* **¿Qué pasa si no tengo Internet al rellenar la encuesta?** Puedes completar los campos y pulsar **Guardar respuesta**. Cuando recuperes la conexión, vuelve a abrir el archivo, entra en el centro y pulsa **Enviar respuesta y cerrar**.
* **¿Se puede enviar un centro dos veces?** Una vez enviado, el botón *Cuestionario* se bloquea en ese navegador. No obstante, si otra persona envía el mismo centro desde otro dispositivo, el sistema conservará ambos archivos JSON sin sobrescribirlos.
* **Precaución antes de cerrar:** No cierres la pestaña ni el navegador hasta ver el aviso verde de confirmación tras pulsar *Enviar respuesta y cerrar*.