# Recepción de resultados

## Método adoptado

El HTML envía el resultado a una aplicación intermedia de Google Apps Script. Apps Script crea un fichero JSON en Google Drive.

Los participantes:

- No necesitan cuenta de Google.
- No necesitan permisos sobre la hoja o la carpeta.
- No necesitan abrir Drive.
- No necesitan instalar una aplicación.
- Solo necesitan abrir el HTML y tener conexión a Internet al pulsar **Enviar**.

## Carpeta de destino

`Encuesta_GR2026/Resultados_JSON`

## Nombre del fichero

`zonaXXXX_divisionYYYY_centroZZZZ_YYYYMMDD_HHMMSS.json`

El nombre incorpora zona, división, centro, fecha y hora del envío.

## Contenido mínimo

```json
{
  "nombre_campana": "GRAN RECOGIDA 2026",
  "codigo_zona": 5,
  "nombre_zona": "OESTE",
  "codigo_division": 3,
  "nombre_division": "POZUELO-MADRID",
  "id_centro": 40276,
  "fecha_envio": "2026-11-19T20:25:00.000Z",
  "respuestas": {
    "plantas": 2,
    "puertas": 3,
    "filas_caja": 5,
    "observaciones": "",
    "voluntarios_minimos": 4,
    "voluntarios_optimos": 8,
    "tuvo_voluntarios_ultima_campana": "Sí",
    "pondria_voluntarios": "Sí",
    "observaciones_voluntarios": ""
  }
}
```

## Duplicados

No se comprueban duplicados. Si se pulsa enviar dos veces o se recibe el mismo resultado desde dos ficheros, se crean y conservan dos JSON. GR Coordinación decidirá posteriormente cuál utilizar.

## Conectividad

El formulario puede abrirse y rellenarse sin conexión. El envío requiere conexión. Si falla, se debe mostrar un mensaje claro para que el usuario vuelva a intentarlo.

## Alternativas descartadas

- Google Forms: sencillo, pero menos adecuado para distribuir HTML personalizado por niveles.
- Google Sheets abierto al público: mayor exposición y más complejidad de permisos.
- Guardar directamente desde el HTML en Drive: no es posible sin un servicio intermedio.
- Aplicación instalada: se descarta por la variedad de dispositivos.
