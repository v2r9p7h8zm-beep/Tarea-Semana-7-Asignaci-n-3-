# Proyecto de Seguridad de Redes – ITLA

**Implementación de infraestructura de red y monitoreo mediante Zabbix**

**VIDEO SUBIDO A YOUTUBE: ** 


## 1. Información general

| Campo                    | Información                                          |
| ------------------------ | ---------------------------------------------------- |
| Institución              | Instituto Tecnológico de las Américas (ITLA)         |
| Asignatura               | Seguridad de Redes                                   |
| Estudiante               | Jennifer Lopez                                       |
| Matrícula                | 20240860                                             |
| Proyecto                 | Implementación de infraestructura de red y monitoreo |
| Plataforma de simulación | GNS3                                                 |
| Servidor de monitoreo    | Ubuntu Server 24.04.5 LTS                            |
| Sistema de monitoreo     | Zabbix 7.0                                           |

## 2. Descripción del proyecto

Este proyecto consiste en la implementación de una infraestructura de red simulada mediante GNS3, integrando dispositivos de red, equipos clientes y un servidor Ubuntu para la administración y supervisión de los recursos de la infraestructura.

Se implementó un router Cisco como dispositivo principal de comunicación, un switch virtual para la interconexión de los equipos, dos estaciones de trabajo y un servidor Ubuntu que aloja la plataforma Zabbix.

El objetivo principal es demostrar la configuración, conectividad y supervisión de una infraestructura de red, utilizando protocolos y herramientas que permiten observar el estado de los dispositivos y los recursos del sistema.

## 3. Objetivos

### Objetivo general

Diseñar e implementar una infraestructura de red que permita la comunicación entre diferentes dispositivos y la supervisión centralizada de su funcionamiento mediante Zabbix.

### Objetivos específicos

- Configurar un router Cisco como gateway de la red local.
- Implementar direccionamiento IP y asignación dinámica mediante DHCP.
- Interconectar los dispositivos mediante Open vSwitch.
- Configurar un servidor Ubuntu para alojar Zabbix.
- Implementar monitoreo de dispositivos mediante SNMP.
- Instalar y configurar Zabbix Agent 2 para supervisar Ubuntu Server.
- Verificar la conectividad entre los dispositivos.
- Visualizar métricas de rendimiento mediante gráficos y elementos de monitoreo.
- Documentar las configuraciones, comandos y evidencias del laboratorio.

## 4. Topología de red

La infraestructura está compuesta por los siguientes elementos:

- **R1:** Router Cisco encargado del enrutamiento y servicio DHCP.
- **Open vSwitch:** Switch virtual utilizado para interconectar los dispositivos.
- **PC1:** Primera estación de trabajo.
- **PC2:** Segunda estación de trabajo.
- **Ubuntu Server:** Servidor que aloja Zabbix Server, MariaDB, Apache y Zabbix Agent 2.

### Esquema lógico

```text
                  RED LAN
              192.168.60.0/24

                 R1 - CISCO
                192.168.60.1
                      |
                      |
                OPEN vSWITCH
                 /    |    \
                /     |     \
              PC1    PC2   UBUNTU
             .21     .22    .23
                            |
                       ZABBIX SERVER
                       ZABBIX AGENT 2
                       MARIADB
                       APACHE
```

<img width="338" height="272" alt="image" src="https://github.com/user-attachments/assets/b54cf234-6b1e-4beb-bc89-b82b5266f0b1" />


La interfaz NAT adicional de Ubuntu permite el acceso a Internet para la instalación y actualización de los componentes necesarios.

## 5. Plan de direccionamiento IP

La red local implementada utiliza el segmento `192.168.60.0/24`.

| Dispositivo   | Interfaz        | Dirección IP     | Función               |
| ------------- | --------------- | ---------------- | --------------------- |
| R1            | FastEthernet0/0 | 192.168.60.1/24  | Gateway               |
| PC1           | Ethernet        | 192.168.60.21/24 | Cliente DHCP          |
| PC2           | Ethernet        | 192.168.60.22/24 | Cliente DHCP          |
| Ubuntu Server | enp0s8          | 192.168.60.23/24 | Servidor de monitoreo |
| Ubuntu Server | enp0s3          | 10.0.2.15/24     | Conexión NAT          |

**Gateway de la LAN:** 192.168.60.1
**Servidor DNS configurado por DHCP:** 8.8.8.8

## 6. Tecnologías y herramientas utilizadas

| Tecnología            | Uso                                     |
| --------------------- | --------------------------------------- |
| GNS3                  | Simulación de la infraestructura de red |
| Cisco IOS             | Configuración del router R1             |
| Open vSwitch          | Conmutación de la red local             |
| VPCS                  | Simulación de estaciones de trabajo     |
| Ubuntu Server 24.04.5 | Sistema operativo del servidor          |
| Zabbix 7.0            | Monitoreo centralizado                  |
| Zabbix Agent 2        | Monitoreo del servidor Ubuntu           |
| SNMPv2c               | Comunicación de monitoreo con el router |
| MariaDB               | Base de datos de Zabbix                 |
| Apache                | Servicio web de Zabbix                  |
| VirtualBox            | Virtualización del servidor             |

## 7. Implementación realizada

### 7.1 Configuración del router Cisco

Se configuró el router R1 con la dirección IP `192.168.60.1/24` en su interfaz FastEthernet0/0.

