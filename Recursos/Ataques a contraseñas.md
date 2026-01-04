
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




