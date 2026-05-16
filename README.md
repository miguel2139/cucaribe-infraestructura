# 🏛️ Infraestructura de Servidores — CUCARIBE

> Proyecto universitario de despliegue de infraestructura de red e intranet corporativa usando máquinas virtuales sobre Ubuntu 22.04 LTS.

---

## 📋 Descripción

Diseño e implementación de una infraestructura de servidores para una institución de educación superior con más de 20.000 estudiantes. El proyecto consiste en una **intranet segura** protegida por firewall, con servicios de web, base de datos, DHCP y DNS desplegados en máquinas virtuales independientes.

El sitio web interno (`http://cucaribe.intranet`) está gestionado con **Joomla** en arquitectura de **dos capas**: capa de aplicación (Apache) y capa de datos (MySQL).

---

## 🗺️ Arquitectura de Red

```
                        [ Clientes ]
                    Laptop / Móvil / PC
                           |
                        [Switch]
                     192.168.201.x
                           |
                       [Firewall]
                  .201.1 ←→ .101.1
                           |
                        [Switch]
                     192.168.101.x
          ┌────────────┬──────────┬─────────┐
     [Web Server]  [MySQL DB]  [DHCP]    [DNS]
      .101.2        .101.3     .101.4    .101.5
```

---

## 🖥️ Máquinas Virtuales

| VM | Rol | SO | IP Estática |
|----|-----|----|-------------|
| **VM1 — Firewall** | Seguridad perimetral y enrutamiento | Ubuntu 22.04 LTS | 192.168.101.1 / 192.168.201.1 |
| **VM2 — Web Server** | Apache 2.4 + Joomla (capa de aplicación) | Ubuntu 22.04 LTS | 192.168.101.2 |
| **VM3 — MySQL Server** | Base de datos Joomla (capa de datos) | Ubuntu 22.04 LTS | 192.168.101.3 |
| **VM4 — DHCP + DNS** | Asignación de IPs dinámicas y resolución de nombres | Ubuntu 22.04 LTS | 192.168.101.4 |

---

## 🔧 Tecnologías Utilizadas

- **Sistema Operativo:** Ubuntu Server 22.04 LTS
- **Web Server:** Apache 2.4
- **CMS:** Joomla (gestión de contenido de la intranet)
- **Base de datos:** MySQL
- **DHCP:** isc-dhcp-server
- **DNS:** BIND9
- **Firewall:** iptables / ufw
- **Virtualización:** VirtualBox / VMware

---

## 🌐 Configuración de Red

### Servidores (IPs estáticas)
| Servidor | IP | Máscara |
|----------|----|---------|
| Firewall (LAN interna) | 192.168.101.1 | 255.255.255.0 |
| Firewall (LAN clientes) | 192.168.201.1 | 255.255.255.0 |
| Web Server | 192.168.101.2 | 255.255.255.0 |
| MySQL Server | 192.168.101.3 | 255.255.255.0 |
| DHCP / DNS | 192.168.101.4 | 255.255.255.0 |

### Clientes (IPs dinámicas vía DHCP)
| Parámetro | Valor |
|-----------|-------|
| Rango DHCP | 192.168.201.2 — 192.168.201.254 |
| Máscara | 255.255.255.0 |
| Gateway | 192.168.201.1 |
| DNS | 192.168.101.4 |

---

## 🔒 Reglas de Firewall (iptables)

| Origen | Destino | Protocolo | Puerto | Acción |
|--------|---------|-----------|--------|--------|
| Clientes (201.x) | Web Server | HTTP | 80 | ✅ ALLOW |
| Clientes (201.x) | DNS | DNS | 53 | ✅ ALLOW |
| Clientes (201.x) | Cualquier servidor | ICMP | — | ✅ ALLOW (PING) |
| Clientes (201.x) | MySQL Server | TCP | 3306 | ❌ DENY |
| Clientes (201.x) | DHCP Server | UDP | 67/68 | ❌ DENY (acceso directo) |

---

## 🚀 Despliegue

### 1. Clonar el repositorio
```bash
git clone https://github.com/miguel2139/cucaribe-infraestructura.git
cd cucaribe-infraestructura
```

### 2. Revisar los scripts de configuración
```bash
ls configs/
# firewall.sh   web-server.sh   mysql.sh   dhcp.conf   named.conf
```

### 3. Importar las máquinas virtuales
Las imágenes `.ova` de las VMs están disponibles en:  
📦 [Google Drive — VMs CUCARIBE](https://drive.google.com/tu-enlace-aqui)

> **Nota:** Las VMs pesan varios GB. Se recomienda descargarlas directamente desde el enlace de Drive.

---

## 📁 Estructura del Repositorio

```
📁 cucaribe-infraestructura/
├── 📄 README.md
├── 📁 configs/
│   ├── firewall.sh          # Reglas iptables
│   ├── web-server.sh        # Instalación Apache + Joomla
│   ├── mysql.sh             # Configuración MySQL
│   ├── dhcp.conf            # Configuración isc-dhcp-server
│   └── named.conf           # Configuración BIND9 (DNS)
├── 📁 screenshots/
│   ├── topologia-red.png    # Diagrama de red
│   ├── joomla-intranet.png  # Sitio web funcionando
│   ├── dhcp-asignacion.png  # DHCP asignando IPs
│   └── firewall-rules.png   # Reglas de firewall activas
└── 📁 docs/
    └── informe-tecnico.pdf  # Documentación completa del proyecto
```


## 👤 Autor

**Miguel Andrés Bohórquez Cárdenas**  
Estudiante de Ingeniería de Sistemas — Último semestre  
🔗 [LinkedIn](https://www.linkedin.com/in/miguel-bohórquez-0b9882178) · [GitHub](https://github.com/miguel2139)

---

## 📌 Nota Académica

Proyecto desarrollado como trabajo final de la asignatura de **Administración de Sistemas / Redes**, implementando infraestructura real de servidores en entorno virtualizado.