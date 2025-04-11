
## Gestión de permisos básica

**Permiso de Lectura**

- Quitar permiso 
```bash
sudo chmod -r [file]
```

- Poner permiso 
```bash
sudo chmod +r [file]
```

**Permiso de escritura**

- Quitar permiso 
```bash
sudo chmod -w [file]
```

- Poner permiso 
```bash
sudo chmod +w [file]
```

**Permiso de ejecución**

- Quitar permiso 
```bash
sudo chmod -x [file]
```

- Poner permisos 
```bash
sudo chmod +x [file]
```

## Gestión de permisos detallada

Permiso de lectura a grupos

- Quitar permiso 
```bash
sudo chmod -g-r [file]
```

- Poner permiso
```bash
sudo chmod -g+r [file]
```

Permiso de escritura a grupos

- Quitar permiso
```bash
sudo chmod -g-w [file]
```

- Poner permiso
```bash
sudo chmod -g+w [file]
```

Permiso de ejecución a grupos

- Quitar permisos
```bash
sudo chmod -g-x [file]
```

- Poner permisos
```bash
sudo chmod -g+x [file]
```

Permiso de lectura a otros

- Quitar permisos
```bash
sudo chmod -o-r [file]
```

- Poner permiso
```bash
sudo chmod -o+r [file]
```

Permiso de escritura a otros

- Quitar permiso
```bash
sudo chmod -o-w [file]
```

- Poner permiso
```bash
sudo chmod -o+w [file]
```

Permiso de ejecución a otros

- Quitar permisos
```bash
sudo chmod -o-x [file]
```

- Poner permisos
```bash
sudo chmod -o+x [file]
```

## Gestión de grupos

**Agregar grupos**

```bash
sudo addgroup [nombre_del_grupo]
```

**Añadir usuarios a grupo**

```bash
sudo usermod -aG [nombre_del_grupo] [usuario]
```

**Cambiar propietario de un archivo**

```bash
sudo chown [usuario]:[nombre_del_grupo] [file]
```

**Eliminar grupo**

```bash
sudo groupdel [nombre_del_grupo]
```

**Cambiar propietario de un archivo**

```bash
sudo chown [usuario]:[nombre_del_grupo] [file]
```

**Agregar Usuarios**

```bash
sudo adduser [usuario]
```

**Eliminar usuario**

```bash
sudo userdel -r [usuario]
```
