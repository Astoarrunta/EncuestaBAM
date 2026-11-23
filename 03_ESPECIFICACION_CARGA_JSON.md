# Especificación del fichero de carga JSON

El fichero inicial contiene una lista de objetos. Cada objeto representa un centro.

## Campos

| Campo | Tipo | Obligatorio | Descripción |
|---|---|---:|---|
| `nombre_campana` | texto | Sí | Nombre de la campaña |
| `codigo_zona` | número | Sí | Código de zona |
| `nombre_zona` | texto | Sí | Nombre de zona |
| `codigo_division` | número | Sí | Código de división |
| `nombre_division` | texto | Sí | Nombre de división |
| `id_centro` | número | Sí | Identificador numérico del centro |
| `direccion_centro` | texto | Sí | Dirección del centro |
| `codigo_postal` | texto | Sí | Código postal |
| `municipio` | texto | Sí | Municipio |
| `ensena_centro` | texto | Sí | Enseña o marca |
| `nombre_coordinador` | texto | Sí | Persona u organización coordinadora |
| `correo_coordinador` | texto | No | Correo del coordinador |
| `telefono_coordinador` | texto | No | Teléfono del coordinador |

## Reglas

- `id_centro`, `codigo_zona` y `codigo_division` son numéricos.
- El código postal y el teléfono se conservan como texto para no perder símbolos o ceros.
- Un coordinador puede tener más de un centro.
- El JSON de carga contiene toda la información necesaria para generar las distribuciones.
- No es necesario añadir manualmente los datos de destinatarios en cada distribución.

## Ejemplo

```json
[
  {
    "nombre_campana": "GRAN RECOGIDA 2026",
    "codigo_zona": 5,
    "nombre_zona": "OESTE",
    "codigo_division": 3,
    "nombre_division": "POZUELO-MADRID",
    "id_centro": 40276,
    "direccion_centro": "CAMINO DE LA ZARZUELA, 3",
    "codigo_postal": "28023",
    "municipio": "Madrid",
    "ensena_centro": "Mercadona",
    "nombre_coordinador": "COLEGIO RETAMAR",
    "correo_coordinador": "alberto.barrera@retamar.es",
    "telefono_coordinador": "+34917141922"
  }
]
```
