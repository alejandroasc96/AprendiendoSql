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
   Otro Ejemplo:
   Vamos a obtener el numero de solicitudes que hay para una empresa, para ello le pasamos  cod_entidad y cod_unidad que nos referencia 
   a la empresa.
   *declaración de la función*
   ```sql
   FUNCTION OBT_NUMERO_SOLICITUD 
    (v_cod_entidad ADEJE.SOLICITUD.COD_ENTIDAD%TYPE,
    v_cod_unidad ADEJE.SOLICITUD.COD_UNIDAD%TYPE)
    RETURN NUMBER;
    ```
    *Cuerpo de la función*
    ```sql
    FUNCTION OBT_NUMERO_SOLICITUD 
    (v_cod_entidad ADEJE.SOLICITUD.COD_ENTIDAD%TYPE,
    v_cod_unidad ADEJE.SOLICITUD.COD_UNIDAD%TYPE)
    RETURN NUMBER IS
    
    num_solicitud NUMBER;
    BEGIN
        SELECT COUNT(ID_SOLICITUD)
        INTO num_solicitud
        FROM SOLICITUD
        WHERE cod_entidad = v_cod_entidad AND cod_unidad = v_cod_unidad;
        RETURN num_solicitud;
    END;
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
   ## GROUP BY
    La sentencia GROUP BY sirve para agrupar resultados en conjuntos y de esta forma aplicar
    operaciones sobre esos conjuntos. Se usan en conjunción con las funciones agregadas.
    Antes, vamos a añadir una columna a la tabla de órdenes, se trata del precio de cada orden:
    ```sql
    ALTER TABLE Ordenes ADD precio REAL;
    ```
    Y ahora añadimos un precio:
    ```sql
    UPDATE ordenes SET precio=12 WHERE o_id=1;
    ```
    Vamos a añadir algunas ordenes mas para hacer pruebas con el group by:
    ```sql
    INSERT INTO Ordenes (orden,c_id,precio) VALUES ("OR-2",1,25);
    INSERT INTO Ordenes (orden,c_id,precio) VALUES ("OR-3",2,13.6);
    ```
    A continuación vamos a obtener todas las ordenes de compra y vamos a agruparlas por
    nombre para además sumar los precios que corresponden a cada nombre:
    ```sql
    SELECT nombre,apellidos,SUM(Ordenes.precio) FROM Contactos INNER JOIN Or
    denes ON Contactos.c_id = Ordenes.c_id GROUP BY nombre;
    ```
    Esto podemos hacerlo porque se ha realizado una agrupación por nombre. Si no hacemos la
    agrupación el resultado es totalmente distinto ya que suma todos los precios de la columna
    precios que sale tras la selección.
    
    ## HAVING
    El having tiene su sentido porque no es posible usar funciones agregadas en el WHERE.
    Por ejemplo:
    Vamos a seleccionar de la tabla Contactos todos aquellos que tengan el mismo nombre, los
    agrupamos y sumamos su edad:
    ```sql
    SELECT nombre,SUM(edad) FROM Contactos GROUP BY nombre HAVING COUNT(edad) > 1;
    ```
    ## Constraints - Restricciones
    Se llama así al restringir los valores que puede tomar una determinada columna de la tabla.
    Estas restricciones las podemos añadir al crear una tabla con CREATE_TABLE o bién al
    modificarla mediante ALTER_TABLE.
    Restricciones típicas pueden ser por ejemplo que una columna no pueda ser NULL, que no
    tome un valor determinado o que no tenga una longitud mayor que una dada.
    Entre estas restricciones típicas tenemos:
    * NOT NULL (Para que no se permita un valor NULL)
    * PRIMARY KEY ( ¿alguien no sabe a estas alturas para que sirve esto?)
    * UNIQUE (Para obligar a que no se repitan valores en esa columna)
    * CHECK ( Para verificar una condición dada)
    ```sql
    CREATE TABLE RSU (ID_RSU TEXT UNIQUE PRIMARY KEY,
    Descr TEXT,
    RSU_type TEXT,
    ComsParams TEXT,
    Road TEXT,
    PK INTEGER,
    Dir TEXT check (length(Dir) <= 2),
    POS_X REAL,
    POS_Y REAL,
    POS_Z REAL,
    RT_Timestamp TEXT,
    RT_Status TEXT,
    Algorithms_path TEXT check(length(Algorithms_path) <= 260));
    ```
    Tenemos en esta tabla cuatro restricciones, una PRIMARY_KEY y UNIQUE y dos CHECKs.
    Como se puede ver el mecanismo es sencillo, se le indica que esos campos no pueden tener
    más de una longitud dada y ya esta.
    Al intentar introducir datos que no cumplen las condiciones se produce un error como este
    “SQL error: constraint failed” en el caso de intentar meter un texto más grande del debido o
    como este “SQL error: column ID_RSU is not unique” en el caso de querer meter una
    columna con valores duplicados.


   
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
# CRUZAR TABLAS
```sql
FUNCTION OBT_NUM_SOLICITUD_SEGUI 
    (v_cod_entidad ADEJE.SOLICITUD.COD_ENTIDAD%TYPE,
     v_cod_unidad ADEJE.SOLICITUD.COD_UNIDAD%TYPE)
    RETURN NUMBER IS
    
    num_solicitud NUMBER;
    BEGIN
        SELECT COUNT(*)
        INTO num_solicitud
        FROM SOLICITUD_SEGUIMIENTOS a,SOLICITUD b
        where a.id_solicitud=b.id_solicitud
        and b.cod_entidad = v_cod_entidad AND b.cod_unidad = v_cod_unidad;
        
        RETURN num_solicitud;
    END;
```

seguir : http://www.it-docs.net/ddata/4829.pdf
