# Ventas

## Contenido

  - [Diagrama-ER](#diagrama-er)
  - [Consultas sencillas](#consultas-sencillas)
  - [Composición interna](#composición-interna)
  - [Composición externa](#composición-externa)
  - [Consultas resumen](#consultas-resumen)
  - [Subconsultas](#subconsultas)
  - [Anexo: Tablas](#tablas)

## Diagrama ER
![diagrama_entidad_relacion_tienda](../diagramas-entidad-relacion/ventas.png)

## Consultas sencillas

1. Devuelve un listado con todos los pedidos que se han realizado. Los pedidos deben estar ordenados por la fecha de realización, mostrando en primer lugar los pedidos más recientes.

```sql
SELECT *
FROM pedido p
ORDER BY p.fecha DESC
```

2. Devuelve todos los datos de los dos pedidos de mayor valor.

```sql
SELECT * 
FROM pedido p
ORDER BY p.total DESC
LIMIT 2
```

3. Devuelve un listado con los identificadores de los clientes que han realizado algún pedido. Tenga en cuenta que no debe mostrar identificadores que estén repetidos.

```sql
SELECT DISTINCT(p.id_cliente)
FROM pedido p
```

4. Devuelve un listado de todos los pedidos que se realizaron durante el año 2017, cuya cantidad total sea superior a 500€.

```sql
SELECT *
FROM pedido p
WHERE YEAR(p.fecha) = "2017" AND p.total>500
```

5. Devuelve un listado con el nombre y los apellidos de los comerciales que tienen una comisión entre 0.05 y 0.11.

```sql
SELECT com.id, com.nombre, com.apellido1, com.apellido2, com.comision
FROM comercial com
WHERE com.comision BETWEEN 0.05 AND 0.11
```

6. Devuelve el valor de la comisión de mayor valor que existe en la tabla comercial.

```sql
SELECT ROUND(MAX(com.comision), 2)
FROM comercial com
```

7. Devuelve el identificador, nombre y primer apellido de aquellos clientes cuyo segundo apellido no es NULL. El listado deberá estar ordenado alfabéticamente por apellidos y nombre.

```sql
SELECT cl.id, cl.nombre, cl.apellido1
FROM cliente cl
WHERE cl.apellido2 IS NOT NULL
ORDER BY 3, 2
```

8. Devuelve un listado de los nombres de los clientes que empiezan por A y terminan por n y también los nombres que empiezan por P. El listado deberá estar ordenado alfabéticamente.

```sql
SELECT c.nombre
FROM cliente c
WHERE (c.nombre LIKE 'a%' AND c.nombre LIKE '%n') OR c.nombre LIKE 'p%'
ORDER BY 1 ASC 
```

9. Devuelve un listado de los nombres de los clientes que no empiezan por A. El listado deberá estar ordenado alfabéticamente.

```sql
SELECT cl.nombre
FROM cliente cl
WHERE cl.nombre NOT LIKE 'a%'
ORDER BY 1 ASC 
```

10. Devuelve un listado con los nombres de los comerciales que terminan por 'el' u 'o'. Tenga en cuenta que se deberán eliminar los nombres repetidos.

```sql
SELECT DISTINCT c.nombre
FROM comercial c
WHERE c.nombre LIKE '%el' OR c.nombre LIKE '%o'
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

1. Calcula la cantidad total que suman todos los pedidos que aparecen en la tabla pedido.

```sql
SELECT SUM(p.total)
FROM pedido p
```

2. Calcula la cantidad media de todos los pedidos que aparecen en la tabla pedido.

```sql
SELECT AVG(p.total)
FROM pedido p
```

3. Calcula el número total de comerciales distintos que aparecen en la tabla pedido.

```sql
SELECT COUNT(DISTINCT(p.id_comercial))
FROM pedido p
```

4. Calcula el número total de clientes que aparecen en la tabla cliente.

```sql
SELECT COUNT(*)
FROM cliente
```

5. Calcula cuál es la mayor cantidad que aparece en la tabla pedido.

```sql
SELECT MAX(p.total)
FROM pedido p
```

6. Calcula cuál es la menor cantidad que aparece en la tabla pedido.

```sql
SELECT MIN(p.total)
FROM pedido p
```

7. Calcula cuál es el valor máximo de categoría para cada una de las ciudades que aparece en la tabla cliente.

```sql
SELECT cl.ciudad, MAX(cl.categoria) AS MAXIMO_CATEGORIA
FROM cliente cl
GROUP BY cl.ciudad
```

8. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes. Es decir, el mismo cliente puede haber realizado varios pedidos de diferentes cantidades el mismo día. Se pide que se calcule cuál es el pedido de máximo valor para cada uno de los días en los que un cliente ha realizado un pedido. Muestra el identificador del cliente, nombre, apellidos, la fecha y el valor de la cantidad.

```sql
SELECT p.id_cliente, cl.nombre, cl.apellido1, cl.apellido2, p.fecha, MAX(p.total) AS PEDIDO_MAXIMO
FROM pedido p
INNER JOIN cliente cl ON cl.id=p.id_cliente
GROUP BY p.id_cliente, p.fecha
```

9. Calcula cuál es el máximo valor de los pedidos realizados durante el mismo día para cada uno de los clientes, teniendo en cuenta que sólo queremos mostrar aquellos pedidos que superen la cantidad de 2000 €.

```sql
SELECT p.id_cliente, cl.nombre, cl.apellido1, cl.apellido2, p.fecha, MAX(p.total) AS PEDIDO_MAXIMO
FROM pedido p
INNER JOIN cliente cl ON cl.id=p.id_cliente
WHERE p.total > 2000
GROUP BY p.id_cliente, p.fecha
```

10. Calcula el máximo valor de los pedidos realizados para cada uno de los comerciales durante la fecha 2016-08-17. Muestra el identificador del comercial, nombre, apellidos y total.

```sql
SELECT com.id, com.nombre, com.apellido1, com.apellido2, MAX(p.total)
FROM comercial com
INNER JOIN pedido p ON p.id_comercial=com.id
WHERE p.fecha="2016-08-17"
GROUP BY com.id
```

11. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes. Tenga en cuenta que pueden existir clientes que no han realizado ningún pedido. Estos clientes también deben aparecer en el listado indicando que el número de pedidos realizados es 0.

```sql
SELECT cl.id, cl.nombre, cl.apellido1, cl.apellido2, COUNT(p.id) AS PEDIDOS_TOTALES
FROM cliente cl
LEFT JOIN pedido p ON p.id_cliente=cl.id
GROUP BY cl.id
```

12. Devuelve un listado con el identificador de cliente, nombre y apellidos y el número total de pedidos que ha realizado cada uno de clientes durante el año 2017.

```sql
SELECT cl.id, cl.nombre, cl.apellido1, cl.apellido2, COUNT(p.id) AS PEDIDOS_TOTALES_2017
FROM pedido p
INNER JOIN cliente cl ON cl.id=p.id_cliente
WHERE YEAR(p.fecha)=2017
GROUP BY cl.id
```

13. Devuelve un listado que muestre el identificador de cliente, nombre, primer apellido y el valor de la máxima cantidad del pedido realizado por cada uno de los clientes. El resultado debe mostrar aquellos clientes que no han realizado ningún pedido indicando que la máxima cantidad de sus pedidos realizados es 0. Puede hacer uso de la función IFNULL.

```sql
SELECT cl.id, cl.nombre, cl.apellido1, cl.apellido2, IFNULL(MAX(p.total), 0)
FROM cliente cl
LEFT JOIN pedido p ON p.id_cliente=cl.id
GROUP BY cl.id
```

14. Devuelve cuál ha sido el pedido de máximo valor que se ha realizado cada año.

```sql
SELECT YEAR(fecha) AS AÑO, MAX(total) AS PRECIO
FROM pedido 
GROUP BY YEAR(fecha)
```

15. Devuelve el número total de pedidos que se han realizado cada año.

```sql
SELECT YEAR(fecha) AS AÑO, COUNT(*)
FROM pedido 
GROUP BY YEAR(fecha)
```

## Subconsultas

1. Devuelve un listado con todos los pedidos que ha realizado Adela Salas Díaz. (Sin utilizar INNER JOIN).

```sql
SELECT *
FROM pedido p
WHERE p.id_cliente=(
    SELECT cl.id 
    FROM cliente cl 
    WHERE cl.nombre="Adela" AND cl.apellido1="Salas" AND cl.apellido2='Díaz'
)
```

2. Devuelve el número de pedidos en los que ha participado el comercial Daniel Sáez Vega. (Sin utilizar INNER JOIN)

```sql
SELECT COUNT(*)
FROM pedido p
WHERE p.id_comercial=(
    SELECT com.id 
    FROM comercial com 
    WHERE com.nombre="Daniel" AND com.apellido1="Sáez" AND com.apellido2='Vega'
)
```

3. Devuelve los datos del cliente que realizó el pedido más caro en el año 2019. (Sin utilizar INNER JOIN)

```sql
SELECT *
FROM cliente cl
WHERE cl.id = (
    SELECT p.id_cliente
    FROM pedido p 
    WHERE YEAR(p.fecha) = "2019"
    ORDER BY p.total DESC
    LIMIT 1
)
```

4. Devuelve la fecha y la cantidad del pedido de menor valor realizado por el cliente Pepe Ruiz Santana.

```sql
SELECT p.fecha, p.total
FROM pedido p
WHERE p.id_cliente = (
    SELECT cl.id
    FROM cliente cl
    WHERE cl.nombre="Pepe" AND cl.apellido1="Ruiz" AND cl.apellido2="Santana"
)
ORDER BY p.total
LIMIT 1
```

5. Devuelve un listado con los datos de los clientes y los pedidos, de todos los clientes que han realizado un pedido durante el año 2017 con un valor mayor o igual al valor medio de los pedidos realizados durante ese mismo año.

```sql
SELECT *
FROM pedido p
INNER JOIN cliente cl ON cl.id=p.id_cliente
WHERE YEAR(p.fecha) = "2017" AND p.total >= (
    SELECT AVG(p2.total)
    FROM pedido p2
    WHERE YEAR(p2.fecha)="2017"
)
```

6. Devuelve el pedido más caro que existe en la tabla pedido si hacer uso de MAX, ORDER BY ni LIMIT.

```sql
SELECT *
FROM pedido p
WHERE p.total >= ALL(SELECT p2.total FROM pedido p2)
```

7. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando ANY o ALL).

```sql
SELECT *
FROM cliente cl
WHERE cl.id <> ALL (SELECT p.id_cliente FROM pedido p)
```

8. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando ANY o ALL).

```sql
SELECT *
FROM comercial com
WHERE com.id <> ALL (SELECT p.id_comercial FROM pedido p)
```

9. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando IN o NOT IN).

```sql
SELECT *
FROM cliente cl
WHERE cl.id NOT IN(SELECT p.id_cliente FROM pedido p)
```

10. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando IN o NOT IN).

```sql
SELECT *
FROM comercial com
WHERE com.id NOT IN (SELECT p.id_comercial FROM pedido p)
```

11. Devuelve un listado de los clientes que no han realizado ningún pedido. (Utilizando EXISTS o NOT EXISTS).

```sql
SELECT *
FROM cliente cl
WHERE NOT EXISTS (
    SELECT p.id_cliente 
    FROM pedido p
    WHERE cl.id = p.id_cliente
)
```

12. Devuelve un listado de los comerciales que no han realizado ningún pedido. (Utilizando EXISTS o NOT EXISTS).

```sql
SELECT *
FROM comercial com
WHERE NOT EXISTS (
    SELECT p.id_comercial 
    FROM pedido p
    WHERE com.id = p.id_comercial
)
```

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

