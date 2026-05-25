# Homelab Windows + Linux | AD + VLANs + ACLs + Correo + Ticketing

**Objetivo:** Infraestructura IT virtualizada para practicar soporte N2/N3: Active Directory, redes segmentadas con VLANs/ACLs, correo SMTP local y sistema de tickets.

**Autor**: Fructuoso Mba Oñana Fernadez | **Estado**: En desarrollo activo | **Ubicación**: Madrid, España

---

## Arquitectura

| Componente | Detalle |
| --- | --- |
| **Dominio** | `milab.local` con DC Windows Server 2022 |
| **Virtualización** | Oracle VirtualBox + VMware Workstation 17 |
| **Clientes** | VMs Windows 10 Pro unidas al dominio |
| **Red** | Simulada con Cisco Packet Tracer (C1111X-8P + Catalyst 1000) |
| **VLANs** | 10 Users, 20 Servers, 30 Guest, 40 Lab |

---

## Componentes y Evidencias

### 1. Active Directory

- **OU y GPOs**: Estructura por departamentos con políticas aplicadas
- **Usuarios y grupos**: Creados con PowerShell
- **Unión al dominio**: VMs clientes operativas

| Usuarios y Grupos | PowerShell | Equipos en Dominio |
| --- | --- | --- |
| [![AD Users](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/ad-usuarios.jpg)](Screenshots/ad-usuarios.jpg) | [![PowerShell](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/ad-powershell.jpg)](Screenshots/ad-powershell.jpg) | [![AD Computers](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/ad-equipos.jpg)](Screenshots/ad-equipos.jpg) |

---

### 2. Redes: VLANs y ACLs (Cisco Packet Tracer)

- **VLANs**: Segmentación en L2/L3 con SVIs en el Catalyst 1000
- **ACLs**: Bloqueo de Guest→Servers, permiso solo HTTP/HTTPS
- **DHCP**: Pools por VLAN en el C1111X-8P

| Topología Packet Tracer | ACL en CLI |
| --- | --- |
| [![Topologia](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/packettracer-topologia.jpg)](Screenshots/packettracer-topologia.jpg) | [![ACL](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/packettracer-cli-acl.jpg)](Screenshots/packettracer-cli-acl.jpg) |

---

### 3. Servidor de Correo Local

- **hMailServer**: Dominio `milab.local` con cuentas de prueba
- **IIS SMTP**: Relay configurado para envío interno

| Cuentas hMailServer | Config IIS SMTP |
| --- | --- |
| [![hMailServer](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/hmailserver-cuentas.jpg)](Screenshots/hmailserver-cuentas.jpg) | [![IIS SMTP](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/iis-smtp.jpg)](Screenshots/iis-smtp.jpg) |

---

### 4. Sistema de Tickets

- **osTicket**: Instalado en Windows Server con IIS + MySQL
- **Flujo**: Email → Ticket automático → Asignación → Cierre

| Dashboard osTicket |
| --- |
| [![osTicket](https://github.com/MbaMrF/homelab-windows-linux/raw/main/Screenshots/osticket-dashboard.jpg)](Screenshots/osticket-dashboard.jpg) |

---

## Comandos Clave

```powershell
# AD: Crear OU y usuario
New-ADOrganizationalUnit -Name "Ventas" -Path "DC=milab,DC=local"
New-ADUser -Name "j.perez" -Path "OU=Ventas,DC=milab,DC=local" -Enabled $true

# Packet Tracer - Crear VLAN y trunk
vlan 10
 name USUARIOS
interface GigabitEthernet0/1
 switchport mode trunk
```

---

## 📞 Contacto 632965062


**LinkedIn**: [linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9](https://www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9) | **Email**: taskien666@gmail.com

Abierto a oportunidades de **Helpdesk N1/N2, NOC, Técnico de Sistemas Junior** en Madrid.
