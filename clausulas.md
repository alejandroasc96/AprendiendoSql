# Cláusulas



Son condiciones de modificación para definir los datos que deseamos seleccionar o manipular.

#### Cláusulas comunes son

* **FROM** Especifica la tabla de la que cogemos los registros
* **WHERE** Indica condiciones que deben cumplir los registros seleccionados
* **GROUP BY** Separa los registros seleccionados en grupos específicos.
* **HAVING** Indica la condición que debe cumplir cada grupo.
* **ORDER BY** Ordena los registros según lo que le indiquemos.

### Operadores para la cláusula WHERE

| Operador | Significado |
| :--- | :--- |
| &lt;&gt; | Distinto |
| &lt; | Menor |
| &gt; | Mayor |
| &lt;= | Menor o igual |
| &gt;= | Mayor o igual |
| BETWEEN | Indica un rango \(numérico, texto,fecha,...\) |
| LIKE | Busca un patrón |
| IN | Indica un valor concreto |
| = | Igual |
| AND | Y lógico |
| OR | O lógico |
| NOT | Negación lógica |

#### Ejemplos:

```sql
SELECT * FROM Contactos WHERE Nombre="Jon" AND Edad >= 30;
```

> Todos los que se llamen Jon y tengan 30 años o más.

```sql
SELECT * FROM Contactos WHERE (Nombre="Jon" OR Nombre="Unai") AND (Edad >= 30);
```

> El AND y el OR se pueden combinar usando paréntesis para agrupar las condiciones:

```sql
SELECT * FROM Contactos ORDER BY Nombre;
```

```sql
SELECT * FROM Contactos ORDER BY Nombre ASC;
```

> Nos da todos los nombre ordenados ascendente

```sql
SELECT * FROM Contactos ORDER BY Nombre DESC;
------------------------------ otro ejemplo-----------------
FUNCTION OBT_SOLICITUD_SEGUIMIENTO
(v_id_solicitud IN SOLICITUD.id_solicitud%TYPE)
 RETURN cursor_simple IS

  c_datos cursor_simple;

BEGIN
  OPEN c_datos FOR
   select ID_SEGUIMIENTO, ASUNTO, DESCRIPCION, to_char(FECHA_INSERCION,'dd/mm/YYYY') FECHA_INSERCION , COD_GESTOR from solicitud_seguimientos
   where id_solicitud = v_id_solicitud
   order by ID_SEGUIMIENTO DESC;
   return c_datos;
END;
```

> Nos da todos los nombre ordenados descendente

```sql
SELECT * From Contactos WHERE Nombre BETWEEN "Arantza" AND "Jon";
```

> Todos los que tengan un nombre entre “Arantza” y “Jon”

```sql
SELECT * From Contactos WHERE Edad IN (30,32);
```

> Todos los que tengan 30 o 32 años

```sql
SELECT * From Contactos WHERE Nombre LIKE '%n';
```

> Todos cuyo nombre acabe en “n”: \(El % indica cualquier número de caracteres antes o después, en este caso antes\).

```sql
SELECT * From Contactos WHERE Nombre NOT LIKE '%n';
```

> Todos los que NO acaben en “n”:

