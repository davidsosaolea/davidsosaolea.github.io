---
layout: single
title: EDA de las ventas en un aserradero
excerpt: "En el mundo empresarial, la capacidad de comprender y utilizar eficazmente los datos es esencial para el éxito y el crecimiento de una organización. En este sentido, el análisis exploratorio de datos (EDA, por sus siglas en inglés) se ha convertido en una herramienta fundamental para examinar y extraer información valiosa de los conjuntos de datos.
El objetivo de este estudio es realizar un análisis exploratorio de datos de las ventas en un aserradero, con el fin de descubrir información relevante que pueda respaldar la toma de decisiones estratégicas y operativas."
date: 2023-06-24
classes: wide
header:
  teaser: assets/images/htb-writeup-EDA-aserradero/EDA.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - Python
  - SQL
tags:  
  - Python
  - Microsoft SQL Server

---

![](/assets/images/htb-writeup-EDA-aserradero/EDA.png)

El EDA nos permitirá examinar las ventas en diferentes dimensiones, como la cantidad de productos vendidos, los ingresos generados, los clientes y los patrones estacionales. Además, exploraremos posibles relaciones entre las ventas y variables externas, como el precio de la madera en el mercado, eventos específicos o cambios en la demanda del mercado.

A través del análisis exploratorio de datos de ventas en un aserradero, esperamos identificar oportunidades de mejora, patrones de demanda y áreas donde se pueden implementar estrategias para maximizar la eficiencia operativa y optimizar los resultados financieros. El conocimiento obtenido a partir de este análisis proporcionará información valiosa para los equipos de gestión, ventas y producción, ayudándoles a tomar decisiones fundamentadas y a adaptar sus estrategias en un mercado en constante cambio.

## __Microsoft SQL Server__

En este estudio, nos enfocaremos en analizar las ventas de productos en un aserradero utilizando datos almacenados en tablas dentro de una base de datos en SQL Server. El primer paso será comprender dónde se encuentran los datos, para identificar cuáles son relevantes y útiles para nuestro análisis.

Al explorar las tablas en la base de datos, buscaremos aquellas que contengan información relacionada con las ventas, como la cantidad de productos vendidos, los ingresos generados y los datos de los clientes. Al identificar estas tablas, podremos determinar qué variables son más relevantes para nuestro análisis y nos proporcionarán información valiosa sobre el desempeño de las ventas en el aserradero.

Nuestos datos se encuentran en tres tablas "ARTICULO", "CAT_CUBICACION_DETALLE" y "CAT_CUBICACION_CABECERA"

```
SELECT detalle.[pies],
       detalle.[precio_venta],
       detalle.[precio_costo],
       cabecera.[fecha],
       cabecera.[numero_doc],
       cabecera.[estado],
	   cabecera.[motivo],
	   cabecera.[punto],
	   cabecera.[total_doc],
       articulo.[descripcion_larga]
FROM [SYSDA_LESLY].[dbo].[CAT_CUBICACION_DETALLE] detalle
JOIN [SYSDA_LESLY].[dbo].[CAT_CUBICACION_CABECERA] cabecera
    ON detalle.[cod_cubicacion_d] = cabecera.[cod_cubicacion_c]
JOIN [SYSDA_LESLY].[dbo].[ARTICULO] articulo
    ON detalle.[cod_articulo] = articulo.[codigo_interno]
WHERE cabecera.[estado] = 1
  AND cabecera.[motivo] = 'V'
  AND cabecera.[punto] = 1
```

1. La cláusula SELECT selecciona las siguientes columnas de las tablas involucradas:
   * detalle.[pies]
   * detalle.[precio_venta]
   * detalle.[precio_costo]
   * cabecera.[fecha]
   * cabecera.[numero_doc]
   * cabecera.[estado]
   * cabecera.[motivo]
   * cabecera.[punto]
   * cabecera.[total_doc]
   * articulo.[descripcion_larga]
<br><br>
2. La cláusula FROM especifica las tres tablas involucradas en la consulta: CAT_CUBICACION_DETALLE, CAT_CUBICACION_CABECERA y ARTICULO. Se les asignan alias para facilitar su referencia en la consulta (detalle, cabecera y articulo respectivamente).  
<br>
3. Los JOINs se utilizan para combinar los registros de las tablas según las siguientes condiciones:

   * ON detalle.[cod_cubicacion_d] = cabecera.[cod_cubicacion_c]: Combina los registros donde el código de cubicación en la tabla de detalle coincide con el código de cubicación en la tabla de cabecera.
   * ON detalle.[cod_articulo] = articulo.[codigo_interno]: Combina los registros donde el código de artículo en la tabla de detalle coincide con el código interno en la tabla de artículo.
<br><br>
4. La cláusula WHERE filtra los resultados según las siguientes condiciones:

   * cabecera.[estado] = 1: Solo se seleccionan los registros donde el estado en la tabla de cabecera es igual a 1.
   * cabecera.[motivo] = 'V': Solo se seleccionan los registros donde el motivo en la tabla de cabecera es igual a 'V'.
   * cabecera.[punto] = 1: Solo se seleccionan los registros donde el punto en la tabla de cabecera es igual a 1.   

