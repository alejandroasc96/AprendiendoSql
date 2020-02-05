# Joins

Join es el comando que utilizaremos para obtener datos de varias tablas relacionadas entre sí. Los tipos de join son:

* **JOIN** \(INNER JOIN\) Devuelve los registros que tienen al menos una coincidencia en las dos tablas
* **LEFT JOIN** Devuelve los registros de la tabla izquierda incluso si no hay coincidencia con la tabla derecha
* **RIGHT JOIN** Devuelve los registros de la tabla derecha incluso si no hay coincidencia con la tabla izquierda. No soportada en SQLite.
* **FULL JOIN** Devuelve los registros que tienen coincidencia en una de las tablas.

  **Ejemplos** :

Buscamos el nombre de las personas que tienen una orden de compra asociada:

```sql
SELECT Contactos.nombre, Ordenes.Orden FROM Contactos INNER JOIN Ordenes ON
Contactos.c_id = Ordenes.c_id;
```

Esto nos devuelve “Unai” y su orden de compra “OR-1”.

Esta otra sentencia SQL nos devolverá todos los datos de la tabla de la izquierda:

```sql
SELECT Contactos.nombre, Ordenes.Orden FROM Contactos LEFT JOIN Ordenes ON
Contactos.c_id = Ordenes.c_id;
```

Esto nos devuelve todos los nombres pero solo “Unai” tendrá una orden, el resto tendrá vacío ese campo.

