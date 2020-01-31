# DDL (Data Definition Language):

Estos nos permitirán crear nuevas bases de datos,
modificarlas y eliminarlas. Estos comandos afectan a tablas, campos e índices.

- CREATE Crea nuevas tablas, campos e índices.
- ALTER Modifica las tablas
- DROP Elimina tablas e índices.

## Creando una tabla (CREATE)

```sql
CREATE TABLE Contactos (Nombre TEXT,Apellidos TEXT,Edad INTEGER);
```

Esta sentencia nos crea una tabla con tres columnas. Nombre,Apellidos y Edad donde iremos
metiendo datos.

## Insertando datos en una tabla (INSERT)

```sql
INSERT INTO Contactos VALUES ("Unai","Estebanez",32);
```

## Actualizando datos de un registro (UPDATE)

```sql
UPDATE Contactos SET Nombre="Arantza" WHERE Nombre="Basilio";
```

## Borrando registros(DELETE)

```sql
DELETE FROM Contactos WHERE Nombre="Andres";
```

## Recuperando datos de una tabla (SELECT)

```sql
SELECT * FROM Contactos;
```

> Recupera todas las columnas de la tabla.

```sql
SELECT Nombre FROM Contactos;
```

> Recupera todos los nombres de la tabla

```sql
SELECT Nombre,Edad FROM Contactos;
```

> Recupera todos los nombres y edades de la tabla

## Borrando una tabla (DROP)

```sql
DROP TABLE IF EXISTS USUARIOS;
```

## Consultas con predicado

Se llama **predicado** a lo que va **entre** el **SELECT** y el primer **nombre** de **columna** que vamos a
recuperar. **Sirven para modificar** el **comportamiento** de las **consultas**.
Ejemplo:

```sql
SELECT DISTINCT Nombre FROM Contactos;
```

> Nos devolverá todos los nombre de la tabla contactos pero omitirá las filas cuyos campos
> seleccionados coincidan totalmente.

```sql
SELECT DISTINCT Nombre,Edad FROM Contactos;
```

> En este otro caso solo nos omitirá las filas en las que coincidan los dos campos.

### Predicados comunes son:

- **DISTINCT** Omitirá las filas cuyos campos seleccionados coincidan totalmente
- **ALL** Devuelve todos los campos de la tabla
- **TOP** Devuelve un determinado número de registros de la tabla. En SQLite se soporta
  como LIMIT.

```sql
 SELECT * FROM Contactos LIMIT 2;
```

- **DISTINCTROW** Omite las filas duplicadas basándose en la totalidad del registro y no
  solo en los campos seleccionados. Esto no lo soporta SQLite.

## Cláusulas

Son condiciones de modificación para definir los datos que deseamos seleccionar o manipular.

### Cláusulas comunes son

- **FROM** Especifica la tabla de la que cogemos los registros
- **WHERE** Indica condiciones que deben cumplir los registros seleccionados
- **GROUP BY** Separa los registros seleccionados en grupos específicos.
  Página 13 de 28
  Revisión: 8
- **HAVING** Indica la condición que debe cumplir cada grupo.
- **ORDER BY** Ordena los registros según lo que le indiquemos.

## Operadores para la cláusula WHERE

| Operador | Significado                                 |
| -------- | ------------------------------------------- |
| <>       | Distinto                                    |
| <        | Menor                                       |
| >        | Mayor                                       |
| <=       | Menor o igual                               |
| >=       | Mayor o igual                               |
| BETWEEN  | Indica un rango (numérico, texto,fecha,...) |
| LIKE     | Busca un patrón                             |
| IN       | Indica un valor concreto                    |
| =        | Igual                                       |
| AND      | Y lógico                                    |
| OR       | O lógico                                    |
| NOT      | Negación lógica                             |

### Ejemplos:

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

> Todos cuyo nombre acabe en “n”: (El % indica cualquier número de caracteres antes o
> después, en este caso antes).

```sql
SELECT * From Contactos WHERE Nombre NOT LIKE '%n';
```

> Todos los que NO acaben en “n”:

### Comodines para el LIKE

| Comodín                                        | Descripción                                |
| ---------------------------------------------- | ------------------------------------------ |
| %                                              | Sustituto para cero o más caracteres.      |
| \_                                             | Sustituto para exactamente un carácter     |
| [lista caracteres]                             | Cualquier carácter de la lista             |
| [^lista caracteres] o bien [!lista caracteres] | Cualquier carácter que no esté en la lista |

