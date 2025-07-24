Esta vulnerabilidad va hacia el inicio de sesión, en donde con un bypass se conseguirá obtener el acceso a la cuenta administrator.

![alt text](/image/sesion1.png)

### Inyección con `administrator'--`

Si el atacante pone en el campo de **usuario**:

`administrator'--`

Y cualquier cosa (o nada) en el campo de **contraseña**, la consulta SQL se vuelve:

`SELECT * FROM usuarios WHERE username = 'administrator'--' AND password = '';`

---

### ⚠️ ¿Qué está pasando aquí?

1. **`--`** inicia un comentario, por lo tanto, **todo lo que sigue (incluido `' AND password = ''`) es ignorado**.
    
2. La consulta final que se ejecuta en el servidor es:
    `SELECT * FROM usuarios WHERE username = 'administrator';`

Esto quiere decir:

✅ **El sistema va a devolver el usuario “administrator” sin verificar su contraseña**.

---

### 🎯 Resultado

- Si el sistema **solo verifica que se devuelva una fila válida**, el atacante entra como el usuario `administrator`.
    
- Si el sistema requiere una coincidencia exacta con usuario _y_ contraseña, esta inyección **bypassea la contraseña**.

![alt text](/image/sesion2.png)

![alt text](/image/sesion3.png)