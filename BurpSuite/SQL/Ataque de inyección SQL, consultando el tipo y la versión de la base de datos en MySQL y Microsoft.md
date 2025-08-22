La siguiente vulnerabilidad es la búsqueda de la versión de la base de datos que trabaja la pagina web.

![alt text](/image/versionDB1.png)

Filtrando por la categoría accesorios busco cuantas columnas trae la base de datos.

- Al colocar 3 columnas generaba error ❌
- Ingresando 2 columnas generó resultado en la pagina sin problemas ✅
(Nota: El uso de almohadilla o numeral es un tipo de comentario en bases de datos de MySQL y Microsoft)

![alt text](/image/versionDB2.png)
![alt text](/image/versionDB3.png)
### Versión Base de datos

| Oracle     | `SELECT banner FROM v$version   SELECT version FROM v$instance   ` |
| ---------- | ------------------------------------------------------------------ |
| Microsoft  | `SELECT @@version`                                                 |
| PostgreSQL | `SELECT version()`                                                 |
| MySQL      | `SELECT @@version`                                                 |
La información anterior son los tipos de formas de obtener la versión de base de datos, si adaptamos esto a la petición tendría lo siguiente.

![alt text](/image/versionDB4.png)

La propia pagina no nos puede dar el resultado esperado, pero el burpsuite trae consigo la ventaja de ver el código fuente y se encuentra la versión de la base de datos que está siendo utilizada

![alt text](/image/versionDB5.png)