También se configuró un pool DHCP denominado `LAN_ITLA`, encargado de asignar automáticamente direcciones IP a los equipos clientes.

Se reservaron las direcciones comprendidas entre `192.168.60.1` y `192.168.60.20` para evitar conflictos con las direcciones utilizadas por los dispositivos de infraestructura.

### 7.2 Configuración de SNMP

Se habilitó SNMPv2c en el router Cisco para permitir que Zabbix obtenga información del dispositivo.

Se configuraron los datos de identificación del equipo:

- Comunidad SNMP de solo lectura.
- Ubicación: ITLA-LAB.
- Contacto: Jennifer.

La comunicación SNMP fue comprobada desde Ubuntu Server mediante una consulta al identificador de descripción del sistema del router.

### 7.3 Configuración de Ubuntu Server

Se instaló Ubuntu Server 24.04.5 LTS y se configuraron dos interfaces de red:

- `enp0s3`: conexión NAT para acceso a Internet.
- `enp0s8`: conexión a la red LAN del laboratorio.

Se utilizó Netplan para configurar las interfaces y evitar conflictos entre las rutas de red.

### 7.4 Instalación de Zabbix

Se instaló Zabbix 7.0 junto con sus componentes principales:

- Zabbix Server.
- Zabbix Frontend.
- Zabbix SQL Scripts.
- MariaDB.
- Apache.

Se creó la base de datos `zabbix`, se configuró el usuario correspondiente y se importó el esquema inicial.

Posteriormente, se configuró la conexión entre Zabbix Server y MariaDB.

### 7.5 Monitoreo del router Cisco

Se registró el router R1 en Zabbix utilizando SNMPv2c.

Se aplicó la plantilla **Cisco IOS by SNMP**, permitiendo obtener información relacionada con el estado y rendimiento del dispositivo.

### 7.6 Monitoreo de Ubuntu Server

Se instaló Zabbix Agent 2 y se configuró la comunicación con el servidor de monitoreo.

Se registró el equipo en Zabbix con el nombre técnico `Ubuntu-ITLA` y se aplicó la plantilla **Linux by Zabbix agent**.

Esto permitió visualizar información sobre el uso de CPU, memoria, disco y otros recursos del servidor.

## 8. Pruebas de conectividad y funcionamiento

Durante la implementación se realizaron las siguientes pruebas:

| Prueba                      | Resultado |
| --------------------------- | --------- |
| PC1 hacia R1                | Correcto  |
| PC2 hacia R1                | Correcto  |
| PC1 hacia PC2               | Correcto  |
| Ubuntu hacia R1             | Correcto  |
| Ubuntu hacia Internet       | Correcto  |
| Resolución DNS desde Ubuntu | Correcto  |
| Consulta SNMP hacia R1      | Correcto  |
| Servicio Zabbix Server      | Activo    |
| Servicio Zabbix Agent 2     | Activo    |
| Monitoreo de R1             | Operativo |
| Monitoreo de Ubuntu         | Operativo |

Los resultados permitieron comprobar la conectividad de la infraestructura y la comunicación entre los dispositivos y la plataforma de monitoreo.

## 9. Evidencias del proyecto

Las evidencias de la implementación se encuentran organizadas dentro del repositorio.

Se incluyen capturas de pantalla de:

- Topología implementada en GNS3.
- Configuración del router Cisco.
- Asignación de direcciones IP mediante DHCP.
- Pruebas de conectividad entre dispositivos.
- Configuración de Ubuntu Server.
- Instalación y configuración de Zabbix.
- Estado de los servicios.
- Registro del router mediante SNMP.
- Registro del servidor Ubuntu mediante Zabbix Agent 2.
- Paneles y gráficos de monitoreo.

Las capturas adicionales de gráficos y métricas se incorporarán a la carpeta de evidencias.

## 10. Estructura del repositorio

\-

## 11. Seguridad y consideraciones

Durante la implementación se utilizaron credenciales y parámetros de laboratorio para configurar los servicios.

Antes de publicar el repositorio se deben aplicar las siguientes medidas:

- No publicar contraseñas reales.
- No incluir claves privadas ni tokens.
- Utilizar archivos de ejemplo para las configuraciones que contengan credenciales.
- Cambiar la comunidad SNMP de laboratorio antes de exponer la infraestructura.
- Verificar que los archivos compartidos no contengan información sensible.
- Mantener los servicios y las configuraciones actualizados.

## 12. Conclusión

La implementación permitió desarrollar una infraestructura de red funcional mediante GNS3, integrando dispositivos Cisco, estaciones de trabajo y un servidor Ubuntu.

La configuración de DHCP permitió la asignación dinámica de direcciones IP, mientras que SNMP y Zabbix Agent 2 hicieron posible incorporar el router y el servidor a una plataforma centralizada de monitoreo.

Las pruebas realizadas confirmaron la comunicación entre los dispositivos y el funcionamiento de los principales servicios implementados.

Este laboratorio permitió aplicar conocimientos relacionados con configuración de redes, administración de servidores, protocolos de monitoreo y supervisión de infraestructura tecnológica.

## 13. Autor

**Jennifer Lopez**
Matrícula: **20240860**
Instituto Tecnológico de las Américas (ITLA)
Asignatura: Seguridad de Redes

---

**Nota:** Este repositorio contiene la documentación técnica, configuraciones, comandos, scripts y evidencias correspondientes al desarrollo del laboratorio.
# Tarea-Semana-7-Asignaci-n-3-
