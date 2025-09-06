Escogí una categoría cualquiera e indagué que tiene dos columnas.

![alt text](/image/01.Valores_UnaColumna.png)
![alt text](/image/02.Valores_UnaColumna.png)

Busqué la columna que me da el valor en texto, ya identificado voy a buscar la demás información sobre las tablas que trae la base de datos.

![alt text](/image/03.Valores_UnaColumna.png)
![alt text](/image/04.Valores_UnaColumna.png)
![alt text](/image/05.Valores_UnaColumna.png)
![alt text](/image/06.Valores_UnaColumna.png)

Ya identificado el nombre de la tabla, busco la columna que está dentro de la tabla USERS.

![alt text](/image/07.Valores_UnaColumna.png)
![alt text](/image/08.Valores_UnaColumna.png)

Ahora la mejor parte, como hago para que la consulta se haga en una columna.
El `||'~'||` concatena el `username` y el `password` en **una sola columna**, separados por `~`.  
Así se puede exfiltrar más de un campo aunque la aplicación solo muestre un valor.

![alt text](/image/09.Valores_UnaColumna.png)
![alt text](/image/10.Valores_UnaColumna.png)
![alt text](/image/11.Valores_UnaColumna.png)
