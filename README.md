# Proyecto Final Laboratorio Kathará

## Octavo registro – Configuración del Servidor DHCP,DNS y OSPF.
 
## 1. Tabla de Routers y Hosts

### Routers

| Dispositivo | Redes administradas /Conexiones | Notas                         |
|------------|--------------------------------- |-------------------------------|
| RA         | LA, EA_H, EA_B                   | Conexión a Internet (bridged) |
| RB         | EB_C, EB_G, LB, EA_B             |                               |
| RC         | EC_F, EC_ZY, LC, EB_C            |                               |
| RZY        | EZY_E, EC_ZY, LY, LZ             |                               |
| RE         | EE_F, EE_L, LE, EZY_E            |                               |
| RF         | EF_G, EF_K, LF, EE_F, EC_F       |                               |
| RG         | EG_H, EG_J, LG, EF_G, EB_G       |                               |
| RH         | EH_WX, LH, EG_H, EA_H            |                               |
| RWX        | LX, LW, EWX_J, EH_WX             |                               |
| RJ         | EJ_K, LJ, EWX_J, EG_J            |                               |
| RK         | EK_L, LK, EJ_K, EF_K             |                               |
| RL         | LL, EK_L                         |LL-Servidor DHCP               |



### Hosts

| Host | Tipo   | Conexión LAN | IP / Comentario |
|------|--------|--------------|-----------------|
| HA1  | Host   | LA           | Servidor DNS    |
| HB1  | Host   | LB           | Estática        |
| HC1  | Host   | LC           | Estática        |
| HE1  | Host   | LE           | Estática        |
| HF1  | Host   | LF           | Estática        |
| HG1  | Host   | LG           | Estática        |
| HH1  | Host   | LH           | Estática        |
| HX1  | Host   | LX           | Estática        |
| HW1  | Host   | LW           | Estática        |
| HJ1  | Host   | LJ           | Estática        |
| HK1  | Host   | LK           | Estática        |
| HL1  | Host   | LL           | Estática        |
| HL2  | Host   | LL           | Estática        |
| HL3  | Host   | LL           | Dinámica        |
| HL4  | Host   | LL           | Dinámica        |
| HL5  | Host   | LL           | Dinámica        |
| HY1  | Host   | LY           | Estática        |
| HZ1  | Host   | LZ           | Estática        |

##

### 2. Estructura del laboratorio

```text
.
├── ha1
│   └── etc
│       └── bind
│           ├── db.empresa.izo
│           ├── db.rev.10
│           ├── db.rev.172.16
│           ├── db.rev.172.16.30
│           ├── db.rev.172.20
│           ├── db.rev.172.31
│           ├── db.rev.192.168
│           ├── db.rev.192.168.0
│           ├── named.conf.local
│           └── named.conf.options
├── ha1.startup
├── hb1.startup
├── hc1.startup
├── he1.startup
├── hf1.startup
├── hg1.startup
├── hh1.startup
├── hj1.startup
├── hk1.startup
├── hl1.startup
├── hl2.startup
├── hl3.startup
├── hl4.startup
├── hl5.startup
├── hw1.startup
├── hx1.startup
├── hy1.startup
├── hz1.startup
├── imagenes
│   ├── captura_general.png  
│   └── topologia_de_red_tp_final.png
│ 
├── lab.conf
├── metricas.txt
├── ra
│   └── etc
│       └── quagga
│           ├── daemons
│           ├── ospfd.conf
│           └── zebra.conf
...
├── rl
│   └── etc
│       ├── default
│       │   └── isc-dhcp-server
│       ├── dhcp
│       │   └── dhcpd.conf
│       └── quagga
│           ├── daemons
│           ├── ospfd.conf
│           └── zebra.conf
└── shared
    ├── capturas
    │   └── General.pcap
    └── dhcp
        └── isc-dhcp-server_4.4.3-P1-2_amd64.deb
##
### 3. Configuración del Servidor DNS

  Instalación y configuración de un servidor DNS en el host **HA1**.  
  Se crearon todas las zonas directas e inversas para hosts y routers.  
  Forwarder configurado hacia Internet `8.8.8.8` para resolución de nombres externos.


### Convención de nombres para zonas internas

  Cada registro de una zona directa e inversa sigue la forma:
 dig <nombre_del_router_o_host>.empresa.izo
 dig -x <ip_correspondiente>

### Verificación rápida del servicio DNS

- Verificar estado del servicio dentro del host HA1:
  systemctl status bind9

- Verificar que el DNS escucha en el puerto 53:
  ss -tulnp | grep :53`

- Resolución de nombres y consultas de prueba:

### Resolución directa interna
dig ha1.empresa.izo

### Resolución inversa interna
dig -x 192.168.10.1

### Resolución directa externa (Internet)
dig www.google.com

### Resolución inversa externa (Internet)
dig -x 8.8.8.8

### Estructura de archivos BIND

- `named.conf.local` → define todas las zonas directas e inversas  
- `named.conf.options` → forwarders, acceso a la LAN y opciones generales  
- Archivos de zona directa: `db.empresa.izo`  
- Archivos de zona inversa: `db.rev.*` (según red IP)

### Optimización del laboratorio

- Toda la configuración de BIND se encuentra en:
  /ha1/etc/bind/
- Los archivos se copian automáticamente al iniciar el laboratorio  
- La configuración respeta la nomenclatura de routers y hosts según la topología  
- Se crearon todas las zonas directas e inversas para hosts y routers  
- Forwarder configurado hacia Internet `8.8.8.8`  

### Enrutamiento dinámico OSPF

- OSPF configurado en todos los routers usando Quagga/Zebra.  
- Algoritmo de cálculo: Dijkstra, utilizado para determinar rutas más cortas.  
- Verificación de comunicación entre todas las interfaces y conexión a Internet.  

