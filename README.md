
## 3️⃣ Estructura recomendada para un README de SOC LAB

Para un laboratorio SOC (Elastic, SIEM, ciberseguridad), esta estructura es ideal y profesional:
Le implementare un nuevo plus, servicios de aws 


# SOC-LAB-1

Laboratorio de Security Operations Center (SOC) enfocado en la detección, monitoreo y análisis de eventos de seguridad utilizando Elastic SIEM.

## 🎯 Objetivo
Demostrar conocimientos prácticos en:
- Monitoreo de eventos de seguridad
- Análisis de logs
- Detección de incidentes
- Uso de herramientas SIEM

## 🛠 Tecnologías utilizadas
- Elastic Stack (Elasticsearch, Kibana)
- Linux
- Docker
- Beats / Elastic Agent

## 🧪 Alcance del laboratorio
- Recolección de logs
- Visualización en dashboards
- Detección de alertas
- Análisis básico de incidentes

## 🚀 Instalación
Primero instalremos Virtual box, dependiendo de su OS la instalcion podra variar, por eso recomiendo seguir los pasos de intalacion dados por Virtual box, aca esta el link de la pagina oficial:
```bash
https://www.virtualbox.org/wiki/Downloads
```

### MÁQUINAS  
Para virtualizar estas 4 máquinas virtuales se requiere de un virtualizador, en esta guía se utiliza **VirtualBox**, pero se puede usar cualquier otro programa con las mismas características.  
Se van a ejecutar 4 máquinas virtuales en total, estas van a ser:  
- Una máquina virtual con [PFsense](https://www.pfsense.org/download/)  
- Una máquina virtual con [Windows Server 2022](https://www.microsoft.com/es-es/evalcenter/download-windows-server-2022)  
- Una máquina virtual con [Windows 10](https://www.microsoft.com/es-es/software-download/windows10)  
- Una máquina virtual con [Linux Ubuntu](https://ubuntu.com/download/desktop)

### RED  
Dentro de VirtualBox utilizaremos 2 tipos de redes:  
* Una es la red WAN para dar internet.  
* Y la red LAN que viene por defecto en VirtualBox.

Todo esto lo tome como base del del repositorio creado por .[Berkeross](https://github.com/Berkeross/Lab-SOC-Elastic)

---

## Identificar FP y PF usando Atomic Red Team y Elastic
