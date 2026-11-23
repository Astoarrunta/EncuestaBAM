# Fases del proyecto y asuntos pendientes

## Fase 1 — Carga y validación del JSON

Pendiente de completar:

- Implementar botón de carga.
- Validar campos y tipos.
- Mostrar errores comprensibles.
- Mostrar un resumen del número de zonas, divisiones y centros cargados.

## Fase 2 — Interfaz de GR Coordinación

- Cabecera BAM y navegación.
- Selector o identificación de campaña.
- Mostrar únicamente el botón **Distribuir zonas**.
- Ocultar todo el panel de configuración de campaña.
- Incorporar botón de ayuda.

## Fase 3 — Generación de HTML por zona

- Agrupar por zona.
- Generar hasta ocho HTML.
- Aplicar nombre de fichero.
- Descargar automáticamente.
- Incluir los datos de los centros dentro del HTML.

## Fase 4 — Generación por división y por centro

- Generar hasta 16 ficheros de división por zona.
- Generar hasta 44 ficheros de centro por división.
- Activar exclusivamente el botón del nivel actual.
- Marcar centros distribuidos y bloquearlos en el HTML de origen.

## Fase 5 — Envío de respuestas

- Conectar el HTML con el despliegue de Google Apps Script.
- Crear el JSON de resultado.
- Guardarlo en `Encuesta_GR2026/Resultados_JSON`.
- Componer nombre con zona, división, centro, fecha y hora.
- Mostrar confirmación o error.

## Fase 6 — Diseño y experiencia de usuario

- Incorporar el anagrama BAM en pestaña y cabecera.
- Revisar el diseño en móvil y ordenador.
- Incorporar `Ref:ChatGpt` en el pie, pequeño y discreto.
- Probar apertura desde adjuntos de correo y WhatsApp.

## Fase 7 — Pruebas

- Probar el JSON de ejemplo.
- Probar distribución de zona, división y centro.
- Probar un coordinador con más de un centro.
- Probar Android, iPhone, Windows y Linux.
- Probar envío con conexión y sin conexión.
- Verificar la creación de JSON en Drive.
- Verificar que dos envíos generan dos archivos.

## Fase 8 — Puesta en marcha

1. Cargar el JSON completo en GR Coordinación.
2. Generar y enviar los HTML de las zonas.
3. Cada zona revisa sus centros, responde los que quiera y distribuye sus divisiones.
4. Cada división revisa sus centros, responde los que quiera y distribuye sus coordinadores.
5. Cada coordinador responde los centros recibidos.
6. GR Coordinación revisa los JSON recibidos en Drive.

## Decisiones todavía abiertas

- Formato de revisión y análisis de los JSON en una fase posterior.
- Diseño definitivo de la pantalla de resultados para GR Coordinación.
- Texto exacto de los mensajes de error.
- Si se desea un botón de descarga manual además de la descarga automática.
