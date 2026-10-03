# Pipeline ETL — Exportaciones Provinciales del NEA

Este repositorio contiene el Trabajo Práctico Final de la Diplomatura en Data Analytics (UNNE). Consiste en un pipeline ETL automatizado, diseñado para extraer, transformar, validar y persistir los registros históricos de las provincias del Nordeste Argentino.

---

## 1. ¿Qué hace este pipeline?

El pipeline ejecuta un flujo de datos en tres etapas:

1. **Extract (`src/extract.py`):** Descarga y consume las series temporales oficiales en formato ancho desde las fuentes abiertas.
2. **Transform (`src/transform.py`):** 
   - Normaliza los datos de formato ancho a formato largo (*tidy data*).
   - Genera variables analíticas derivadas: década, región geopolítica de destino, porcentaje de participación sobre el total provincial, variación interanual porcentual y ranking por monto exportado (con bandera booleana para Top 3).
   - Enlaza (*left join*) la información de destinos con los rubros productivos principales (PP, MOA, MOI, CyE) y la representatividad de productos primarios.
3. **Load (`src/load.py`):**
   - Ejecuta controles de calidad automáticos: cantidad mínima esperada, esquema de 13 columnas, unicidad por tupla `(provincia, anio, destino)` y verificación de rangos de exportación plausibles.
   - Persiste tres artefactos de salida:
     - `data/processed/exportaciones_nea.csv`: Dataset final procesado (1.408 filas x 13 columnas).
     - `data/processed/resumen.json`: Ficha técnica descriptiva y metadatos con el estado de las validaciones.
     - `logs/pipeline.log`: Historial y registro de auditoría de cada ejecución del pipeline.

---

## 2. Instalación y ejecución

### Requisitos previos
- Python 3.8 o superior instalado.

### Instalación de dependencias
Clonar el repositorio y situarse en la raíz del proyecto:
```bash
git clone <URL_DE_TU_REPOSITORIO>
cd tp-final-etl
pip install -r requirements.txt

## 3. Fuentes de Datos
  Los datos crudos provienen de las publicaciones oficiales del Instituto Nacional de Estadisticas y Censos (INDEC) y la Dirección Nacional de Estadísticas del Sector Externo, abarcando las series temporales completas entre 1993 y 2024 para las provincias de Chaco, Corrientes, Formosa y Misiones.

  ## 4. Hallazgos en los datos
  Al explorar las 1.408 observaciones del dataset, se observa un cambio estructural en los socios comerciales del NEA a lo largo de las décadas: durante la década de 1990 y principios de los 2000, los destinos del Mercosur (principalmente Brasil) lideraban los primeros puestos del ranking provincial. Sin embargo, en la última década (2014-2024), China pasó a consolidarse de forma sostenida como el destino número uno para provincias como Chaco (alcanzando más de 110 MUSD en 2024, con una participación superior al 27% del total provincial), traccionado fuertemente por el rubro de Productos primarios (que explica más del 81% de su canasta exportadora).