
## 3️⃣ Estructura recomendada para un README de SOC LAB

Para un laboratorio SOC (Elastic, SIEM, ciberseguridad), esta estructura es ideal y profesional:


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

## INSTALACIÓN Y CONFIGURACIÓN DE MÁQUINAS

vamos a empezar con el tema de las maquinas Virtuales, iniciemos con:
### Pfsense

hay quue descargarlo del sitio web oficial, para esto les pasare el Link oficial,
```bash
https://www.pfsense.org/download/
```
Luego de descargar PFsense, hay que darle al botón de "Nuevo" en VirtualBox, se elige el archivo `.iso` y lo colocamos como "Type: Linux" y "Version: Other Linux x64".  
Antes de iniciar la máquina hay que darle <ins>click derecho</ins> y entrar en configuración. Dentro de configuración vamos a la pestaña <ins>Network</ins>, donde hay que habilitar el <ins>Adapter 2</ins>. Ahí cambiamos la opción WAN por INTERNAL NETWORK, y en opciones avanzadas, en **Promiscuous Mode**, habilitamos **Allow All** y le damos a Accept.

Ahora iniciamos PFsense y, dentro de la instalación, aceptamos todo hasta llegar a la **configuración de red**, donde elegiremos cuál será la red *WAN* y la red *LAN*.  
La red WAN será la red NAT y la red LAN será la red del <ins>Adapter 2</ins>. Luego de eso, elegimos el disco donde instalar el sistema y seleccionamos *Reboot*.

Luego de que se reinicie, nos dirigimos a *Devices*, luego a *Optical Units*, y destildamos la opción donde está la <ins>.iso</ins> de PFsense. Después reiniciamos el sistema.  
Con el sistema ya instalado, esperamos a que cargue por completo (esto puede tomar aproximadamente 1 o 2 minutos). Luego de que termine la carga, deberían aparecer 16 opciones.

Para que PFsense detecte cuál es cada red, se asignan manualmente de esta manera:
* Elegimos la opción 1.
* En la opción de VLANs damos simplemente Enter.
* Cuando pida identificar la red WAN, hay que elegir la opción que esté configurada en VirtualBox como Adapter 1.
* Cuando pida identificar la red LAN, hay que elegir la opción que esté configurada en VirtualBox como Local Network, que en este caso sería Adapter 2.

> [!NOTE]  
> Teniendo en cuenta cómo identifica las redes PFsense, se puede definir que Adapter1 = em0 - Adapter2 = em1 - Adapter3 = em2 - Adapter4 = em3. Siempre teniendo en cuenta que estamos utilizando VirtualBox.

Cuando se termine de configurar, hay que ir a la opción 2 donde:

* Seleccionaremos la red LAN con una opción numérica.
* Luego elegiremos la IP de la LAN (Ej: 192.168.1.1).
* Luego la máscara, que será de 24.
* En la opción de ingresar una IP Gateway, simplemente se le da al botón Enter.
* En la opción de ingresar una IP v6, se da Enter nuevamente.
* Cuando se solicite elegir si queremos DHCP para la red LAN, damos "Y".
* El rango IP para el DHCP empezaría (utilizando el ejemplo anterior) con 192.168.1.2 a 192.168.1.10. Luego damos Enter y esperamos a que se guarden los cambios.

Cuando se termine de configurar, nos va a dar una dirección web que, utilizando el ejemplo anterior, quedaría "http://192.168.1.1/" y con eso ya estaría terminada la configuración de PFsense.

> [!IMPORTANT]  
> Para loguearse en la página de PFsense utilizar:  
> **Login**: admin  
> **Password**: pfsense

### <ins>Windows Server</ins>
Al colocar la ISO de Windows Server, esta se configura de manera automática. Para evitar esto, destildaremos la opción <ins>Skip Unattended Installation</ins> para poder configurarlo manualmente. Colocamos las especificaciones mencionadas en "Requisitos del sistema".  
Antes de iniciar la máquina, hay que ir a la configuración y en la opción de Network elegiremos en Adapter1 la opción de "**Internal Network**".  
Al iniciar la instalación dejaremos el lenguaje del sistema en inglés, luego elegiremos la opción "Windows Server 2022 Datacenter Evolution (Desktop Edition)" y para finalizar elegimos la opción *Custom Install* y seleccionamos el *Drive 0* para que finalice la instalación. Antes de reiniciarse, te pedirá una contraseña para establecer el administrador, la cual es a gusto propio.  

Ya con la instalación finalizada, hay que loguearse con la cuenta de *Administrator*. Ya en el inicio, dejamos que se inicie el $Server Manager$ y nos dirigimos a:
* Network Connections
  * Click derecho en Properties
    * Doble click en Internet Protocol V4

Aquí configuramos la dirección IP manualmente con alguna IP dentro del rango DHCP. Colocamos una máscara de /24, la Gateway va a ser la IP de la red LAN, y el servidor DNS va a ser la misma IP que colocamos como dirección IP. También, como DNS secundaria, se puede poner alguna de las bien conocidas como Google: 8.8.8.8 o 4.4.8.8.  

Para probar que la conexión es estable, se puede ingresar al servicio interno de PFsense que te da al finalizar la configuración (en el ejemplo sería "http://192.168.1.1/"). Si entra, quiere decir que la conexión está establecida y que la red LAN funciona correctamente.  

> [!TIP]  
> Para más comodidad a la hora de identificar las máquinas, se puede cambiar el nombre dirigiéndote al buscador: *About your PC* -> *Rename This PC* y cambiarlo a, por ejemplo, `WIN-AD`. Esto nos va a servir más adelante.

Una vez conectado, hay que crear un servidor dedicado de **Active Directory**. Para esto, vamos al $Server Manager$ y nos dirigimos a:
* Add Roles and Features
  * Next tres veces seguidas
    * Buscamos y tildamos "Active Directory Domain Services"
      * Add Features
        * Next tres veces más
          * Install

Esperamos a que se termine de instalar y luego clic en "Close". Seleccionamos el banderín que se encuentra en la parte superior izquierda y nos dirigimos a la opción "Promote this server to a domain controller". Los siguientes pasos son:

* Seleccionar "Add New Forest"
  * En "Root Domain Name" colocar un nombre de dominio como "soc.lab" o "prueba.local", y luego Next
    * Elegimos una contraseña, la colocamos, repetimos en "Confirm Password" y damos Next
      * Dejar la parte de DNS por defecto y Next
        * El NetBIOS lo escribimos físicamente (en papel) para recordarlo y damos Next
          * Las siguientes dos opciones las dejamos por defecto y Next
            * Damos a Install y reiniciamos cuando se solicite

Una vez reiniciado, el Active Directory quedará activo.

