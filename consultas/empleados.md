# Empleados

![diagrama_entidad_relacion_empleados](../diagramas-entidad-relación/empleados.png)

## Consultas sencillas

1. Lista el primer apellido de todos los empleados.

```sql
SELECT e.apellido1
FROM empleado e
```

2. Lista el primer apellido de los empleados eliminando los apellidos que estén repetidos.

```sql
SELECT DISTINCT e.apellido1
FROM empleado e
```

3. Lista todas las columnas de la tabla empleado.

```sql
SELECT *
FROM empleado
```

4. Lista el nombre y los apellidos de todos los empleados.

```sql
SELECT e.nombre, e.apellido1, e.apellido2
FROM empleado e
```

5. Lista el identificador de los departamentos de los empleados que aparecen en la tabla empleado.

```sql
SELECT id_departamento
FROM empleado
```

6. Lista el identificador de los departamentos de los empleados que aparecen en la tabla empleado, eliminando los identificadores que aparecen repetidos.

```sql
SELECT DISTINCT id_departamento
FROM empleado
```

7. Lista el nombre y apellidos de los empleados en una única columna.

```sql
SELECT CONCAT_WS(' ', e.nombre, e.apellido1, e.apellido2)
FROM empleado e
```

8. Lista el nombre y apellidos de los empleados en una única columna, convirtiendo todos los caracteres en mayúscula.

```sql
SELECT UPPER(CONCAT_WS(' ', e.nombre, e.apellido1, e.apellido2))
FROM empleado e
```

9. Lista el nombre y apellidos de los empleados en una única columna, convirtiendo todos los caracteres en minúscula.

```sql
SELECT LOWER(CONCAT_WS(' ', e.nombre, e.apellido1, e.apellido2))
FROM empleado e
```

10. Lista el identificador de los empleados junto al nif, pero el nif deberá aparecer en dos columnas, una mostrará únicamente los dígitos del nif y la otra la letra.

```sql
SELECT e.id, SUBSTRING(e.nif, 1, 8) AS NUMERO, SUBSTRING(e.nif, -1) AS LETRA
FROM empleado e
```

11. Lista el nombre de cada departamento y el valor del presupuesto actual del que dispone. Para calcular este dato tendrá que restar al valor del presupuesto inicial (columna presupuesto) los gastos que se han generado (columna gastos). Tenga en cuenta que en algunos casos pueden existir valores negativos. Utilice un alias apropiado para la nueva columna columna que está calculando.

```sql
SELECT d.nombre, d.presupuesto - d.gastos AS PRESUPUESTO
FROM departamento d
```

12. Lista el nombre de los departamentos y el valor del presupuesto actual ordenado de forma ascendente.

```sql
SELECT d.nombre, d.presupuesto - d.gastos AS PRESUPUESTO
FROM departamento d
ORDER BY PRESUPUESTO ASC
```

13. Lista el nombre de todos los departamentos ordenados de forma ascendente.

```sql
SELECT d.nombre
FROM departamento d
ORDER BY d.nombre ASC
```

14. Lista el nombre de todos los departamentos ordenados de forma descendente.

```sql
SELECT d.nombre
FROM departamento d
ORDER BY d.nombre DESC
```

15. Lista los apellidos y el nombre de todos los empleados, ordenados de forma alfabética tendiendo en cuenta en primer lugar sus apellidos y luego su nombre.

```sql
SELECT e.apellido1, e.apellido2, e.nombre
FROM empleado e
ORDER BY e.apellido1 ASC, e.apellido2 ASC, e.nombre ASC
```

16. Devuelve una lista con el nombre y el presupuesto, de los 3 departamentos que tienen mayor presupuesto.

```sql
SELECT d.nombre, d.presupuesto
FROM departamento d
ORDER BY d.presupuesto DESC
LIMIT 3
```

17. Devuelve una lista con el nombre y el presupuesto, de los 3 departamentos que tienen menor presupuesto.

```sql
SELECT d.nombre, d.presupuesto
FROM departamento d
ORDER BY d.presupuesto ASC
LIMIT 3
```

18. Devuelve una lista con el nombre y el gasto, de los 2 departamentos que tienen mayor gasto.

```sql
SELECT d.nombre, d.gastos
FROM departamento d
ORDER BY d.gastos DESC
LIMIT 2
```

19. Devuelve una lista con el nombre y el gasto, de los 2 departamentos que tienen menor gasto.

```sql
SELECT d.nombre, d.gastos
FROM departamento d
ORDER BY d.gastos ASC
LIMIT 2
```

20. Devuelve una lista con 5 filas a partir de la tercera fila de la tabla empleado. La tercera fila se debe incluir en la respuesta. La respuesta debe incluir todas las columnas de la tabla empleado.

```sql
SELECT *
FROM empleado e
LIMIT 5
OFFSET 2
```

21. Devuelve una lista con el nombre de los departamentos y el presupuesto, de aquellos que tienen un presupuesto mayor o igual a 150000 euros.

```sql
SELECT d.nombre, d.presupuesto
FROM departamento d
WHERE d.presupuesto>=150000
```

22. Devuelve una lista con el nombre de los departamentos y el gasto, de aquellos que tienen menos de 5000 euros de gastos.

```sql
SELECT d.nombre, d.gastos
FROM departamento d
WHERE d.gastos<5000
```

## Composición interna

## Composición externa

## Consultas resumen

## Subconsultas