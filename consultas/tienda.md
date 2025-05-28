# Tienda Informática

## Contenido

  - [Diagrama-ER](#diagrama-er)
  - [Consultas sencillas](#consultas-sencillas)
  - [Composición interna](#composición-interna)
  - [Composición externa](#composición-externa)
  - [Consultas resumen](#consultas-resumen)
  - [Subconsultas](#subconsultas)
  - [Anexo: Tablas](#tablas)

## Diagrama ER

![diagrama_entidad_relacion_tienda](../diagramas-entidad-relación/tienda.png)

## Consultas sencillas

1. Lista el nombre de todos los productos que hay en la tabla `producto`.

```sql
SELECT p.nombre
FROM producto p
```

2. Lista los nombres y los precios de todos los productos de la tabla `producto`.

```
SELECT p.nombre, p.precio
FROM producto p
```

3. Lista todas las columnas de la tabla producto.

```sql
SELECT *
FROM producto
```

4. Lista el nombre de los productos, el precio en euros y el precio en dólares estadounidenses (USD).

```
SELECT p.nombre, p.precio AS EUROS, p.precio*1.15 AS DOLARES
FROM producto p
```

5. Lista el nombre de los productos, el precio en euros y el precio en dólares estadounidenses (USD). Utiliza los siguientes alias para las columnas: nombre de producto, euros, dólares.

```sql
SELECT p.nombre AS NOMBRE_PRODUCTO, p.precio AS EUROS, p.precio*1.15 AS DOLARES
FROM producto p
```

6. Lista los nombres y los precios de todos los productos de la tabla producto, convirtiendo los nombres a mayúscula.

```sql
SELECT UPPER(p.nombre), p.precio
FROM producto p
```

7. Lista los nombres y los precios de todos los productos de la tabla producto, convirtiendo los nombres a minúscula.

```sql
SELECT LOWER(p.nombre), p.precio
FROM producto p
```

8. Lista el nombre de todos los fabricantes en una columna, y en otra columna obtenga en mayúsculas los dos primeros caracteres del nombre del fabricante.


## Composición interna

1. Devuelve una lista con el nombre del producto, precio y nombre de fabricante de todos los productos de la base de datos.

```
SELECT p.nombre, p.precio, f.nombre
FROM producto p
INNER JOIN fabricante f ON f.id=p.id_fabricante
```

## Composición externa

## Consultas resumen

## Subconsultas

## Tablas

### Producto

| id  | producto                            | precio  | stock |
|-----|-------------------------------------|---------|--------|
| 1   | Disco duro SATA3 1TB                | 86.99   | 5      |
| 2   | Memoria RAM DDR4 8GB                | 120     | 6      |
| 3   | Disco SSD 1 TB                      | 150.99  | 4      |
| 4   | GeForce GTX 1050Ti                  | 185     | 7      |
| 5   | GeForce GTX 1080 Xtreme             | 755     | 6      |
| 6   | Monitor 24 LED Full HD              | 202     | 1      |
| 7   | Monitor 27 LED Full HD              | 245.99  | 1      |
| 8   | Portátil Yoga 520                   | 559     | 2      |
| 9   | Portátil Ideapd 320                 | 444     | 2      |
| 10  | Impresora HP Deskjet 3720           | 59.99   | 3      |
| 11  | Impresora HP Laserjet Pro M26nw     | 180     | 3      |


### Fabricante
| id  | nombre           |
|-----|------------------|
| 1   | Asus             |
| 2   | Lenovo           |
| 3   | Hewlett-Packard  |
| 4   | Samsung          |
| 5   | Seagate          |
| 6   | Crucial          |
| 7   | Gigabyte         |
| 8   | Huawei           |
| 9   | Xiaomi           |
