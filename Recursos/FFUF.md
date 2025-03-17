## Fuzzing de directorios

Podemos colocar la palabra clave `FUZZ`  donde estaría el directorio dentro de nuestra URL

```bash
ffuf -w [wordlist] -u http://[SERVER_IP:PORT]:PUERTO/FUZZ
```

## Fuzzing de extensión

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://[SERVER_IP:PORT]/index.FUZZ
```

## Fuzzing de página

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://[SERVER_IP:PORT]/blog/FUZZ.php
```

## Escaneo Recursivo

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://[SERVER_IP:PORT]/FUZZ -recursion -recursion-depth 1 -e .php -v
```

## Fuzzing de subdominos

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://FUZZ.[SERVER_IP:PORT]/
```

La flag -fs filtra el tamaño en bytes dentro de la búsqueda de subdominios

```bash
ffuf -w [wordlis.txt]:FUZZ -u http://FUZZ.[SERVER_IP:PORT]/ -H "HOST: FUZZ.SERVER_IP" -fs xxx
```

## Parámetro Fuzzing - GET

Para peticiones cambiamos la wordlist por una que contenga parámetros que pueden llegar a darse en la web.

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://[SERVER_IP:PORT]/admin/admin.php?FUZZ=key -fs xxx
```

## Parámetro Fuzzing - POST

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://[SERVER_IP:PORT]/admin/admin.php -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```

## VALOR FUZZING

```bash
ffuf -w [wordlist.txt]:FUZZ -u http://[SERVER_IP:PORT]/admin/admin.php -X POST -d 'id=FUZZ' -H 'Content-Type: application/x-www-form-urlencoded' -fs xxx
```