# DISTINCTROW



Omite las filas duplicadas basándose en la totalidad del registro y no solo en los campos seleccionados. Esto no lo soporta SQLite. Si la tabla empleados contiene dos registros: Antonio López y Marta López el ejemplo del predicado DISTINCT devuleve un único registro con el valor López en el campo Apellido ya que busca no duplicados en dicho campo. Este último ejemplo devuelve dos registros con el valor López en el apellido ya que se buscan no duplicados en el registro completo.

```sql
SELECT DISTINCTROW Apellido FROM Empleados;
```