## Alias

Sirven para asignar un nombre alternativo a alguna tabla o columna.
Es útil cuando tenemos nombre muy largos de columna o tabla y queremos referirnos a ellos
de forma más cómoda.

**Ejemplo**:

```sql
SELECT Apellidos AS A FROM Contactos WHERE A LIKE "%a%";
```

## Joins

Join es el comando que utilizaremos para obtener datos de varias tablas relacionadas entre sí.
Los tipos de join son:

- **JOIN** (INNER JOIN) Devuelve los registros que tienen al menos una coincidencia en las
  dos tablas

- **LEFT JOIN** Devuelve los registros de la tabla izquierda incluso si no hay coincidencia con
  la tabla derecha

- **RIGHT JOIN** Devuelve los registros de la tabla derecha incluso si no hay coincidencia
  con la tabla izquierda. No soportada en SQLite.
- **FULL JOIN** Devuelve los registros que tienen coincidencia en una de las tablas.

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

Esto nos devuelve todos los nombres pero solo “Unai” tendrá una orden, el resto tendrá vacío
ese campo.

## Uniones (UNION y UNION ALL)

Los operadores UNION y UNION ALL sirve para combinar los datos de dos consultas.

Por ejemplo, los que tienen 28 y los que tienen 32 años:

```sql
SELECT Nombre,Apellidos,Edad FROM Contactos Where Edad = 28
UNION ALL
SELECT Nombre,Apellidos,Edad FROM Contactos Where Edad = 32;
```

El UNION ALL permite duplicados y el UNION no.

## Funciones

Sirven para hacer cálculos sobre grupos de datos2
. Hay dos tipos de funciones:

1. Funciones escalares Devuelven un valor único resultante de procesar un dato.
   - UCASE En SQLite es upper
   - LCASE En SQLite es lower
   - MID
   - LEN
   - ROUND
   - NOW
   - FORMAT
2. Funciones agregadas Devuelven un valor único resultante de procesar una columna.

   - AVG -> saca la media
   - COUNT -> cuenta los registros de un campo
   - FIRST -> Primer elemento
   - LAST -> último
   - MAX -> Devuelve el máximo de un campo
   - MIN -> Devuelve el mínimo de un campo
   - SUM -> Suma los valores de un campo

   ### Ejemplos de funciones agregadas:

   Cuenta las columnas “nombre” de la tabla contactos.

   ```sql
   SELECT COUNT(nombre) FROM Contactos;
   ```

   La media de los precios de las ordenes:

   ```sql
   SELECT AVG(precio) FROM Ordenes;
   ```

   ### Ejemplos de funciones escalares:

   Saca los nombres en mayúsculas:

   ```sql
   SELECT upper(nombre) FROM Contactos;
   ```
   
   # Ejemplo pasando id por url para la búsqueda
    ## vista
    le pasamos el id por url
    ```html
    <a class="a-oferta"  href="/gestion/seguimiento_solicitud?id_solicitud=<?php echo $id_solicitud;?>">[Solicitud]</a>
    la recogemos en la nueva pag
    ```
    
```php
$id_soli = $_REQUEST['id_solicitud'];
  print_r($_REQUEST['id_solicitud']);
```
# DAR FORMATO A UNA FECHA
```sql
FUNCTION OBT_SOLICITUD_SEGUIMIENTO
(v_id_solicitud IN SOLICITUD.id_solicitud%TYPE)
 RETURN cursor_simple IS
  
  c_datos cursor_simple;

BEGIN
  OPEN c_datos FOR
   select ID_SEGUIMIENTO, ASUNTO, DESCRIPCION, to_char(FECHA_INSERCION,'dd/mm/YYYY') FECHA_INSERCION , COD_GESTOR from solicitud_seguimientos
   where id_solicitud = v_id_solicitud;
   return c_datos;
END;
```
# DAR FORMATO OBJETO CLOB
```PHP
 $descripcion = is_object($seguimiento_solicitud[$i]['DESCRIPCION']) ? utf8_encode($seguimiento_solicitud[$i]['DESCRIPCION']->load()) : null;
 echo "<div class='dato'>".$descripcion."</div>";
 ```


seguir : http://www.it-docs.net/ddata/4829.pdf