### Métricas definidas en `metricas.txt`

ra,1,25
ra,2,20
ra,3,10
rb,0,32
rb,1,33
...
rl,0,33
rl,3,66

-  Cada línea define el costo OSPF de una interfaz. 
-  El protocolo utilizará estos valores para calcular la mejor ruta (menor costo).

### Configuración del protocolo OSPF

En cada router se edita el archivo:
   /etc/quagga/ospfd.conf

Ejemplo de configuración:
hostname ra
password zebra
enable password zebra

router ospf
 router-id 1.1.1.1
 network 192.168.100.28/30 area 0
 network 192.168.0.0/24 area 0

interface eth1
 ip ospf cost 25

interface eth2
 ip ospf cost 20

- `router-id`: identificador único del router  
- `network`: redes que el router anuncia en OSPF  
- `ip ospf cost`: costo asignado a la interfaz  

Este procedimiento se repite en todos los routers del laboratorio.

### Configuración del servicio Zebra

Archivo:        
       /etc/quagga/zebra.conf

Ejemplo básico:

hostname ra
password zebra
enable password zebra

Zebra se encarga de instalar las rutas en la tabla del sistema.

### Inicialización de los servicios
Antes de iniciar OSPF se eliminan procesos anteriores:

pkill ospfd

Luego se inician los servicios:

ospfd -d -f /etc/quagga/ospfd.conf

### Verificación del funcionamiento
Los siguientes cComandos se ejecutan dentro de un router (nodo) del laboratorio:
ping  <IP_destino>

Accede a la consola de Quagga:
vtysh

Comandos OSPF
show ip ospf neighbor
show ip route ospf
show ip ospf database
show ip ospf interface

##

### 4. Configuración DHCP 
    Se implementó un servidor DHCP en el router RL, el cual actúa como gateway de la red LAN (192.168.0.254).
    El servicio DHCP opera sobre la interfaz eth1, conectada a la red LL.
    Se configuraron dos hosts con IP fija y el resto con asignación dinámica:

    | Host | Tipo de IP           | Dirección                   |
    | ---- | -------------------- | ----------------------------|
    | HL1  | Estática (reservada) | 192.168.0.1                 |
    | HL2  | Estática (reservada) | 192.168.0.2                 |
    | HL3  | Dinámica             | 192.168.0.3 – 192.168.0.253 |
    | HL4  | Dinámica             | 192.168.0.3 – 192.168.0.253 |
    | HL5  | Dinámica             | 192.168.0.3 – 192.168.0.253 |
    

### Lease Time configurado
    El tiempo de concesión DHCP fue configurado en 5 minutos (300 segundos) según la consigna del laboratorio:
    default-lease-time 300;
    max-lease-time 300;
 
### Configuración del servidor DHCP
    los archivos necesarios para el funcionamiento del servicio son:
    rl/etc/dhcp/dhcpd.conf
    rl/etc/default/isc-dhcp-server

###  verirficación del servicio
    En el router RL:   ps aux | grep dhcpd
                     ss -lunp | grep 67
     En los hots hl3,hl4,hl5 hacemos prueba de conectividad 
     ping -c3 192.168.0.254
     ping -c3 8.8.8.8  

### Procedimiento de renovación de dirección IP mediante DHCP
   Liberación de la dirección IP actual:
   dhclient -r eth1
   Se envía un mensaje al servidor DHCP indicando la liberación de la concesión actual, permitiendo que la dirección IP vuelva a estar disponible en el pool.

   Solicitud de una nueva dirección IP:
   dhclient eth1
   El cliente DHCP inicia el proceso de obtención de una nueva dirección IP sobre la interfaz eth1.
   
   Intercambio de mensajes del protocolo DHCP:
   Durante este proceso se produce la siguiente secuencia:

   DHCPDISCOVER (broadcast): el cliente busca servidores DHCP disponibles en la red.
   DHCPOFFER: el servidor (RL) responde ofreciendo una dirección IP disponible.
   DHCPREQUEST: el cliente solicita formalmente la IP ofrecida.
   DHCPACK: el servidor confirma la asignación y entrega los parámetros de red.
   Asignación de dirección IP:
   El cliente recibe una dirección IP dentro del rango configurado en el servidor DHCP (192.168.0.3 – 192.168.0.253).
   Verificación de la configuración:
   ip addr show eth1
   Se verifica que la interfaz eth1 tenga asignada correctamente una dirección IP válida.
   Prueba de conectividad al gateway:
   ping 192.168.0.1
   Se comprueba la conectividad hacia el gateway, validando el correcto funcionamiento del servicio DHCP.                
##

### 5 Captura de tráfico para análisis 
   Para la captura de tráfico de red y su posterior análisis en Wireshark, se utilizó la herramienta `tcpdump.
   Esta herramienta permite capturar paquetes directamente desde las interfaces de red de los hosts o routers, generando archivos en formato `.pcap
   El procedimiento se realizó tomando como ejemplo el host **HL3**.

#### Procedimiento para realizar  la escuhca y captura
   Captura de tráfico (Wireshark)
   Se realizó la captura de tráfico utilizando tcpdump dentro de los hosts del laboratorio, con el objetivo de analizar protocolos como DHCP y DNS.
   Ejecución básica
   Conectarse a un host (ej: HL3) y comenzar la captura:
   kathara connect hl3
   tcpdump -i eth1 -w /shared/capturas/captura_general.pcap
   Generar tráfico de red:
   dhclient -r eth1
   dhclient eth1
   dig ha1.empresa.izo
   ping -c3 192.168.0.1
   La captura generada puede analizarse posteriormente en Wireshark.
