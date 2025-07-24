Tenemos una pagina normal, una tienda común y corriente, pero tiene una mala configuración a la hora de escoger cualquiera de los filtros que hay para comprar.

![alt text](/image/Sql1.png)

Comenzaré filtrando solo la información de comida y bebidas, si sigo esta navegación con el BurpSuite puedo modificar la opcion categoria por `' OR+1=1 --` 

![alt text](/image/Sql2.png)
![alt text](/image/Sql3.png)

La cadena `'+OR+1=1--` es un clásico ejemplo de **inyección SQL** que intenta **manipular una consulta SQL** para alterar su lógica y, generalmente, **omitir la autenticación** o acceder a datos no autorizados.

---

### 🔍 ¿Qué hace específicamente?

Vamos a suponer que tienes un formulario de login con los siguientes campos:

- Usuario: `admin`
    
- Contraseña: `' OR 1=1--`
    

Y el servidor construye la siguiente consulta SQL (de forma **vulnerable**, concatenando texto directamente):

`SELECT * FROM usuarios WHERE username = 'admin' AND password = '' OR 1=1--';`

---

### 📌 ¿Cómo se interpreta esta consulta?

- `' OR 1=1` siempre es **verdadero**, porque 1=1 siempre es cierto.
    
- `--` es un comentario en SQL, así que **todo lo que viene después se ignora** (por ejemplo, `'` final, etc.).

Así que esto hace que muestre todas las categorías, hasta las que estaban ocultas.

![alt text](/image/Sql4.png)
