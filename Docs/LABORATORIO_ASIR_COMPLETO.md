# 🖥️ LABORATORIO ASIR - DOCUMENTACIÓN COMPLETA

## 📋 Información del Proyecto

- **Autor:** [Tu Nombre]
- **Fecha de inicio:** Diciembre 2025
- **Última actualización:** 1 de febrero de 2026
- **Versión:** 3.0
- **Entorno:** VirtualBox en Windows 11
- **Objetivo:** Implementar infraestructura de laboratorio para prácticas de ASIR

---

## 📑 ÍNDICE

1. [Configuración del Entorno Virtual](#fase-1-configuración-del-entorno-virtual)
2. [Configuración de Red](#fase-2-configuración-de-red)
3. [Active Directory Domain Services](#fase-3-active-directory-domain-services)
4. [Unión de Windows 10 al Dominio](#fase-4-unión-de-windows-10-al-dominio)
5. [Resolución de Problemas](#resolución-de-problemas)
6. [Anexos](#anexos)

---

## 🎯 Resumen Ejecutivo

Este documento detalla la implementación completa de un laboratorio virtualizado para prácticas de Administración de Sistemas Informáticos en Red (ASIR). El proyecto abarca desde la configuración inicial de máquinas virtuales hasta la implementación de un Controlador de Dominio con Active Directory.

**Componentes principales:**
- 3 máquinas virtuales configuradas
- Red aislada para laboratorio (192.168.100.0/24)
- Dominio Active Directory: `milab.local`
- Servidor DNS integrado
- Estructura organizativa de usuarios y grupos
- Cliente Windows 10 Pro unido al dominio
- Sistema de permisos basado en grupos funcional

---

# FASE 1: Configuración del Entorno Virtual

## 1.1 Infraestructura del Laboratorio

### Especificaciones del Host
- **Sistema Operativo:** Windows 11
- **Hypervisor:** Oracle VirtualBox 7.x
- **Almacenamiento:**
  - Programa VirtualBox: `C:\Program Files\Oracle\VirtualBox`
  - Máquinas virtuales: `D:\VirtualBox VMs`

### Máquinas Virtuales Implementadas

| Nombre | Sistema Operativo | RAM | Disco Virtual | IP Estática | Rol |
|--------|-------------------|-----|---------------|-------------|-----|
| MBA-DOMINIO | Windows Server 2022 | 4 GB | 60 GB | 192.168.100.10 | Controlador de Dominio |
| Cliente01 | Windows 10 Pro | 4 GB | 50 GB | 192.168.100.20 | Cliente del dominio |
| SRV-Files | Ubuntu 24.04 LTS | 2 GB | 40 GB | 192.168.100.30 | Servidor de archivos |

---

## 1.2 Proceso de Instalación de VirtualBox

### Paso 1: Descarga e Instalación
1. Descargué VirtualBox desde el sitio oficial de Oracle
2. Instalé en la ruta predeterminada: `C:\Program Files\Oracle\VirtualBox`
3. Instalé las 3 máquinas virtuales con sus respectivos sistemas operativos

### Paso 2: Migración de VMs a Partición D:\

**Problema identificado:**
- Las VMs ocupaban mucho espacio en `C:\Users\[Usuario]\VirtualBox VMs`
- Partición C:\ quedaba sin espacio disponible

**Solución implementada:**

1. **Cerrar VirtualBox completamente**
   - Verifiqué que no hubiera procesos en segundo plano

2. **Crear estructura en destino**
   ```
   D:\VirtualBox VMs\
   ├── windows10\
   ├── Windows Server 2022\
   └── Ubuntu\
   ```

3. **Copiar las carpetas de las VMs**
   - Origen: `C:\Users\[Usuario]\VirtualBox VMs`
   - Destino: `D:\VirtualBox VMs`

4. **Cambiar ruta predeterminada en VirtualBox**
   - Abrí VirtualBox
   - Menú: **Archivo → Preferencias → General**
   - Cambié "Carpeta predeterminada de máquinas" a: `D:\VirtualBox VMs`

5. **Registrar las VMs en la nueva ubicación**
   - Para cada VM: **Máquina → Agregar**
   - Seleccioné el archivo `.vbox` correspondiente en `D:\VirtualBox VMs\[nombre-vm]\`

**Nota importante:** Archivos `.vbox` estaban ocultos. Tuve que:
- Abrir Explorador de archivos
- Menú Ver → Mostrar → Elementos ocultos ☑
- Entonces aparecieron como "VirtualBox Machine Definition (.vbox)"

**Resultado:** ✅ VMs operativas en D:\, liberados ~150 GB en C:\

---

## 1.3 Resolución de Errores Post-Migración

### Error 1: VERR_PATH_NOT_FOUND en archivo .vbox

**Mensaje de error completo:**
```
Runtime error opening 'C:\Users\[Usuario]\VirtualBox VMs\windows10\windows10.vbox' 
for reading VERR_PATH_NOT_FOUND (Path not found.)
Código de resultado: E_FAIL (0x80004005)
Componente: MachineWrap
Interfaz: IMachine {e36a5081-a82a-40bd-9e4e-42a44d6ce50f}
```

**Causa:** VirtualBox seguía buscando el archivo `.vbox` en la ruta antigua (C:\)

**Solución paso a paso:**

1. **Localizar archivo de configuración de VirtualBox**
   - Presioné `Win + R`
   - Escribí: `%USERPROFILE%\.VirtualBox`
   - Se abrió carpeta oculta de configuración

2. **Editar el registro de máquinas**
   - Hice copia de seguridad de `VirtualBox.xml`
   - Abrí `VirtualBox.xml` con Bloc de notas
   - Busqué la línea:
     ```xml
     <MachineEntry uuid="{...}" src="C:\\Users\\[Usuario]\\VirtualBox VMs\\windows10\\windows10.vbox"/>
     ```
   - **BORRÉ únicamente esa línea** (no modificar, solo eliminar)
   - Guardé el archivo

3. **Registrar la VM correctamente**
   - Abrí VirtualBox
   - Menú: **Máquina → Agregar**
   - Navegué a: `D:\VirtualBox VMs\windows10\`
   - Seleccioné: `windows10.vbox`

**Resultado:** ✅ Windows 10 VM registrada correctamente

---

### Error 2: VERR_PATH_NOT_FOUND en disco virtual (.vdi)

**Mensaje de error completo:**
```
Could not open the medium 'C:\Users\[Usuario]\VirtualBox VMs\Windows 10\Windows 10.vdi'
VD: error VERR_PATH_NOT_FOUND opening image file
Código de resultado: E_FAIL (0x80004005)
Componente: MediumWrap
Interfaz: IMedium {7d510820-a678-4730-a862-818dcd3fbed0}
```

**Causa:** El archivo `.vbox` apuntaba a la ruta antigua del disco virtual

**Solución paso a paso:**

1. **Abrir configuración de la VM**
   - Click derecho en Windows 10 → Configuración
   - Pestaña: **Almacenamiento**

2. **Reasignar disco virtual**
   - En "Controlador: SATA" hice click en el disco que mostraba error
   - Click en el icono del disco (junto al desplegable)
   - Seleccioné: **Elegir un archivo de disco duro virtual**
   - Navegué a: `D:\VirtualBox VMs\Windows 10\`
   - Seleccioné: `Windows 10.vdi`

3. **Guardar y verificar**
   - Click en Aceptar
   - Inicié la VM

**Resultado:** ✅ Windows 10 arrancó correctamente con el disco reasignado

---

### Repetir proceso para todas las VMs

Apliqué el mismo procedimiento para:
- ✅ Windows Server 2022
- ✅ Ubuntu

**Estado final:** Todas las VMs operativas en `D:\VirtualBox VMs`

---

## 1.4 Intento No Exitoso: macOS en VMware

**Objetivo inicial:** Instalar macOS Ventura en VMware para diversificar el laboratorio

**Pasos realizados:**
1. Descargué e instalé VMware Workstation Pro
2. Descargué imagen ISO de macOS Ventura
3. Descargué "Unlocker" para habilitar macOS en VMware

**Problema encontrado:**
- macOS requería mínimo 8 GB de RAM
- Mi hardware disponible: 8 GB totales
- Asignar 8 GB a macOS dejaría sin recursos al sistema host

**Decisión tomada:** Pausar el proyecto de macOS hasta mejorar hardware

**Consideración técnica - Windows Hypervisor Platform:**

Durante la instalación de VMware, investigué si activar o no esta característica:

| Aspecto | Activado | Desactivado |
|---------|----------|-------------|
| Compatibilidad | Mejor para VMs modernas de 64 bits | Limitado a sistemas antiguos |
| Rendimiento | Aprovecha virtualización de hardware | Menor rendimiento |
| Conflictos | Puede afectar VirtualBox antiguo | Compatible con VirtualBox legacy |
| WSL2 | Requerido | No disponible |

**Mi decisión:** NO activar por ahora para evitar conflictos con VirtualBox

**Nota:** Si en el futuro necesito desactivar Hypervisor Platform temporalmente:
```cmd
bcdedit /set hypervisorlaunchtype off
```
(Requiere reinicio)

**Aprendizaje:** Planificar recursos de hardware antes de diseñar laboratorios virtuales

---

# FASE 2: Configuración de Red

## 2.1 Diseño de la Topología de Red

### Arquitectura Implementada

```
HOST (Windows 11)
│
├── Internet
│
└── VirtualBox
    │
    ├── MBA-DOMINIO (Windows Server 2022)
    │   ├── Adaptador 1: Red Interna "red_empresa" - 192.168.100.10/24
    │   └── Adaptador 2: NAT (DHCP automático) - Internet
    │
    ├── Cliente01 (Windows 10)
    │   ├── Adaptador 1: Red Interna "red_empresa" - 192.168.100.20/24
    │   └── Adaptador 2: NAT (DHCP automático) - Internet
    │
    └── SRV-Files (Ubuntu)
        ├── Adaptador 1: Red Interna "red_empresa" - 192.168.100.30/24
        └── Adaptador 2: NAT (DHCP automático) - Internet
```

### Esquema de Direccionamiento

| Elemento | Valor |
|----------|-------|
| Red del laboratorio | 192.168.100.0/24 |
| Máscara de subred | 255.255.255.0 |
| Gateway | No configurado (red aislada) |
| DNS primario | 8.8.8.8 (Google DNS) |
| DNS secundario | 192.168.100.10 (tras instalar AD) |
| Nombre de red interna | red_empresa |

**Nota importante:** Esta red es completamente independiente de la red doméstica del router.

---

## 2.2 Configuración de Adaptadores en VirtualBox

### Concepto: Doble Adaptador de Red

Cada VM tiene **dos adaptadores de red** con funciones diferentes:

| Adaptador | Tipo | Función | Configuración IP |
|-----------|------|---------|------------------|
| Adaptador 1 | Red Interna | Comunicación entre VMs del laboratorio | IP estática manual |
| Adaptador 2 | NAT | Acceso a Internet para actualizaciones | DHCP automático |

### Procedimiento de Configuración

**Para cada VM (repetir 3 veces):**

1. **Apagar la máquina virtual**
   - Click derecho → Apagar (no guardar estado)

2. **Configurar Adaptador 1**
   - Click derecho en VM → Configuración → Red
   - Pestaña: **Adaptador 1**
   - Habilitar adaptador de red: ☑
   - Conectado a: **Red interna**
   - Nombre: `red_empresa`

3. **Configurar Adaptador 2**
   - Pestaña: **Adaptador 2**
   - Habilitar adaptador de red: ☑
   - Conectado a: **NAT**

4. **Guardar configuración**
   - Click en Aceptar

**Resultado:** Cada VM tiene acceso a Internet (NAT) y puede comunicarse con otras VMs (red_empresa)

---

## 2.3 Configuración de IP Estática en Windows Server

### Identificar el adaptador correcto

1. **Abrir configuración de red**
   - Panel de control → Redes e Internet
   - Centro de redes y recursos compartidos
   - Cambiar configuración del adaptador

2. **Identificar adaptadores**
   - Aparecen dos conexiones:
     - **Ethernet** → Suele ser NAT
     - **Ethernet 2** → Suele ser Red Interna
   
3. **Verificar cuál es la Red Interna**
   - Click derecho en cada adaptador → Estado → Detalles
   - El que muestra IP `169.254.x.x` (APIPA) es la red interna

**Nota:** La IP 169.254.x.x indica que VirtualBox asignó una IP automática temporal porque no hay servidor DHCP en la red interna.

### Configurar IP estática

1. **Abrir propiedades del adaptador correcto**
   - Click derecho en "Ethernet 2" (Red Interna)
   - Propiedades

2. **Configurar IPv4**
   - Doble click en "Protocolo de Internet versión 4 (TCP/IPv4)"
   - Seleccionar: **Usar la siguiente dirección IP**

3. **Introducir configuración**
   ```
   Dirección IP:      192.168.100.10
   Máscara de subred: 255.255.255.0
   Puerta de enlace:  (dejar en blanco)
   
   DNS preferido:     8.8.8.8
   DNS alternativo:   (vacío)
   ```

4. **Aplicar cambios**
   - Aceptar → Cerrar

### Verificación

```cmd
ipconfig
```

**Salida esperada:**
```
Adaptador de Ethernet (Red Interna):
   Dirección IPv4. . . . . . . . . . . . : 192.168.100.10
   Máscara de subred . . . . . . . . . . : 255.255.255.0
```

**Prueba de conectividad:**
```cmd
ping 192.168.100.10
ping 127.0.0.1
```

Ambos deben responder correctamente.

---

## 2.4 Configuración de IP Estática en Windows 10

### Proceso similar al servidor

1. **Identificar adaptador de Red Interna**
   - Panel de control → Red → Cambiar configuración del adaptador
   - Buscar el adaptador con IP 169.254.x.x

2. **Configurar IPv4**
   ```
   Dirección IP:      192.168.100.20
   Máscara de subred: 255.255.255.0
   Puerta de enlace:  (vacío por ahora)
   
   DNS preferido:     192.168.100.10  ← Apunta al Windows Server
   DNS alternativo:   8.8.8.8
   ```

### Diferencia clave: DNS apunta al servidor

**Razón:** Windows 10 usará el servidor (192.168.100.10) como DNS. Esto será fundamental cuando instalemos Active Directory.

### Verificación de DNS

```cmd
nslookup google.com
```

**Salida esperada:**
```
Servidor:  192.168.100.10
Dirección:  192.168.100.10
```

Esto confirma que Windows 10 está consultando DNS al servidor.

---

## 2.5 Configuración de IP Estática en Ubuntu

### Método gráfico

1. **Abrir configuración de red**
   - Aplicaciones → Configuración → Red

2. **Identificar adaptadores**
   - Aparecen dos conexiones cableadas:
     - `enp0s3` → Suele ser NAT
     - `enp0s8` → Suele ser Red Interna

3. **Configurar enp0s8 (Red Interna)**
   - Click en el icono de engranaje junto a enp0s8
   - Pestaña: **IPv4**
   - Método: **Manual**

4. **Introducir configuración**
   ```
   Dirección:  192.168.100.30
   Máscara:    255.255.255.0
   Gateway:    (vacío)
   
   DNS:        8.8.8.8
   ```

5. **Aplicar**
   - Click en "Aplicar"
   - Desconectar y reconectar la red

### Verificación

```bash
ip addr show enp0s8
```

**Salida esperada:**
```
inet 192.168.100.30/24
```

**Prueba de conectividad:**
```bash
ping -c 4 192.168.100.10  # Al servidor
ping -c 4 192.168.100.20  # Al cliente Windows
```

---

## 2.6 Resolución de Problemas de Conectividad

### Problema: Los pings entre VMs no funcionan

**Síntomas:**
- Las IPs están configuradas correctamente
- `ping localhost` funciona
- `ping` a otras VMs: "Tiempo de espera agotado" o "Destino inaccesible"

**Causa:** Firewall de Windows bloqueando ICMP (protocolo de ping)

### Solución: Habilitar ICMP en Firewall de Windows

**En Windows Server y Windows 10:**

1. **Abrir Firewall con seguridad avanzada**
   - Panel de control → Sistema y seguridad
   - Firewall de Windows Defender
   - Configuración avanzada (lateral izquierdo)

2. **Habilitar reglas de ICMP**
   - Click en "Reglas de entrada"
   - Buscar: "Solicitud de eco ICMPv4"
   - Encontrarás varias reglas con este nombre
   - Para cada una:
     - Click derecho → **Habilitar regla**

3. **Cerrar el Firewall**

4. **Verificar conectividad**
   
   **Desde Windows Server:**
   ```cmd
   ping 192.168.100.20  # Debe responder ahora
   ping 192.168.100.30
   ```

   **Desde Windows 10:**
   ```cmd
   ping 192.168.100.10  # Debe responder ahora
   ping 192.168.100.30
   ```

   **Desde Ubuntu:**
   ```bash
   ping -c 4 192.168.100.10  # Debe responder ahora
   ping -c 4 192.168.100.20
   ```

**Resultado:** ✅ Todas las VMs se comunican correctamente entre sí

---

## 2.7 Configuración Final de Red

### Tabla de verificación

| Origen | Destino | Comando | Resultado |
|--------|---------|---------|-----------|
| Server (100.10) | Cliente (100.20) | `ping 192.168.100.20` | ✅ Responde |
| Server (100.10) | Ubuntu (100.30) | `ping 192.168.100.30` | ✅ Responde |
| Cliente (100.20) | Server (100.10) | `ping 192.168.100.10` | ✅ Responde |
| Cliente (100.20) | Ubuntu (100.30) | `ping 192.168.100.30` | ✅ Responde |
| Ubuntu (100.30) | Server (100.10) | `ping 192.168.100.10` | ✅ Responde |
| Ubuntu (100.30) | Cliente (100.20) | `ping 192.168.100.20` | ✅ Responde |
| Cualquier VM | Internet | `ping 8.8.8.8` | ✅ Responde (vía NAT) |

### Resumen de la configuración de red

✅ **Red aislada "red_empresa" configurada**
- Segmento: 192.168.100.0/24
- Aislada de la red doméstica
- Sin puerta de enlace (no se enrutan paquetes fuera)

✅ **Acceso a Internet vía NAT**
- Cada VM puede descargar actualizaciones
- Navegación web disponible
- No interfiere con la red interna

✅ **Comunicación entre VMs funcional**
- Ping exitoso entre todas las máquinas
- ICMP habilitado en firewalls
- Preparado para Active Directory

---

# FASE 3: Active Directory Domain Services

## 3.1 Información del Despliegue

### Datos del dominio implementado

| Parámetro | Valor |
|-----------|-------|
| Nombre del dominio | milab.local |
| Nombre NetBIOS | MILAB |
| Nivel funcional del bosque | Windows Server 2016 |
| Nivel funcional del dominio | Windows Server 2016 |
| Nombre del DC | MBA-DOMINIO |
| IP del DC | 192.168.100.10 |
| Roles instalados | AD DS, DNS Server |
| Fecha de implementación | 25 de enero de 2026 |

---

## 3.2 Instalación del Rol AD DS

### Procedimiento paso a paso

1. **Abrir el Administrador del servidor**
   - Inicio → Server Manager (se abre automáticamente al iniciar sesión)

2. **Iniciar asistente de roles**
   - Menú: **Administrar** → **Agregar roles y características**
   - Antes de comenzar: Click en "Siguiente"

3. **Tipo de instalación**
   - Seleccionar: **Instalación basada en características o en roles**
   - Siguiente

4. **Selección del servidor**
   - Seleccionar: **MBA-DOMINIO** (servidor local)
   - Siguiente

5. **Selección de roles**
   - Marcar: ☑ **Servicios de dominio de Active Directory**
   - Aparece popup "Agregar características requeridas"
     - Incluye herramientas de administración
     - Click en "Agregar características"
   - Siguiente

6. **Características adicionales**
   - Dejar selección por defecto
   - Siguiente

7. **Servicios de dominio de AD**
   - Leer información sobre AD DS
   - Siguiente

8. **Confirmación**
   - Revisar selecciones
   - Marcar: ☑ "Reiniciar automáticamente el servidor de destino en caso necesario"
   - Click en **Instalar**

9. **Proceso de instalación**
   - Duración: 5-10 minutos
   - Progreso visible en la ventana
   - NO cerrar la ventana

10. **Finalización**
    - Mensaje: "Instalación correcta en MBA-DOMINIO"
    - Click en "Cerrar"

**Resultado:** ✅ Rol AD DS instalado, pero NO configurado aún

**Nota importante:** El rol está instalado pero el servidor AÚN NO es un Controlador de Dominio. Falta la promoción.

---

## 3.3 Promoción a Controlador de Dominio

### Iniciar el asistente de configuración

1. **Localizar la notificación**
   - En Server Manager, buscar bandera amarilla con signo de exclamación (esquina superior derecha)
   - Click en la bandera
   - Mensaje: "Se requiere configuración posterior a la implementación"
   - Click en: **"Promover este servidor a controlador de dominio"**

### Configuración del despliegue

2. **Configuración de implementación**
   - Seleccionar: **Agregar un nuevo bosque**
   - Nombre del dominio raíz: `milab.local`
   - Siguiente

**Nota sobre el nombre de dominio:**
- `.local` es común en laboratorios y redes privadas
- En producción se usarían dominios reales (ejemplo: `empresa.com`)

3. **Opciones del controlador de dominio**
   
   **Nivel funcional:**
   - Nivel funcional del bosque: **Windows Server 2016**
   - Nivel funcional del dominio: **Windows Server 2016**
   
   **Nota:** Elegí 2016 en lugar de 2022 para mayor compatibilidad con otras VMs futuras.
   
   **Capacidades del controlador de dominio:**
   - ☑ Servidor de sistema de nombres de dominio (DNS)
   - ☑ Catálogo global (GC)
   - ☐ Controlador de dominio de solo lectura (RODC) - NO marcar
   
   **Contraseña DSRM:**
   - Introduce contraseña segura
   - Confirmar contraseña
   - **ANOTAR EN LUGAR SEGURO** (se usa para recuperación de AD)
   
   - Siguiente

**¿Qué es DSRM?**
Directory Services Restore Mode - modo de recuperación si AD falla. Nunca se usa en operación normal.

4. **Opciones de DNS**
   
   - Aparece advertencia: "No se puede crear una delegación para este servidor DNS..."
   - **Esto es NORMAL** en un nuevo bosque
   - Marcar: ☑ No mostrar este mensaje nuevamente
   - Siguiente

5. **Opciones adicionales**
   
   - Nombre NetBIOS del dominio: **MILAB** (generado automáticamente)
   - Este nombre se usa para compatibilidad con sistemas antiguos
   - Siguiente

6. **Rutas de acceso**
   
   - Carpeta de base de datos: `C:\Windows\NTDS`
   - Carpeta de archivos de registro: `C:\Windows\NTDS`
   - Carpeta SYSVOL: `C:\Windows\SYSVOL`
   
   - Dejar valores predeterminados
   - Siguiente

7. **Revisar opciones**
   
   - Revisar toda la configuración implementada
   - Verificar que todo esté correcto
   - Siguiente

8. **Comprobación de requisitos previos**
   
   - El asistente verifica que el sistema puede ser DC
   - Pueden aparecer advertencias en amarillo:
     - "Configuración de criptografía..."
     - "Delegación DNS..."
   - **Estas advertencias son NORMALES** en entornos de laboratorio
   - Si NO aparecen errores rojos, se puede continuar
   
   - Click en **Instalar**

9. **Proceso de instalación**
   
   - Duración: 15-20 minutos
   - Progreso visible
   - NO interrumpir el proceso
   - El servidor se **reiniciará automáticamente** al finalizar

10. **Reinicio automático**
    
    - El servidor se reinicia solo
    - Esperar a que arranque completamente

**Problema encontrado:** En mi caso, la VM intentó arrancar por PXE (red) después del reinicio. Tuve que hacer un reset de la VM para que arrancara normalmente desde el disco.

---

## 3.4 Verificación Post-Instalación

### Cambios visibles tras el reinicio

1. **Pantalla de inicio de sesión**
   - ANTES: `Administrador`
   - AHORA: `MILAB\Administrador` ✅
   
   Esto confirma que el dominio existe.

2. **Server Manager**
   - Roles activos visibles:
     - ☑ AD DS (Servicios de dominio de Active Directory)
     - ☑ DNS (Servidor DNS)
   
3. **Herramientas administrativas nuevas**
   - Menú Herramientas → ahora incluye:
     - Usuarios y equipos de Active Directory
     - Dominios y confianzas de Active Directory
     - Sitios y servicios de Active Directory
     - DNS

### Verificación desde línea de comandos

```cmd
dcdiag
```

Este comando verifica el estado del Controlador de Dominio.

**Resultado esperado:** Tests pasados sin errores críticos.

```cmd
nslookup milab.local
```

**Resultado esperado:**
```
Servidor:  MBA-DOMINIO.milab.local
Dirección:  192.168.100.10

Nombre:  milab.local
Dirección:  192.168.100.10
```

Esto confirma que el DNS está resolviendo el nombre del dominio.

**Resultado:** ✅ Active Directory y DNS funcionando correctamente

---

## 3.5 Creación de Unidades Organizativas (OUs)

### ¿Qué son las OUs?

Las Unidades Organizativas son contenedores lógicos dentro de AD que permiten:
- Organizar usuarios, equipos y grupos
- Aplicar políticas de grupo (GPO) de forma segmentada
- Delegar administración
- Reflejar la estructura de la organización

### Acceder a la consola de administración

1. **Abrir Usuarios y equipos de Active Directory**
   - Server Manager → Herramientas → Usuarios y equipos de Active Directory
   - O bien: Inicio → Herramientas administrativas → AD Users and Computers

### Crear Unidad Organizativa: Usuarios

1. **Localizar el dominio**
   - En el árbol lateral, expandir: `milab.local`

2. **Crear nueva OU**
   - Click derecho en `milab.local`
   - Nuevo → **Unidad organizativa**

3. **Configurar OU**
   - Nombre: `Usuarios`
   - ☑ Proteger contenedor contra eliminación accidental
   - Click en Aceptar

### Crear Unidad Organizativa: Equipos

Repetir el proceso anterior:
- Nombre: `Equipos`
- ☑ Proteger contenedor contra eliminación accidental

### Crear Unidad Organizativa: Grupos

Repetir el proceso anterior:
- Nombre: `Grupos`
- ☑ Proteger contenedor contra eliminación accidental

### Estructura resultante

```
milab.local
├── Builtin (predeterminado)
├── Computers (predeterminado)
├── Domain Controllers (predeterminado)
├── Equipos ← OU creada
├── ForeignSecurityPrincipals (predeterminado)
├── Grupos ← OU creada
├── Managed Service Accounts (predeterminado)
├── Users (predeterminado)
└── Usuarios ← OU creada
```

**Notas:**
- Las OUs con nombres en inglés son contenedores predeterminados de Windows
- Las OUs nuevas están en español según nuestra convención
- "Computers" se usa por defecto cuando un equipo se une al dominio sin especificar OU
- "Users" contiene usuarios predeterminados del sistema

**Resultado:** ✅ Estructura organizativa básica creada

---

## 3.6 Creación de Usuarios de Dominio

### Ubicación de los usuarios

Los usuarios se crean dentro de la OU "Usuarios" para:
- Mantener organización
- Facilitar aplicación de GPOs específicas
- Separar usuarios de dominio de usuarios del sistema

### Procedimiento para crear un usuario

**Usuario de ejemplo: Juan Pérez**

1. **Navegar a la OU correcta**
   - En "Usuarios y equipos de Active Directory"
   - Expandir `milab.local`
   - Click en OU `Usuarios`

2. **Iniciar creación**
   - Click derecho en espacio vacío dentro de "Usuarios"
   - Nuevo → **Usuario**

3. **Información del usuario**
   - Nombre: `Juan`
   - Apellidos: `Pérez`
   - Nombre completo: `Juan Pérez` (se genera automáticamente)
   - Nombre de inicio de sesión de usuario: `jperez`
   - Dominio: `@milab.local` (predeterminado)
   - Siguiente

4. **Configuración de contraseña**
   - Contraseña: `Temporal123!`
   - Confirmar contraseña: `Temporal123!`
   - ☐ El usuario debe cambiar la contraseña en el siguiente inicio de sesión - **DESMARCAR**
   - ☑ La contraseña nunca expira - **MARCAR**
   - ☐ El usuario no puede cambiar la contraseña
   - ☐ La cuenta está deshabilitada
   
   **IMPORTANTE:** "La contraseña nunca expira" solo para entorno de laboratorio. En producción NUNCA usar esta opción.
   
   - Siguiente

5. **Finalizar**
   - Revisar información
   - Click en **Finalizar**

### Usuarios creados en el laboratorio

| Nombre completo | Nombre de inicio de sesión | Contraseña | Departamento (simulado) |
|-----------------|----------------------------|------------|------------------------|
| Juan Pérez | jperez | Temporal123! | IT / Soporte |
| María López | mlopez | Temporal123! | IT / Soporte |
| Carlos García | cgarcia | Temporal123! | Ventas |
| Laura Martínez | lmartinez | Temporal123! | Administración |
| Antonio Ruiz | aruiz | Temporal123! | Recursos Humanos |

**Todos creados con:**
- ☑ La contraseña nunca expira
- ☐ El usuario debe cambiar la contraseña

### Verificación

1. **Comprobar que aparecen en la OU**
   - En "Usuarios y equipos de Active Directory"
   - OU "Usuarios" debe mostrar los 5 usuarios

2. **Verificar propiedades de un usuario**
   - Doble click en cualquier usuario
   - Pestaña "Cuenta": verificar nombre de inicio de sesión
   - Pestaña "General": verificar nombre completo

**Resultado:** ✅ 5 usuarios de dominio operativos

---

## 3.7 Creación de Grupo de Seguridad

### ¿Qué es un grupo de seguridad?

Los grupos de seguridad en AD permiten:
- Asignar permisos a múltiples usuarios simultáneamente
- Simplificar administración de accesos
- Organizar usuarios por función/departamento
- Aplicar políticas de grupo

### Tipos de grupos

**Por ámbito:**
- **Local de dominio:** Solo en el dominio actual
- **Global:** Puede incluir usuarios del dominio y usarse en cualquier dominio del bosque
- **Universal:** Puede incluir usuarios de cualquier dominio y usarse en todo el bosque

**Por tipo:**
- **Seguridad:** Se usa para permisos de acceso
- **Distribución:** Solo para listas de correo

### Crear grupo: IT_Soporte

1. **Navegar a la OU Grupos**
   - En "Usuarios y equipos de Active Directory"
   - Click en OU `Grupos`

2. **Iniciar creación**
   - Click derecho en espacio vacío
   - Nuevo → **Grupo**

3. **Configuración del grupo**
   - Nombre de grupo: `IT_Soporte`
   - Nombre de grupo (anterior a Windows 2000): `IT_Soporte` (igual)
   - Ámbito del grupo:
     - ◉ Global
     - ○ Local de dominio
     - ○ Universal
   - Tipo de grupo:
     - ◉ Seguridad
     - ○ Distribución
   
   - Click en Aceptar

4. **Añadir miembros al grupo**
   - Doble click en el grupo recién creado `IT_Soporte`
   - Pestaña: **Miembros**
   - Click en **Agregar**

5. **Seleccionar usuarios**
   - En "Escriba los nombres de objeto":
     ```
     jperez; mlopez
     ```
   - Click en **Comprobar nombres** (los nombres se subrayarán)
   - Click en Aceptar

6. **Verificar miembros**
   - La pestaña "Miembros" debe mostrar:
     - Juan Pérez (MILAB\jperez)
     - María López (MILAB\mlopez)
   
   - Click en Aplicar → Aceptar

### Verificación desde usuario

Podemos verificar que un usuario pertenece al grupo:

1. **Abrir propiedades del usuario**
   - Doble click en "Juan Pérez" (en OU Usuarios)

2. **Verificar pertenencia**
   - Pestaña: **Miembro de**
   - Debe aparecer: `IT_Soporte`

**Resultado:** ✅ Grupo de seguridad creado con 2 miembros

---

## 3.8 Configuración de Carpeta Compartida

### Objetivo

Crear una carpeta compartida en el servidor que solo sea accesible para los miembros del grupo IT_Soporte.

### Crear estructura de carpetas

1. **Crear carpeta raíz**
   - Abrir Explorador de archivos
   - Navegar a `C:\`
   - Click derecho → Nuevo → Carpeta
   - Nombre: `Recursos_Compartidos`

2. **Crear subcarpetas**
   - Dentro de `Recursos_Compartidos`, crear:
     - Carpeta: `IT`
     - Carpeta: `General`

**Estructura:**
```
C:\
└── Recursos_Compartidos\
    ├── IT\
    └── General\
```

### Compartir carpeta IT

1. **Abrir propiedades**
   - Click derecho en carpeta `IT`
   - Propiedades

2. **Pestaña Compartir**
   - Click en botón **Uso compartido avanzado**

3. **Habilitar recurso compartido**
   - ☑ **Compartir esta carpeta**
   - Nombre del recurso compartido: `IT` (sin cambios)
   - Comentario: `Carpeta exclusiva del departamento IT` (opcional)

4. **Configurar permisos de red**
   - Click en botón **Permisos**
   
   **Remover grupo "Todos":**
   - Seleccionar: `Todos`
   - Click en **Quitar**
   
   **Agregar grupo IT_Soporte:**
   - Click en **Agregar**
   - Escribir: `IT_Soporte`
   - Click en Comprobar nombres
   - Aceptar
   
   **Asignar permisos completos:**
   - Seleccionar: `IT_Soporte`
   - En "Permisos para IT_Soporte":
     - ☑ Control total
     - ☑ Cambiar
     - ☑ Lectura
   
   - Click en Aplicar → Aceptar

5. **Finalizar configuración**
   - Click en Aceptar en "Uso compartido avanzado"
   - Click en Cerrar en "Propiedades de IT"

### Verificar acceso al recurso compartido

**Ruta de acceso:**
```
\\MBA-DOMINIO\IT
\\milab.local\IT
\\192.168.100.10\IT
```

Todas estas rutas son equivalentes.

### Probar acceso (después de unir cliente al dominio)

**Desde Windows 10 (después de unirse al dominio):**

1. **Con usuario jperez (miembro de IT_Soporte):**
   - Abrir Explorador de archivos
   - En la barra de direcciones: `\\192.168.100.10\IT`
   - ✅ Debe permitir acceso

2. **Con usuario cgarcia (NO miembro de IT_Soporte):**
   - Misma ruta
   - ❌ Debe denegar acceso

**Resultado:** ✅ Recurso compartido configurado con permisos correctos

---

## 3.9 Resumen de Active Directory Implementado

### Componentes configurados

| Componente | Estado | Detalles |
|------------|--------|----------|
| Rol AD DS | ✅ Instalado | Windows Server 2022 |
| Controlador de Dominio | ✅ Activo | MBA-DOMINIO.milab.local |
| DNS Server | ✅ Operativo | Integrado con AD |
| Dominio | ✅ Creado | milab.local (NetBIOS: MILAB) |
| Nivel funcional | Windows Server 2016 | Bosque y dominio |
| Unidades Organizativas | ✅ 3 creadas | Usuarios, Equipos, Grupos |
| Usuarios de dominio | ✅ 5 creados | jperez, mlopez, cgarcia, lmartinez, aruiz |
| Grupos de seguridad | ✅ 1 creado | IT_Soporte (2 miembros) |
| Carpetas compartidas | ✅ 1 configurada | \\MBA-DOMINIO\IT |

### Conocimientos aplicados

- ✅ Instalación de roles en Windows Server
- ✅ Promoción de servidor a Controlador de Dominio
- ✅ Configuración de niveles funcionales de AD
- ✅ Administración de DNS integrado con Active Directory
- ✅ Creación de estructura organizativa (OUs)
- ✅ Gestión de usuarios y contraseñas de dominio
- ✅ Creación y administración de grupos de seguridad
- ✅ Configuración de recursos compartidos con permisos basados en grupos
- ✅ Verificación de servicios de dominio

---

# FASE 4: Unión de Windows 10 al Dominio

## 4.1 Información General

### Objetivo de esta fase
Unir un equipo cliente Windows 10 al dominio `milab.local` y verificar el funcionamiento completo del sistema de autenticación y permisos de Active Directory.

### Fecha de implementación
- **Inicio:** 29 de enero de 2026
- **Completado:** 1 de febrero de 2026
- **Duración total:** 3 días (incluyendo troubleshooting)

---

## 4.2 Problema Inicial: Windows 10 Home

### Limitación detectada

Durante el primer intento de unión al dominio con la VM original de Windows 10, se encontró el siguiente problema:

**Error mostrado:**
```
"No puede unir un dominio un equipo en el que se ejecute esta edición de Windows 10."
```

**Causa raíz:**
- La VM original tenía **Windows 10 Home Edition**
- Windows 10 Home **NO soporta** unión a dominios de Active Directory
- Esta funcionalidad solo está disponible en:
  - Windows 10 Pro
  - Windows 10 Enterprise
  - Windows 10 Education

### Análisis del problema

| Característica | Windows 10 Home | Windows 10 Pro |
|----------------|-----------------|----------------|
| Unión a dominio | ❌ NO | ✅ SÍ |
| Group Policy (GPO) | ❌ NO | ✅ SÍ |
| BitLocker | ❌ NO | ✅ SÍ |
| Remote Desktop (host) | ❌ NO | ✅ SÍ |
| Hyper-V | ❌ NO | ✅ SÍ |

**Lección aprendida:** Antes de configurar un entorno de dominio, verificar que los clientes tengan ediciones compatibles.

---

## 4.3 Solución: Instalación de Windows 10 Pro

### Decisión tomada

Instalar una nueva máquina virtual con Windows 10 Pro para completar la práctica correctamente.

### Proceso de instalación

**1. Descarga de Windows 10 Pro**
- Fuente: Microsoft oficial
- Versión: Windows 10 Pro (64-bit)
- Tamaño: ~5 GB
- Método: Media Creation Tool

**2. Creación de nueva VM**

| Parámetro | Valor |
|-----------|-------|
| Nombre | Windows 10 cl (Cliente) |
| RAM | 4 GB |
| Disco virtual | 50 GB (dinámico) |
| Procesadores | 2 CPUs |
| Gráficos | 128 MB VRAM |

**3. Configuración de adaptadores de red**

Igual que en la configuración original:
- **Adaptador 1:** Red Interna (`red_empresa`)
- **Adaptador 2:** NAT (para actualizaciones)

**4. Instalación de Windows 10 Pro**
- Tiempo de instalación: ~25 minutos
- Configuración: Cuenta local (sin cuenta Microsoft)
- Nombre del equipo: `DESKTOP-MQEAF54` (generado automáticamente)

---

## 4.4 Configuración de Red en Windows 10 Pro

### Configuración de IP estática

**Adaptador de Red Interna (red_empresa):**

```
Dirección IP:      192.168.100.20
Máscara de subred: 255.255.255.0
Puerta de enlace:  (vacío)
DNS preferido:     192.168.100.10  ← Windows Server
DNS alternativo:   (vacío)
```

**Procedimiento:**
1. Panel de control → Redes e Internet
2. Centro de redes y recursos compartidos
3. Cambiar configuración del adaptador
4. Click derecho en adaptador de Red Interna
5. Propiedades → Protocolo IPv4 → Usar dirección IP manual
6. Introducir configuración

---

## 4.5 Verificación Previa a la Unión

### Test de conectividad

**Desde Windows 10 Pro, se verificó:**

```cmd
ping 192.168.100.10
```

**Resultado:**
```
Respuesta desde 192.168.100.10: bytes=32 tiempo<1ms TTL=128
Respuesta desde 192.168.100.10: bytes=32 tiempo<1ms TTL=128
Respuesta desde 192.168.100.10: bytes=32 tiempo<1ms TTL=128
Respuesta desde 192.168.100.10: bytes=32 tiempo<1ms TTL=128

Paquetes: enviados = 4, recibidos = 4, perdidos = 0 (0% perdidos)
```

✅ **Conectividad de red confirmada**

---

### Test de resolución DNS

**Primer intento (FALLÓ):**

```cmd
nslookup milab.local
```

**Resultado inicial:**
```
Servidor: UnKnown
Address: 100.100.1.1

*** UnKnown no encuentra milab.local: Non-existent domain
```

❌ **DNS no funcionaba**

**Causa identificada:**
El sistema estaba usando el DNS del adaptador NAT (100.100.1.1) en lugar del DNS del servidor (192.168.100.10).

**Solución aplicada:**
Desactivar temporalmente el adaptador NAT para forzar el uso del DNS correcto.

**Segundo intento (ÉXITO):**

```cmd
nslookup milab.local
```

**Resultado:**
```
Servidor: UnKnown
Address: 192.168.100.10

Nombre: milab.local
Addresses: fd17:625c:f037:3:6011:1bc9:fef8:5d2
          192.168.100.10
          10.0.3.15
```

✅ **DNS resolviendo correctamente**

---

## 4.6 Problema: Error al Unir al Dominio

### Primer intento de unión

**Procedimiento seguido:**
1. `Win + R` → `sysdm.cpl`
2. Pestaña "Nombre de equipo" → Cambiar
3. Miembro de: ◉ Dominio → `milab`
4. Aceptar

**Error recibido:**
```
No se puede poner en contacto con un controlador de dominio de 
Active Directory (AD DC) en el dominio 'milab'.

Asegúrese de que el nombre de dominio esté escrito correctamente.
```

❌ **Unión fallida**

---

### Diagnóstico del problema

**Verificaciones realizadas:**

1. **Servidor encendido:** ✅ Confirmado
2. **Ping al servidor:** ✅ Funciona
3. **DNS resuelve:** ✅ Funciona
4. **Servicios AD en el servidor:**

```cmd
dcdiag
```

**Resultado:** ✅ Todos los tests pasados

**Conclusión:** El problema NO era de red ni de DNS, sino del **Firewall del servidor**.

---

### Análisis: Firewall bloqueando Active Directory

**Puertos necesarios para unión a dominio:**

| Puerto | Protocolo | Servicio | Estado |
|--------|-----------|----------|--------|
| 88 | TCP/UDP | Kerberos | Bloqueado ❌ |
| 135 | TCP | RPC Endpoint Mapper | Bloqueado ❌ |
| 389 | TCP/UDP | LDAP | Bloqueado ❌ |
| 445 | TCP | SMB | Bloqueado ❌ |
| 464 | TCP/UDP | Kerberos (cambio password) | Bloqueado ❌ |
| 636 | TCP | LDAPS | Bloqueado ❌ |
| 3268-3269 | TCP | Global Catalog | Bloqueado ❌ |

**Nota importante:** El ping funcionaba porque el puerto ICMP estaba habilitado (configurado en la Fase 2), pero los puertos de Active Directory estaban bloqueados.

---

### Solución: Desactivar Firewall temporalmente

**En el Windows Server:**

1. Panel de control → Sistema y seguridad
2. Firewall de Windows Defender
3. Activar o desactivar Firewall de Windows
4. **Desactivar** para:
   - Red de dominio
   - Red privada
   - Red pública

**Nota de seguridad:** Esta es una solución temporal para entornos de laboratorio. En producción, se deben crear reglas específicas para cada puerto necesario.

---

## 4.7 Unión Exitosa al Dominio

### Segundo intento (CON ÉXITO)

Con el Firewall del servidor desactivado:

**Procedimiento:**
1. `Win + R` → `sysdm.cpl`
2. Pestaña "Nombre de equipo" → Cambiar
3. Miembro de: ◉ Dominio → `milab`
4. Aceptar

**Ventana de credenciales:**
- Usuario: `Administrador`
- Contraseña: [contraseña del administrador del dominio]

**Mensaje recibido:**
```
✅ "Se unió correctamente al dominio milab."
```

**¡ÉXITO!** 🎉

---

### Reinicio del sistema

**Mensaje mostrado:**
```
"Debe reiniciar el equipo para aplicar los cambios.
¿Desea reiniciar el equipo ahora?"
```

- Click en **Reiniciar ahora**
- El sistema se reinició correctamente

---

### Verificación en pantalla de login

**Después del reinicio, la pantalla de login mostró:**

- ✅ **"Iniciar sesión en: MILAB"**
- ✅ Opción "¿Cómo puedo iniciar sesión en otro dominio?"
- ✅ Selector de usuario con "Otro usuario"

**Confirmación visual:** El equipo ahora es parte del dominio `milab.local`

---

## 4.8 Pruebas de Autenticación

### Test 1: Inicio de sesión con usuario del dominio (jperez)

**Procedimiento:**
1. Click en "Otro usuario"
2. Nombre de usuario: `jperez`
3. Contraseña: `Temporal123!`
4. Enter

**Primera vez:**
- Mensaje: "Preparando Windows..."
- Tiempo de espera: ~2 minutos (creación de perfil)
- Escritorio cargado correctamente

---

### Verificación de identidad

**Comando ejecutado:**

```cmd
whoami
```

**Resultado:**
```
milab\jperez
```

✅ **Usuario autenticado correctamente en el dominio**

---

### Test 2: Acceso a carpeta compartida (jperez)

**Recordatorio:** 
- jperez es miembro del grupo `IT_Soporte`
- El grupo `IT_Soporte` tiene permisos sobre la carpeta compartida `\\192.168.100.10\IT`

**Procedimiento:**
1. Explorador de archivos (Win + E)
2. Barra de direcciones: `\\192.168.100.10\IT`
3. Enter

**Resultado:**
- ✅ Carpeta abierta correctamente
- ✅ Carpeta inicialmente vacía (esperado)

---

### Test de escritura

**Prueba:**
1. Click derecho en la carpeta → Nuevo → Documento de texto
2. Nombre: `Prueba_jperez.txt`
3. Contenido: "aqui probando haber si puedo editar este archivo, esta carpeta esta en red"
4. Guardar

**Resultado:**
- ✅ Archivo creado exitosamente
- ✅ Tamaño: 1 KB
- ✅ Fecha de modificación: 01/02/2026

**Conclusión:** jperez tiene **permisos de escritura** en la carpeta compartida IT. ✅

---

### Test 3: Acceso denegado (cgarcia)

**Objetivo:** Verificar que usuarios NO miembros de IT_Soporte NO pueden acceder.

**Recordatorio:**
- cgarcia NO es miembro del grupo `IT_Soporte`
- Solo IT_Soporte tiene permisos sobre `\\192.168.100.10\IT`

---

**Procedimiento:**

1. **Cerrar sesión de jperez**
   - Inicio → Usuario → Cerrar sesión

2. **Iniciar sesión con cgarcia**
   - Otro usuario
   - Usuario: `cgarcia`
   - Contraseña: `Temporal123!`
   - Enter

3. **Intentar acceder a la carpeta compartida**
   - Explorador de archivos
   - Barra de direcciones: `\\192.168.100.10\IT`
   - Enter

---

**Resultado:**

```
Error de red

Windows no puede obtener acceso a \\192.168.100.10\it

No tiene permiso para obtener acceso a \\192.168.100.10\it.
Póngase en contacto con el administrador de la red para solicitar acceso.
```

❌ **Acceso denegado correctamente** ✅

**Conclusión:** El control de acceso basado en grupos funciona perfectamente.

---

## 4.9 Resumen de Pruebas Completadas

### Tabla de verificación

| Test | Usuario | Acción | Resultado Esperado | Resultado Real | Estado |
|------|---------|--------|-------------------|----------------|--------|
| 1 | jperez | `whoami` | `milab\jperez` | `milab\jperez` | ✅ PASS |
| 2 | jperez | Acceder a IT | Acceso permitido | Acceso permitido | ✅ PASS |
| 3 | jperez | Crear archivo en IT | Creado | `Prueba_jperez.txt` creado | ✅ PASS |
| 4 | cgarcia | `whoami` | `milab\cgarcia` | `milab\cgarcia` | ✅ PASS |
| 5 | cgarcia | Acceder a IT | Acceso denegado | Error de red | ✅ PASS |

**Resultado final:** 5/5 pruebas exitosas (100%) ✅

---

## 4.10 Componentes Verificados

### Active Directory

| Componente | Estado | Verificado |
|------------|--------|------------|
| Controlador de Dominio | ✅ Operativo | `dcdiag` - todos los tests pasados |
| Servicio DNS | ✅ Funcional | `nslookup milab.local` resuelve correctamente |
| Servicio LDAP | ✅ Funcional | Usuarios se autentican correctamente |
| Catálogo Global | ✅ Operativo | Consultas de dominio funcionan |
| Replicación | ✅ N/A | Solo un DC en el laboratorio |

---

### Autenticación y Autorización

| Función | Estado | Evidencia |
|---------|--------|-----------|
| Login con usuario de dominio | ✅ Funciona | jperez y cgarcia inician sesión |
| Resolución de SID | ✅ Funciona | `whoami` muestra dominio\usuario |
| Permisos de red (SMB) | ✅ Funciona | Carpeta compartida accesible |
| Control de acceso basado en grupos | ✅ Funciona | IT_Soporte accede, otros no |
| Creación de perfil de usuario | ✅ Funciona | Perfil creado en primer login |

---

## 4.11 Problemas Encontrados y Soluciones

### Problema 1: Windows 10 Home no soporta dominios

**Síntomas:**
- Botón "Dominio" deshabilitado
- Mensaje: "No puede unir un dominio..."

**Solución:**
- Instalar Windows 10 Pro

**Tiempo invertido:** 1 día (descarga e instalación)

---

### Problema 2: DNS no resolvía el dominio

**Síntomas:**
- `nslookup milab.local` devolvía "Non-existent domain"
- DNS usado: 100.100.1.1 (NAT)

**Causa:**
- Windows priorizaba DNS del adaptador NAT

**Solución:**
- Desactivar adaptador NAT temporalmente
- Forzar uso del DNS del servidor (192.168.100.10)

**Tiempo invertido:** 2 horas

---

### Problema 3: Firewall bloqueando unión al dominio

**Síntomas:**
- Ping funciona
- DNS funciona
- Error: "No se puede contactar con un controlador de dominio"

**Causa:**
- Firewall del servidor bloqueando puertos AD (88, 135, 389, 445, etc.)

**Solución:**
- Desactivar Firewall temporalmente (entorno de laboratorio)

**Solución en producción:**
- Crear reglas específicas para cada puerto necesario

**Tiempo invertido:** 3 horas (diagnóstico)

---

## 4.12 Conocimientos Aplicados

### Habilidades técnicas demostradas

- ✅ Instalación y configuración de Windows 10 Pro
- ✅ Configuración de red TCP/IP en Windows
- ✅ Diagnóstico de problemas de DNS
- ✅ Unión de equipos a dominios de Active Directory
- ✅ Autenticación de usuarios en dominio
- ✅ Acceso a recursos compartidos de red (SMB)
- ✅ Verificación de permisos basados en grupos
- ✅ Troubleshooting de Firewall de Windows
- ✅ Uso de herramientas de diagnóstico:
  - `ping`
  - `nslookup`
  - `ipconfig`
  - `whoami`
  - `dcdiag`
  - `net view`

---

### Conceptos comprendidos

- Diferencias entre ediciones de Windows (Home vs Pro)
- Requisitos para unión a dominios
- Funcionamiento del DNS en Active Directory
- Puertos utilizados por Active Directory
- Proceso de autenticación Kerberos
- Control de acceso basado en grupos (RBAC)
- Permisos de red vs permisos NTFS
- Troubleshooting sistemático de problemas de red

---

## 4.13 Configuración Final del Sistema

### Estado de las máquinas virtuales

**Windows Server 2022 (MBA-DOMINIO):**
- IP: 192.168.100.10
- Rol: Controlador de Dominio
- Servicios activos: AD DS, DNS
- Firewall: Desactivado (temporal)
- Estado: ✅ Operativo

**Windows 10 Pro (Cliente01):**
- IP: 192.168.100.20
- Miembro de: Dominio `milab.local`
- Adaptador NAT: Desactivado
- Estado: ✅ Operativo

**Ubuntu 24.04 (SRV-Files):**
- IP: 192.168.100.30
- Miembro de: Red interna (no unido a dominio)
- Estado: ✅ Operativo

---

### Usuarios y grupos activos

**Usuarios del dominio:**
- Administrador (admin del dominio)
- jperez (miembro de IT_Soporte)
- mlopez (miembro de IT_Soporte)
- cgarcia (usuario estándar)
- lmartinez (usuario estándar)
- aruiz (usuario estándar)

**Grupos de seguridad:**
- IT_Soporte (2 miembros: jperez, mlopez)

**Recursos compartidos:**
- \\MBA-DOMINIO\IT (permisos: IT_Soporte)

---

## 4.14 Próximos Pasos Recomendados

### Mejoras de seguridad

1. **Reactivar Firewall del servidor**
   - Crear reglas específicas para puertos de AD
   - Documentar las reglas creadas

2. **Configurar políticas de contraseñas**
   - Longitud mínima: 12 caracteres
   - Complejidad requerida
   - Historial de contraseñas

3. **Implementar auditoría**
   - Eventos de inicio de sesión
   - Cambios en Active Directory
   - Acceso a recursos compartidos

---

### Expansión del laboratorio

1. **Configurar DHCP**
   - Evitar configuración manual de IPs
   - Rango: 192.168.100.100-200

2. **Implementar Group Policy Objects (GPO)**
   - Fondo de pantalla corporativo
   - Restricciones de software
   - Scripts de inicio de sesión

3. **Unir Ubuntu al dominio**
   - Usar Samba + Winbind
   - O SSSD + Kerberos
   - Autenticación centralizada Linux

4. **Crear más carpetas compartidas**
   - Carpetas personales (home folders)
   - Carpetas departamentales
   - Permisos NTFS + permisos de red

5. **Implementar segundo DC**
   - Redundancia
   - Replicación de Active Directory
   - Alta disponibilidad

---

## 4.15 Lecciones Aprendidas

### Técnicas

1. **Planificación previa es fundamental**
   - Verificar ediciones de Windows antes de comenzar
   - Documentar requisitos de puertos
   - Tener plan B para problemas comunes

2. **Troubleshooting sistemático**
   - Verificar conectividad de red primero
   - Luego DNS
   - Luego servicios
   - Finalmente Firewall

3. **Entornos de laboratorio vs producción**
   - Desactivar Firewall es aceptable en lab
   - En producción, crear reglas específicas

4. **Importancia de DNS**
   - Active Directory depende completamente de DNS
   - DNS mal configurado = dominio no funcional

---

### Personales

1. **Persistencia en troubleshooting**
   - Algunos problemas requieren múltiples intentos
   - Documentar cada paso ayuda a identificar patrones

2. **Aprender de los errores**
   - Windows 10 Home fue un error útil
   - Ahora sé las diferencias entre ediciones

3. **Valor de la documentación**
   - Documentar mientras haces > documentar después
   - Las capturas de pantalla son evidencia valiosa

---

## 4.16 Conclusión de la Fase 4

### Resumen ejecutivo

Se completó exitosamente la unión de un equipo cliente Windows 10 Pro al dominio `milab.local`. A pesar de encontrar varios obstáculos (Windows Home, problemas de DNS, Firewall), se aplicó troubleshooting sistemático para resolverlos.

### Objetivos cumplidos

✅ Windows 10 Pro instalado y configurado  
✅ Cliente unido al dominio milab.local  
✅ Usuarios del dominio pueden iniciar sesión  
✅ Control de acceso basado en grupos funciona  
✅ Carpetas compartidas accesibles según permisos  
✅ Sistema de autenticación operativo al 100%  

### Estado del proyecto

**LABORATORIO COMPLETADO** 🎉

El laboratorio de Active Directory está completamente funcional y listo para:
- Prácticas adicionales
- Implementación de GPOs
- Configuración de servicios adicionales
- Demostración de habilidades técnicas

---

**Fecha de finalización:** 1 de febrero de 2026  
**Tiempo total invertido:** 3 días  
**Nivel de completitud:** 100% ✅

---

# RESOLUCIÓN DE PROBLEMAS

## Problema 1: VERR_PATH_NOT_FOUND (.vbox)

**Error completo:**
```
Runtime error opening 'C:\Users\[Usuario]\VirtualBox VMs\windows10\windows10.vbox'
for reading VERR_PATH_NOT_FOUND
```

**Causa:** VirtualBox busca archivo de configuración en ruta antigua

**Solución:**
1. Editar `%USERPROFILE%\.VirtualBox\VirtualBox.xml`
2. Eliminar línea `<MachineEntry>` de la VM afectada
3. Registrar VM desde nueva ubicación

**Ver sección 1.3 para detalles completos**

---

## Problema 2: VERR_PATH_NOT_FOUND (.vdi)

**Error completo:**
```
Could not open the medium 'C:\...\Windows 10.vdi'
VD: error VERR_PATH_NOT_FOUND
```

**Causa:** Archivo .vbox apunta a disco virtual en ruta antigua

**Solución:**
1. Configuración de VM → Almacenamiento
2. Reasignar disco .vdi desde nueva ubicación
3. Guardar y arrancar

**Ver sección 1.3 para detalles completos**

---

## Problema 3: Ping no funciona entre VMs

**Síntomas:**
- IPs configuradas correctamente
- `ping localhost` funciona
- `ping` a otras VMs falla

**Causa:** Firewall de Windows bloquea protocolo ICMP

**Solución:**
1. Firewall de Windows Defender → Configuración avanzada
2. Reglas de entrada
3. Buscar "Solicitud de eco ICMPv4"
4. Habilitar todas las reglas relacionadas

**Ver sección 2.6 para detalles completos**

---

## Problema 4: IP 169.254.x.x (APIPA)

**Síntomas:**
- Adaptador de red muestra IP que empieza por 169.254

**Causa:** No hay servidor DHCP en la red interna y no hay IP estática configurada

**Solución:**
- Esto es normal y esperado en red interna de VirtualBox
- Configurar IP estática manualmente
- La IP APIPA se reemplazará automáticamente

**Ver secciones 2.3, 2.4, 2.5 para configuración de IPs estáticas**

---

## Problema 5: VM arranca por PXE tras instalar AD

**Síntomas:**
- Después de promover a DC, la VM intenta arrancar desde red
- Mensaje: "PXE boot"

**Causa:** Orden de arranque de VirtualBox tras reinicio

**Solución:**
1. Apagar VM (forzar apagado si es necesario)
2. Configuración → Sistema → Orden de arranque
3. Mover "Disco duro" a primera posición
4. Desmarcar "Red"
5. Guardar y arrancar

---

## Problema 6: Archivos .vbox no visibles

**Síntomas:**
- Carpeta de VM no muestra archivo .vbox

**Causa:** Windows 11 oculta ciertos archivos por defecto

**Solución:**
1. Explorador de archivos
2. Menú Ver → Mostrar
3. ☑ Elementos ocultos
4. Ahora aparece archivo "VirtualBox Machine Definition (.vbox)"

---

## Problema 7: Windows 10 Home no soporta unión a dominios

**Síntomas:**
- Botón "Dominio" deshabilitado en propiedades del sistema
- Mensaje: "No puede unir un dominio un equipo en el que se ejecute esta edición de Windows 10"

**Causa:** Windows 10 Home Edition no incluye la funcionalidad de unión a dominios de Active Directory

**Solución:**
- Instalar Windows 10 Pro, Enterprise o Education
- No existe workaround para Home Edition

**Lección aprendida:** Verificar ediciones de Windows antes de configurar entornos de dominio

**Ver sección 4.2 para detalles completos**

---

## Problema 8: DNS no resuelve dominio tras reinicio

**Síntomas:**
- `nslookup milab.local` devuelve "Non-existent domain"
- DNS usado: 100.100.1.1 o similar (DNS del NAT)
- `nslookup` funciona desde el servidor pero no desde el cliente

**Causa:** Windows prioriza el DNS del adaptador NAT sobre el DNS del adaptador de red interna

**Solución:**
1. Desactivar temporalmente el adaptador NAT:
   - Panel de control → Red → Cambiar configuración del adaptador
   - Click derecho en adaptador NAT → Deshabilitar

2. Limpiar caché DNS:
   ```cmd
   ipconfig /flushdns
   ipconfig /registerdns
   ```

3. Verificar:
   ```cmd
   nslookup milab.local
   ```

**Solución permanente:**
- Configurar métricas de interfaz para priorizar red interna
- O mantener NAT desactivado durante operaciones de dominio

**Ver sección 4.5 para detalles completos**

---

## Problema 9: Error al unir al dominio - "No se puede contactar con DC"

**Síntomas:**
- Ping al servidor funciona ✅
- DNS resuelve correctamente ✅
- `dcdiag` en el servidor pasa todos los tests ✅
- Error: "No se puede poner en contacto con un controlador de dominio de Active Directory (AD DC) en el dominio 'milab'"

**Causa:** Firewall del servidor bloqueando puertos necesarios para Active Directory

**Puertos bloqueados:**
- 88 (Kerberos)
- 135 (RPC)
- 389 (LDAP)
- 445 (SMB)
- 464 (Kerberos password change)
- 636 (LDAPS)
- 3268-3269 (Global Catalog)

**Solución temporal (laboratorio):**
1. En Windows Server:
2. Panel de control → Firewall de Windows
3. Activar o desactivar Firewall
4. Desactivar para todas las redes
5. Intentar unión al dominio nuevamente

**Solución en producción:**
Crear reglas de Firewall específicas para cada puerto necesario:

```powershell
# Ejemplo de regla para Kerberos
New-NetFirewallRule -DisplayName "AD-Kerberos-TCP" -Direction Inbound -Protocol TCP -LocalPort 88 -Action Allow

# Ejemplo de regla para LDAP
New-NetFirewallRule -DisplayName "AD-LDAP-TCP" -Direction Inbound -Protocol TCP -LocalPort 389 -Action Allow

# (Repetir para todos los puertos necesarios)
```

**Ver sección 4.6 para detalles completos**

---

# ANEXOS

## Anexo A: Comandos Útiles

### Comandos de red - Windows

```cmd
# Ver configuración IP
ipconfig /all

# Probar conectividad
ping 192.168.100.10
ping -t 192.168.100.20  # Ping continuo

# Liberar y renovar IP (si fuera DHCP)
ipconfig /release
ipconfig /renew

# Ver tabla de rutas
route print

# Ver caché ARP
arp -a

# Resolver nombre DNS
nslookup milab.local
nslookup MBA-DOMINIO.milab.local
```

### Comandos de red - Linux (Ubuntu)

```bash
# Ver configuración IP
ip addr show
ifconfig  # (si está instalado)

# Probar conectividad
ping -c 4 192.168.100.10

# Ver tabla de rutas
ip route show

# Resolver nombre DNS
nslookup milab.local
dig milab.local
```

### Comandos de Active Directory

```cmd
# Verificar estado del DC
dcdiag

# Ver información del dominio
nltest /dsgetdc:milab.local

# Listar usuarios del dominio
net user /domain

# Información de un usuario específico
net user jperez /domain

# Listar grupos del dominio
net group /domain

# Ver miembros de un grupo
net group "IT_Soporte" /domain

# Verificar relación de confianza
nltest /sc_query:milab.local
```

---

## Anexo B: Estructura de Archivos de VirtualBox

### Archivos principales de una VM

```
D:\VirtualBox VMs\Windows Server 2022\
├── Windows Server 2022.vbox           # Configuración de la VM
├── Windows Server 2022.vbox-prev      # Backup de configuración
├── Windows Server 2022.vdi            # Disco virtual
├── Logs\                              # Logs de ejecución
│   ├── VBox.log
│   ├── VBox.log.1
│   └── ...
└── Snapshots\                         # Instantáneas (si existen)
```

### Archivo de configuración global

```
C:\Users\[Usuario]\.VirtualBox\
├── VirtualBox.xml                     # Registro de todas las VMs
├── VirtualBox.xml-prev                # Backup
└── ...
```

---

## Anexo C: Esquemas de Red

### Red Interna (red_empresa)

```
192.168.100.0/24
├── 192.168.100.1-9    → Reservadas para gateways (futuro)
├── 192.168.100.10     → Windows Server (DC)
├── 192.168.100.20     → Windows 10 (Cliente)
├── 192.168.100.30     → Ubuntu (Servidor archivos)
└── 192.168.100.40-254 → Disponibles para futuras VMs
```

### Adaptadores de cada VM

```
Adaptador 1 (Red Interna "red_empresa")
├── Windows Server → 192.168.100.10
├── Windows 10     → 192.168.100.20
└── Ubuntu         → 192.168.100.30

Adaptador 2 (NAT)
├── Windows Server → DHCP (10.0.2.15 típicamente)
├── Windows 10     → DHCP (10.0.2.15 típicamente)
└── Ubuntu         → DHCP (10.0.2.15 típicamente)
```

---

## Anexo D: Buenas Prácticas de Seguridad

### En entorno de laboratorio (permisible)

- ☑ "La contraseña nunca expira" para usuarios de prueba
- ☑ Contraseñas simples como "Temporal123!"
- ☑ Desactivar verificación de complejidad de contraseña
- ☑ Misma contraseña para todos los usuarios de prueba

### En entorno de producción (OBLIGATORIO)

- ☑ Contraseñas complejas (mínimo 12 caracteres)
- ☑ "El usuario debe cambiar la contraseña en el siguiente inicio"
- ☑ Expiración de contraseñas (30-90 días)
- ☑ Historial de contraseñas (no repetir las últimas 24)
- ☑ Bloqueo de cuenta tras intentos fallidos
- ☑ Auditoría de cambios en AD
- ☑ Contraseña DSRM única y muy robusta
- ☑ Backups regulares de Active Directory
- ☑ Monitorización de eventos de seguridad

---

## Anexo E: Próximos Pasos del Laboratorio

### Tareas pendientes

1. **Unir Windows 10 al dominio**
   - Cambiar DNS a 192.168.100.10
   - Unirse a milab.local
   - Probar inicio de sesión con usuarios del dominio

2. **Configurar carpetas personales**
   - Crear carpetas home para cada usuario
   - Configurar permisos NTFS
   - Mapear unidades de red automáticamente

3. **Implementar Políticas de Grupo (GPO)**
   - Fondo de pantalla corporativo
   - Restricciones de software
   - Configuración de seguridad
   - Scripts de inicio de sesión

4. **Instalar y configurar DHCP**
   - Evitar configuración manual de IPs
   - Rango: 192.168.100.100-200
   - Reservas para servidores

5. **Configurar DNS adicional**
   - Zonas inversas
   - Registros personalizados
   - Forwarders

6. **Implementar servidor de archivos en Ubuntu**
   - Samba para compatibilidad Windows
   - Compartir recursos desde Linux
   - Integración con AD (opcional avanzado)

7. **Backup y recuperación**
   - Snapshots de VMs
   - Backup de Active Directory
   - Plan de recuperación ante desastres

---

## Anexo F: Glosario de Términos

| Término | Significado |
|---------|-------------|
| **AD DS** | Active Directory Domain Services - Servicio de directorio de Microsoft |
| **DC** | Domain Controller - Controlador de Dominio |
| **DNS** | Domain Name System - Sistema de nombres de dominio |
| **DSRM** | Directory Services Restore Mode - Modo de recuperación de AD |
| **FQDN** | Fully Qualified Domain Name - Nombre completo del dominio |
| **GC** | Global Catalog - Catálogo global |
| **GPO** | Group Policy Object - Objeto de directiva de grupo |
| **NAT** | Network Address Translation - Traducción de direcciones de red |
| **NetBIOS** | Network Basic Input/Output System - Protocolo legacy de red |
| **OU** | Organizational Unit - Unidad organizativa |
| **RODC** | Read-Only Domain Controller - DC de solo lectura |
| **SYSVOL** | System Volume - Volumen del sistema compartido en DCs |
| **VDI** | Virtual Disk Image - Imagen de disco virtual de VirtualBox |

---

## Anexo G: Referencias y Recursos

### Documentación oficial

- [Microsoft Learn - Active Directory](https://learn.microsoft.com/es-es/windows-server/identity/ad-ds/)
- [VirtualBox Manual](https://www.virtualbox.org/manual/)
- [Windows Server 2022 Documentation](https://learn.microsoft.com/es-es/windows-server/)

### Herramientas utilizadas

- Oracle VirtualBox 7.x
- Windows Server 2022 Standard
- Windows 10 Pro (build 19045)
- Ubuntu 24.04 LTS

---

## 📝 Control de Cambios

| Versión | Fecha | Cambios |
|---------|-------|---------|
| 1.0 | Dic 2025 | Configuración inicial de VirtualBox y red |
| 1.5 | Ene 2026 | Resolución de problemas de migración |
| 2.0 | 25-26 Ene 2026 | Implementación de Active Directory y documentación unificada |
| 3.0 | 29 Ene - 1 Feb 2026 | Unión de Windows 10 Pro al dominio, pruebas completas de autenticación y permisos |

---

## 👤 Información del Autor

**Técnico Superior en Administración de Sistemas Informáticos en Red (ASIR)**  
Laboratorio personal de prácticas  
Febrero 2026

---

**FIN DEL DOCUMENTO**

---

*Este documento ha sido creado como portfolio técnico para demostrar habilidades prácticas en:*
- *Virtualización con VirtualBox*
- *Administración de redes*
- *Configuración de Windows Server*
- *Implementación de Active Directory*
- *Unión de clientes a dominios*
- *Control de acceso basado en grupos*
- *Troubleshooting sistemático*
- *Documentación técnica profesional*
