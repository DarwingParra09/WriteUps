Tengo una pagina con múltiples categorías, el objetivo de esta vulnerabilidad es encontrar la columna con permisos de contenido textual.

![alt text](/image/01.SQL_contenidoTexual.png)

Ya que encuentro las columnas que tiene la base de datos lo siguiente es revisar cual de las posiciones recibe texto.

![alt text](/image/02.SQL_contenidoTexual.png)
![alt text](/image/03.SQL_contenidoTexual.png)

La primera posición hace que la pagina se caiga, ya con eso puedo tachar de que esta no devuelve la información que busco.

![alt text](/image/04.SQL_contenidoTexual.png)
![alt text](/image/05.SQL_contenidoTexual.png)

Probando la segunda columna pude ver que al enviar el payload me devolvió el dato que había ingresado, de aquí se pueden hacer peticiones en busca de credenciales.

![alt text](/image/06.SQL_contenidoTexual.png)
![alt text](/image/07.SQL_contenidoTexual.png)