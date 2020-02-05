# DISTINCT



Omite los registros que contienen datos duplicados en los campos seleccionados. Para que los valores de cada campo listado en la instrucción _SELECT_ se incluyan en la consulta deben ser únicos.

Por ejemplo, varios empleados listados en la tabla Empleados pueden tener el mismo apellido. Si dos registros contienen López en el campo Apellido, la siguiente instrucción SQL devuelve un único registro:

```sql
SELECT DISTINCT Apellido FROM Empleados;
```

Con otras palabras el predicado _DISTINCT_ devuelve aquellos registros cuyos campos indicados en la cláusula SELECT posean un contenido diferente. El resultado de una consulta que utiliza DISTINCT no es actualizable y no refleja los cambios subsiguientes realizados por otros usuarios. Ejemplo:

```sql
SELECT DISTINCT Nombre FROM Contactos;
```

> Nos devolverá todos los nombre de la tabla contactos pero omitirá las filas cuyos campos seleccionados coincidan totalmente.

```sql
SELECT DISTINCT Nombre,Edad FROM Contactos;
```

> En este otro caso solo nos omitirá las filas en las que coincidan los dos campos.

```sql
 SELECT * FROM Contactos LIMIT 2;
```

