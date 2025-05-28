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


## Composición interna

## Composición externa

## Consultas resumen

## Subconsultas

## Anexo: Tablas

### Gama producto


### Producto


### Detalle Pedido


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
