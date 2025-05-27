# Nombre de la Base de Datos

## Contenido

  - [Diagrama-ER](#diagrama-er)
  - [Consultas sencillas](#consultas-sencillas)
  - [Composición interna](#composición-interna)
  - [Composición externa](#composición-externa)
  - [Consultas resumen](#consultas-resumen)
  - [Subconsultas](#subconsultas)
  - [Anexo: Tablas](#tablas)

## Diagrama ER
![diagrama_entidad_relacion_tienda](../diagramas-entidad-relación/ventas.png)

## Consultas sencillas

1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.

```sql
SELECT *
FROM pedido p
ORDER BY p.fecha DESC
```

2. Lista los nombres y los precios de todos los productos de la tabla producto.

```sql
SELECT p.nombre, p.precio
FROM producto p
```

## Composición interna

1. Devuelve un listado con el identificador, nombre y los apellidos de todos los clientes que han realizado algún pedido. El listado debe estar ordenado alfabéticamente y se deben eliminar los elementos repetidos.

```sql
SELECT DISTINCT c.id, c.apellido1, c.apellido2, c.nombre
FROM cliente c
INNER JOIN pedido p ON p.id_cliente=c.id
ORDER BY c.apellido1
```

2. Devuelve un listado que muestre todos los pedidos que ha realizado cada cliente. El resultado debe mostrar todos los datos de los pedidos y del cliente. El listado debe mostrar los datos de los clientes ordenados alfabéticamente.

```sql
SELECT c.id, c.nombre, c.apellido1, p.id AS ID_PEDIDO
FROM cliente c
INNER JOIN pedido p ON c.id=p.id_cliente
ORDER BY c.apellido1
```

3. Devuelve un listado que muestre todos los pedidos en los que ha participado un comercial. El resultado debe mostrar todos los datos de los pedidos y de los comerciales. El listado debe mostrar los datos de los comerciales ordenados alfabéticamente.

```sql
SELECT *
FROM comercial c
INNER JOIN pedido p ON c.id=p.id_comercial
ORDER BY c.apellido1 ASC
```

4. Devuelve un listado que muestre todos los clientes, con todos los pedidos que han realizado y con los datos de los comerciales asociados a cada pedido.

```sql
SELECT *
FROM cliente cl
INNER JOIN pedido p ON p.id_cliente=cl.id
INNER JOIN comercial co ON co.id=p.id_comercial
```

5. Devuelve un listado de todos los clientes que realizaron un pedido durante el año 2017, cuya cantidad esté entre 300 € y 1000 €

```sql
SELECT DISTINCT cl.id, cl.nombre 
FROM pedido p
INNER JOIN cliente cl ON p.id_cliente=cl.id
WHERE YEAR(p.fecha) = 2017 AND p.total BETWEEN 300 AND 1000
```

6. Devuelve el nombre y los apellidos de todos los comerciales que ha participado en algún pedido realizado por María Santana Moreno.

```sql
SELECT DISTINCT  com.apellido1, com.apellido2, com.nombre
FROM comercial com
INNER JOIN pedido p ON p.id_comercial=com.id
INNER JOIN cliente cl ON p.id_cliente=cl.id
WHERE cl.nombre="María" AND cl.apellido1="Santana" AND cl.apellido2="Moreno"
```

7. Devuelve el nombre de todos los clientes que han realizado algún pedido con el comercial Daniel Sáez Vega

```sql
SELECT DISTINCT cl.apellido1, cl.apellido2, cl.nombre
FROM cliente cl
INNER JOIN pedido p ON p.id_cliente=cl.id
INNER JOIN comercial com ON p.id_comercial=com.id
WHERE com.nombre="Daniel" AND com.apellido1="Sáez" AND com.apellido2="Vega"
```

## Composición externa

1. Devuelve un listado con todos los clientes junto con los datos de los pedidos que han realizado. Este listado también debe incluir los clientes que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los clientes.

```sql
SELECT cl.apellido1, cl.apellido2, cl.nombre, p.id AS ID_PEDIDO
FROM cliente cl
LEFT JOIN pedido p ON p.id_cliente=cl.id
ORDER BY cl.apellido1 ASC, cl.apellido2 ASC, cl.nombre ASC
```

2. Devuelve un listado con todos los comerciales junto con los datos de los pedidos que han realizado. Este listado también debe incluir los comerciales que no han realizado ningún pedido. El listado debe estar ordenado alfabéticamente por el primer apellido, segundo apellido y nombre de los comerciales.

```sql
SELECT com.apellido1, com.apellido2, com.nombre, p.id AS ID_PEDIDO
FROM comercial com
LEFT JOIN pedido p ON p.id_comercial=com.id
ORDER BY com.apellido1 ASC, com.apellido2 ASC, com.nombre ASC
```

3. Devuelve un listado que solamente muestre los clientes que no han realizado ningún pedido.

```sql
SELECT *
FROM cliente cl
LEFT JOIN pedido p ON p.id_cliente=cl.id
WHERE p.id IS NULL
```

4. Devuelve un listado que solamente muestre los comerciales que no han realizado ningún pedido.

```sql
SELECT *
FROM comercial com
LEFT JOIN pedido p ON p.id_comercial=com.id
WHERE p.id IS NULL
```

5. Devuelve un listado con los clientes que no han realizado ningún pedido y de los comerciales que no han participado en ningún pedido. Ordene el listado alfabéticamente por los apellidos y el nombre. En en listado deberá diferenciar de algún modo los clientes y los comerciales.

```sql
SELECT cl.apellido1, cl.apellido2, cl.nombre, 'Cliente' AS TIPO
FROM cliente cl
LEFT JOIN pedido p ON p.id_cliente=cl.id
WHERE p.id IS NULL
UNION
SELECT com.apellido1, com.apellido2, com.nombre, 'Comercial' AS TIPO
FROM comercial com
LEFT JOIN pedido p ON p.id_comercial=com.id
WHERE p.id IS NULL
ORDER BY 1 ASC, 2 ASC, 3 ASC
```

## Consultas resumen

## Subconsultas

## Tablas

### Comercial

| id | nombre  | apellido1  | apellido2  | comisión |
|----|---------|------------|------------|----------|
| 1  | Daniel  | Sáez       | Vega       | 0.15     |
| 2  | Juan    | Gómez      | López      | 0.13     |
| 3  | Diego   | Flores     | Salas      | 0.11     |
| 4  | Marta   | Herrera    | Gil        | 0.14     |
| 5  | Antonio | Carretero  | Ortega     | 0.12     |
| 6  | Manuel  | Domínguez  | Hernández  | 0.13     |
| 7  | Antonio | Vega       | Hernández  | 0.11     |
| 8  | Alfredo | Ruiz       | Flores     | 0.05     |

### Pedido

| id  | total   | fecha       | id_cliente | id_comercial |
|-----|---------|-------------|------------|--------------|
| 1   | 150.5   | 2017-10-05  | 5          | 2            |
| 2   | 270.65  | 2016-09-10  | 1          | 5            |
| 3   | 65.26   | 2017-10-05  | 2          | 1            |
| 4   | 110.5   | 2016-08-17  | 8          | 3            |
| 5   | 948.5   | 2017-09-10  | 5          | 2            |
| 6   | 2400.6  | 2016-07-27  | 7          | 1            |
| 7   | 5760    | 2015-09-10  | 2          | 1            |
| 8   | 1983.43 | 2017-10-10  | 4          | 6            |
| 9   | 2480.4  | 2016-10-10  | 8          | 3            |
| 10  | 250.45  | 2015-06-27  | 8          | 2            |
| 11  | 75.29   | 2016-08-17  | 3          | 7            |
| 12  | 3045.6  | 2017-04-25  | 2          | 1            |
| 13  | 545.75  | 2019-01-25  | 6          | 1            |
| 14  | 145.82  | 2017-02-02  | 6          | 1            |
| 15  | 370.85  | 2019-03-11  | 1          | 5            |
| 16  | 2389.23 | 2019-03-11  | 1          | 5            |

### Cliente

| id  | nombre    | apellido1 | apellido2 | ciudad   | categoria |
|-----|-----------|-----------|-----------|----------|-----------|
| 1   | Aarón     | Rivero    | Gómez     | Almería  | 100       |
| 2   | Adela     | Salas     | Díaz      | Granada  | 200       |
| 3   | Adolfo    | Rubio     | Flores    | Sevilla  |           |
| 4   | Adrián    | Suárez    |           | Jaén     | 300       |
| 5   | Marcos    | Loyola    | Méndez    | Almería  | 200       |
| 6   | María     | Santana   | Moreno    | Cádiz    | 100       |
| 7   | Pilar     | Ruiz      |           | Sevilla  | 300       |
| 8   | Pepe      | Ruiz      | Santana   | Huelva   | 200       |
| 9   | Guillermo | López     | Gómez     | Granada  | 225       |
| 10  | Daniel    | Santana   | Loyola    | Sevilla  | 125       |

