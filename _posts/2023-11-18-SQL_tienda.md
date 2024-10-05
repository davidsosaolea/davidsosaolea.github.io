---
layout: single
title: "¡Diseñando la Base de Datos! 🚀"
excerpt: "El diseño de una base de datos es crucial para organizar, almacenar, y gestionar la información de manera eficiente. Una base de datos bien estructurada permite realizar consultas rápidas, garantiza la integridad de los datos, y facilita la escalabilidad del sistema. En una tienda, por ejemplo, la base de datos es el núcleo que gestiona las interacciones entre clientes, productos, ventas, inventario, y más."
date: 2024-10-04
classes: wide
header:
  teaser: assets/images/htb-writeup-sunedu/porcentaje.png
  teaser_home_page: true
  icon: /assets/images/hackthebox.webp
categories:
  - Python
  - SQL
tags:  
  - Python
  - SQL
  - PostgreSQL
---

### ¡Diseñando la Base de Datos! 🚀

Imagina que eres el dueño de una tienda fantástica, donde vendes productos tanto en línea como en una sucursal física. Tienes clientes felices, un inventario que se mueve rápido y empleados súper eficientes. Pero... ¡todo ese caos necesita organización! ¿Cómo puedes manejar todo eso sin perder la cabeza? La respuesta es simple: ¡una base de datos!

Aquí es donde entra en acción **PostgreSQL**, que será el cerebro detrás de tu tienda. Vamos a crear las tablas principales que organizarán todo: desde clientes hasta ventas, pasando por productos y sucursales.

### Paso 1: ¡Conoce a tus entidades!
Primero, identificamos a nuestros protagonistas (las tablas). Cada uno de ellos tiene su propio rol en el universo de tu tienda:
- **Clientes**: porque sin ellos no hay ventas.
- **Productos**: tu mercancía estrella.
- **Proveedores**: los que mantienen tu tienda abastecida.
- **Ventas**: el momento mágico en el que todo cobra sentido.
- **Detalle_Venta**: cada detalle cuenta.
- **Inventario**: ¡para que nunca te quedes sin stock!
- **Empleados** y **Sucursales**: porque tu equipo y tus tiendas también son clave.

### Paso 2: ¡Esquema de Tablas!
Ahora, vamos a darle vida a estas entidades con un poco de código SQL para que puedas crear la estructura de tu base de datos en PostgreSQL.

#### **Clientes**
```sql
CREATE TABLE Clientes (
    id_cliente SERIAL PRIMARY KEY,
    nombre VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    telefono VARCHAR(15),
    direccion TEXT,
    tipo_cliente VARCHAR(20) -- 'Online' o 'Físico'
);
```

#### **Productos**
```sql
CREATE TABLE Productos (
    id_producto SERIAL PRIMARY KEY,
    nombre VARCHAR(100),
    descripcion TEXT,
    precio_unitario DECIMAL(10, 2),
    stock INT,
    categoria VARCHAR(50),
    proveedor_id INT REFERENCES Proveedores(id_proveedor)
);
```

#### **Proveedores**
```sql
CREATE TABLE Proveedores (
    id_proveedor SERIAL PRIMARY KEY,
    nombre_proveedor VARCHAR(100),
    contacto VARCHAR(100), -- Teléfono o email del proveedor
    direccion TEXT
);
```

#### **Ventas**
```sql
CREATE TABLE Ventas (
    id_venta SERIAL PRIMARY KEY,
    fecha_venta TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    cliente_id INT REFERENCES Clientes(id_cliente),
    total DECIMAL(10, 2),
    tipo_venta VARCHAR(20) -- 'Online' o 'Físico'
);
```

#### **Detalle_Venta**
```sql
CREATE TABLE Detalle_Venta (
    id_detalle SERIAL PRIMARY KEY,
    id_venta INT REFERENCES Ventas(id_venta),
    id_producto INT REFERENCES Productos(id_producto),
    cantidad INT,
    precio_unitario DECIMAL(10, 2)
);
```

#### **Inventario**
```sql
CREATE TABLE Inventario (
    id_inventario SERIAL PRIMARY KEY,
    id_producto INT REFERENCES Productos(id_producto),
    cantidad INT,
    ubicacion VARCHAR(100) -- Ej: 'Almacén principal', 'Tienda física'
);
```

#### **Empleados**
```sql
CREATE TABLE Empleados (
    id_empleado SERIAL PRIMARY KEY,
    nombre_empleado VARCHAR(100),
    cargo VARCHAR(50),
    sucursal_id INT REFERENCES Sucursales(id_sucursal)
);
```

#### **Sucursales**
```sql
CREATE TABLE Sucursales (
    id_sucursal SERIAL PRIMARY KEY,
    nombre_sucursal VARCHAR(100),
    direccion_sucursal TEXT
);
```

### Paso 3: ¡El Toque Final!
Con estas tablas, ya tienes todo lo necesario para que tu tienda funcione como una máquina bien engrasada. Desde los clientes que compran, pasando por las ventas, hasta el control de inventario, todo estará al alcance de una consulta SQL.

¡Ahora tu tienda tiene un sistema organizado que te permitirá enfocarte en lo más importante: vender y hacer crecer tu negocio! 🎉

## __GITHUB__
En [mi github](https://github.com/davidsosaolea/){:target="_blank"} puedes encontrar el código de lectura de archivos PDF y el web scraping realizado para la página web de SUNEDU.