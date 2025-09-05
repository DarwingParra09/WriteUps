La siguiente vulnerabilidad trata de listar bases de datos que no son de Oracle. Lo primero a buscar es cuantas columnas trae la base de datos de la pagina.

![alt text](/image/DB_NoOracle1.png)

Entre prueba y error consigo que la base de datos trae dos columnas, en donde cada una trabaja con tipo de texto.

![alt text](/image/DB_NoOracle2.png)

### Contenido de la base de datos

![alt text](/image/DB_NoOracle3.png)
```
SELECT * FROM information_schema.tables
SELECT * FROM information_schema.columns WHERE table_name = 'TABLE-NAME-HERE'
```
- `information_schema.tables` → lista todas las tablas.
- `information_schema.columns` → lista todas las columnas de una tabla concreta.
- `table_name` → columna de la vista `information_schema.tables` que contiene el **nombre de todas las tablas** de la base de datos.

![alt text](/image/DB_NoOracle4.png)

![alt text](/image/DB_NoOracle5.png)

- `column_name` = el campo dentro de `information_schema.columns` que te da el **nombre real de la columna**.
- `information_schema.columns` = mapa de **todas las columnas** de esas tablas.

![alt text](/image/DB_NoOracle6.png)

![alt text](/image/DB_NoOracle7.png)

Utilicé la siguiente carga útil (reemplazando los nombres de tabla y columna) para recuperar los nombres de usuario y las contraseñas de todos los usuarios.

![alt text](/image/DB_NoOracle8.png)
![alt text](/image/DB_NoOracle9.png)
![alt text](/image/DB_NoOracle10.png)

