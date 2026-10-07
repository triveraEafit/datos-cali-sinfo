# Lago de datos — Cali · Taller de Datos (Entrega 1)

Grupo: Tomas Gañan Rivera y David Ramirez ·
Repositorio: https://github.com/triveraEafit/datos-cali-sinfo

Este `lago/` acompaña el Excel `Diccionario_de_datos_TallerDatos_Cali.xlsx` y el
documento `Preguntas_por_que_estos_datos_Cali.docx`. Todo sale del portal de
datos abiertos de la Alcaldía de Santiago de Cali (https://datos.cali.gov.co) y
se documenta con su ID de fuente (F01, F02…) igual que en el Excel.

## Qué hay en cada carpeta

| Carpeta | Archivo | Fuente | En el lago |
|---|---|---|---|
| `seguridad/` | `homicidios_comuna_1993_2021.csv` | F01 | **Completo:** 1.392 filas (comuna × año × sexo) |
| `movilidad/` | `lesionados_transito_2016_2025.csv` | F02 | **Completo:** 51.236 filas |
| `educacion/` | `programas_etdh.csv` | F04 | **Completo:** 2.427 filas |
| `demografia/` | `poblacion_censada_historica.csv` | F09 | **Completo:** 9 filas |
| `ambiente/` | `calidad_aire_ica.csv` | F03 | **Completo:** 3.926 filas (una por día) |

Cada columna de cada archivo tiene su fila en la hoja "2. Diccionario" del Excel.

## Advertencia sobre F01 (homicidios): 2019–2021 no son creíbles

En la fuente, el total de la ciudad pasa de 1.170 homicidios en 2018 a 15.647
en 2019. En las comunas 2 a 22, todas las cifras de 2019, 2020 y 2021 son
múltiplos exactos del número de la comuna, como si se hubieran multiplicado
por él. **No se corrigieron:** quedaron como vienen y marcadas con
`dato_dudoso = 1`. Para comparar en el tiempo, usar solo 1993–2018
(`dato_dudoso = 0`).

## Qué se limpió en F01, F03, F04 y F09

- **F03:** la serie es diaria, del 2010-01-01 al 2020-09-30. La fuente trae día
  y mes invertidos cuando el día es ≤ 12 (el 2 de enero aparece como
  `2010-02-01`); se corrigieron 1.419 fechas y se comprobó contra el orden del
  archivo. `ND` y `NA` → vacío. Se corrigió el encabezado `Cañav eralejo`.
- **F01:** la fuente trae una columna por año y sexo (87 columnas); se pasó a
  una fila por comuna, año y sexo. `SD` y celdas vacías → vacío. Se quitaron la
  fila «Total» y los totales por año, que se recalculan sumando.
- **F04:** solo se quitó la columna `_id`. El texto viene todo en minúsculas.
- **F09:** se quitó el punto de miles (`637.929` → `637929`) y el símbolo `%`;
  `ND` → vacío. La columna «Día» de la fuente repite la tasa de crecimiento en
  vez del día del censo, y se quitó.

## Qué se limpió en F02 (lesionados en tránsito)

- Codificación a UTF-8 y separador de `;` a `,`.
- **Fechas:** la fuente trae día/mes/año hasta el 6 de septiembre de 2025 y
  mes/día/año desde el 7. Se unificó a `AAAA-MM-DD`.
- **Horas:** venían en varios formatos (24 h, a. m./p. m., con `-05` al final,
  como fracción de día de Excel). Se unificó a `HH:MM`.
- `.` de la fuente → vacío.
- Se quitó la columna con la placa del agente de tránsito y la primera columna
  `Tipo_Confirmado`, que vale «Con lesionado» en todas las filas.
- **Datos de personas:** en 15 direcciones la fuente traía escritos nombre y
  documento de lesionados, un teléfono, placas de vehículos o número de agente.
  Se borró esa parte y se dejó solo la dirección. La revisión fue por búsqueda
  de patrones, no fila por fila.
- No se tocó: las categorías de `tipo_incidente` cambian en 2024; la dirección
  viene vacía en 2018–2020; las columnas de actor vial solo se llenan desde 2021.

## Fuentes que no están en el lago

- **F05** (trata de personas) y **F06** (líderes sociales amenazados):
  descartadas por decisión del grupo. No traen nombre, pero sí una fila por
  persona, y con tan pocos casos la combinación de campos puede señalar a
  alguien. Ver la pregunta 5 del documento.
- **F07** (POT — suelo de expansión urbana): candidata. La descarga devolvió
  error 403 a la herramienta automática; falta abrirla en el navegador.
- **F08** (vistas de Wikipedia del artículo «Cali»): no responde desde la
  herramienta de verificación; falta abrirla en un navegador.
- **F10** (GTFS del sistema MIO): descartada, no se encontró descarga oficial.
