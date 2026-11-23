# Encuesta GR 2026 — Documentación del proyecto

Esta carpeta contiene la documentación de la aplicación de encuesta para la recopilación de datos de tiendas tras la Gran Recogida 2026.

## Documentos

1. [PRD completo](01_PRD_ENCUESTA_GR2026.md)
2. [Resumen del proceso](02_RESUMEN_PROCESO.md)
3. [Formato de carga JSON](03_ESPECIFICACION_CARGA_JSON.md)
4. [Distribución de los ficheros HTML](04_ESPECIFICACION_DISTRIBUCION.md)
5. [Recepción de resultados](05_RECEPCION_RESULTADOS.md)
6. [Manual de usuario](06_MANUAL_USUARIO.md)
7. [Fases pendientes](07_FASES_PENDIENTES.md)

## Decisiones ya adoptadas

- La aplicación se distribuye como fichero HTML autocontenido.
- No requiere instalación, cuenta de Google ni inicio de sesión.
- Los datos iniciales se cargan mediante un fichero JSON.
- Cada nivel recibe únicamente los centros que le corresponden.
- Solo aparece habilitado el botón de distribución del nivel actual.
- Los resultados se envían como ficheros JSON a Google Drive mediante Google Apps Script.
- No se realiza comprobación ni bloqueo de duplicados en el servidor.
- Los ficheros de resultado incluyen fecha y hora en el nombre.
- La organización responsable se denomina **GR Coordinación**, no Dirección.