## __Python__
## Limpieza de datos

Utilizaré Python en Visual Studio Code, junto con la biblioteca Pandas, para analizar y procesar datos. Antes de comenzar el análisis, verificaré la correcta carga de nuestros datos, asegurándome de que estén listos para su manipulación y exploración.

![](/assets/images/htb-writeup-EDA-aserradero/df1.png)

- precio_costo -> precio de compra
- fecha        -> fecha en la que se realizó la venta
- numero_doc   -> indentificador seriado de la venta
- estado       -> documento realizada(1), documento anulada(2)
- motivo       -> venta(V), cotizacion(C)
- punto        -> lugar de la venta
- total_doc    -> monto total de la venta por documento
- descripcion_larga -> descripcion del articulo

Crearemos una columna adicional en nuestro dataframe para calcular el importe unitario de cada artículo, el cual se calcula a partir de los pies tablares y el precio de venta.

![](/assets/images/htb-writeup-EDA-aserradero/importe.bmp)

Verificamos que nuestros datos tengan el formato correcto.

![](/assets/images/htb-writeup-EDA-aserradero/tipo.png)

Cambiamos el formato a la columna fecha que es de tipo object por tipo fecha usando:

```
df1['fecha'] = pd.to_datetime(df1['fecha'])
```

Con la ayuda de Matplotlib, graficamos nuestras ventas según la fecha. Sin embargo, se observa una inconsistencia, ya que las ventas comenzaron a registrarse en 2011 y no desde el 2008 como se muestra en la grafica.

![](/assets/images/htb-writeup-EDA-aserradero/output.png)

![](/assets/images/htb-writeup-EDA-aserradero/df2.png)

Una vez identificados los datos erroneos, procedemos a eliminar las filas con estos datos usando:
```
df1 = df1.drop([121283, 121284]).reset_index(drop=True)
```

En la exploración de nuestro conjunto de datos, buscamos valores nulos y encontramos algunos en la columna de descripción. Para solucionarlo, procedimos a eliminarlos usando.

```
df3 = df3.dropna()
```

![](/assets/images/htb-writeup-EDA-aserradero/df3.png)

En la lista de productos vendidos, encontramos productos distintos a la venta de madera. Sin embargo, hemos decidido dejar de lado estos productos en este análisis, ya que son productos descontinuados o de oferta limitada por temporadas.

![](/assets/images/htb-writeup-EDA-aserradero/df4.png)

Creamos una lista con todos estos valores que no son necesarios y eliminamos.

![](/assets/images/htb-writeup-EDA-aserradero/df5.png)

Una vez eliminados los valores nulos, aún encontramos artículos con nombres como 'CATAHUA 78', 'CATAHUA 79', 'TORNILLO PTO BLANCO 81', 'CATAHUA 83', que parecen hacer referencia a proveedores o fecha de llegada en lugar de describir los productos en sí.

Con el uso de expresiones regulares, podemos reemplazar cada artículo por el nombre de la madera a la que pertenecen, evitando hacer referencia al proveedor o a la fecha de llegada.

![](/assets/images/htb-writeup-EDA-aserradero/df6.png)

## Exploración de datos

Con los datos ya limpios, podemos avanzar con la exploración de datos para obtener insights y comprender mejor el comportamiento de nuestras ventas en el aserradero.

Realizaremos análisis descriptivos, como la generación de estadísticas resumidas y la identificación de patrones y tendencias en las ventas. Utilizaremos gráficos y visualizaciones para visualizar la distribución de las ventas en función de variables clave, como el tiempo, los productos o los clientes.

Además, evaluaremos la relación entre las ventas y otras variables relevantes, como el precio de venta, el costo de los productos y el punto de venta. Esto nos ayudará a identificar factores que influyen en el desempeño de las ventas y a tomar decisiones informadas para mejorar la rentabilidad y eficiencia del aserradero.

El análisis exploratorio de datos nos permitirá descubrir información valiosa sobre las ventas en el aserradero, como los productos más populares, los períodos de mayor demanda, los segmentos de clientes más rentables, entre otros. Estos hallazgos nos ayudarán a definir estrategias de marketing, ajustar los niveles de inventario y optimizar la producción.

En nuestro caso la exploración de datos, también analizaremos la distribución de las ventas por mes y por día de la semana. Esto nos permitirá identificar patrones estacionales y tendencias en el comportamiento de las ventas.

![](/assets/images/htb-writeup-EDA-aserradero/venta.png)

El gráfico nos muestra un incremento en las ventas hacia finales del 2020 y el 2021. Sin embargo, en cuanto a la cantidad de producto vendido, no se observa una diferencia notable. Esto podría deberse a un aumento en los precios de venta, lo que resultó en mayores ingresos sin necesariamente reflejar un aumento significativo en la cantidad de productos vendidos.

![](/assets/images/htb-writeup-EDA-aserradero/catidad.png)

asdasd
<iframe src="https://github.com/davidsosaolea/davidsosaolea.github.io/blob/master/_includes/mi_grafico_interactivo.html" height="500" width="100%"></iframe>