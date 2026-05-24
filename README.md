# 👋 Hola, soy Fructuoso Mba Oñana Fernadez

> **Técnico de Sistemas con 9 años en NOC | Especializándome en Cloud Azure | Busco Helpdesk N1 / SysAdmin Junior en Madrid**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Perfil-blue?style=for-the-badge&logo=linkedin)](https://www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9)
[![AZ-900](https://img.shields.io/badge/AZ--900-En_curso_%7C_10_Jun_2026-0078D4?style=for-the-badge&logo=microsoftazure&logoColor=white)](https://learn.microsoft.com/es-es/credentials/certifications/azure-fundamentals/)
[![GitHub](https://img.shields.io/badge/GitHub-MbaMrF-181717?style=for-the-badge&logo=github)](https://github.com/MbaMrF)

---

## 🎯 Objetivo

Demostrar con labs prácticos mi experiencia de 9 años en NOC adaptada a entornos Windows/Linux y Cloud Azure. Actualmente preparando certificación **Microsoft AZ-900 | Examen: 10 Junio 2026**.

Abierto a oportunidades de **Helpdesk N1, Soporte IT o Técnico de Sistemas Junior** en Madrid. Híbrido / Remoto.

---

## 👤 Sobre mí

9 años como técnico NOC en telecomunicaciones (GECOMSA, Guinea Ecuatorial) gestionando incidencias, monitorización de red y SLAs en entornos de producción 24x7. Formación técnica en ZTE University (Shenzhen, 2009). ASIR completado en España.

Combino experiencia real en operaciones críticas con administración de sistemas para incorporarme al sector IT en Madrid. Dominio avanzado del inglés, nativo en español, francés básico.

---

## 🛠 Stack Tecnológico

**Virtualización**
- Oracle VirtualBox, VMware Workstation Pro 17

**Sistemas Operativos**
- Windows Server 2022 / 2016, Windows 10/11 Pro
- Ubuntu Server 22.04, Ubuntu Desktop

**Servicios Windows**
- Active Directory DS, DNS, DHCP, Print Services, RDP
- PowerShell (scripting y automatización)

**Servicios Linux**
- SSH, Apache, MariaDB, osTicket v1.18.1

**Redes**
- Subnetting, VLANs, NAT, Red Interna VirtualBox
- TCP/IP, Configuración de Firewall

**Ticketing & ITSM**
- osTicket, ServiceNow (conocimiento), Jira, Freshdesk

**Cloud**
- Microsoft Azure | AZ-900 en curso

---

## 📸 Labs Documentados

### 🔬 Lab Principal: Infraestructura de Virtualización

Homelab con múltiples VMs corriendo simultáneamente para simular entorno de empresa real:

- **MBA-DOMINIO** — Windows Server 2022 (DC) · `192.168.100.10`
- **WS2016** — Windows Server 2016 (DHCP, Print Services, RDP) · `192.168.100.15`
- **Cliente01** — Windows 10 Pro (unido al dominio) · `192.168.100.20`
- **Ubuntu Server** — osTicket + MariaDB · `192.168.100.30`

![Homelab VirtualBox](Screenshots/virtualbox-lab.png)
![Homelab VMware](Screenshots/vmware-lab.png)

---

### 🏢 Lab 1 — Active Directory en Windows Server 2022

Implementación completa de entorno de dominio desde cero:

- Instalación del rol AD DS y promoción a Domain Controller `milab.local`
- Estructura de OUs, 5 usuarios y grupos de seguridad
- Control de acceso basado en roles (RBAC) con carpetas compartidas SMB
- Unión de cliente Windows 10 Pro al dominio
- Resolución de 9 problemas reales documentados (DNS, Firewall, edición Windows)
- 6/6 tests de verificación superados — incluyendo denegación de acceso a usuarios sin permiso

![AD DS](Screenshots/ad-usuarios.png)

---

### 📧 Lab 2 — Servidor de Email con hMailServer

Instalación y configuración de servidor de correo en Windows Server:

- hMailServer 5.6.8 + Thunderbird como cliente
- Simulación de cuarentena y reglas antispam
- Configuración de puertos 25/143/110 y reglas de Firewall
- Resolución de bug en campos vacíos de reglas de filtrado

---

### 🎫 Lab 3 — Sistema de Ticketing con osTicket

Instalación y configuración de osTicket v1.18.1 sobre Ubuntu Server:

- Stack LAMP: Apache + MariaDB + PHP
- 8 Help Topics configurados, 4 SLA Plans, departamentos y agentes
- Simulación de casos reales de Helpdesk N1/N2

---

### 🖨 Lab 4 — Servicios Windows Server 2016

- DHCP con scope y reservas configuradas via PowerShell
- Print Services con impresoras virtuales compartidas en red
- Scripts PowerShell guardados en `C:\Scripts\`

---

### 🐧 Lab 5 — Conectividad Linux-Windows

Cliente Ubuntu haciendo ping al Domain Controller por nombre, validando resolución DNS en entorno mixto.

![Ping Ubuntu a DC](Screenshots/ubuntu-ping-dc.png)

---

## 📅 Roadmap Certificaciones

- [ ] **AZ-900: Azure Fundamentals** → 10/06/2026
- [ ] **AZ-104: Azure Administrator** → Q3 2026
- [ ] **CompTIA Network+** → Q4 2026

---

## 📊 GitHub Stats

[GitHub Stats](https://github-readme-stats.vercel.app/api?username=MbaMrF&show_icons=true&theme=dark&hide_border=true)

---

## 📫 Contacto

- 💼 **LinkedIn:** [linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9](https://www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9)
- 📧 **Email:** taskien666@gmail.com
- 📍 **Ubicación:** Madrid, España

---

<div align="center">

*"9 años resolviendo incidencias en producción. Ahora construyo labs para demostrarlo."*

</div>
