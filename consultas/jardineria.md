# Jardinería

## Contenido

  - [Diagrama-ER](#diagrama-er)
  - [Consultas sencillas](#consultas-sencillas)
  - [Composición interna](#composición-interna)
  - [Composición externa](#composición-externa)
  - [Consultas resumen](#consultas-resumen)
  - [Subconsultas](#subconsultas)
  - [Anexo: Tablas](#tablas)

## Diagrama ER
![diagrama_entidad_relacion_jardineria](../diagramas-entidad-relacion/jardineria.png)

## Consultas sencillas

1. Devuelve un listado con el código de oficina y la ciudad donde hay oficinas.

```sql
SELECT of.codigo_oficina, of.ciudad
FROM oficina of
```

2. Devuelve un listado con la ciudad y el teléfono de las oficinas de España.

```sql
SELECT of.ciudad, of.telefono
FROM oficina of
WHERE of.pais="España"
```

3. Devuelve un listado con el nombre, apellidos y email de los empleados cuyo jefe tiene un código de jefe igual a 7.

```sql
SELECT empl.nombre, empl.apellido1, empl.apellido2, empl.email, empl.codigo_jefe
FROM empleado empl
WHERE empl.codigo_jefe=7
```

4. Devuelve el nombre del puesto, nombre, apellidos y email del jefe de la empresa.

```sql
SELECT empl.puesto, empl.nombre, empl.apellido1, empl.apellido2, empl.email
FROM empleado empl
WHERE empl.codigo_jefe IS NULL
```

5. Devuelve un listado con el nombre, apellidos y puesto de aquellos empleados que no sean representantes de ventas.

```sql
SELECT empl.nombre, empl.apellido1, empl.apellido2, empl.puesto
FROM empleado empl
WHERE empl.puesto != "Representante Ventas"
```

6. Devuelve un listado con el nombre de los todos los clientes españoles.

```sql
SELECT cl.nombre_cliente
FROM cliente cl
WHERE cl.pais="Spain"
```

7. Devuelve un listado con los distintos estados por los que puede pasar un pedido.

```sql
SELECT DISTINCT(p.estado)
FROM pedido p
```

8. Devuelve un listado con el código de cliente de aquellos clientes que realizaron algún pago en 2008. Tenga en cuenta que deberá eliminar aquellos códigos de cliente que aparezcan repetidos. Resuelva la consulta:

Utilizando la función YEAR de MySQL.
```sql
SELECT DISTINCT(cl.codigo_cliente)
FROM cliente cl
INNER JOIN pago p ON p.codigo_cliente=cl.codigo_cliente
WHERE YEAR(p.fecha_pago) = "2008"
```

Utilizando la función DATE_FORMAT de MySQL.
```sql
SELECT DISTINCT(cl.codigo_cliente)
FROM cliente cl
INNER JOIN pago p ON p.codigo_cliente=cl.codigo_cliente
WHERE DATE_FORMAT(p.fecha_pago, "%Y") = "2008"
```

Sin utilizar ninguna de las funciones anteriores.
```sql
SELECT DISTINCT(cl.codigo_cliente)
FROM cliente cl
INNER JOIN pago p ON p.codigo_cliente=cl.codigo_cliente
WHERE SUBSTRING(p.fecha_pago, 1, 4) = "2008"
```

9. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos que no han sido entregados a tiempo.

```sql
SELECT p.codigo_pedido, p.codigo_cliente, p.fecha_esperada, p.fecha_entrega
FROM pedido p
WHERE p.fecha_esperada < p.fecha_entrega
```

10. Devuelve un listado con el código de pedido, código de cliente, fecha esperada y fecha de entrega de los pedidos cuya fecha de entrega ha sido al menos dos días antes de la fecha esperada.

Utilizando la función ADDDATE de MySQL.

```sql
SELECT p.codigo_pedido, p.codigo_cliente, p.fecha_esperada, p.fecha_entrega
FROM pedido p
WHERE p.fecha_esperada >= ADDDATE(p.fecha_entrega, INTERVAL 2 DAY)
```

Utilizando la función DATEDIFF de MySQL.

```sql
SELECT p.codigo_pedido, p.codigo_cliente, p.fecha_esperada, p.fecha_entrega
FROM pedido p
WHERE DATEDIFF(p.fecha_esperada, p.fecha_entrega) >= 2
```

¿Sería posible resolver esta consulta utilizando el operador de suma + o resta -?
```
SELECT p.codigo_pedido, p.codigo_cliente, p.fecha_esperada, p.fecha_entrega
FROM pedido p
WHERE p.fecha_entrega <= p.fecha_esperada - 2
```

11. Devuelve un listado de todos los pedidos que fueron rechazados en 2009.

```sql
SELECT *
FROM pedido p
WHERE YEAR(p.fecha_pedido) = 2009 AND p.estado="Rechazado"
```

12. Devuelve un listado de todos los pedidos que han sido entregados en el mes de enero de cualquier año.

```sql
SELECT *
FROM pedido p
WHERE p.estado="Entregado" AND MONTH(p.fecha_pedido) = 1
```

13. Devuelve un listado con todos los pagos que se realizaron en el año 2008 mediante Paypal. Ordene el resultado de mayor a menor.

```sql
SELECT *
FROM pago p
WHERE YEAR(p.fecha_pago)=2008 AND p.forma_pago="PayPal"
ORDER BY p.total DESC
```

14. Devuelve un listado con todas las formas de pago que aparecen en la tabla pago. Tenga en cuenta que no deben aparecer formas de pago repetidas.

```sql
SELECT DISTINCT(p.forma_pago)
FROM pago p
```

15. Devuelve un listado con todos los productos que pertenecen a la gama Ornamentales y que tienen más de 100 unidades en stock. El listado deberá estar ordenado por su precio de venta, mostrando en primer lugar los de mayor precio.

```sql
SELECT *
FROM producto p
WHERE p.gama = 'Ornamentales' AND p.cantidad_en_stock > 100
ORDER BY p.precio_venta DESC
```

16. Devuelve un listado con todos los clientes que sean de la ciudad de Madrid y cuyo representante de ventas tenga el código de empleado 11 o 30.

```sql
SELECT *
FROM cliente cl
WHERE cl.ciudad="Madrid" AND (cl.codigo_empleado_rep_ventas IN (11, 30))
```

## Composición interna
1. Obtén un listado con el nombre de cada cliente y el nombre y apellido de su representante de ventas.

```SELECT cl.nombre_cliente, em.nombre, em.apellido1
FROM cliente cl
INNER JOIN empleado em ON em.codigo_empleado=cl.codigo_empleado_rep_ventas
```

2. Muestra el nombre de los clientes que hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
SELECT cl.nombre_cliente, emp.nombre, emp.apellido1
FROM cliente cl
INNER JOIN pago p ON cl.codigo_cliente=p.codigo_cliente
INNER JOIN empleado emp ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
GROUP BY p.codigo_cliente
```

3. Muestra el nombre de los clientes que no hayan realizado pagos junto con el nombre de sus representantes de ventas.

```sql
SELECT cl.nombre_cliente, emp.nombre, emp.apellido1
FROM cliente cl
LEFT JOIN pago p ON cl.codigo_cliente=p.codigo_cliente
INNER JOIN empleado emp ON emp.codigo_empleado=cl.codigo_empleado_rep_ventas
WHERE p.id_transaccion IS NULL
```

4. Devuelve el nombre de los clientes que han hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
SELECT cl.nombre_cliente, emp.nombre, emp.apellido1, emp.apellido2, of.ciudad
FROM cliente cl
INNER JOIN pago p ON cl.codigo_cliente=p.codigo_cliente
INNER JOIN empleado emp ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
INNER JOIN oficina of ON of.codigo_oficina=emp.codigo_oficina
```

5. Devuelve el nombre de los clientes que no hayan hecho pagos y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
SELECT cl.nombre_cliente, emp.nombre, emp.apellido1, emp.apellido2, of.ciudad
FROM cliente cl
LEFT JOIN pago p ON cl.codigo_cliente=p.codigo_cliente
INNER JOIN empleado emp ON emp.codigo_empleado=cl.codigo_empleado_rep_ventas
INNER JOIN oficina of ON of.codigo_oficina=emp.codigo_oficina
WHERE p.id_transaccion IS NULL
```

6. Lista la dirección de las oficinas que tengan clientes en Fuenlabrada.

```sql
SELECT of.linea_direccion1, of.linea_direccion2, of.ciudad
FROM cliente cl
INNER JOIN empleado emp ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
INNER JOIN oficina of ON of.codigo_oficina=emp.codigo_oficina
WHERE cl.ciudad="Fuenlabrada"
```

7. Devuelve el nombre de los clientes y el nombre de sus representantes junto con la ciudad de la oficina a la que pertenece el representante.

```sql
SELECT cl.nombre_cliente, empl.nombre, empl.apellido1, empl.apellido2, of.ciudad
FROM cliente cl
INNER JOIN empleado empl ON empl.codigo_empleado=cl.codigo_empleado_rep_ventas
INNER JOIN oficina of ON empl.codigo_oficina=of.codigo_oficina
```

8. Devuelve un listado con el nombre de los empleados junto con el nombre de sus jefes.

```sql
SELECT emp.nombre AS NOMBRE_EMPLEADO, emp.apellido1, emp.apellido2, emp2.nombre AS NOMBRE_JEFE, emp2.apellido1, emp2.apellido2
FROM empleado emp
INNER JOIN empleado emp2 ON emp.codigo_jefe=emp2.codigo_empleado
```

Se podría mostrar también al jefe, cambiando el `inner` por un `left`

9. Devuelve un listado que muestre el nombre de cada empleados, el nombre de su jefe y el nombre del jefe de sus jefe.

```sql
SELECT emp.nombre AS NOMBRE_EMPLEADO, emp.apellido1, emp.apellido2, emp2.nombre AS NOMBRE_JEFE, emp2.apellido1, emp2.apellido2, emp3.nombre AS NOMBRE_SUPERJEFE, emp3.apellido1, emp3.apellido2
FROM empleado emp
INNER JOIN empleado emp2 ON emp.codigo_jefe=emp2.codigo_empleado
INNER JOIN empleado emp3 ON emp2.codigo_jefe=emp3.codigo_empleado
```

10. Devuelve el nombre de los clientes a los que no se les ha entregado a tiempo un pedido.

```sql
SELECT cl.nombre_cliente, p.fecha_entrega, p.fecha_esperada
FROM pedido p
INNER JOIN cliente cl ON cl.codigo_cliente=p.codigo_cliente
WHERE p.fecha_entrega>p.fecha_esperada
```

11. Devuelve un listado de las diferentes gamas de producto que ha comprado cada cliente.

```sql
SELECT DISTINCT pr.gama, c.nombre_cliente
FROM cliente c 
INNER JOIN pedido pe ON c.codigo_cliente = pe.codigo_cliente
INNER JOIN detalle_pedido dp ON dp.codigo_pedido = pe.codigo_pedido
INNER JOIN producto pr ON pr.codigo_producto = dp.codigo_producto
```

## Composición externa

1. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pago.

```sql
SELECT *
FROM cliente cl
LEFT JOIN pago p ON p.codigo_cliente=cl.codigo_cliente
WHERE p.id_transaccion IS NULL
```

2. Devuelve un listado que muestre solamente los clientes que no han realizado ningún pedido.

```sql
SELECT *
FROM cliente cl
LEFT JOIN pedido p ON p.codigo_cliente=cl.codigo_cliente
WHERE p.codigo_pedido IS NULL
```

3. Devuelve un listado que muestre los clientes que no han realizado ningún pago y los que no han realizado ningún pedido.

No sé muy bien qué es lo que pide aquí, pero:

```sql
SELECT cl.nombre_cliente, 'SIN PEDIDOS' AS TIPO
FROM cliente cl
LEFT JOIN pedido pe ON pe.codigo_cliente=cl.codigo_cliente
WHERE pe.codigo_cliente IS NULL 
UNION
SELECT cl2.nombre_cliente, 'SIN PAGOS' AS TIPO
FROM cliente cl2
LEFT JOIN pago pa ON pa.codigo_cliente=cl2.codigo_cliente
WHERE pa.codigo_cliente IS NULL
```

o bien:

```sql
SELECT cl.nombre_cliente
FROM cliente cl
LEFT JOIN pedido pe ON pe.codigo_cliente=cl.codigo_cliente
LEFT JOIN pago pa ON pa.codigo_cliente=cl.codigo_cliente
WHERE pe.codigo_cliente IS NULL AND pa.codigo_cliente IS NULL
```

4. Devuelve un listado que muestre solamente los empleados que no tienen una oficina asociada.

```sql
SELECT *
FROM empleado emp
WHERE emp.codigo_oficina IS NULL
```

5. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado.

```sql
SELECT *
FROM empleado emp
LEFT JOIN cliente cl ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
WHERE cl.codigo_cliente IS NULL
```

6. Devuelve un listado que muestre solamente los empleados que no tienen un cliente asociado junto con los datos de la oficina donde trabajan.

```sql
SELECT emp.nombre, emp.apellido1, emp.apellido2, of.telefono
FROM empleado emp
LEFT JOIN cliente cl ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
INNER JOIN oficina of ON of.codigo_oficina=emp.codigo_oficina
WHERE cl.codigo_cliente IS NULL
```

7. Devuelve un listado que muestre los empleados que no tienen una oficina asociada y los que no tienen un cliente asociado.

```sql
SELECT emp.nombre, emp.apellido1, emp.apellido2
FROM empleado emp
WHERE emp.codigo_oficina IS NULL
UNION
SELECT emp2.nombre, emp2.apellido1, emp2.apellido2
FROM empleado emp2
LEFT JOIN cliente cl ON cl.codigo_empleado_rep_ventas=emp2.codigo_empleado
WHERE cl.codigo_cliente IS NULL
```

8. Devuelve un listado de los productos que nunca han aparecido en un pedido.

```sql
SELECT *
FROM producto prod
LEFT JOIN detalle_pedido dp ON dp.codigo_producto=prod.codigo_producto
WHERE dp.codigo_pedido IS NULL
```
9. Devuelve un listado de los productos que nunca han aparecido en un pedido. El resultado debe mostrar el nombre, la descripción y la imagen del producto

```sql
SELECT p.nombre, gp.descripcion_texto, gp.imagen
FROM producto p
LEFT JOIN detalle_pedido dp ON p.codigo_producto=dp.codigo_producto
INNER JOIN gama_producto gp ON gp.gama=p.gama
WHERE dp.codigo_pedido IS NULL
```

10.Devuelve las oficinas donde no trabajan ninguno de los empleados que hayan sido los representantes de ventas de algún cliente que haya realizado la compra de algún producto de la gama Frutales.

```sql
SELECT *
FROM oficina of
WHERE of.codigo_oficina NOT IN (
  SELECT DISTINCT emp.codigo_oficina
  FROM producto p
  INNER JOIN gama_producto gp ON p.gama=gp.gama
  INNER JOIN detalle_pedido dp ON p.codigo_producto=dp.codigo_producto
  INNER JOIN pedido pe ON pe.codigo_pedido=dp.codigo_pedido
  INNER JOIN cliente cl ON cl.codigo_cliente=pe.codigo_cliente
  INNER JOIN empleado emp ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
  WHERE gp.gama="Frutales"
)
```

11. Devuelve un listado con los clientes que han realizado algún pedido pero no han realizado ningún pago.

```sql
SELECT DISTINCT cl.codigo_cliente, cl.nombre_cliente
FROM cliente cl
LEFT JOIN pago pa ON pa.codigo_cliente=cl.codigo_cliente
INNER JOIN pedido p ON p.codigo_cliente=cl.codigo_cliente
WHERE pa.id_transaccion IS NULL
```

12.Devuelve un listado con los datos de los empleados que no tienen clientes asociados y el nombre de su jefe asociado.

```sql
SELECT emp.nombre, emp.apellido1, emp.apellido2, emp2.nombre, emp2.apellido1, emp2.apellido2
FROM empleado emp
LEFT JOIN cliente cl ON cl.codigo_empleado_rep_ventas=emp.codigo_empleado
INNER JOIN empleado emp2 ON emp.codigo_jefe=emp2.codigo_empleado
WHERE cl.codigo_cliente IS NULL
```

## Consultas resumen

## Subconsultas

## Tablas

### Gama producto

| gama         | descripcion_texto                              | descripcion_html | imagen |
|--------------|------------------------------------------------|------------------|--------|
| Aromáticas   | Plantas aromáticas                             |                  |        |
| Frutales     | Árboles pequeños de producción frutal          |                  |        |
| Herbaceas    | Plantas para jardín decorativas                |                  |        |
| Herramientas | Herramientas para todo tipo de acción          |                  |        |
| Ornamentales | Plantas vistosas para la decoración del jardín |                  |        |


### Producto

| código_producto | nombre                  | gama        | dimensiones | proveedor           | descripcion                                                                                                                                                                                                                                  | cantidad_en_stock | precio_venta | precio_proveedor |
|-----------------|-------------------------|-------------|-------------|----------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-------------------|--------------|-------------------|
| 11679           | Sierra de Poda 400MM    | Herramientas| 0,258       | HiperGarden Tools   | Gracias a la poda se consigue manipular la naturaleza, facilitando el crecimiento equilibrado y la floración. Herramientas adecuadas son vitales.                                                                                           | 15                | 14.00        | 11.00             |
| 21636           | Pala                    | Herramientas| 0,156       | HiperGarden Tools   | Palas de acero con buena penetración en terrenos compactos.                                                                                                                                                                                  | 15                | 14.00        | 13.00             |
| 22225           | Rastrillo de Jardín     | Herramientas| 1,064       | HiperGarden Tools   | Rastillo útil para eliminar elementos no deseados del jardín.                                                                                                                                                                                | 15                | 12.00        | 11.00             |
| 30310           | Azadón                  | Herramientas| 0,168       | HiperGarden Tools   | Herramienta de acero con diseño ergonómico y protección contra la corrosión.                                                                                                                                                                 | 15                | 12.00        | 11.00             |
| AR-001          | Ajedrea                 | Aromáticas  | 15-20       | Murcia Seasons       | Planta usada fresca o seca para condimentar diversos platos.                                                                                                                                                                                 | 140               | 1.00         | 0.00              |
| AR-002          | Lavándula Dentata       | Aromáticas  | 15-20       | Murcia Seasons       | Mata aromática usada en jardinería. Ahuyenta insectos y es decorativa.                                                                                                                                                                       | 140               | 1.00         | 0.00              |
| AR-003          | Mejorana                | Aromáticas  | 15-20       | Murcia Seasons       | Hierba con sabor dulce y aroma delicado, usada en muchas recetas.                                                                                                                                                                            | 140               | 1.00         | 0.00              |
| AR-004          | Melissa                 | Aromáticas  | 15-20       | Murcia Seasons       | Planta perenne con aroma a limón, fácil de cultivar y con múltiples usos.                                                                                                                                                                    | 140               | 1.00         | 0.00              |
| AR-005          | Mentha Sativa           | Aromáticas  | 15-20       | Murcia Seasons       | Hierbabuena, planta perenne muy conocida. Necesita agua abundante y vive bien en semisombra.                                                                                                                                                | 140               | 1.00         | 0.00              |
| AR-006          | Petrosilium Hortense    | Aromáticas  | 15-20       | Murcia Seasons       | Perejil común o rizado. Usado como condimento, adorno o en ensaladas.                                                                                                                                                                        | 140               | 1.00         | 0.00              |
...

### Detalle Pedido

| codigo_pedido | codigo_producto | cantidad | precio_unidad | numero_linea |
|----------------|------------------|----------|----------------|---------------|
| 1              | FR-67            | 10       | 70.00          | 3             |
| 1              | OR-127           | 40       | 4.00           | 1             |
| 1              | OR-141           | 25       | 4.00           | 2             |
| 1              | OR-241           | 15       | 19.00          | 4             |
| 1              | OR-99            | 23       | 14.00          | 5             |
| 2              | FR-4             | 3        | 29.00          | 6             |
| 2              | FR-40            | 7        | 8.00           | 7             |
| 2              | OR-140           | 50       | 4.00           | 3             |
| 2              | OR-141           | 20       | 5.00           | 2             |
| 2              | OR-159           | 12       | 6.00           | 5             |
...

### Cliente

| codigo_cliente | nombre_cliente                  | nombre_contacto | apellido_contacto | telefono         | fax             | linea_direccion1                                | linea_direccion2        | ciudad                   | region               | pais              | codigo_postal | codigo_empleado_rep_ventas | limite_credito |
|----------------|----------------------------------|------------------|-------------------|------------------|------------------|--------------------------------------------------|--------------------------|---------------------------|----------------------|--------------------|----------------|-----------------------------|----------------|
| 1              | DGPRODUCTIONS GARDEN            | Daniel G         | GoldFish          | 5556901745       | 5556901746       | False Street 52 2 A                             |                          | San Francisco             |                      | USA                | 24006         | 19                          | 3000.00        |
| 3              | Gardening Associates            | Anne             | Wright            | 5557410345       | 5557410346       | Wall-e Avenue                                   |                          | Miami                     | Miami               | USA                | 24010         | 19                          | 6000.00        |
| 4              | Gerudo Valley                   | Link             | Flaute            | 5552323129       | 5552323128       | Oaks Avenue nº22                                |                          | New York                 |                      | USA                | 85495         | 22                          | 12000.00       |
| 5              | Tendo Garden                    | Akane            | Tendo             | 55591233210      | 55591233211      | Null Street nº69                                |                          | Miami                     |                      | USA                | 696969        | 22                          | 600000.00      |
| 6              | Lasas S.A.                      | Antonio          | Lasas             | 34916540145      | 34914851312      | C/Leganes 15                                    |                          | Fuenlabrada               | Madrid              | Spain              | 28945         | 8                           | 154310.00      |
| 7              | Beragua                         | Jose             | Bermejo           | 654987321        | 916549872        | C/pintor segundo                                |                          | Getafe                    | Madrid              | Spain              | 28942         | 11                          | 20000.00       |
| 8              | Club Golf Puerta del hierro     | Paco             | Lopez             | 62456810         | 919535678        | C/sinesio delgado                               |                          | Madrid                    | Madrid              | Spain              | 28930         | 11                          | 40000.00       |
| 9              | Naturagua                       | Guillermo        | Rengifo           | 689234750        | 916428956        | C/majadahonda                                   |                          | Boadilla                  | Madrid              | Spain              | 28947         | 11                          | 32000.00       |
| 10             | DaraDistribuciones              | David            | Serrano           | 675598001        | 916421756        | C/azores                                        |                          | Fuenlabrada               | Madrid              | Spain              | 28946         | 11                          | 50000.00       |
...

### Pago
| codigo_cliente | forma_pago | id_transaccion  | fecha_pago | total    |
|----------------|------------|------------------|-------------|----------|
| 1              | PayPal     | ak-std-000001     | 2008-11-10  | 2000.00  |
| 1              | PayPal     | ak-std-000002     | 2008-12-10  | 2000.00  |
| 3              | PayPal     | ak-std-000003     | 2009-01-16  | 5000.00  |
| 3              | PayPal     | ak-std-000004     | 2009-02-16  | 5000.00  |
| 3              | PayPal     | ak-std-000005     | 2009-02-19  | 926.00   |
| 4              | PayPal     | ak-std-000006     | 2007-01-08  | 20000.00 |
| 4              | PayPal     | ak-std-000007     | 2007-01-08  | 20000.00 |
| 4              | PayPal     | ak-std-000008     | 2007-01-08  | 20000.00 |
| 4              | PayPal     | ak-std-000009     | 2007-01-08  | 20000.00 |
| 4              | PayPal     | ak-std-000010     | 2007-01-08  | 1849.00  |
...


### Pedido

| codigo_pedido | fecha_pedido | fecha_esperada | fecha_entrega | estado     | comentarios                                                                 | codigo_cliente |
|---------------|--------------|----------------|----------------|------------|-----------------------------------------------------------------------------|----------------|
| 1             | 2006-01-17   | 2006-01-19     | 2006-01-19     | Entregado  | Pagado a plazos                                                             | 5              |
| 2             | 2007-10-23   | 2007-10-28     | 2007-10-26     | Entregado  | La entrega llego antes de lo esperado                                       | 5              |
| 3             | 2008-06-20   | 2008-06-25     |                | Rechazado  | Limite de credito superado                                                  | 5              |
| 4             | 2009-01-20   | 2009-01-26     |                | Pendiente  |                                                                             | 5              |
| 8             | 2008-11-09   | 2008-11-14     | 2008-11-14     | Entregado  | El cliente paga la mitad con tarjeta y la otra mitad con efectivo, se le realizan dos facturas | 1      
...        |


### Empleado
| codigo_empleado | nombre        | apellido1     | apellido2     | extension | email                      | codigo_oficina | codigo_jefe | puesto                 |
|-----------------|---------------|---------------|---------------|-----------|-----------------------------|----------------|-------------|------------------------|
| 1               | Marcos        | Magaña        | Perez         | 3897      | marcos@jardineria.es        | TAL-ES         |             | Director General       |
| 2               | Ruben         | López         | Martinez      | 2899      | rlopez@jardineria.es        | TAL-ES         | 1           | Subdirector Marketing  |
| 3               | Alberto       | Soria         | Carrasco      | 2837      | asoria@jardineria.es        | TAL-ES         | 2           | Subdirector Ventas     |
| 4               | Maria         | Solís         | Jerez         | 2847      | msolis@jardineria.es        | TAL-ES         | 2           | Secretaria             |
| 5               | Felipe        | Rosas         | Marquez       | 2844      | frosas@jardineria.es        | TAL-ES         | 3           | Representante Ventas   |
| 6               | Juan Carlos   | Ortiz         | Serrano       | 2845      | cortiz@jardineria.es        | TAL-ES         | 3           | Representante Ventas   |
| 7               | Carlos        | Soria         | Jimenez       | 2444      | csoria@jardineria.es        | MAD-ES         | 3           | Director Oficina       |
...



### Oficina

| codigo_oficina | ciudad               | pais      | region            | codigo_postal | telefono        | linea_direccion1                 | linea_direccion2         |
|----------------|----------------------|-----------|-------------------|----------------|------------------|----------------------------------|---------------------------|
| BCN-ES         | Barcelona            | España    | Barcelona         | 08019         | +34 93 3561182   | Avenida Diagonal, 38            | 3A escalera Derecha       |
| BOS-USA        | Boston               | EEUU      | MA                | 02108         | +1 215 837 0825  | 1550 Court Place                | Suite 102                 |
| LON-UK         | Londres              | Inglaterra| EMEA              | EC2N 1HN      | +44 20 78772041  | 52 Old Broad Street            | Ground Floor              |
| MAD-ES         | Madrid               | España    | Madrid            | 28032         | +34 91 7514487   | Bulevar Indalecio Prieto, 32    |                           |
| PAR-FR         | Paris                | Francia   | EMEA              | 75017         | +33 14 723 4404  | 29 Rue Jouffroy d'abbans       |                           |
| SFC-USA        | San Francisco        | EEUU      | CA                | 94080         | +1 650 219 4782  | 100 Market Street              | Suite 300                 |
| SYD-AU         | Sydney               | Australia | APAC              | NSW 2010      | +61 2 9264 2451  | 5-11 Wentworth Avenue          | Floor #2                  |
| TAL-ES         | Talavera de la Reina | España    | Castilla-LaMancha | 45632         | +34 925 867231   | Francisco Aguirre, 32          | 5º piso (exterior)        |
| TOK-JP         | Tokyo                | Japón     | Chiyoda-Ku        | 102-8578      | +81 33 224 5000  | 4-1 Kioicho                    |                           |
