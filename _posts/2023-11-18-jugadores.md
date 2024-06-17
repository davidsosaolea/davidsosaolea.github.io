---
layout: single
title: "Transformación y Carga de Datos desde Google Sheets a Power BI usando Python"
excerpt: "En este artículo, te mostraré cómo realicé un proyecto de ETL (Extract, Transform, Load) para convertir datos almacenados en Google Sheets a un formato adecuado para su análisis en Power BI. Este proceso implica la extracción de datos, su transformación y limpieza utilizando Python, y la posterior creación de un archivo CSV que se carga en Power BI para la creación de dashboards interactivos."

classes: wide
header:
  teaser: assets/images/htb-writeup-whatsapp/wa.jpg
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - Python
  
tags:  
  - Python
  - Power BI
  - Google Sheets

---

## Transformación y Carga de Datos desde Google Sheets a Power BI usando Python

### Introducción

En este artículo, te mostraré cómo realicé un proyecto de ETL (Extract, Transform, Load) para convertir datos almacenados en Google Sheets a un formato adecuado para su análisis en Power BI. Este proceso implica la extracción de datos, su transformación y limpieza utilizando Python, y la posterior creación de un archivo CSV que se carga en Power BI para la creación de dashboards interactivos.

### Objetivos del Proyecto

1. **Extracción de datos**: Obtener datos desde Google Sheets usando Python.
2. **Transformación y limpieza de datos**: Procesar y limpiar los datos para asegurar su calidad.
3. **Carga de datos**: Exportar los datos transformados a un archivo CSV.
4. **Visualización de datos**: Importar el archivo CSV a Power BI y crear un dashboard interactivo.

### Herramientas y Tecnologías Utilizadas

- **Python**: Para la extracción, transformación y limpieza de los datos.
- **Google Sheets API**: Para acceder y extraer datos desde Google Sheets.
- **Pandas**: Para la manipulación y análisis de datos.
- **Power BI**: Para la visualización de datos.
- **Google Cloud Platform**: Para la configuración de credenciales y acceso a la API de Google Sheets.

### Paso a Paso del Proceso ETL

#### 1. Configuración del Entorno

Antes de comenzar, asegúrate de tener Python y las bibliotecas necesarias instaladas. Puedes instalar las bibliotecas necesarias con pip:

```bash
pip install pandas gspread oauth2client
```

#### 2. Acceso a Google Sheets

Para acceder a Google Sheets, necesitas configurar un proyecto en Google Cloud Platform y habilitar la API de Google Sheets. Luego, descarga las credenciales JSON y guárdalas en tu proyecto.

```python
import gspread
from oauth2client.service_account import ServiceAccountCredentials

# Configuración de la conexión con Google Sheets
scope = ["https://spreadsheets.google.com/feeds", 'https://www.googleapis.com/auth/spreadsheets',
         "https://www.googleapis.com/auth/drive.file", "https://www.googleapis.com/auth/drive"]

creds = ServiceAccountCredentials.from_json_keyfile_name('path/to/creds.json', scope)
client = gspread.authorize(creds)

# Acceso al documento de Google Sheets
sheet = client.open("Nombre del Documento").sheet1

# Extracción de datos
data = sheet.get_all_records()
```

#### 3. Transformación y Limpieza de Datos

Con los datos extraídos, utilizamos Pandas para transformar y limpiar los datos.

```python
import pandas as pd

# Convertir los datos a un DataFrame de Pandas
df = pd.DataFrame(data)

# Ejemplo de limpieza: eliminar filas con valores nulos
df = df.dropna()

# Ejemplo de transformación: convertir una columna a un tipo de datos específico
df['Fecha'] = pd.to_datetime(df['Fecha'])

# Más transformaciones y limpiezas según sea necesario
```

#### 4. Exportación a CSV

Una vez que los datos están limpios y transformados, los exportamos a un archivo CSV.

```python
# Exportar el DataFrame a un archivo CSV
df.to_csv('datos_limpios.csv', index=False)
```

#### 5. Creación del Dashboard en Power BI

1. **Importar el archivo CSV**: Abre Power BI y selecciona la opción para importar datos desde un archivo CSV.
2. **Crear visualizaciones**: Utiliza las herramientas de Power BI para crear visualizaciones y dashboards interactivos.

### Resultados

- Puedes ver el código completo del proyecto en mi repositorio de en mi [Github.](https://github.com/davidsosaolea/Player_poker/blob/main/Jugadores_de_poker.ipynb){:target="_blank"}

- El dashboard final creado en Power BI está disponible [AQUI.](https://app.powerbi.com/view?r=eyJrIjoiOTcyZTA3Y2MtNjE2Zi00N2M1LWJmNjgtNTZmN2I5NzRjZTQ2IiwidCI6Ijc1MDRlMzE4LThlMWUtNGQ1NS1iZmZkLTg3NWI0ZGVlODI2MCIsImMiOjR9 ){:target="_blank"}

### Conclusión

En este proyecto, hemos recorrido todo el proceso de ETL utilizando Python, desde la extracción de datos desde Google Sheets, pasando por su transformación y limpieza, hasta la carga de los datos en Power BI para la creación de dashboards. Este flujo de trabajo es esencial para cualquier analista de datos, ya que garantiza que los datos sean precisos y estén listos para ser analizados de manera efectiva.

### Referencias

- [Google Sheets API Documentation](https://developers.google.com/sheets/api){:target="_blank"}
- [Pandas Documentation](https://pandas.pydata.org/){:target="_blank"}
- [Power BI Documentation](https://docs.microsoft.com/en-us/power-bi/){:target="_blank"}
