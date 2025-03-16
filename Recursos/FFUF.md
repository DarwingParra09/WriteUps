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

```bash
ffuf -w [wordlis.txt]:FUZZ -u http://FUZZ.[SERVER_IP:PORT]/ -H "HOST: FUZZ.SERVER_IP"
```

