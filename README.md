# Homelab IT: Dominio AD Virtualizado + Laboratorio Redes N1/N2

**Proyecto para reincorporación al sector IT como Técnico N1/N2 en Madrid**

[![LinkedIn](https://img.shields.io/badge/LinkedIn-Fructuoso_Mba-blue)](https://www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9/)
[![Email](https://img.shields.io/badge/Email-Contacto-red)](#-contacto)

---

## 🎯 Objetivo

Tras 9 años de experiencia en NOC de telecomunicaciones y unos años fuera del sector, he creado este laboratorio virtualizado para actualizar competencias y optar a puestos de Técnico de Sistemas N1/N2.

## 🛠️ Entorno del Laboratorio

| Tecnología | Uso en el Lab |
| --- | --- |
| **Virtualización** | VMware Workstation / Hyper-V |
| **Servidor** | Windows Server 2022 Datacenter |
| **Dominio** | `milab.local` - AD DS, DNS, DHCP, GPOs |
| **Clientes** | Windows 10 Pro unido al dominio |
| **Redes** | Laboratorio virtual para VLANs y routing básico |
| **Servicios** | hMailServer, IIS SMTP, MySQL, osTicket |
| **Scripting** | PowerShell para gestión de AD |

> **Importante:** Todo el entorno está montado sobre máquinas virtuales. No se utiliza hardware de red físico dedicado.

## 📋 Tareas Realizadas

1.  **Instalación AD DS:** Promoción de DC, creación de 25 usuarios en 4 OUs mediante PowerShell
2.  **GPOs aplicadas:** Mapeo de unidad de red Z:, fondo de escritorio, políticas de contraseña
3.  **Servicios de red:** DNS con zona `milab.local`, DHCP con ámbito para clientes
4.  **Servidor de correo:** hMailServer con dominio `@milab.local` + IIS SMTP
5.  **Helpdesk N1:** osTicket instalado sobre MySQL, recepción de tickets por email
6.  **Troubleshooting:** Resolución de errores DNS, permisos NTFS y fallos de unión al dominio

## 📸 Evidencias

### Active Directory y Red
| Usuarios y Equipos de AD | Administrador de DNS |
| --- | --- |
| ![AD Users](images/01-ad-users.png) | ![DNS](images/02-dns.png) |

| Servidor DHCP | Directiva de Grupo |
| --- | --- |
| ![DHCP](images/03-dhcp.png) | ![GPO](images/04-gpos.png) |

### Servicios Desplegados
| IIS SMTP | hMailServer |
| --- | --- |
| ![IIS](images/05-iis.png) | ![hMail](images/06-hmail.png) |

| osTicket Helpdesk | PowerShell |
| --- | --- |
| ![osTicket](images/07-osticket.png) | ![PowerShell](images/08-powershell.png) |

## 🚀 Competencias para N1/N2

- Instalación y administración básica de Windows Server 2022
- Gestión de usuarios, equipos y permisos en Active Directory
- Configuración de DNS y DHCP para redes corporativas
- Aplicación de GPOs para estandarizar puestos de usuario
- Montaje de servicios: correo interno + sistema de tickets
- Documentación técnica y resolución de incidencias

## 📞 Contacto 666552626

Busco activamente una oportunidad como **Técnico de Sistemas N1/N2 / Helpdesk en Madrid**.

- **LinkedIn:** [Fructuoso Mba O. Fernandez](https://www.linkedin.com/in/fructuoso-mba-o-fernadez-3a64142a9/)
- **GitHub:** [MbaMrF](https://github.com/MbaMrF)
- **Email:** taskien666@gmail.com

**Abierto a entrevistas. Incorporación inmediata.**

---

**Tags:** `Windows Server` `Active Directory` `DNS` `DHCP` `GPO` `PowerShell` `Helpdesk` `N1` `N2` `Madrid`
