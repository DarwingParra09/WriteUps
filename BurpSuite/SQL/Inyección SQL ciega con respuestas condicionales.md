
La siguiente vulnerabilidad es acerca de una injección SQL ciega, está no nos mostrará los datos en pantalla, pero si nos da una pequeña pista. *WELCOME BACK!*

![alt text](/image/1.SQL_Ciega.png)
![alt text](/image/2.SQL_Ciega.png)

Cuando edito el siguiente Payload que da como resultado negativo el response nos deja de mostrar el *WELCOME BACK!* esto ya es una pista valiosa porque puede ayudarme a encontrar o verificar información en la base de datos.

![alt text](/image/3.SQL_Ciega.png)
![alt text](/image/4.SQL_Ciega.png)

Ese payload **comprueba si la tabla `users` existe** → usando una condición booleana que solo devuelve TRUE si la tabla está presente. 

![alt text](/image/5.SQL_Ciega.png)
![alt text](/image/6.SQL_Ciega.png)

Aquí la condición booleana al ser False pues no me da la pista.

![alt text](/image/7.SQL_Ciega.png)
![alt text](/image/8.SQL_Ciega.png)

Lo siguiente es terminar de modificar el payload, ahora que sabemos que hay una tabla usuarios voy a buscar dentro de las columnas un usuario de nombre *administrator*, haciendo referencia que seguimos teniendo una condición booleana True.

![alt text](/image/9.SQL_Ciega.png)
![alt text](/image/10.SQL_Ciega.png)

Lo siguiente es saber en cuanto puede estar el tamaño de la contraseña, asi que con el ejemplo anterior sobre la pista puedo facilitar las cosas y dar con que hay un tamaño de 20 caracteres para la el usuario Administrator.

![alt text](/image/11.SQL_Ciega.png)
![alt text](/image/12.SQL_Ciega.png)
![alt text](/image/13.SQL_Ciega.png)
![alt text](/image/14.SQL_Ciega.png)
![alt text](/image/15.SQL_Ciega.png)
![alt text](/image/16.SQL_Ciega.png)

- Es un ataque **Blind Boolean-based SQLi** para **adivinar contraseñas letra por letra**.
    
- Hago la  **fuerza bruta sobre cada posición**:
    
    - Pruebo `'a'`, `'b'`, `'c'` … hasta `'z'` para la posición 1.
        
    - Luego `'SUBSTRING(password,2,1)` para la segunda letra.
        
    - Repeto hasta reconstruir toda la contraseña.

![alt text](/image/17.SQL_Ciega.png)
![alt text](/image/18.SQL_Ciega.png)

Para esto voy a utilizar la herramienta CAIDO, aunque también funciona en Burpsuite, pero es más lento por el hecho de tener el Community.
La idea de usar Caido es seleccionar dos payloads.

![alt text](/image/19.SQL_Ciega.png)

El primer payload seria el tamaño de caracteres para la contraseña de administrator, así que va de 1 a 20.

![alt text](/image/20.SQL_Ciega.png)

El segundo Payload será unas lista alfanumerico en minusculas para esta vulnerabilidad. Al correr ya todo nos dará un tamaño en bits.

![alt text](/image/21.SQL_Ciega.png)

Caido tiene un facilidad en la carga de los payloads, dentro del tamaño de bits puede ver ciertas diferencias así que filtro por un tamaño distinto de 5388 y con ello consigo la contraseña de cada posición entre los 20 caracteres de la contraseña de *administrator*

![alt text](/image/22.SQL_Ciega.png)
![alt text](/image/23.SQL_Ciega.png)
