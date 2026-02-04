# 🖥️ Laboratorio Active Directory - Windows Server 2022

[![Windows Server](https://img.shields.io/badge/Windows%20Server-2022-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://www.microsoft.com/windows-server)
[![Windows 10](https://img.shields.io/badge/Windows%2010-Pro-0078D4?style=for-the-badge&logo=windows&logoColor=white)](https://www.microsoft.com/windows)
[![VirtualBox](https://img.shields.io/badge/VirtualBox-7.x-183A61?style=for-the-badge&logo=virtualbox&logoColor=white)](https://www.virtualbox.org/)
[![Ubuntu](https://img.shields.io/badge/Ubuntu-24.04%20LTS-E95420?style=for-the-badge&logo=ubuntu&logoColor=white)](https://ubuntu.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge)](LICENSE)

> **Implementación completa de un entorno de dominio Active Directory para prácticas de Administración de Sistemas Informáticos en Red (ASIR)**

---

## 📋 Tabla de Contenidos

- [Descripción](#-descripción)
- [Características](#-características)
- [Arquitectura](#-arquitectura)
- [Requisitos](#-requisitos)
- [Instalación](#-instalación)
- [Configuración](#-configuración)
- [Pruebas Realizadas](#-pruebas-realizadas)
- [Capturas de Pantalla](#-capturas-de-pantalla)
- [Problemas Resueltos](#-problemas-resueltos)
- [Aprendizajes](#-aprendizajes)
- [Próximos Pasos](#-próximos-pasos)
- [Recursos](#-recursos)
- [Autor](#-autor)
- [Licencia](#-licencia)

---

## 🎯 Descripción

Este proyecto documenta la implementación completa de un **laboratorio de Active Directory** desde cero, incluyendo:

- 🔧 Configuración de infraestructura virtualizada con VirtualBox
- 🌐 Diseño e implementación de red aislada
- 🏢 Instalación y configuración de Windows Server 2022 como Controlador de Dominio
- 👥 Gestión de usuarios, grupos y permisos
- 🔐 Control de acceso basado en roles (RBAC)
- 🐛 Troubleshooting completo de problemas reales

**Objetivo:** Demostrar habilidades prácticas en administración de sistemas Windows Server y Active Directory, con documentación técnica profesional de nivel enterprise.

---

## ✨ Características

### Infraestructura

- ✅ **3 máquinas virtuales** configuradas y operativas
- ✅ **Red aislada** (192.168.100.0/24) independiente de la red doméstica
- ✅ **Active Directory Domain Services** completamente funcional
- ✅ **DNS integrado** con resolución de nombres automática
- ✅ **Servidor de archivos** con permisos granulares

### Active Directory

- ✅ **Dominio:** `milab.local` (NetBIOS: MILAB)
- ✅ **5 usuarios** de dominio creados
- ✅ **Grupos de seguridad** con miembros asignados
- ✅ **Unidades Organizativas (OUs)** para estructura jerárquica
- ✅ **Carpetas compartidas** con control de acceso basado en grupos
- ✅ **Cliente Windows 10 Pro** unido al dominio

### Seguridad

- ✅ Control de acceso verificado (positivo y negativo)
- ✅ Autenticación centralizada
- ✅ Permisos de red SMB configurados
- ✅ Firewall configurado y documentado

---

## 🏗️ Arquitectura

### Topología de Red

```
┌─────────────────────────────────────────────────────────┐
│                    HOST (Windows 11)                     │
│                                                          │
│  ┌────────────────────────────────────────────────┐    │
│  │              VirtualBox 7.x                     │    │
│  │                                                 │    │
│  │  ┌──────────────────┐  ┌──────────────────┐   │    │
│  │  │  MBA-DOMINIO     │  │   Cliente01      │   │    │
│  │  │  Win Server 2022 │  │   Windows 10 Pro │   │    │
│  │  │  192.168.100.10  │  │  192.168.100.20  │   │    │
│  │  │                  │  │                  │   │    │
│  │  │  • AD DS         │  │  • Unido al      │   │    │
│  │  │  • DNS Server    │  │    dominio       │   │    │
│  │  │  • File Server   │  │  • Autenticación │   │    │
│  │  └────────┬─────────┘  └────────┬─────────┘   │    │
│  │           │                     │             │    │
│  │           └──────────┬──────────┘             │    │
│  │                      │                        │    │
│  │         ┌────────────▼─────────────┐          │    │
│  │         │   Red Interna            │          │    │
│  │         │   "red_empresa"          │          │    │
│  │         │   192.168.100.0/24       │          │    │
│  │         └────────────┬─────────────┘          │    │
│  │                      │                        │    │
│  │           ┌──────────▼─────────────┐          │    │
│  │           │   SRV-Files            │          │    │
│  │           │   Ubuntu 24.04         │          │    │
│  │           │   192.168.100.30       │          │    │
│  │           └────────────────────────┘          │    │
│  │                                                │    │
│  └────────────────────────────────────────────────┘    │
│                                                          │
└─────────────────────────────────────────────────────────┘
```

### Especificaciones de las VMs

| VM | Sistema Operativo | RAM | Disco | IP | Rol |
|----|-------------------|-----|-------|----|----|
| **MBA-DOMINIO** | Windows Server 2022 | 4 GB | 60 GB | 192.168.100.10 | Controlador de Dominio |
| **Cliente01** | Windows 10 Pro | 4 GB | 50 GB | 192.168.100.20 | Cliente del dominio |
| **SRV-Files** | Ubuntu 24.04 LTS | 2 GB | 40 GB | 192.168.100.30 | Servidor de archivos |

---

## 🔧 Requisitos

### Hardware Mínimo

- **Procesador:** Intel/AMD con soporte de virtualización (VT-x/AMD-V)
- **RAM:** 12 GB (8 GB para VMs + 4 GB para host)
- **Disco:** 150 GB de espacio libre
- **Sistema Operativo Host:** Windows 10/11, Linux o macOS

### Software Necesario

- [Oracle VirtualBox](https://www.virtualbox.org/) 7.0 o superior
- [Windows Server 2022](https://www.microsoft.com/es-es/evalcenter/download-windows-server-2022) ISO
- [Windows 10 Pro](https://www.microsoft.com/es-es/software-download/windows10) ISO
- [Ubuntu 24.04 LTS](https://ubuntu.com/download/desktop) ISO

### Conocimientos Previos

- Conceptos básicos de redes TCP/IP
- Administración básica de Windows
- Uso de VirtualBox
- (Opcional) PowerShell básico

---

## 📥 Instalación

### 1. Clonar el repositorio

```bash
git clone https://github.com/[tu-usuario]/asir-active-directory-lab.git
cd asir-active-directory-lab
```

### 2. Configurar VirtualBox

1. Instalar Oracle VirtualBox
2. Crear las 3 máquinas virtuales según especificaciones
3. Configurar adaptadores de red:
   - **Adaptador 1:** Red Interna (`red_empresa`)
   - **Adaptador 2:** NAT (para actualizaciones)

### 3. Instalar sistemas operativos

Seguir la guía completa en [`docs/LABORATORIO_ASIR_COMPLETO.md`](docs/LABORATORIO_ASIR_COMPLETO.md)

**Documentación paso a paso:**
- ✅ [Fase 1: Configuración de VirtualBox](docs/LABORATORIO_ASIR_COMPLETO.md#fase-1-configuración-del-entorno-virtual)
- ✅ [Fase 2: Configuración de Red](docs/LABORATORIO_ASIR_COMPLETO.md#fase-2-configuración-de-red)
- ✅ [Fase 3: Active Directory](docs/LABORATORIO_ASIR_COMPLETO.md#fase-3-active-directory-domain-services)
- ✅ [Fase 4: Unión al Dominio](docs/LABORATORIO_ASIR_COMPLETO.md#fase-4-unión-de-windows-10-al-dominio)

---

## ⚙️ Configuración

### Configuración de Red

```yaml
Red del Laboratorio: 192.168.100.0/24
Máscara de subred: 255.255.255.0
Gateway: No configurado (red aislada)

Servidor DNS: 192.168.100.10
Dominio: milab.local
NetBIOS: MILAB
```

### Usuarios Creados

| Usuario | Contraseña | Grupo | Descripción |
|---------|------------|-------|-------------|
| Administrador | - | Admins. del dominio | Cuenta de administración |
| jperez | Temporal123! | IT_Soporte | Usuario del departamento IT |
| mlopez | Temporal123! | IT_Soporte | Usuario del departamento IT |
| cgarcia | Temporal123! | Usuarios del dominio | Usuario estándar |
| lmartinez | Temporal123! | Usuarios del dominio | Usuario estándar |
| aruiz | Temporal123! | Usuarios del dominio | Usuario estándar |

⚠️ **Nota:** Estas contraseñas son solo para entorno de laboratorio. En producción usar políticas de contraseñas robustas.

### Recursos Compartidos

| Recurso | Ruta UNC | Permisos | Descripción |
|---------|----------|----------|-------------|
| IT | `\\192.168.100.10\IT` | IT_Soporte (Control total) | Carpeta del departamento IT |

---

## 🧪 Pruebas Realizadas

### Suite de Tests Completa

| # | Test | Estado | Evidencia |
|---|------|--------|-----------|
| 1 | Ping entre todas las VMs | ✅ PASS | 0% pérdida de paquetes |
| 2 | Resolución DNS de `milab.local` | ✅ PASS | Resuelve a 192.168.100.10 |
| 3 | Inicio de sesión con `jperez` | ✅ PASS | `whoami` = `milab\jperez` |
| 4 | Acceso a carpeta IT con `jperez` | ✅ PASS | Lectura y escritura |
| 5 | Denegación de acceso a `cgarcia` | ✅ PASS | Error de permiso |
| 6 | Verificación del DC con `dcdiag` | ✅ PASS | Todos los tests OK |

**Resultado:** 6/6 tests exitosos (100%)

### Comandos de Verificación

```powershell
# Verificar estado del Controlador de Dominio
dcdiag

# Comprobar replicación (si hubiera múltiples DCs)
repadmin /showrepl

# Listar usuarios del dominio
Get-ADUser -Filter * | Select-Object Name, SamAccountName

# Listar grupos
Get-ADGroup -Filter * | Select-Object Name

# Verificar servicios
Get-Service -Name "NTDS", "DNS", "Netlogon" | Select-Object Name, Status
```

---

## 📸 Capturas de Pantalla

### VMs Configuradas en VirtualBox

![VMs](screenshots/01-virtualbox/01-vms-configuradas.png)
*Las 3 máquinas virtuales corriendo: Ubuntu-cliente, WS-lab (Server 2022), Windows 10 cl*

### Conectividad de Red Verificada

![Ping](screenshots/02-red/04-ping-server.png)
*Ping desde Windows Server a todos los equipos: 0% de paquetes perdidos*

![DNS](screenshots/02-red/01-nslookup-milab.png)
*nslookup resolviendo milab.local correctamente desde el DNS del servidor*

### Instalación de Active Directory

![AD instalado](screenshots/03-active-directory/02-instalacion-completada.png)
*Instalación de AD DS completada, listo para promover a Controlador de Dominio*

### Estructura de Active Directory

![OUs](screenshots/03-active-directory/05-estructura-OUs.png)
*Estructura organizativa: milab.local con OUs Usuarios, Equipos y Grupos*

![Usuarios](screenshots/03-active-directory/06-usuarios-creados.png)
*Los 5 usuarios del dominio creados: Antonio Ruiz, Carlos Garcia, Juan Perez, Laura Martinez, Maria Lopez*

![Grupo IT](screenshots/03-active-directory/07-grupo-IT-miembros.png)
*Grupo IT soporte con sus 2 miembros: Juan Perez y Maria Lopez*

![Permisos carpeta](screenshots/03-active-directory/08-permisos-carpeta-IT.png)
*Carpeta IT compartida con permisos de Control Total para IT soporte*

### Proceso de Unión al Dominio

![Error Home](screenshots/04-union-dominio/01-error-win10-home.png)
*Error en Windows 10 Home: "No puede unir a un dominio esta edición"*

![Error DC](screenshots/04-union-dominio/03-error-no-contacta-DC.png)
*Error de Firewall: "No se puede poner en contacto con un controlador de dominio"*

![dcdiag](screenshots/04-union-dominio/04-dcdiag-todos-pasados.png)
*dcdiag: todas las pruebas superadas en el servidor (Connectivity, Advertising, NetLogons...)*

![Unión exitosa](screenshots/04-union-dominio/05-union-correcta-mensaje.png)
*Mensaje de éxito: "Se unió correctamente al dominio milab"*

### Autenticación y Permisos

![Login MILAB](screenshots/04-union-dominio/06-pantalla-login-MILAB.png)
*Pantalla de login mostrando "Iniciar sesión en MILAB" con opción "Otro usuario"*

![whoami](screenshots/04-union-dominio/07-whoami-jperez.png)
*Comando whoami confirmando autenticación: milab\jperez*

![Carpeta IT](screenshots/04-union-dominio/08-carpeta-IT-archivo-creado.png)
*jperez accede a \\192.168.100.10\it y crea archivo Prueba_jperez.txt*

![Acceso denegado](screenshots/04-union-dominio/09-acceso-denegado-cgarcia.png)
*cgarcia recibe "Error de red: No tiene permiso para obtener acceso"*

> **Nota:** Las 22 capturas completas están en la carpeta [`screenshots/`](screenshots/)

---

## 🐛 Problemas Resueltos

### 1. Windows 10 Home no soporta dominios

**Problema:** Botón "Dominio" deshabilitado en Windows 10 Home.

**Solución:** Instalar Windows 10 Pro.

**Lección aprendida:** Verificar ediciones de Windows antes de configurar entornos de dominio.

---

### 2. DNS no resolvía el dominio

**Problema:** `nslookup milab.local` devolvía "Non-existent domain".

**Causa:** Windows priorizaba DNS del adaptador NAT.

**Solución:** Desactivar adaptador NAT temporalmente.

---

### 3. Firewall bloqueando unión al dominio

**Problema:** Error "No se puede contactar con DC" pese a que ping y DNS funcionaban.

**Causa:** Firewall bloqueando puertos de Active Directory (88, 135, 389, 445).

**Solución:** 
- **Laboratorio:** Desactivar firewall temporalmente
- **Producción:** Crear reglas específicas para cada puerto

**Código de reglas para producción:**

```powershell
# Habilitar puertos de Active Directory
$ports = @(88, 135, 389, 445, 464, 636, 3268, 3269)
foreach ($port in $ports) {
    New-NetFirewallRule -DisplayName "AD-Port-$port-TCP" `
        -Direction Inbound -Protocol TCP -LocalPort $port -Action Allow
    New-NetFirewallRule -DisplayName "AD-Port-$port-UDP" `
        -Direction Inbound -Protocol UDP -LocalPort $port -Action Allow
}
```

---

### Documentación Completa de Problemas

Ver [`docs/LABORATORIO_ASIR_COMPLETO.md#resolución-de-problemas`](docs/LABORATORIO_ASIR_COMPLETO.md#resolución-de-problemas) para la lista completa de 9 problemas documentados.

---

## 📚 Aprendizajes

### Habilidades Técnicas Desarrolladas

#### Virtualización
- ✅ Configuración avanzada de VirtualBox
- ✅ Gestión de redes virtuales (NAT, Red Interna)
- ✅ Migración de VMs entre discos
- ✅ Troubleshooting de problemas de virtualización

#### Networking
- ✅ Diseño de topologías de red aisladas
- ✅ Configuración de IPs estáticas
- ✅ Resolución de problemas DNS
- ✅ Configuración de Firewall de Windows

#### Windows Server
- ✅ Instalación de roles y características
- ✅ Promoción a Controlador de Dominio
- ✅ Configuración de niveles funcionales
- ✅ Administración de servicios críticos

#### Active Directory
- ✅ Diseño de estructura organizativa (OUs)
- ✅ Gestión de usuarios y grupos
- ✅ Control de acceso basado en roles (RBAC)
- ✅ Configuración de permisos de red SMB
- ✅ Integración DNS-AD

#### Troubleshooting
- ✅ Diagnóstico sistemático de problemas
- ✅ Uso de herramientas de diagnóstico:
  - `dcdiag`
  - `nslookup`
  - `ipconfig`
  - `ping`
  - `whoami`
  - `net view`

### Soft Skills

- ✅ **Documentación técnica profesional**
- ✅ **Resolución de problemas complejos**
- ✅ **Persistencia ante obstáculos**
- ✅ **Aprendizaje autónomo**
- ✅ **Atención al detalle**

---

## 🚀 Próximos Pasos

### Mejoras Planificadas

- [ ] **Implementar DHCP** para asignación automática de IPs
- [ ] **Configurar GPOs** (Group Policy Objects)
  - Fondo de pantalla corporativo
  - Restricciones de software
  - Scripts de inicio de sesión
- [ ] **Crear segundo Controlador de Dominio** para redundancia
- [ ] **Unir Ubuntu al dominio** con Samba/SSSD
- [ ] **Implementar PKI** (Public Key Infrastructure)
- [ ] **Configurar Remote Desktop Services**
- [ ] **Añadir monitorización** con herramientas de logging
- [ ] **Scripts de automatización** con PowerShell

### Laboratorios Futuros

- 🔄 **Replicación de Active Directory** (multi-DC)
- 🌐 **Windows Server Update Services (WSUS)**
- 📊 **System Center Configuration Manager (SCCM)**
- 🔐 **Implementación de Certificate Authority**
- 📁 **DFS (Distributed File System)**
- 💾 **Backup y recuperación de Active Directory**

---

## 📖 Recursos

### Documentación del Proyecto

- 📄 [Documentación Completa (80+ páginas)](docs/LABORATORIO_ASIR_COMPLETO.md)
- 🔧 [Guía de Troubleshooting](docs/LABORATORIO_ASIR_COMPLETO.md#resolución-de-problemas)
- 📋 [Comandos Útiles](docs/LABORATORIO_ASIR_COMPLETO.md#anexo-a-comandos-útiles)

### Referencias Externas

- [Microsoft Learn: Active Directory](https://learn.microsoft.com/es-es/windows-server/identity/ad-ds/)
- [VirtualBox Documentation](https://www.virtualbox.org/manual/)
- [Windows Server 2022 Documentation](https://learn.microsoft.com/es-es/windows-server/)
- [Active Directory Best Practices](https://learn.microsoft.com/es-es/windows-server/identity/ad-ds/plan/security-best-practices/best-practices-for-securing-active-directory)

### Herramientas Utilizadas

| Herramienta | Versión | Propósito |
|-------------|---------|-----------|
| Oracle VirtualBox | 7.x | Plataforma de virtualización |
| Windows Server | 2022 Standard | Sistema operativo del servidor |
| Windows 10 | Pro (build 19045) | Sistema operativo del cliente |
| Ubuntu | 24.04 LTS | Sistema operativo del servidor de archivos |

---

## 👤 Autor

**[Tu Nombre]**

- 🎓 Técnico Superior en Administración de Sistemas Informáticos en Red (ASIR)
- 💼 LinkedIn: [tu-perfil-linkedin](https://linkedin.com/in/tu-perfil)
- 🐙 GitHub: [@tu-usuario](https://github.com/tu-usuario)
- 📧 Email: tu-email@ejemplo.com

---

## 📜 Licencia

Este proyecto está bajo la Licencia MIT - ver el archivo [LICENSE](LICENSE) para más detalles.

```
MIT License

Copyright (c) 2026 [Tu Nombre]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 🌟 Agradecimientos

- Microsoft Learn por la excelente documentación
- Comunidad de VirtualBox por el soporte
- Comunidad ASIR por compartir conocimientos

---

## 📊 Estadísticas del Proyecto

![GitHub repo size](https://img.shields.io/github/repo-size/tu-usuario/asir-active-directory-lab?style=flat-square)
![GitHub last commit](https://img.shields.io/github/last-commit/tu-usuario/asir-active-directory-lab?style=flat-square)
![GitHub stars](https://img.shields.io/github/stars/tu-usuario/asir-active-directory-lab?style=social)

---

<div align="center">

**⭐ Si este proyecto te fue útil, considera darle una estrella ⭐**

**Desarrollado con 💙 para la comunidad ASIR**

[⬆ Volver arriba](#-laboratorio-active-directory---windows-server-2022)

</div>
