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

## Microsoft SQL Server

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

## Python

Utilizaré Python en Visual Studio Code, junto con la biblioteca Pandas, para analizar y procesar datos. Antes de comenzar el análisis, verificaré la correcta carga de nuestros datos, asegurándome de que estén listos para su manipulación y exploración.

![](/assets/images/htb-writeup-delivery/website1.png)
