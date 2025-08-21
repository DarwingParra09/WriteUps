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

![alt text](/image/basedatosOracle3.png)

Al encontrar que la consulta trae 3 columnas la query es valida y nos da la información de la categoría en la que se filtró.

### El comentario

3. **`--`**
    
    - Es un comentario en SQL.
        
    - Sirve para ignorar el resto de la query que el backend hubiera agregado.

![alt text](/image/basedatosOracle4.png)

### Objetivo del payload

Este payload busca:

- Ver si la aplicación es vulnerable a **SQL Injection**.
    
- Descubrir cuántas columnas tiene la query.
    
- Una vez que sabes el número, puedes reemplazar un `NULL` con algo útil:
    
    - `UNION SELECT username, password, NULL FROM users--`

![alt text](/image/basedatosOracle5.png)
