# Homelab Windows + Linux | AD + VLANs + ACLs + Correo + Ticketing

**Objetivo:** Infraestructura IT real en casa para practicar soporte N2/N3: Active Directory, redes segmentadas con VLANs/ACLs, correo SMTP local y sistema de tickets.

**Autor**: Fructuoso Mba Oñana Fernadez | **Estado**: En desarrollo activo | **Ubicación**: Madrid, España

---

## Arquitectura

| Componente | Detalle |
| --- | --- |
| **Dominio** | `milab.local` con DC Windows Server 2022 |
| **Equipos** | 2x PCs Windows 11 Pro unidos al dominio |
| **Red** | Router Cisco C1111X-8P + Switch Catalyst 1000 |
| **VLANs** | 10 Users, 20 Servers, 30 Guest, 40 Lab |
| **Servidor** | HPE ProLiant DL20 Gen10 |

---

## Componentes y Evidencias

### 1. Active Directory

- **OU y GPOs**: Estructura por departamentos con políticas aplicadas
- **Usuarios y grupos**: Creados con PowerShell
- **Unión al dominio**: PCs clientes operativos

| Usuarios y Grupos | PowerShell | Equipos en Dominio |
| --- | --- | --- |
| ![AD Users](Screenshots/ad-usuarios.jpg) | ![PowerShell](Screenshots/ad-powershell.jpg) | ![AD Computers](Screenshots/ad-equipos.jpg) |

---

### 2. Redes: VLANs y ACLs

- **VLANs**: Segmentación en L2/L3 con SVIs en el Catalyst
- **ACLs**: Bloqueo de Guest→Servers, permiso solo HTTP/HTTPS
- **DHCP**: Pools por VLAN

| Topología Packet Tracer | ACL en CLI |
| --- | --- |
| ![Topologia](Screenshots/packettracer-topologia.jpg) | ![ACL](Screenshots/packettracer-cli-acl.jpg) |

---

### 3. Servidor de Correo Local

- **hMailServer**: Dominio `milab.local` con cuentas de prueba
- **IIS SMTP**: Relay configurado para envío interno

| Cuentas hMailServer | Config IIS SMTP |
| --- | --- |
| ![hMailServer](Screenshots/hmailserver-cuentas.jpg) | ![IIS SMTP](Screenshots/iis-smtp.jpg) |

---

### 4. Sistema de Tickets

- **osTicket**: Instalado en Windows Server con IIS + MySQL
- **Flujo**: Email → Ticket automático → Asignación → Cierre

| Dashboard osTicket |
| --- |
| ![osTicket](Screenshots/osticket-dashboard.jpg) |

---

## Comandos Clave

```powershell
# AD: Crear OU y usuario
New-ADOrganizationalUnit -Name "Ventas" -Path "DC=milab,DC=local"
New-ADUser -Name "j.perez" -Path "OU=Ventas,DC=milab,DC=local" -Enabled $true
```

---

## 📞 Contacto

**LinkedIn**: [linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9](https://www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9) | **Email**: taskien666@gmail.com

Abierto a oportunidades de **Helpdesk N1/N2, NOC, Técnico de Sistemas Junior** en Madrid.
