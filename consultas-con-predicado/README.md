# Consultas con predicado



Se llama **predicado** a lo que va **entre** el **SELECT** y el primer **nombre** de **columna** que vamos a recuperar. **Sirven para modificar** el **comportamiento** de las **consultas**. Los posibles predicados son:

* **ALL**:  Devuelve todos los campos de la tabla.
* **TOP**:Devuelve un determinado número de registros de la tabla. En SQLite se soporta como _LIMIT_.
* **DISTINCT**: Omite los registros cuyos campos seleccionados coincidan totalmente.
* **DISTINCTROW**: Omite los registros duplicados basándose en la totalidad del registro y no sólo en los campos seleccionados.

