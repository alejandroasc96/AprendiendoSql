# ALTER TABLE - ALTER / MODIFICAR COLUMNA



\*\*\*\*

### ALTER TABLE - ALTER / MODIFICAR COLUMNA

**SQL Server / MS Access**:

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype;

-- Queremos cambiar el tipo de datos de la columna denominada "DateOfBirth" en la tabla "Personas".
ALTER TABLE Persons
ALTER COLUMN DateOfBirth year;
-- Tenga en cuenta que la columna "DateOfBirth" es ahora del tipo año y su formato será dos o cuatro dígitos.
```

**My SQL / Oracle \(versión anterior 10G\)**:

```sql
ALTER TABLE table_name
MODIFY COLUMN column_name datatype;
```

**Oracle 10G y posterior**:

```sql
ALTER TABLE table_name
MODIFY column_name datatype;
```

