En cualquier tipo de categoria utilizo la herramienta burpsuite y modifico la petición para encontrar las columnas que trae la base de datos. Sabes que la base de datos está hecha con Oracle.
![alt text](/image/01.DatosOracle.png)
![alt text](/image/02.DatosOracle.png)
![alt text](/image/03.DatosOracle.png)
![alt text](/image/04.DatosOracle.png)

Utilizo el payload para encontrar la información de todas las tablas que puedan encontrarse en la base de datos.

![alt text](/image/05.DatosOracle.png)
![alt text](/image/06.DatosOracle.png)

Identificado el nombre de la tabla buscamos las columnas que haya dentro de esta. Consigo las columnas de la tabla usuarios

![alt text](/image/07.DatosOracle.png)
![alt text](/image/08.DatosOracle.png)
![alt text](/image/09.DatosOracle.png)

Modifico nuevamente el payload para llamar la columna usuario y contraseña de la tabla de usuarios. Al enviar la petición el codigo fuente me da la información de las credenciales, el laboratorio tenia como objetivo hallar el usuario ADMINISTRADOR.

![alt text](/image/10.DatosOracle.png)
![alt text](/image/11.DatosOracle.png)
![alt text](/image/12.DatosOracle.png)

