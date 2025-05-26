# Nombre de la Base de Datos

## Contenido

  - [Diagrama-ER](#diagrama-er)
  - [Tablas](#tablas)
  - [Consultas sencillas](#consultas-sencillas)
  - [Composición interna](#composición-interna)
  - [Composición externa](#composición-externa)
  - [Consultas resumen](#consultas-resumen)
  - [Subconsultas](#subconsultas)

## Diagrama ER
![diagrama_entidad_relacion_tienda](../diagramas-entidad-relación/ventas.png)

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


## Consultas sencillas

1. Lista el nombre de todos los productos que hay en la tabla `producto`.

```sql
SELECT p.nombre
FROM producto p
```

## Composición interna

## Composición externa

## Consultas resumen

## Subconsultas

