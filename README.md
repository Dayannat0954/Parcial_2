
# 📊 Flujo de Trabajo para la Transformación y Análisis de Datos Tabulares

Este script de Python está diseñado para limpiar, reestructurar y analizar un archivo de datos tabulares, transformándolo de un formato ancho (columnas representando trimestres) a un formato largo o vertical (una fila por cada valor trimestral). Además, calcula y visualiza la suma total de valores por año.

## 🎯 Objetivo del Flujo de Trabajo

El objetivo principal es:

1.  **Limpiar la cabecera:** Crear un encabezado de columna significativo a partir de múltiples filas del archivo de origen.
2.  **Reestructurar los datos:** Convertir el *DataFrame* de un formato ancho (múltiples columnas de tiempo/trimestres) a un formato largo (una columna para el trimestre y otra para el valor). Este proceso se conoce como **derretir (*melt*)** el *DataFrame*.
3.  **Análisis Básico:** Calcular la suma total de la columna de valores por cada año.
4.  **Visualización y Exportación:** Generar un gráfico de la suma anual y guardar los resultados transformados en nuevos archivos Excel.

 🛠️ Dependencias

Este flujo de trabajo requiere las librerías **`pandas`** para la manipulación de datos y **`matplotlib`** para la visualización.

```python
import pandas as pd
import matplotlib.pyplot as plt
```


## 💻 Pasos Detallados del Flujo de Trabajo (Workflow)

A continuación, se describen los pasos ejecutados por el script para lograr la transformación de los datos:

### 1\. Carga Inicial y Limpieza de Encabezados

Se lee el archivo de datos de origen (`Datos_sucios.xlsx`) y se realiza una limpieza inicial de la cabecera.

  * **Carga de datos:** El *DataFrame* (`df`) se carga desde la ruta especificada.
  * **Creación del Nuevo Encabezado:** Se combinan los valores de las **filas 1 y 2** (`df.iloc[1]` y `df.iloc[2]`) para formar una cabecera más informativa (ej. "Trimestre 1 - 2018").
  * **Asignación:** Este nuevo *array* de encabezados se asigna a `df.columns`.
  * **Eliminación de Filas de Metadatos:** Se eliminan las filas que contenían los encabezados antiguos y otros metadatos (filas **0, 1, 2, 3, 4 y 6**) para que el *DataFrame* comience con los datos útiles. El índice se restablece.

-----

### 2\. Estandarización de Columnas Identificadoras

Una vez que los datos están limpios, se renombra la primera y la segunda columna para darles nombres estándar que serán usados como identificadores clave.

  * **Columna 1:** Se renombra a **`Unidade da Federação`** (Unidad de la Federación).
  * **Columna 2:** Se renombra a **`Tipo de inspeção`** (Tipo de inspección).

-----

### 3\. Transformación a Formato Largo (*Melting*)

Este es el paso crucial donde el *DataFrame* se reestructura de ancho a largo utilizando la función `melt()`.

  * **Definición de Variables:**
      * **`id_vars`:** Columnas identificadoras: `['Unidade da Federação', 'Tipo de inspeção']`.
      * **`value_vars`:** Columnas de valor (los trimestres) que se apilarán.
  * **Reestructuración:**
      * Los encabezados de las columnas en `value_vars` se mueven a una nueva columna llamada **`Trimestre`**.
      * Los valores correspondientes se apilan en una nueva columna llamada **`Valor`**.

-----

### 4\. Limpieza y Creación de la Columna Anual

Se limpia y estandariza la nueva columna `Valor` y se extrae el año del `Trimestre`.

  * **Extracción de Año:** Se utiliza una expresión regular para extraer el año de 4 dígitos de la columna `Trimestre` y se guarda en **`Ano`**.
  * **Conversión a Numérico:** La columna **`Valor`** se convierte al tipo de dato numérico, forzando a `NaN` cualquier valor no convertible.

-----

### 5\. Análisis y Visualización

Se realiza un análisis agregado y se genera un gráfico.

  * **Agregación:** Se agrupan los datos por la columna **`Ano`** y se calcula la **suma total** de la columna `Valor` para cada año, guardándose en `total_por_año`.
  * **Visualización:** Se genera un **gráfico de línea** mostrando la tendencia de la suma total de valores a lo largo de los años.

-----

### 6\. Exportación de Resultados

Finalmente, los *DataFrames* transformados se exportan a archivos Excel en las rutas especificadas.

| Archivo de Salida | Contenido |
| :--- | :--- |
| `datos_verticales.xlsx` | La tabla de datos reestructurada en formato largo/vertical. |
| `sumatoria_por_año.xlsx` | La tabla simple que contiene el total agregado de los valores por cada año. |
