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

![diagrama_entidad_relacion_tienda](../diagramas-entidad-relacion/tienda.png)

## Consultas sencillas

1. Lista el nombre de todos los productos que hay en la tabla `producto`.

```sql
SELECT p.nombre
FROM producto p
```

2. Lista los nombres y los precios de todos los productos de la tabla `producto`.

```sql
SELECT p.nombre, p.precio
FROM producto p
```

3. Lista todas las columnas de la tabla producto.

```sql
SELECT *
FROM producto
```

4. Lista el nombre de los productos, el precio en euros y el precio en dólares estadounidenses (USD).

```sql
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

```sql
SELECT f.Nombre, UPPER(SUBSTR(f.nombre, 1, 2))
FROM fabricante f
```

9. Lista los nombres y los precios de todos los productos de la tabla producto, redondeando el valor del precio.

```sql
SELECT nombre, ROUND(precio) AS 'Precio'
FROM producto
```

10. Lista los nombres y los precios de todos los productos de la tabla producto, truncando el valor del precio para mostrarlo sin ninguna cifra decimal.

```sql
SELECT nombre, TRUNCATE(precio, 0) AS 'Precio'
FROM producto
```

11. Lista el código de los fabricantes que tienen productos en la tabla producto.

```sql
SELECT id_fabricante
FROM producto
```

12. Lista el código de los fabricantes que tienen productos en la tabla producto, eliminando códigos repetidos.

```sql
SELECT id_fabricante
FROM producto
```

13. Lista los nombres de los fabricantes ordenados de forma ascendente.

```sql
SELECT nombre 
FROM fabricante
ORDER BY nombre ASC
```

14. Lista los nombres de los fabricantes ordenados de forma descendente.

```sql
SELECT nombre 
FROM fabricante
ORDER BY nombre DESC
```

15. Lista los nombres de los productos ordenados en primer lugar por el nombre de forma ascendente y en segundo lugar por el precio de forma descendente.

```sql
SELECT nombre 
FROM producto
ORDER BY nombre ASC, PRECIO DESC
```

16. Devuelve una lista con las 5 primeras filas de la tabla fabricante.

```sql
SELECT *
FROM fabricante
LIMIT 5
```

17. Devuelve una lista con 2 filas a partir de la cuarta fila de la tabla fabricante. La cuarta fila también se debe incluir en la respuesta.

```sql
SELECT *
FROM fabricante
LIMIT 3, 2
```

18. Lista el nombre y el precio del producto más barato. (Utilice solamente las cláusulas ORDER BY y LIMIT)

```sql
SELECT nombre, precio
FROM producto
ORDER BY precio ASC
LIMIT 1
```

19. Lista el nombre y el precio del producto más caro. (Utilice solamente las cláusulas ORDER BY y LIMIT)

```sql
SELECT nombre, precio
FROM producto
ORDER BY precio DESC
LIMIT 1
```

20. Lista el nombre de todos los productos del fabricante cuyo código de fabricante es igual a 2.

```sql
SELECT nombre 
FROM producto p
WHERE p.id_fabricante = 2
```

21. Lista el nombre de los productos que tienen un precio menor o igual a 120€.

```sql
SELECT nombre
FROM producto
WHERE precio <= 120
```

22. Lista el nombre de los productos que tienen un precio mayor o igual a 400€.

```sql
SELECT nombre
FROM producto
WHERE precio >= 400
```

23. Lista el nombre de los productos que no tienen un precio mayor o igual a 400€.

```sql
SELECT nombre
FROM producto
WHERE NOT (precio >= 400)
```

24. Lista todos los productos que tengan un precio entre 80€ y 300€. Sin utilizar el operador BETWEEN.

```sql
SELECT nombre
FROM producto
WHERE precio > 80 AND precio < 300
```

25. Lista todos los productos que tengan un precio entre 60€ y 200€. Utilizando el operador BETWEEN.

```sql
SELECT nombre
FROM producto
WHERE precio BETWEEN 60 AND 200
```

26. Lista todos los productos que tengan un precio mayor que 200€ y que el código de fabricante sea igual a 6.

```sql
SELECT p.nombre
FROM producto p
WHERE p.precio > 200 AND p.id_fabricante = 6
```

27. Lista todos los productos donde el código de fabricante sea 1, 3 o 5. Sin utilizar el operador IN.

```sql
SELECT p.nombre
FROM producto p
WHERE p.id_fabricante = 1 OR p.id_fabricante = 3 OR p.id_fabricante = 5
```

28. Lista todos los productos donde el código de fabricante sea 1, 3 o 5. Utilizando el operador IN.

```sql
SELECT p.nombre
FROM producto p
WHERE p.id_fabricante IN (1, 3, 5)
```

29. Lista el nombre y el precio de los productos en céntimos (Habrá que multiplicar por 100 el valor del precio). Cree un alias para la columna que contiene el precio que se llame céntimos.

```sql
SELECT nombre, (precio * 100) AS Céntimos
FROM producto 
```

30. Lista los nombres de los fabricantes cuyo nombre empiece por la letra S.

```
SELECT nombre 
FROM fabricante 
WHERE nombre LIKE "s%"
```

31. Lista los nombres de los fabricantes cuyo nombre termine por la vocal e.

```sql
SELECT nombre 
FROM fabricante 
WHERE nombre LIKE "%e"
```

32. Lista los nombres de los fabricantes cuyo nombre contenga el carácter w.

```sql
SELECT nombre 
FROM fabricante 
WHERE nombre LIKE "%w%"
```

33. Lista los nombres de los fabricantes cuyo nombre sea de 4 caracteres.

```sql
SELECT nombre 
FROM fabricante 
WHERE LENGTH(nombre) = 4
```

34. Devuelve una lista con el nombre de todos los productos que contienen la cadena Portátil en el nombre.

```sql
SELECT nombre 
FROM producto 
WHERE nombre LIKE "%Portátil%"
```

35. Devuelve una lista con el nombre de todos los productos que contienen la cadena Monitor en el nombre y tienen un precio inferior a 215 €.

```sql
SELECT nombre 
FROM producto 
WHERE nombre LIKE "%Monitor%" AND precio < 215
```

36. Lista el nombre y el precio de todos los productos que tengan un precio mayor o igual a 180€. Ordene el resultado en primer lugar por el precio (en orden descendente) y en segundo lugar por el nombre (en orden ascendente).

```sql
SELECT nombre, precio
FROM producto
WHERE precio >= 180
ORDER BY precio DESC, nombre ASC
```

## Composición interna

1. Devuelve una lista con el nombre del producto, precio y nombre de fabricante de todos los productos de la base de datos.

```sql
SELECT p.nombre, p.precio, f.nombre
FROM producto p
INNER JOIN fabricante f ON f.id=p.id_fabricante
```

2. Devuelve una lista con el nombre del producto, precio y nombre de fabricante de todos los productos de la base de datos. Ordene el resultado por el nombre del fabricante, por orden alfabético.

```sql
SELECT p.nombre, p.precio, f.nombre
FROM producto p
INNER JOIN fabricante f ON f.id=p.id_fabricante
ORDER BY f.nombre ASC
```

3. Devuelve una lista con el identificador del producto, nombre del producto, identificador del fabricante y nombre del fabricante, de todos los productos de la base de datos.

```sql
SELECT p.id, p.nombre, f.id, f.nombre
FROM producto p
INNER JOIN fabricante f ON f.id=p.id_fabricante
```

4. Devuelve el nombre del producto, su precio y el nombre de su fabricante, del producto más barato.

```sql
SELECT p.nombre, p.precio, f.nombre
FROM producto p
INNER JOIN fabricante f ON f.id=p.id_fabricante
ORDER BY p.precio
LIMIT 1
```

5. 

## Composición externa

## Consultas resumen

## Subconsultas

## Tablas

### Producto

| id  | producto                            | precio  | id_fabricante |
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
