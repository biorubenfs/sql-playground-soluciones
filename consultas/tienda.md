# Tienda Informática

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
SELECT p.nombre, p.precio, p.precio * 1.15
FROM producto p
```

5. Lista el nombre de los productos, el precio en euros y el precio en dólares estadounidenses (USD). Utiliza los siguientes alias para las columnas: nombre de producto, euros, dólares.

```
SELECT p.nombre AS "nombre de producto", p.precio AS euros, p.precio * 1.15 AS dólares
FROM producto p
```


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