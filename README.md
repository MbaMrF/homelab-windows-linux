# 🚀 Homelab Windows + Linux | SysAdmin & Helpdesk Portfolio

Laboratorio virtualizado desde cero para simular una infraestructura IT empresarial completa. El objetivo: demostrar competencias prácticas en **administración de sistemas Windows/Linux, redes, seguridad y soporte N1/NOC** para la búsqueda activa de empleo en Helpdesk/SysAdmin Junior.

**Autor**: Fructuoso | **Estado**: En desarrollo activo | **Ubicación**: Madrid, España

---

## 🗺️ Arquitectura del Laboratorio

Infraestructura multisede simulada con VirtualBox + VMware para entornos mixtos:

| Máquina Virtual | S.O. | Rol / Servicios | IP |
| --- | --- | --- | --- |
| `DC01` | Windows Server 2025 | **AD DS, DNS, DHCP, Print Services** | `192.168.10.10` |
| `WS2016` | Windows Server 2016 | **Servidor miembro, Carpetas compartidas, GPO** | `192.168.10.11` |
| `WIN10-IT` | Windows 10 Pro | **Equipo técnico IT, RSAT, Thunderbird** | `192.168.10.101` |
| `Ubuntu-Helpdesk` | Ubuntu 22.04 LTS | **osTicket, LAMP Stack, SSH** | `192.168.10.20` |

![Topología VirtualBox](Screenshots/virtualbox-lab.png)
![Topología VMware](Screenshots/vmware-lab.png)

---

## 🖥️ Administración de Active Directory

Dominio `milab.local` desplegado desde cero con estructura de UOs, usuarios y políticas de seguridad.

**Implementado:**
- **Estructura Organizativa**: UOs `Usuarios`, `Grupos`, `Equipos`, `Servidores` para delegación
- **Gestión de Usuarios**: 10+ usuarios creados vía PowerShell `New-ADUser` con contraseña forzada al primer inicio
- **Grupos de Seguridad**: `IT_Soporte`, `Ventas`, `Marketing` con permisos NTFS diferenciados
- **Unión al dominio**: Equipos Windows 10 integrados correctamente al dominio

![Usuarios y Equipos de AD](Screenshots/ad-usuarios.png)
![Creación masiva PowerShell](Screenshots/ad-powershell.png)

---

## 📧 Servicios de Correo y Colaboración

Despliegue de servidor de correo interno para simular comunicaciones corporativas.

**Implementado:**
- **hMailServer**: Instalado en `WS2016` con dominio `milab.local`
- **Buzones corporativos**: Creados para `jperez@milab.local`, `mlopez@milab.local`, `nruiz@milab.local`
- **Cliente Thunderbird**: Configurado en `WIN10-IT` con recepción/envío funcional entre usuarios
- **IIS SMTP**: Servidor SMTP virtual para aplicaciones internas como osTicket

![hMailServer Cuentas](Screenshots/hmailserver-cuentas.png)
![Thunderbird con usuarios](Screenshots/thunderbird-buzones.png)

---

## 🌐 Diseño de Redes con Cisco Packet Tracer

Durante mi formación ASIR en CCC diseñé topologías multisede para entender enrutamiento y seguridad perimetral.

**Proyecto: Conexión Oficinas Madrid - Sucursal Valencia**
- **Direccionamiento IP**: Segmentación `192.168.1.0/24` Madrid y `170.160.1.0/16` Valencia
- **Enrutamiento**: Protocolo RIP entre routers Cisco 1941 para comunicación WAN
- **Seguridad**: Pruebas con ACLs extendidas en CLI para filtrar tráfico Telnet `puerto 23`
- **Servicios**: Integración de PCs, servidores, impresoras y APs WiFi
- **Troubleshooting**: Verificación ICMP extremo a extremo con resultado `Successful`

![Topología Packet Tracer](Screenshots/packettracer-topologia.png)
![Configuración ACL en CLI](Screenshots/packettracer-cli-acl.png)

---

## 🎫 Sistema de Tickets - osTicket

Plataforma de helpdesk desplegada en Ubuntu para gestionar incidencias como en un entorno real.

**Implementado:**
- **Stack LAMP**: Apache + MySQL + PHP 8.1 configurado desde cero
- **Acceso**: Panel agente en `http://192.168.10.20/osticket/scp`
- **Flujo real**: Tickets de prueba creados para simular soporte a usuarios del dominio
- **Integración**: Preparado para notificar vía SMTP a `hMailServer`

![osTicket Dashboard](Screenshots/osticket-dashboard.png)

---

## 🔐 Seguridad, Permisos y GPOs

Aplicación de principio de mínimo privilegio y hardening básico.

**Implementado:**
- **Permisos NTFS**: Carpeta `\\WS2016\IT` con acceso solo a grupo `IT_Soporte`
- **GPOs**: Políticas para forzar fondo de escritorio corporativo y bloqueo de Panel de Control
- **UAC**: Prompt de credenciales al ejecutar `lusrmgr.msc` como usuario estándar `jperez@MILAB`
- **Firewall**: Reglas de entrada/salida para DNS, DHCP, SMB y HTTP

![UAC en cliente](Screenshots/uac-jperez.png)
![Permisos NTFS](Screenshots/ntfs-permisos.png)

---

## 📡 Pruebas de Conectividad y Servicios de Red

Validación de que todos los servicios core funcionan entre máquinas.

**Resultados:**
1. **ICMP**: `ping` exitoso de Ubuntu a `DC01.milab.local` y a clientes
2. **DNS**: `nslookup milab.local` resuelve a `192.168.10.10` desde Ubuntu
3. **DHCP**: `WS2016` entregando IPs en rango `192.168.10.100-150`
4. **Print Server**: Impresoras compartidas desde `DC01` visibles en red

![Ping Ubuntu a DC](Screenshots/ubuntu-ping-dc.png)
![DNS nslookup](Screenshots/dns-nslookup.png)
![DHCP Scope](Screenshots/dhcp-scope.png)

---

## 🛠️ Stack Tecnológico

**Virtualización**: Oracle VirtualBox 7, VMware Workstation 17  
**Sistemas**: Windows Server 2025/2016, Windows 10, Ubuntu Server 22.04  
**Redes**: Cisco Packet Tracer, TCP/IP, DNS, DHCP, RIP, ACLs  
**Microsoft**: Active Directory DS, GPO, DNS, DHCP, Print Services, IIS, hMailServer  
**Linux**: Bash, Apache2, MySQL, PHP, SSH, UFW  
**Scripting**: PowerShell, Bash  
**Helpdesk**: osTicket, Thunderbird  

---

## 🎯 Roadmap 

- [ ] Desplegar WDS/MDT para instalación desatendida de Windows
- [ ] Configurar VPN Site-to-Site entre sedes con pfSense
- [ ] Integrar Zabbix/Grafana para monitorización
- [ ] Automatizar alta de usuarios con CSV + PowerShell
- [ ] Implementar WSUS para gestión de parches

---

## 📞 Contacto 623965062

**LinkedIn**: [www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9] | **Email**: [taskien666@gmail.com]  
Abierto a oportunidades de **Helpdesk N1/N2, NOC, Técnico de Sistemas Junior** en Madrid.
