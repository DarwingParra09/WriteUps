Este es uno de los muchos retos que hay en Hack the box, este reto trata sobre reversing y directamente no es como las maquinas virtuales.
La plataforma nos trae un archivo zip el cual debemos descomprimir en nuestro ordenar, este nos deja una carpeta e ingresamos a ella

![alt text](../../image/spook1.png)

La carpeta tiene un ejecutable llamado *pass*, al ingresar me pide una contraseña que no tengo 

![alt text](../../image/spook2.png)
![alt text](../../image/spook3.png)

La herramienta **LTRACE** nos permite rastrear las llamadas a funciones de bibliotecas dinámicas que hace un programa en ejecución.
Cuando escribo una contraseña mal, sigue ejecutándose el programa pero esta vez nos muestra cual es la posible contraseña que nos daría acceso.
Aunque la contraseña al parecer esta siendo mostrada no me da acceso.

![alt text](../../image/spook4.png)

Otra forma de poder ver los datos dentro de un ejecutable sin desensamblarlos es con el comando **strings**, aquí también encuentro la contraseña y el carácter que faltaba para que me diera entrada. Ya con esto consigo la flag y termina el reto.

![alt text](../../image/spook5.png)
![alt text](../../image/spook6.png)

Otra herramienta que puede ayudarme para este reto es **RADARE2** es un framework de código abierto para ingeniería inversa.

Abro un ejecutable sin ejecutarlo y de paso analizo el binario completamente

![alt text](../../image/spook7.png)

Busco las funciones detectadas en este ejecutable 

![alt text](../../image/spook8.png)

Desensamblo la entrada main donde posiblemente puede estar la información del script y mientras scrolleo puedo encontrar tanto la contraseña como la flag.

![alt text](../../image/spook9.png)
![alt text](../../image/spook10.png)