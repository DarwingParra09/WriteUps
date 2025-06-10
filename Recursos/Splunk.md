**Splunk es una plataforma de análisis de datos en tiempo real**, especialmente diseñada para recolectar, indexar, buscar, visualizar y analizar grandes volúmenes de datos generados por máquinas (logs, eventos, métricas, etc.).

## **¿Para qué se usa en ciberseguridad (SOC)?**

Splunk funciona como un **SIEM** (Security Information and Event Management), lo cual lo hace ideal para:

✅ **Monitorear eventos de seguridad**  
✅ **Detectar actividades sospechosas y amenazas**  
✅ **Correlacionar eventos entre distintos sistemas**  
✅ **Crear alertas personalizadas**  
✅ **Visualizar en dashboards ataques, incidentes y estadísticas de red**

### **¿Qué puede recolectar Splunk?**

- Logs de Windows (`eventvwr`)
- Logs de Linux (`/var/log/`)
- Tráfico de red (usando herramientas como Zeek o Suricata)
- Logs de firewall, IDS, antivirus, proxies
- Datos de servicios en la nube (AWS, Azure, GCP)
- Información de endpoints con Splunk Universal Forwarder

## Busquedas Comunes en Windows

**DETECCION DE BLOODHOUND**

El recopilador BloodHound ejecuta númerosas consultas LDAP dirigidas al controlador de dominio, con el objetivo de acumular información sobre el dominio.
Windows puede sugerir usar el registro de supervision del rendimiento de LDAP `Event 1644`. Aun con ello puede que no se generen muchos de los eventos.

| 🧭 **Objetivo**                                            | 🔍 **Filtro LDAP / Tool**                          | 💡 **SPL en Splunk (equivalente)**                                         | 📝 **Notas / Eventos clave**                                                           |
| ---------------------------------------------------------- | -------------------------------------------------- | -------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 🔑 Buscar usuarios con `pass` en descripción o comentarios | `(                                                 | (description=_pass_)(comment=_pass_))`**Metasploit**                       | `index=wineventlog (description="*pass*" OR comment="*pass*")`                         |
| 🖥️ Enumerar computadoras Windows Server                   | `(operatingSystem=*server*)`**Metasploit**         | `index=wineventlog operatingSystem="*Server*"`                             | Alternativo: buscar hosts con nombre `SRV` o eventos de logon tipo server.             |
| 👥 Enumerar grupos de AD                                   | `(objectClass=group)`**Metasploit**                | `index=wineventlog (EventCode=4731 OR EventCode=4732 OR EventCode=4733)`   | 4731 = Grupo creado4732 = Usuario agregado4733 = Usuario removido                      |
| 🧑‍💼 Grupos con `managedBy`                               | `(objectClass=group)(managedBy=*)`**Metasploit**   | No directo en Splunk sin integración LDAP.Alternativa: `EventCode=4728`    | Para ver cambios en grupos de seguridad (requiere logging de AD detallado).            |
| 🖧 Enumerar computadoras                                   | `sAMAccountType=805306369`**PowerView**            | `index=wineventlog EventCode=4624 Logon_Type=3``                           | stats count by host`                                                                   |
| 👤 Enumerar usuarios de dominio                            | `samAccountType=805306368`**PowerView**            | `index=wineventlog EventCode=4720 OR EventCode=4722 OR EventCode=4723`     | 4720 = Usuario creado4722 = Habilitado4723 = Cambio de password                        |
| 🎭 Buscar SPNs (Kerberoast)                                | `servicePrincipalName=*`**PowerView**              | `index=wineventlog EventCode=4769`                                         | TGS requested (Kerberos), clave para ataques de SPN                                    |
| 📂 Buscar DFS Shares                                       | `objectClass=msDFS-Linkv2`**PowerView**            | `index=* "DFS"`                                                            | Solo si hay eventos relacionados con DFS; también buscar acceso SMB (`EventCode=5140`) |
| 🗃️ Enumerar Organizational Units (OUs)                    | `(objectCategory=organizationalUnit)`**PowerView** | No directo sin integración con AD schema.Alternativa: ver `EventCode=5136` | 5136 = Cambios en objetos del directorio                                               |
| 🔍 Buscar usuarios con `samAccountType=805306368`          | `samAccountType=805306368`**Empire**               | `index=wineventlog EventCode=4720 OR EventCode=4722`                       | Eventos de usuario, útil para detectar cambios sospechosos                             |

**EventCodes más usados en Splunk para ataques AD**

| 🛠️ **Acción**                    | ⚙️ **EventCode** | 📌 **Descripción**      |
| --------------------------------- | ---------------- | ----------------------- |
| Inicio de sesión exitoso          | `4624`           | Revisión de logins      |
| Fallo de login                    | `4625`           | Intentos fallidos       |
| Creación de usuario               | `4720`           | Nuevo usuario           |
| Cambio de contraseña              | `4723`, `4724`   | Modificaciones de pass  |
| Creación de grupo                 | `4731`           | Nuevos grupos           |
| Usuario agregado a grupo          | `4728`, `4732`   | Privilegios modificados |
| Solicitud de TGS (SPN)            | `4769`           | Para Kerberoasting      |
| Acceso a recurso compartido (SMB) | `5140`           | DFS, C$ y otros shares  |

## Detección de rociado de contraseñas

Este es muy diferente a los ataques de fuerza bruta donde un atacante prueba numerosas contraseñas hacia una sola cuenta de usuario.
Un patrón que se asocia mucho es el intento fallido de inicio de sesión `Event ID 4625 - Failed Logon`.

Otros registros de eventos que pueden ayudar a la detección de robo de contraseñas:

- `4768 and ErrorCode 0x6 - Kerberos Invalid Users`
- `4768 and ErrorCode 0x12 - Kerberos Disabled Users`
- `4776 and ErrorCode 0xC000006A - NTLM Invalid Users`
- `4776 and ErrorCode 0xC0000064 - NTLM Wrong Password`
- `4648 - Authenticate Using Explicit Credentials`
- `4771 - Kerberos Pre-Authentication Failed`

**Búsqueda de rociado de contraseñas con Splunk**

```shell-session
index=main earliest=1690280680 latest=1690289489 source="WinEventLog:Security" EventCode=4625
| bin span=15m _time
| stats values(user) as Users, dc(user) as dc_user by src, Source_Network_Address, dest, EventCode, Failure_Reason
```

*Nota: En caso de no encontrar nada se puede cambiar el rango de tiempo y modificar la búsqueda eliminando los filtros de tiempo earliest y latest.*

## Detección de ataques tipo respondedor

**Intoxicación por LLMNR/NBT-NS/mDNS**

`LLMNR (Link-Local Multicast Name Resolution) and NBT-NS (NetBIOS Name Service) poisoning`, también conocidos como suplantación de NBNS, son ataques a nivel de red que aprovechan las ineficiencias de estos protocolos de resolución de nombres.

**Detección de ataques tipo respondedor con Splunk**

```shell-session
index=main earliest=1690290078 latest=1690291207 SourceName=LLMNRDetection
| table _time, ComputerName, SourceName, Message
```

El ID de evento 22 de Sysmon también se puede utilizar para rastrear consultas DNS asociadas con recursos compartidos de archivos inexistentes o mal escritos.

```shell-session
index=main earliest=1690290078 latest=1690291207 EventCode=22 
| table _time, Computer, user, Image, QueryName, QueryResults
```



