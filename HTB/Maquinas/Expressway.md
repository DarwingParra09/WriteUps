
|  MAQUINA   |  OS   | DIFICULTAD |  PLATAFORMA  |     IP      |
| :--------: | :---: | :--------: | :----------: | :---------: |
| Expressway | Linux |   Facil    | Hack The Box | 10.10.11.87 |
## RECONOCIMIENTO

Comencé con un escaneo de puertos para identificar que servicios están abiertos, revisar la versión de estos y los posibles exploits que puedan encontrar de ellos.

![alt text](/image/expres1.png)

Como solo encontré un solo puerto por el lado de TPC pues se me hizo raro y opté por hacer ahora el scanner hacia UDP. Encontrando el servicio isakmp en el puerto 500 busqué sobre como podia vulnerar este servicio y dentro de la busqueda me mencionan que puedo hacer un cracking para autenticarme con PSK.

![alt text](/image/expres2.png)

## ANALISIS DE VULNERABILIDAD

Para extraer el hash y crackear hago la siguiente petición.

![alt text](/image/expres3.png)

`-M -A` = Aggressive mode; `--pskcrack=hashes.txt` guarda los parámetros necesarios para el cracking offline en `hashes.txt`

![alt text](/image/expres4.png)

Una vez obtengo el archivo hash utilizo la herramienta hashcat para descifrar la password.
**Hashcat Puede** crackear hashes de IKE (Aggressive Mode PSK). Hay dos modos relevantes según el algoritmo de hash que me devolvió el servidor:

- **Modo 5300** → _IKE-PSK MD5_. 
- **Modo 5400** → _IKE-PSK SHA1_.

![alt text](/image/expres5.png)
![alt text](/image/expres6.png)

La contraseña una vez dada la utilizo para conseguir una conexión por medio del servicio ssh y obtengo inicio de sesión.

![alt text](/image/expres7.png)

## EXPLOTACION

Encuentro la flag de usuario. Ahora debo buscar como elevar privilegios y algo muy importante es revisar la version del sudo si estamos trabajando en OS Linux.

![alt text](/image/expres8.png)

Investigando encuentro una vulnerabilidad hacia la versión del Sudo, adjunto el enlace donde encontré el exploit.

![alt text](/image/expres9.png)

## POST-EXPLOTACION

Comparto el exploit al usuario ike, le doy permisos de ejecución y ejecuto el exploit el cual me da la escala de privilegios y obtengo la flag de root.

https://github.com/kh4sh3i/CVE-2025-32463?tab=readme-ov-file

![alt text](/image/expres10.png)
![alt text](/image/expres11.png)
