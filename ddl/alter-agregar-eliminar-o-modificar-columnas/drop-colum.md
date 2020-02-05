# DROP COLUM

Para eliminar una columna de una tabla, use la siguiente sintaxis \(observe que algunos sistemas de bases de datos no permiten eliminar una columna:

```sql
ALTER TABLE table_name
DROP COLUMN column_name;
-- Se desea eliminar la columna denominada "DateOfBirth" en la tabla "Personas".
ALTER TABLE Persons
DROP COLUMN DateOfBirth;
```

