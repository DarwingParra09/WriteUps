### **🛠️ Comandos básicos en Radare2**

📌 **Abrir un ejecutable sin ejecutarlo**

```bash
r2 ./programa
```

📌 **Abrirlo en modo debug (ejecutando el binario paso a paso)**

```
r2 -d ./programa
```


📌 **Mostrar información del binario**

```
i
```

📌 **Ver las secciones del binario**

```
iS
```

📌 **Ver las funciones detectadas**

```
afl
```

📌 **Ver las strings dentro del binario**

```
iz
```

📌 **Analizar el binario completamente**

```
aaa
```

📌 **Desensamblar el punto de entrada**

```
pdf
```

📌 **Ejecutar en modo gráfico (GUI: Cutter)**

```
cutter
```

### **Uso en Pentesting y Ciberseguridad**

🔹 **Análisis de malware** (extraer strings, detectar funciones sospechosas).  
🔹 **Exploit Development** (buscar vulnerabilidades en binarios).  
🔹 **Reversing de software propietario** (encontrar funciones ocultas).  
🔹 **Patching** (modificar ejecutables sin recompilar el código fuente).